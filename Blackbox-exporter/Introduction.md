# Blackbox exporter:
- The Blackbox exporter is a probing exporter used to **monitor network endpoints** such as HTTP, HTTPS, DNS, ICMP or TCP endpoints.
- It is majorly used to **measure response times**
- It provides the metrics about **HTTP latencies, DNS lookups latencies** as well as SSL certificates expiration.
-  if the Blackbox exporter is running on port 9115, and if we query metrics for google.com, this is the endpoint that we can query from the exporter.
  
    `$ http://localhost:9115/probe?target=https://google.com&module=https_2xx`
  
- **[Blackbox Modules](https://github.com/prometheus/blackbox_exporter/blob/master/CONFIGURATION.md):**
  - the blackbox exporter is configured with a YAML configuration file.
  - The Blackbox exporter configuration file is made of modules.
  - A module can be seen as one probing configuration for the Blackbox exporter.
  
- **How does the Blackbox exporter differs from application instrumenting?**
  - application instrumenting means that you are adding client libraries to an application in order to expose metrics to Prometheus, i.e. doing the changes in application code.
  - While using Blackbox exporter, we don't have to make any changes in the code. 
  - It only gives a request failure or request success metrics.
  - The Blackbox exporter only focuses on availability while instrumentations can go more into details about performance like performance of single SQL Query.
  
  
### Downloading the blackbox exporter:
  
- `wget https://github.com/prometheus/blackbox_exporter/releases/download/v0.16.0/blackbox_exporter-0.16.0.linux-amd64.tar.gz`
- `tar xvzf blackbox_exporter-0.16.0.linux-amd64.tar.gz`
- It will contain following files:
  ```
  -rwxr-xr-x 1 3434 3434 17050332 Nov 11 21:57 blackbox_exporter
  -rw-r--r-- 1 3434 3434      629 Nov 11 22:08 blackbox.yml
  -rw-r--r-- 1 3434 3434    11357 Nov 11 22:08 LICENSE
  -rw-r--r-- 1 3434 3434       94 Nov 11 22:08 NOTICE
  ```
  - **blackbox_exporter:** the executable for the Blackbox exporter. This is the executable that you are going to launch via your service file in order to probe targets.
  - **blackbox.yml:** configuration file for the Blackbox exporter. This is where you are going to define your modules and probers.
  
### Create a service file for blackbox exporter

- `sudo mv blackbox_exporter /usr/local/bin` : Make the executable accessible for your local user account.
- Create configuration folder for blackbox exporter: `sudo mkdir -p /etc/blackbox`
- `sudo mv blackbox.yml /etc/blackbox` : Move the configuration file to that folder
- For safety purposes, the Blackbox exporter is going to be run by its own user account (named blackbox), we need to create a user account for the same.
  
  `sudo useradd -rs /bin/false blackbox`
- Then, make sure that the blackbox binary can be run by your newly created user.
  
  `sudo chown blackbox:blackbox /usr/local/bin/blackbox_exporter`
- Give the correct permissions to your configuration folders recursively.
  
   `sudo chown -R blackbox:blackbox /etc/blackbox/*`
- To create the Blackbox exporter service, go to the `/lib/systemd/system` folder and create a service named `blackbox.service`
  ```
  cd /lib/systemd/system
  sudo touch blackbox.service
  ```
- Edit your service file, and paste the following content in it.
  ```
    [Unit]
    Description=Blackbox Exporter Service
    Wants=network-online.target
    After=network-online.target

    [Service]
    Type=simple
    User=blackbox
    Group=blackbox
    ExecStart=/usr/local/bin/blackbox_exporter \
      --config.file=/etc/blackbox/blackbox.yml \
      --web.listen-address=":9115"

    Restart=always

    [Install]
    WantedBy=multi-user.target
    ```
- make sure that your service is enabled at boot time.
    ```
    sudo systemctl enable blackbox.service
    sudo systemctl start blackbox.service
    ```
- If your service is correctly running, you can check the metrics using 

  `curl http://localhost:9115/metrics`
  
### Binding the blackbox exporter with prometheus
- To bind the Blackbox exporter with Prometheus, you need to add it as a scrape target in Prometheus configuration file which is stored at `/etc/prometheus/prometheus.yml`
- Edit that configuration file, and make the following changes:
  ```
  global:
  scrape_interval:     15s
  evaluation_interval: 15s

  scrape_configs:
  - job_name: 'prometheus'
    static_configs:
    - targets: ['localhost:9090', 'localhost:9115']
  ```
- `ps aux | grep prometheus`
- `sudo kill -HUP <pid>`
- Go to http://localhost:9090/targets and check you are correctly scraping blackbox exporter.
 
### Monitoring HTTPS endpoints with the Blackbox Exporter:
- Creating a Blackbox module
  - Add the following content in `/etc/blackbox/blackbox.yml`
    ```
    modules:
    http_prometheus:
      prober: http
      timeout: 5s
      http:
        valid_http_versions: ["HTTP/1.1", "HTTP/2"]
        method: GET
        fail_if_ssl: false
        fail_if_not_ssl: true
        tls_config:
          insecure_skip_verify: true
        basic_auth:
          username: "username"
          password: "password"
    ```
  - Parameters that we are checking:
    - fail_if_not_ssl: as we are actively monitoring a HTTPS endpoint, we need to make sure that we are retrieving the page with SSL encryption. Otherwise, we count it as a failure.
    - insecure_skip_verify: if you followed our previous tutorial, we generated our certificates with self-signed certificates. As a consequence, you are not able to verify it with a certificate authority.
    - basic_auth: the reverse proxy endpoint is configured with a basic username/password authentication. The Blackbox exporter needs to be aware of those to probe the Prometheus server.
  - To check the configuration, use `blackbox_exporter --config.check`
  - Restart the blackbox. (Similarly to the prometheus)
  
- Binding the Blackbox exporter module in prometheus
  ```
    - job_name: 'blackbox'
      metrics_path: /probe
      params:
        module: [http_prometheus] 
      static_configs:
        - targets:
          - https://localhost:8080    # Target to probe with https.
      relabel_configs:
        - source_labels: [__address__]
          target_label: __param_target
        - source_labels: [__param_target]
          target_label: instance
        - target_label: __address__
          replacement: localhost:9115  # The blackbox exporter's real hostname:port.
  ```
  - Save the changes and restart the prometheus.
  
  
  
