# Monitoring-with-Prometheus-and-Grafana

## [Prometheus](https://github.com/prometheus/prometheus):
  - Open source **monitoring tool**
  - time series database
  - built by soundcloud (2012-13), later open sourced (2015)
  - joined CNCF in 2015
  - ideal for monitoring **on premise** as well as **cloud workloads.**
  - written in Go
  - includes flexible query language.
  - It collects the metrics from monitored targets by **scraping metrics HTTP endpoints.**
  
## [Node Exporter](https://github.com/prometheus/node_exporter):
  - used for monitoring the Nodes (Servers) with prometheus.
  - It is a prometheus exporter for hardware and OS metrics exposed by LINUX/UNIX kernels, written in Go with pluggable metric collectors.
  - The [WMI exporter](https://github.com/martinlindhe/wmi_exporter) is recommended for Windows users.
  - Node exporter will expose machine metrics of Linux/Unix machines. e.g. CPU Usage, memory usage,etc
  - by default it runs on port 9100.
  
## Service Discovery
  - Having to manually update list of machines in configuration file gets quite annoying after a while. Service discovery allows you to automatically discover and monitor your service or services.
  - Definition: Service discovery is automatic detection of devices and services offered by these devices on computer network.
  - Prometheus has support for 
    - cloud providers: AWS, Azure, Google, etc.
    - cluster managers: Kubernetes, Marathon, etc.
    - generic mechanisms: DNS,Consul, Zookeeper,etc.

## [Exporters](https://prometheus.io/docs/instrumenting/exporters/)
  - exporters are build for exporting prometheus metrics from existing 3rd party metrics.
  - Examples: MySQL server exporter, Memcatched exporter, consul exporter,Node/system exporter, etc.
  
## [Alerting](https://prometheus.io/docs/alerting/overview/)
  - Alerting with Prometheus is separated into two parts. 
    - Alerting rules in Prometheus server.
    - The Alertmanager
  - The main steps to setting up alerting and notifications are:
    - Setup and configure the Alertmanager
    - Configure Prometheus to talk to the Alertmanager
    - Create alerting rules in Prometheus
  - the best prectice is to separate the alerts from prometheus config. Add an include in prometheus.yml
    ```
    rule_files:
      - "etc/prometheus/alerts.rules"
    ```
  - Alert format:
    ```
     alert <alert name>
        if <expression>
          [for <duration>]
          [labels <label set>]
          [annotations <label set>]
    ```
    - [DEfining Alerting rules](https://prometheus.io/docs/prometheus/latest/configuration/alerting_rules/#defining-alerting-rules)
   
   - **Alertmanager**
      - handles the alerts fired by prometheus server
      - handles deduplication, grouping and routing of alerts
      - routes alerts to receivers (Pagerduty, Opsgenie, email, slack, etc)
      - Alertmanager configuration is written in: /etc/alertmanager/alertmanager.yml
      - you can create a high available alertmanager cluster using mesh confug.
      - Do not load balence this service
        - use list of alertmanager nodes in prometheus config.
      - All alerts are sent to all known alertmanager nodes
      - Guarantees the notification is at least send once.
      - Alertmanager runs on port 9093
      - Alertmanager Concepts:
        - Grouping: groups similar alerts into one notification.
        - Inhibition: Silence other alerts if one specified alert is already fixed.
        - Silences: A simple way to mute certain notifications.
      - Alert states:
        - Inactive: No rule is met
        - Pending: Rule is met but suppressed due to validations.
        - Firing: Alert is sent to configured channel. (mail, slack, etc)
    
## Important Links:
  - [Prometheus Documentation](https://prometheus.io/docs/)
  - [promQL Basics](https://prometheus.io/docs/prometheus/latest/querying/basics/)
  - [Querying Examples](https://prometheus.io/docs/prometheus/latest/querying/examples/#query-examples)
  - [Configuration file](https://prometheus.io/docs/prometheus/latest/configuration/configuration/)
