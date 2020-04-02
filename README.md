# Monitoring-with-Prometheus-and-Grafana

## Prometheus:
  - Open source **monitoring tool**
  - time series database
  - built by soundcloud (2012-13), later open sourced (2015)
  - joined CNCF in 2015
  - ideal for monitoring **on premise** as well as **cloud workloads.**
  - written in Go
  - includes flexible query language.
  - It collects the metrics from monitored targets by **scraping metrics HTTP endpoints.**
  
## Node Exporter:
  - used for monitoring the Nodes (Servers) with prometheus.
  - It is a prometheus exporter for hardware and OS metrics exposed by LINUX/UNIX kernels, written in Go with pluggable metric collectors.
  - The [WMI exporter](https://github.com/martinlindhe/wmi_exporter) is recommended for Windows users.
  - Node exporter will expose machine metrics of Linux/Unix machines. e.g. CPU Usage, memory usage,etc
  - by default it runs on port 9100.
