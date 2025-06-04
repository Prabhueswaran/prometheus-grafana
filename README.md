# Promethus and Grafana on windows docker desktop

## 🔧 Prerequisites

- Docker Desktop installed and running on Windows.

- A basic understanding of Docker and docker-compose.

## 🚀 Running the Stack

- Clone the repo 

    `git clone https://github.com/Prabhueswaran/prometheus-grafana.git`

- navigate to the project directory

    `cd prometheus-grafana`

- Run the docker compose 
 
    `docker-compose up -d`

## 🌐 Access the Interfaces

- Prometheus UI: http://localhost:9090

- Grafana UI: http://localhost:3000

    - Default login: admin / admin

## 🧠 Next Steps in Grafana

- Log in to Grafana at http://localhost:3000.

- Go to Configuration > Data Sources > Add data source.

- Choose Prometheus.

- Enter the URL: http://prometheus:9090

- Save & test.

Now you can import dashboards or build your own from Prometheus metrics.

## Enhance your Prometheus + Grafana setup with:

1. ✅ A pre-built Grafana dashboard for system metrics (like CPU, memory, etc.).

2. 🐳 Optional: Add Docker metrics using cadvisor.


### ✅ 1. Import a Pre-Built Grafana Dashboard

Grafana provides many ready-to-use dashboards. For example, the popular Node Exporter Full dashboard (ID: 1860).

### Steps:
   1. Open Grafana at http://localhost:3000.

   2. Login (default is admin/admin).

   3. Go to Dashboards → Import.

   4. In the “Import via grafana.com” field, enter:
👉 1860

   1. Click Load, then select your Prometheus data source and Import.

You’ll now see detailed system metrics!


### 🐳 2. (Optional) Add Docker Metrics with cadvisor

To monitor Docker container metrics (CPU, memory, etc.), use cAdvisor.

#### Update your docker-compose.yml:
#### Add the following service under services::

```json
  cadvisor:
    image: gcr.io/cadvisor/cadvisor:latest
    container_name: cadvisor
    ports:
      - "8080:8080"
    volumes:
      - /:/rootfs:ro
      - /var/run:/var/run:rw
      - /sys:/sys:ro
      - /var/lib/docker/:/var/lib/docker:ro

```

#### Update your prometheus.yml to add cadvisor:

```
  - job_name: 'cadvisor'
    static_configs:
      - targets: ['cadvisor:8080']

```

Re-run the Stack
Once you've updated both files:

```
docker-compose down
docker-compose up -d

```

Then go to Prometheus and check targets: http://localhost:9090/targets

You should see **cadvisor**, **node_exporter**, and **prometheus** all scraping properly.

## 📊 Import Docker Dashboard

Import Docker monitoring dashboard (e.g., ID: 193):

- Go to Grafana → Dashboards → Import

- Enter ID: 193

- Load and select Prometheus as the source