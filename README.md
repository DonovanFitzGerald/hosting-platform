## Traefik Intergration

Traefik is the reverse proxy for routing requests. Websites are discovered by Traefik by adding labels to their web server container:

```yaml
labels:
  - traefik.enable=true
  - traefik.docker.network=${PROXY_NETWORK}
  - traefik.http.routers.${SITE_HOSTNAME}.rule=Host(`${SITE_DOMAIN}`)
  - traefik.http.routers.${SITE_HOSTNAME}.entrypoints=websecure
  - traefik.http.routers.${SITE_HOSTNAME}.tls=true
  - traefik.http.services.${SITE_HOSTNAME}.loadbalancer.server.port=80
```

The container must be attached to the `proxy` network so Traefik can discover it and route traffic.

## Blackbox Discovery

Supports enpoint probing for website health and uptime metrics. Endpoints are discovered through:

- `prometheus/blackbox-targets-static.yml` for static probes.
- Docker label discovery for containers attached to the `proxy` network.

To opt a proxied container into automatic blackbox probing, add labels like:

```yaml
labels:
  - monitoring.blackbox.enable=true
  - monitoring.blackbox.hostname=app.example.com
  - monitoring.blackbox.scheme=https
  - monitoring.blackbox.path=/health
```

You can also bypass hostname and path assembly and set the full probe URL directly:

```yaml
labels:
  - monitoring.blackbox.enable=true
  - monitoring.blackbox.target=https://app.example.com/health
```

Optional labels:

- `monitoring.blackbox.module` overrides the blackbox exporter module. Default: `http_2xx`.
- `monitoring.blackbox.path` defaults to `/`.
- `monitoring.blackbox.scheme` defaults to `https`.

The container must be attached to the `proxy` network so Prometheus can discover it through the Docker API filter.

### Observation Technology

- **Grafana** - Web app for visualizing metrics and configuring alerts.
- **Prometheus** - Stores metrics as time series data.
- **Loki** - Stores logs.
- **Grafana Alloy** - Monitors logs from containers running on host machine.
- **Node Exporter** - Monitors resource usage on host machine.
- **Cadvisor** - Monitors resource usage per container.
- **Blackbox Exporter** - Probes endpoints for uptime and health metrics.
- **Redis Exporter** - Exports Redis cache metrics.

### Repo Structure

```text
hosting-platform/
|-- compose.yml
|-- .env.example
|-- README.md
|-- traefik/
|   |-- dynamic/
|   |   |-- middlewares.yml
|   |   `-- tls.yml
|-- prometheus/
|   |-- prometheus.yml
|   |-- blackbox-targets-static.yml
|   `-- rules/
|       `-- alerts.yml
|-- loki/
|   `-- config.yml
|-- alloy/
|   `-- config.alloy
|-- scripts/
|   |-- init-networks.sh
|   |-- deploy.sh
|   `-- update.sh
`-- docs/
    |-- architecture.md
    |-- networks.md
    `-- operations.md
```
