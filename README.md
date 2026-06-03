# Indigo BioMed Diagnostic API — Sample App

This is the sample application for the IBM Associates DevOps &amp; Infrastructure Specialist induction case study.

Associates will **fork** this repository and use it as the application they deploy through their CI/CD pipeline in Phase 3 and monitor with Prometheus + Grafana in Phase 4.

---

## What the app does

It simulates a stripped-down version of Indigo BioMed's internal diagnostic data API. It is deliberately simple — associates are not here to learn Flask development. The app exists so they have something real to deploy, monitor, and observe.

| Endpoint | Description |
|----------|-------------|
| `GET /` | Welcome message |
| `GET /health` | Health check — returns 200 when running |
| `GET /metrics` | Prometheus metrics scrape endpoint |
| `GET /diagnose` | Simulated diagnostic job — ~10% error rate, variable latency |
| `GET /slow` | Intentionally slow response — tests latency panels |

---

## Running locally

```bash
pip install -r requirements.txt
python app.py
```

Then open:
- http://localhost:5000/health
- http://localhost:5000/metrics

---

## For associates — how to fork and use this repo

1. Go to this GitHub repository page
2. Click the **Fork** button (top-right corner)
3. This creates a copy under your own GitHub account
4. Clone your fork to your laptop:

```bash
git clone https://github.com/YOUR_USERNAME/indigo-diagnostic-api.git
cd indigo-diagnostic-api
```

5. Run it locally to verify it works before setting up the pipeline.

---

## Prometheus scrape config

When you reach Phase 4 (Task 4), add this to your `prometheus.yml` to collect metrics from the running app:

```yaml
scrape_configs:
  - job_name: 'indigo-app'
    static_configs:
      - targets: ['YOUR_VM_PRIVATE_IP:5000']
```

The `/metrics` endpoint returns data in Prometheus format — no extra configuration needed on the app side.

---

## Generating test traffic for your Grafana dashboard

Once the app is deployed on your VM, run this from any terminal to generate realistic traffic:

```bash
# Generate a mix of normal and slow requests
for i in {1..100}; do
  curl -s http://YOUR_VM_IP:5000/diagnose > /dev/null
  sleep 0.5
done
```

This will populate your Grafana panels with request rate, error rate, and latency data within a few minutes.
