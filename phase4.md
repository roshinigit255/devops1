## Setting up Monitering for the Boardgame

- Create a ec2 instance and install prometheus

```bash
wget https://github.com/prometheus/prometheus/releases/download/v3.11.1/prometheus-3.11.1.linux-amd64.tar.gz
tar -xvf prometheus-3.11.1.linux-amd64.tar.gz
rm -rf prometheus-3.11.1.linux-amd64.tar.gz
```
Switch to promatheus directory and exicute it ,u can acess the server through port 9090.

- Install Grafana
  
```bash 
sudo apt-get install -y adduser libfontconfig1 musl
wget https://dl.grafana.com/grafana-enterprise/release/12.4.2/grafana-enterprise_12.4.2_23531306697_linux_amd64.deb
sudo dpkg -i grafana-enterprise_12.4.2_23531306697_linux_amd64.deb
sudo /bin/systemctl start grafana-server
```
Can acess the grafana UI through port number 3000.

- Install Blackbox exporter

```bash
wget https://github.com/prometheus/blackbox_exporter/releases/download/v0.28.0/blackbox_exporter-0.28.0.linux-amd64.tar.gz
tar -xvf blackbox_exporter-0.28.0.linux-amd64.tar.gz 
rm -rf blackbox_exporter-0.28.0.linux-amd64.tar.gz 
```

