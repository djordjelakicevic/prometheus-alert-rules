# Prometheus Alert Rules

Production-tested alert rules for Prometheus, covering databases, networking, infrastructure, and observability components.

## Coverage

| Category | Services |
|----------|----------|
| **Databases** | ClickHouse, Aerospike, ZooKeeper, MariaDB Galera, PostgreSQL |
| **Networking** | MikroTik (SNMP), Arista (SNMP), general SNMP targets |
| **Infrastructure** |  Telegraf SNMP |
| **Observability** | Thanos, Prometheus self-monitoring |

## Quick Start

### 1. Clone the repo

```bash
git clone https://github.com/djordjelakicevic/prometheus-alert-rules.git
```

### 2. Pick the rules you need

Copy relevant `.yml` files into your Prometheus rules directory:

```bash
cp rules/databases/clickhouse.yml /etc/prometheus/rules/
```

### 3. Validate

```bash
promtool check rules rules/**/*.yml
```

### 4. Reload Prometheus

```bash
curl -X POST http://localhost:9090/-/reload
```

## Alert Design Principles

Every rule in this repo follows these principles:

- **Actionable**: every alert has a clear `summary`, `description` and `resolved summary`
- **Tiered severity**: `critical`, `warning`
- **Low noise**: `for` durations tuned to avoid flapping — no 0s alerts unless genuinely instant
- **Labeled consistently**: most alerts include `severity`, `grouping_label`, and `type` labels for routing

## Alertmanager Routing Example

A sanitized Alertmanager routing config is provided in `rules/templates/alertmanager-routing.yml`.

It demonstrates:
- Grouping by `alertname`, `grouping_label`
- Inhibition rules (critical silences warning for the same service)

> **Note**: Replace placeholder values (`<SLACK_WEBHOOK>`, `<PAGERDUTY_KEY>`) with your own.

## Contributing

Contributions are welcome. If you have production-tested rules for services not covered here, feel free to open a PR.

Please ensure:
- Rules pass `promtool check rules`
- Alerts follow the severity/labeling conventions above
- No hardcoded hostnames, IPs, or secrets

## Author

**Djordje Lakicevic** — DevOps-oriented System Administrator  
[LinkedIn](https://www.linkedin.com/in/djordje-lakicevic-813756227/) · [GitHub](https://github.com/djordjelakicevic)