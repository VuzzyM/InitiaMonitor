## InitiaMonitor

InitiaMonitor: This tool is to monitor the status of the node/validator.

### This tool is intended to monitor and alert on telegram any problem related to the cosmos node:

- Server down
- Out of memory (<10%)
- Out of disk space (<10%)
- Out of disk space within 24h
- High CPU load (>85%)

### Cosmos-based validator alerts

- Missing blocks
- Degraded syncing (sync less than 40 blocks in last 5 min)
- Low peers count (<5)

### Used containers from Docker

- [grafana sever](https://hub.docker.com/r/grafana/grafana)
- [node_exporter](https://hub.docker.com/r/prom/node-exporter)
- [prometheus](https://hub.docker.com/r/prom/prometheus)
- [alertmanager](https://hub.docker.com/r/prom/alertmanager)

### Login Data http://<your_server_ip>:3000

 - username: admin
 - default password: admin
 
## Coommands for run this monitoring tool:

 1. Install Docker first
```
bash <(curl -s https://raw.githubusercontent.com/VuzzyM/InitiaMonitor/main/utils/install_docker.sh)
```
![1](https://github.com/VuzzyM/initia-monitor/assets/66425682/64bb4955-4262-4daa-9f0b-323fcc565bac)

2. Clone the repo
```
cd ~
git clone https://github.com/VuzzyM/InitiaMonitor.git
```
![2](https://github.com/VuzzyM/initia-monitor/assets/66425682/39ffee41-813e-410e-8e0b-8e83fac1ca91)

3. Create configuration files from examples
```
cd monitoring-tool
cp prometheus/prometheus.yml.example prometheus/prometheus.yml
cp alertmanager/config.yml.example alertmanager/config.yml
```
![3](https://github.com/VuzzyM/initia-monitor/assets/66425682/7379f23b-3d72-4641-b0c6-cdf78f34ef57)

4. Start containers
```
sudo docker compose up -d
```
![5](https://github.com/VuzzyM/initia-monitor/assets/66425682/c1b96bf7-2483-4032-9ed8-49f5f1da82a2)


  ## How to set up
 ### Servers to monitor
Add your servers with installed [node_exporter](https://github.com/prometheus/node_exporter) or installed cosmos-based node with enabled prometheus port to file `prometheus/prometheus.yml`

![7](https://github.com/VuzzyM/initia-monitor/assets/66425682/29f03cc1-e08b-41d9-9ea6-3508ccfd04c4)

```
  # example for servers with node_exporter installed
  - job_name: "my-servers"
    static_configs:
    - targets: ["172.0.0.1:9100"]
      labels:
        instance: "server1"
    - targets: ["172.0.0.2:9100"]
      labels:
        instance: "server2"
    
  # example for servers with node_exporter and cosmos-based node installed
  - job_name: "cosmos-validator-nodes"
    static_configs:
    - targets: ["192.0.0.1:9100","192.0.0.1:26660"]
      labels:
        instance: "validator1"
    - targets: ["192.0.0.2:9100","192.0.0.2:26660"]
      labels:
        instance: "validator2"
    - targets: ["192.0.0.3:9100","192.0.0.3:26660"]
      labels:
        instance: "validator3"
```

## Don't forgot to turn on Prometheus metrics

```
sudo nano $HOME/.initia/config/app.toml
```

![8](https://github.com/VuzzyM/initia-monitor/assets/66425682/b518df67-551f-4ee3-9c56-af5408c85624)

![9](https://github.com/VuzzyM/initia-monitor/assets/66425682/42ddf3ea-fe20-4315-a84d-7afa7e946b96)


### Telegram notifications
In order to enable telegram notifications, create your own bot and fill in the following fields in the file <b>alertmanager/config.yml</b>
```
chat_id=1111111                 # your telegram user id
bot_token=11111111:AAG_XXXXXXX  # your telegram bot token
```

### You can see the metrics and graphs below

![10](https://github.com/VuzzyM/InitiaMonitor/assets/66425682/e342e616-38e3-4b78-abca-7ae86c2f3690)

![11](https://github.com/VuzzyM/InitiaMonitor/assets/66425682/a3fa8c08-6a7a-4f98-9eac-4dcf039fc0b2)

![12](https://github.com/VuzzyM/InitiaMonitor/assets/66425682/4b1c249c-ca03-4791-9166-a70b81d705d7)

## How to update
How to update the docker container
```
cd monitoring-tool
sudo docker-compose pull
sudo docker-compose up -d
```
Thanks for the support received from the [nodejumper](https://github.com/nodejumper-org) team
