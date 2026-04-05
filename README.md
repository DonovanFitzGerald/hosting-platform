## Repo Structure

```
hosting-platform/
├── compose.yaml
├── .env.example
├── README.md
├── /traefik
│   ├── dynamic
│   │   ├── middlewares.yml
│   │   └── tls.yml
│   └── certs
├── /prometheus
│   ├── prometheus.yml
│   └── rules
│       └── alerts.yml
├── /grafana
│   ├── provisioning
│   │   ├── datasources
│   │   └── dashboards
│   └── dashboards
├── /loki
│   └── config.yml
├── /alloy
│   └── config.alloy
├── /scripts
│   ├── init-networks.sh
│   ├── deploy.sh
│   └── update.sh
└── /docs
    ├── architecture.md
    ├── networks.md
    └── operations.md
```
