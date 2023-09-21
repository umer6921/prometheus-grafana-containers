# Prometheus and Grafana intergration
Prometheus is a open source that allows you to use the observability principles like alerting, monitoring for metrics using time series db. Metrics include CPU usage, CPU load, RAM, Nodes status etc.
## Prometheus Architectrue 
Prometheus architecture contains the three main components:
1) **Retriver** that gather metrics data from different target data sources and store them in time series db.
2) **Time series db** is like a data store where the metrics is stored. It support PromQL query language
3) **HTTP** server is used to fetch the data from db and query them into UI representation.
## What is cAdvisor?
cAdvisor is a tool that allows you to make containers group to collect their resources collectively and expose them in a unified URL. This approach is used in prometheus to monitor the running
containers metrics.

![image](https://github.com/umer6921/prometheus-grafana-containers/assets/75561123/b43058e1-c39c-41fd-99c6-99b600dcfd54)

## What is Redis?
Redis is a document cache needs to store the data of the cAdvisor. In other words, When to use cAdvisor, we also need Redis.
## What is node-exporter?
Node exporter is a tool that allow you to gather the nodes metrics. The prometheus used the pull mechanism so the retriver component pull the metrics from the node exporter to prometheus db.  
## Steps to install prometheus
1) Download the prometheus config file
  ``Wget https://raw.githubusercontent.com/prometheus/prometheus/main/documentation/examples/prometheus.yml``
2) Install Prometheus, cAdvisor, Redis using Docker-compose
3) Add cAdvisor target in the prometheus confg file
``scrape_configs:
    -job_name: cadvisor
        scrape_interval: 5s
        static_configs:
          - targets:
             - cadvisor:8080``
4) Run the docker-compose
   ``docker-compose up -d``

## To intergrate with Grafana
Add data source of Prometheus and generate the UIs for your desired metrics
### Steps to install Grafana
1) Install the prerequisite packages ``sudo apt-get install -y apt-transport-https software-properties-common wget``
2) Import the GPG key ``sudo mkdir -p /etc/apt/keyrings/`` ``wget -q -O - https://apt.grafana.com/gpg.key | gpg --dearmor | sudo tee /etc/apt/keyrings/grafana.gpg > /dev/null``
3) To add a repository for stable releases ``echo "deb [signed-by=/etc/apt/keyrings/grafana.gpg] https://apt.grafana.com stable main" | sudo tee -a /etc/apt/sources.list.d/grafana.list``
4) Run the following command to update the list of available packages ``sudo apt-get update``
5) To install Grafana OSS ``sudo apt-get install grafana``
6) To start Grafana Server ``sudo /bin/systemctl start grafana-server``
7) Open port 3000 from inbound traffic rules for Grafana ``http://54.197.44.172:3000/``

![garfana graphs](https://github.com/umer6921/prometheus-grafana-containers/assets/75561123/b46d90ca-6df2-4030-9e1f-81f3932697f1)
