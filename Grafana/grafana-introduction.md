## Grafana Provisioning

  - we can setup Grafana using the UI
  - Rather than using UI, you can use yaml or json files to provision Grafana with datasources and dashboards.
  - This is much more powerful way of using grafana, as you can test new dashboards first on dev / test server, then import the newly created dashboard to productuin.
  
- The configuration of grafana is in `/etc/grafana` :
  ```
  -rw-r-----   1 root grafana 21339 Jan 27 15:58 grafana.ini
  -rw-r-----   1 root grafana  2289 Jan 27 15:58 ldap.toml
  drwxr-xr-x   5 root grafana  4096 Jan 27 15:58 provisioning
  ```
- `/etc/grafana/provisioning/` :
  ```
    drwxr-xr-x 2 root grafana 4096 Jan 27 15:58 dashboards
    drwxr-xr-x 2 root grafana 4096 Jan 27 15:58 datasources
    drwxr-xr-x 2 root grafana 4096 Jan 27 15:58 notifiers
  ```
- the data is kept in `/var/lib/grafana` :
    ```
      -rw-r-----  1 grafana grafana 585728 Apr  6 10:04 grafana.db
      drwxr-x---  3 grafana grafana   4096 Feb 20 17:11 plugins
      drwx------  2 grafana grafana   4096 Jan 27 15:59 png
    ```
- We can change the database and paths in `/etc/grafan/grafan.ini`


## Demo (Provisioning grafana using console)
- Install grafana using `2-install-grafana.sh` script
- Copy the `datasource-prometheus.yml` file into `etc/grafana/provisioning/datasources` folder
- Copy the `dashboards.yml` file into `etc/grafana/provisioning/dashboards` folder.
- Copy `node-dashboard.json` (you can use any exported json file) to `var/lib/grafana/dashboards` (this path is written in dashboards.yml file) folder.
- Go to `server-ip:3000` in browser, go to Home and under `General` header, you will see Node Statistics Dashboard.
- In this way, we have created a dashboard using console

- To export the chart, go to settings -> JSON Model
