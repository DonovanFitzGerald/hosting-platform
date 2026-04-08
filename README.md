## Repo Structure

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

## Blackbox Discovery

Prometheus supports two blackbox target sources in this stack:

- `prometheus/blackbox-targets-static.yml` for static probes.
- Docker label discovery for containers attached to `${PROXY_NETWORK}`.

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

The container must be attached to the proxy network so Prometheus can discover it through the Docker API filter.
