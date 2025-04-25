
# 📈 Monitoring with Prometheus & Grafana

> A complete monitoring stack using **Node Exporter**, **Prometheus**, and **Grafana** to visualize system-level metrics from a Linux/WSL environment.



---

## 🧰 Requirements

- 🐳 Docker & Docker Compose
- 🐧 Linux/WSL or any Unix-like system
- 🌐 Internet access to pull images

---

## ⚙️ Step 1: Install Node Exporter (Linux/WSL)

> Node Exporter exposes system metrics like CPU, memory, disk, etc., at `http://localhost:9100/metrics`

### 🛠 Install & Run

```bash
wget https://github.com/prometheus/node_exporter/releases/download/v1.9.1/node_exporter-1.9.1.linux-amd64.tar.gz
tar xvfz node_exporter-1.9.1.linux-amd64.tar.gz
cd node_exporter-1.9.1.linux-amd64
./node_exporter
```
<p align="center">
  <img src="ss\Screenshot 2025-04-25 223525.png">
</p>

---


✅ **Verify:**
- Open browser: [http://localhost:9100/metrics](http://localhost:9100/metrics)
- Or run:
```bash
curl http://localhost:9100/metrics | grep "node_"
```


🔍 **Find WSL IP (for Prometheus):**
```bash
hostname -I
```

---

## 🐳 Step 2: Start Prometheus & Grafana with Docker Compose

> Prometheus scrapes metrics, Grafana visualizes them.

### 📁 Create project structure:
```bash
mkdir monitoring-stack
cd monitoring-stack
```

### 📄 `docker-compose.yml`
```yaml
services:
  prometheus:
    image: prom/prometheus
    ports:
      - "9090:9090"
    volumes:
      - ./prometheus.yml:/etc/prometheus/prometheus.yml

  grafana:
    image: grafana/grafana
    ports:
      - "3000:3000"
```

---

### 📄 `prometheus.yml`

> Replace `<WSL_IP>` with your actual IP address from `hostname -I`.

```yaml
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: 'node-exporter'
    static_configs:
      - targets: ['<WSL_IP>:9100']
```

---

### ▶️ Start the services:

```bash
docker-compose up -d
```
<p align="center">
  <img src="ss\Screenshot 2025-04-25 223540.png">
</p>
---

🌐 Access:
- Prometheus: [http://localhost:9090](http://localhost:9090)
 ---
<p align="center">
<img src="https://github.com/Himanshu5619/DevOps/blob/main/Monitoring%20with%20Prometheus%20and%20Grafana/ss/Screenshot%202025-04-20%20161019.png" alt="Streamlit App Screenshot">
</p>
---
- Grafana: [http://localhost:3000](http://localhost:3000)
---
<p align="center">
<img src="https://github.com/Himanshu5619/DevOps/blob/main/Monitoring%20with%20Prometheus%20and%20Grafana/ss/Screenshot%202025-04-20%20161008.png" alt="Streamlit App Screenshot">
</p>
---
---
<p align="center">
<img src="https://github.com/Himanshu5619/DevOps/blob/main/Monitoring%20with%20Prometheus%20and%20Grafana/ss/Screenshot%202025-04-20%20161145.png" alt="Streamlit App Screenshot">
</p>
---
## 📊 Step 3: Configure Grafana Dashboard

### 1. 🧾 Login to Grafana

- Visit: [http://localhost:3000](http://localhost:3000)
- Default credentials:  
  **Username:** `admin`  
  **Password:** `admin`  
- Change the password when prompted

### 2. ➕ Add Prometheus Data Source

- Go to `Configuration → Data Sources → Add Prometheus`
- URL: `http://prometheus:9090`
- Click **Save & Test**

### 3. 📥 Import Node Exporter Dashboard

- Go to `+ Create → Import`
- Enter **Dashboard ID:** `1860`
- Select Prometheus as the data source
- Click **Import**

---

## ✅ Final Results

🎉 You now have a complete monitoring dashboard with:

- ✅ Real-time **CPU**, **Memory**, **Disk**, and **Network** stats
- ✅ Dynamic visualizations powered by Grafana
- ✅ Efficient data scraping with Prometheus

---

## 🔧 Troubleshooting

| Problem | Solution |
|--------|----------|
| `Permission denied to Docker socket` | Run: `sudo usermod -aG docker $USER && newgrp docker` |
| Grafana not loading | Check if port 3000 is in use |
| Prometheus not scraping | Ensure Node Exporter is running, use correct IP in `prometheus.yml` |
| Docker Compose errors | Ensure correct file structure and spacing in `.yml` files |

---

## 📂 Project Structure

```
monitoring-stack/
├── docker-compose.yml
└── prometheus.yml
```

---

## 🧠 Useful Links

- 🌐 [Prometheus Official](https://prometheus.io/)
- 📊 [Grafana Official](https://grafana.com/)
- 🧪 [Node Exporter GitHub](https://github.com/prometheus/node_exporter)
- 📈 [Node Exporter Dashboard #1860](https://grafana.com/grafana/dashboards/1860)

---