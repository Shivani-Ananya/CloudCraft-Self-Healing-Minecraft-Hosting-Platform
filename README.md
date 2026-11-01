# CloudCraft — Self-Healing Minecraft Hosting Platform

**An automated Minecraft server monitoring and recovery system powered by DevOps tools.**

CloudCraft is a self-healing Minecraft hosting prototype designed to monitor server health, detect unexpected process termination, automatically restart the server, and visualize operational metrics through a monitoring dashboard.

The project combines PowerShell automation, Python, Docker, Prometheus, and Grafana to demonstrate how monitoring and recovery mechanisms can improve service reliability.

## Architecture

Minecraft Server → PowerShell Server Manager → JSON Metrics → Python Exporter → Prometheus → Grafana

## Features

- **Automated Recovery:** Detects unexpected Minecraft server process termination and attempts to restart the server.
- **Health Monitoring:** Checks server availability through TCP connectivity on port 25565.
- **Incident Logging:** Records server startup, recovery, and crash events.
- **Metrics Collection:** Tracks server status, crash count, restart count, and the last restart timestamp.
- **Prometheus Integration:** Exposes application metrics for periodic scraping.
- **Grafana Dashboard:** Visualizes server availability and recovery statistics.
- **Docker-Based Monitoring:** Runs the monitoring components in containers for easier local setup.

## Technology Stack

| Technology | Purpose |
|---|---|
| Minecraft Paper Server | Application being monitored |
| PowerShell | Server process management and automated recovery |
| Python | Custom metrics exporter |
| Docker | Containerized monitoring stack |
| Prometheus | Metrics collection and querying |
| Grafana | Monitoring dashboard and visualization |
| JSON | Metrics storage |

## Metrics Monitored

| Metric | Description |
|---|---|
| `cloudcraft_server_up` | Indicates whether the server is online (`1`) or offline (`0`). |
| `cloudcraft_crash_count` | Counts unexpected server process exits detected by the manager. |
| `cloudcraft_restart_count` | Tracks automatic restart attempts. |
| `cloudcraft_last_restart_timestamp_seconds` | Unix timestamp of the most recent restart. |

## How Self-Healing Works

1. The PowerShell manager launches the Minecraft server.
2. It checks whether the server is accepting TCP connections.
3. Server health and recovery statistics are written to a JSON metrics file.
4. The Python exporter exposes the metrics through an HTTP endpoint.
5. Prometheus periodically scrapes the exporter.
6. Grafana visualizes the collected metrics.
7. If the Minecraft process exits unexpectedly, the manager logs the event and attempts to restart it.

## Validation

The local prototype was tested to verify that:

- The Minecraft server starts successfully.
- The exporter exposes metrics over HTTP.
- Prometheus collects the custom metrics.
- Grafana displays server status, crash count, and restart count.
- An unexpected server process termination triggers the recovery mechanism.

## Running the Monitoring Stack

Prerequisites:
- Docker Desktop
- Python 3, if running the exporter outside Docker
- Java version compatible with your Paper server build
- A configured Minecraft Paper server

Start the monitoring stack from the directory containing `docker-compose.yml`:

```powershell
docker-compose up -d
```

Check container status:

```powershell
docker-compose ps
```

Access the local services:

- **Prometheus:** `http://localhost:9090`
- **Grafana:** `http://localhost:3000`
- **Metrics endpoint:** `http://localhost:9187/metrics`

The metrics endpoint should return Prometheus-formatted metrics when the exporter is running.

To stop the monitoring containers:

```powershell
docker-compose down
```

> These instructions cover the local monitoring stack. The Minecraft server manager must be started separately using its PowerShell script.

## Future Enhancements

- Configure Grafana alerts for server downtime.
- Containerize the Minecraft server itself.
- Introduce infrastructure provisioning using Terraform.
- Deploy the platform to AWS with cost controls.
- Expand monitoring with resource utilization and server performance metrics.
- Improve graceful shutdown and recovery handling.

## Project Status

**Completed: Local monitoring and self-healing prototype**

The local implementation demonstrates automated process recovery, custom metrics collection, and dashboard-based observability. Cloud deployment and Terraform-based infrastructure provisioning remain future enhancements.

## Author

Developed as a hands-on DevOps project to explore automation, observability, containerization, and service reliability.
