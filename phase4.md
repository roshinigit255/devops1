## Setting up Monitering for the Boardgame

- Create a ec2 instance and install prometheus

```bash
wget https://github.com/prometheus/prometheus/releases/download/v3.11.1/prometheus-3.11.1.linux-amd64.tar.gz
tar -xvf prometheus-3.11.1.linux-amd64.tar.gz
rm -rf prometheus-3.11.1.linux-amd64.tar.gz
```
Switch to promatheus directory and exicute it ,u can acess the server through port 9090.

Edit the promatheus.yml file 
```bash
- job_name: 'blackbox'
    metrics_path: /probe
    params:
      module: [http_2xx]  # Look for a HTTP 200 response.
    static_configs:
      - targets:
        - http://prometheus.io    # Target to probe with http.
        - http://13.233.162.197:32378   # Target to probe with https.
    relabel_configs:
      - source_labels: [__address__]
        target_label: __param_target
      - source_labels: [__param_target]
        target_label: instance
      - target_label: __address__
        replacement: 13.206.98.139:9115  # The blackbox exporter's real hostname:port.
```

- Install Grafana
  
```bash 
sudo apt-get install -y adduser libfontconfig1 musl
wget https://dl.grafana.com/grafana-enterprise/release/12.4.2/grafana-enterprise_12.4.2_23531306697_linux_amd64.deb
sudo dpkg -i grafana-enterprise_12.4.2_23531306697_linux_amd64.deb
sudo /bin/systemctl start grafana-server
```
Can acess the grafana UI through port number 3000.

We can create datasource for prometheus and import dashborad ,can moniter the metrics.

- Install Blackbox exporter

```bash
wget https://github.com/prometheus/blackbox_exporter/releases/download/v0.28.0/blackbox_exporter-0.28.0.linux-amd64.tar.gz
tar -xvf blackbox_exporter-0.28.0.linux-amd64.tar.gz 
rm -rf blackbox_exporter-0.28.0.linux-amd64.tar.gz 
```
Switch to blackbox directory and exicute ./blackbox_exporter ,u can acess the server through port 9115.




