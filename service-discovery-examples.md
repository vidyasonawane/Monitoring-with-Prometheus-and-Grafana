### EC2
  - Add the following in prometheus.yml
  ```
scrape_configs:
  - job_name: 'node'
    ec2_sd_configs:
      - region: eu-west-1
        access_key: <put access key here>
        secret_key: <put secret key here>
        port: 9100
    relabel_configs:
        # Only monitor instances with tag name starting with "PROD"
      - source_labels: [_meta_ec2_tag_name]
        regex: PROD.*
        action: keep
        # Use the instance id as instance label
      - source_labels: [_meta_ec2_instance_id]
        target_label: instance
```
  - Make sure user has the following IAM role: AmazonEC2ReadOnlyAccess
  - Make sure your security groups allow access to port (9100, 9090).
  - the more information can be found [here](https://prometheus.io/docs/prometheus/latest/configuration/configuration/#ec2_sd_config).
  - Relabeling is a powerful tool to dynamically rewrite the label set of a target before it gets scraped. Multiple relabeling steps can be configured per scrape configuration. They are applied to the label set of each target in order of their appearance in the configuration file.
  - The relabel_config documentation can be found [here](https://prometheus.io/docs/prometheus/latest/configuration/configuration/#relabel_config)
  
  
### Kubernetes
  - Add the following in prometheus.yml
  ```
scrape_configs:
  - job_name: 'kubernetes'
    kubernetes_sd_configs:
      - api_servers:
        - https://kubernets.default.svc
        in_cluster: true
        
        basic_auth:
        username: prometheus
        password: secret
        retry_interval: 5s
        
  - job_name: 'kubernetes-service-endpoints'
    kubernetes_sd_configs:
      - api_servers:
        - https://kube-master.prometheuscourse.com
        in_cluster: true

```
  - the more information can be found [here](https://prometheus.io/docs/prometheus/latest/configuration/configuration/#kubernetes_sd_config).
  
### DNS
  - Add the following in prometheus.yml
  ```
scrape_configs:
  - job_name: 'mysql'
    dns_sd_configs:
      - names:
        - metrics.mysql.example.com 
  - job_name: 'haproxy'
    dns_sd_configs:
      - names:
        - metrics.haproxy.example.com
```
  - the more information can be found [here](https://prometheus.io/docs/prometheus/latest/configuration/configuration/#dns_sd_config).
  

### File based service discovery
  - Add the following in prometheus.yml
  ```
scrape_configs:
  - job_name: 'dummy'
    file_sd_configs:
      - files:
        - targets.json
```
  - Format target.json
  ```
[
  {
    "targets": ["myslavel"9104","myslave2:9104"],
    "labels": {
      "env":"prod",
      "job": "mysql_slave"
    }
  },
  {
    "targets": ["mymaster:9104"],
    "labels": {
      "env":"prod",
      "job": "mysql_master"
    }
  }
]
```
  - the complete guide can be found [here](https://prometheus.io/docs/guides/file-sd/).
