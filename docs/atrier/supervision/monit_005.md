---
title: Monitoring your system with Docker + Grafana + Prometheus + Node
description: 
published: true
date: 2023-06-10T06:57:36.899Z
tags: 
editor: markdown
dateCreated: 2023-06-10T06:56:11.998Z
---

# Monitoring your system with Docker + Grafana + Prometheus + Node

In this guide, we will learn how to run Grafana, Prometheus and Node Exporter as Docker containers, with the containers managed by [Docker Compose](https://docs.docker.com/compose/). You’ll mount the relevant host directories into the Node Exporter, Prometheus and Grafana containers, configure Prometheus to scrape Node Exporter metrics.

**Prometheus** is an open-source software application used for event monitoring and alerting. It records real-time metrics in a [time series database](https://en.wikipedia.org/wiki/Time_series_database) with flexible queries and real-time alerting. A simple user interface where you can visualize, query, and monitor all the metrics.

**Grafana** is an open-source platform for data visualization, monitoring, and analysis. In Grafana, users can create dashboards with panels representing specific metrics over a set time frame. Grafana supports graphs, tables, heatmap, and free text panels and integration with official and community-built plugins and apps that could be visualized.

## Let’s build our Docker-Compose

```yaml
version: ‘3.8’
volumes:
prometheus_data:
grafana-data:
services:
node-exporter:
image: prom/node-exporter:latest
container_name: node-exporter
restart: unless-stopped
volumes:
- /proc:/host/proc:ro
- /sys:/host/sys:ro
- /:/rootfs:ro
command:
- ‘ — path.procfs=/host/proc’
- ‘ — path.rootfs=/rootfs’
- ‘ — path.sysfs=/host/sys’
- ‘ — collector.filesystem.mount-points-exclude=^/(sys|proc|dev|host|etc)($$|/)’
ports:
- 9100:9100
networks:
- monitoring
prometheus:
image: prom/prometheus:latest
container_name: prometheus
restart: unless-stopped
volumes:
- D:/Docker/prometheus/prometheus.yml:/etc/prometheus/prometheus.yml
- D:/Docker/prometheus:/prometheus
command:
- ‘ — config.file=/etc/prometheus/prometheus.yml’
- ‘ — storage.tsdb.path=/prometheus’
- ‘ — web.console.libraries=/etc/prometheus/console_libraries’
- ‘ — web.console.templates=/etc/prometheus/consoles’
- ‘ — web.enable-lifecycle’
ports:
- 9090:9090
networks:
- monitoring
grafana:
image: grafana/grafana:7.5.7
container_name: grafana
ports:
- 3000:3000
restart: unless-stopped
volumes:
- D:/Docker/grafana/provisioning/datasources:/etc/grafana/provisioning/datasources
- D:/Docker/grafana/grafana:/var/lib/grafana
networks:
- monitoring
networks:
monitoring:
driver: bridge
```


Go to file `prometheus.yml`. We need to change the configuration of Prometheus.

```yaml
global:
scrape_interval: 1m
scrape_configs:
- job_name: “prometheus”
scrape_interval: 1m
static_configs:
- targets: [“172.25.0.4:9090”]
- job_name: “node”
static_configs:
- targets: [“172.25.0.3:9100”]
```

Change the targets with your container IP. And restart the Prometheus container.

To see if it work, you can check the logs or go to http://localhost:9090/

Press **Status**, and then **Service Discovery**.

<img src="https://miro.medium.com/max/640/1*6YqQUaKvRCiI0HJv2mKzyg.webp">


Open the Grafana, and you have to log in (http://localhost:3000/).

**Username**: admin

**Password**: admin

We need to add a **Data Store Source**. To do this, go to **Configuration** and **Add data source**.

<img src="https://miro.medium.com/max/4800/1*pmyVRRAkZv7w81POPla9Ag.webp">

Select Prometheus, and you can configure everything you need to get the data. In my case, I just need the URL.

<img src="https://miro.medium.com/max/720/1*-8zYJX6clssYP0rfPFRCWQ.webp">

Now we are ready to show the data in a friendly dashboard. You have choices, create your dashboard or import a dashboard. I leave the link [Dashboards | Grafana Labs](https://grafana.com/grafana/dashboards/), so you can search.

## The dashboard

<img src="https://miro.medium.com/max/4800/1*Zrb92Rp0wx8HdTQ_n2lLdw.webp">

Source : https://medium.com/geekculture/monitoring-your-system-with-docker-grafana-prometheus-node-d7fae11416f3