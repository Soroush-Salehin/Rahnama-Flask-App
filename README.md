# Instrumented Flask App with Redis, Prometheus, and Grafana

This project is a sample Flask application that demonstrates monitoring and metrics collection using Redis, Prometheus, and Grafana. The entire stack runs seamlessly using Docker Compose.

## Features

* Flask REST API to manage key-value pairs in Redis.
* Redis caching to store and retrieve items efficiently.
* Prometheus metrics exported from Flask:

  * `redis_app_cache_hit_total` – counts cache hits.
  * `redis_app_cache_miss_total` – counts cache misses.
  * HTTP request metrics (`http_requests_total`, `http_request_duration_seconds`).
* Grafana dashboard to visualize metrics such as cache hit/miss rates.

## Project Structure

```
instrumented-flask-app/
├── app.py                     # Main Flask application
├── Dockerfile                 # Dockerfile for building the Flask app
├── docker-compose.yml         # Docker Compose configuration for all services
├── requirements.txt           # Python dependencies
├── requirements-test.txt      # Test dependencies
├── configs/
│   └── grafana-dashboard.json # Preconfigured Grafana dashboard
├── run-test.sh                # Script to test the application
└── README.md                  # Project documentation
```

## Setup Instructions

1. Clone the repository:

```bash
git clone https://github.com/Soroush-Salehin/Rahnama-Flask-App.git
cd Rahnama-Flask-App
```

2. Run all services with Docker Compose:

```bash
docker compose up --build
```

* Flask API: [http://localhost:9000](http://localhost:9000)
* Prometheus: [http://localhost:9090](http://localhost:9090)
* Grafana: [http://localhost:3000](http://localhost:3000) (default credentials: `admin/admin`)

3. Configure Grafana:

* Add Prometheus as a data source: `http://prometheus:9090`
* Import the dashboard from `configs/grafana-dashboard.json`
* Add a panel for **Cache Miss Rate** using the Prometheus query: `rate(redis_app_cache_miss_total[1m])`

4. Test the application:

```bash
curl -X POST http://localhost:9000/items \
     -H "Content-Type: application/json" \
     -d '{"key":"test","value":"123"}'

curl http://localhost:9000/items/test
curl http://localhost:9000/metrics | grep redis_app_cache_miss
```

Each cache hit or miss updates the corresponding Prometheus metric, which can be visualized in Grafana.

## Prometheus Metrics

* **Cache Hits:** `redis_app_cache_hit_total{key="<key_name>"}`
* **Cache Misses:** `redis_app_cache_miss_total{key="<key_name>"}`
* **HTTP Request Count:** `http_requests_total{method,endpoint,status_code}`
* **HTTP Request Duration:** `http_request_duration_seconds{method,endpoint}`

## Notes

* All services run using Docker Compose, so no manual installation is required.
* Ports can be adjusted in `docker-compose.yml` if needed.
* This project demonstrates integration of Flask, Redis, Prometheus, and Grafana for monitoring purposes.

## Author

**Soroush Salehin**
