# Monitoring Stack for Docker Swarm

This repository provides a complete monitoring solution for Docker Swarm environments. All services and configurations are designed specifically for Docker Swarm deployments, including service discovery and network settings.

## Components

- **Prometheus**: Collects and stores metrics from services and hosts.
- **Alertmanager**: Manages alerts sent by Prometheus and routes them to notification channels (e.g., Telegram).
- **Grafana**: Visualizes metrics and logs with customizable dashboards.
- **Loki**: Stores and queries logs.
- **Promtail**: Collects logs and ships them to Loki.
- **cAdvisor**: Provides container resource usage and performance metrics.
- **Node Exporter**: Exposes hardware and OS metrics for Prometheus.
- **Cloudflared Tunnel**: Provides secure remote access to Grafana.

## Folder Structure

```
monitoring/
├── .env.example
├── README.md
├── monitoring.yml
├── alertmanager/
│   ├── alertmanager.yml
│   └── templates/
│       └── telegram.tmpl
├── grafana/
│   ├── dashboards/
│   │   ├── container-metrics.json
│   │   ├── loki-logs.json
│   │   ├── node-exporter-full.json
│   │   └── node-metrics.json
│   └── provisioning/
│       ├── dashboard.yml
│       └── datasource.yml
├── loki/
│   └── loki-config.yaml
├── prometheus/
│   ├── alert-rules.yml
│   └── prometheus.yml
├── promtail/
│   └── promtail-config.yml
```

## Configuration & Environment Variables

Sensitive information (such as credentials and tokens) is managed via environment variables. These must be set in the `monitoring.yml` under the `environment:` section for each service that requires them.

```
environment:
  - SOME_API_URL=https://api.example.com
  - SOME_SERVICE_TOKEN=your_token
  - SOME_CHAT_ID=your_chat_id
```

You can also use a `.env` file for global variables, but ensure they are passed to the container if referenced in configuration files.

The path to configuration files can be set using the `MONITORING_CONFIG_PATH` variable:
```
MONITORING_CONFIG_PATH=/absolute/or/relative/path/to/configs
```

## Environment Variables

This stack uses environment variables for configuration and secrets. You can set them in a `.env` file at the root of the repository. See `.env.example` for all required variables:

```
MONITORING_CONFIG_PATH=/home/user/stacks/monitoring
GRAFANA_USER=admin
GRAFANA_PASSWORD=admin
GRAFANA_ALLOW_SIGN_UP=false
TUNNEL_TOKEN=your_cloudflare_tunnel_token
```

Copy `.env.example` to `.env` and fill in your values before deploying.

## Deployment

1. Set all required environment variables in your `.env` file or directly in `monitoring.yml`.
2. Deploy the stack with Docker Swarm:
   ```sh
   docker stack deploy -c monitoring.yml monitoring
   ```
3. Access Grafana via the exposed port or through the Cloudflared tunnel.

## Security Notes

- Never commit sensitive credentials (tokens, passwords) directly in configuration files.
- Always use environment variables for secrets.
- Restrict access to monitoring endpoints and dashboards.

## Contributing

Feel free to open issues or submit pull requests for improvements or bug fixes.

## Contact

For questions or support, please open an issue in this repository.
