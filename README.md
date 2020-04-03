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

## Important Links:
  - [Prometheus Documentation](https://prometheus.io/docs/)
  - [promQL Basics](https://prometheus.io/docs/prometheus/latest/querying/basics/)
  - [Querying Examples](https://prometheus.io/docs/prometheus/latest/querying/examples/#query-examples)
  - [Configuration file](https://prometheus.io/docs/prometheus/latest/configuration/configuration/)
