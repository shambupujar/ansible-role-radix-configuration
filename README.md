# Ansible Role: Radix Configuration

Configures monitoring and security for Radix Babylon nodes:
- **Promtail** for log collection to Grafana Loki
- **Grafana Agent** scrape config for Radix API metrics, JMX, and cAdvisor
- **JMX Exporter** for Java metrics
- **iptables** firewall with configurable allowed TCP ports
- **fail2ban** with Cloudflare IP banning and Nginx rate limiting

## Role Variables

See [defaults/main.yml](defaults/main.yml) for the full list of variables.

### Key variables

| Variable | Default | Description |
|----------|---------|-------------|
| `node_deployment_mode` | `systemd` | Deployment mode (`systemd` or `docker`) |
| `promtail_version` | `v3.5.11` | Promtail version |
| `loki_url` | (required) | Loki push endpoint |
| `remote_write_url` | (required) | Prometheus remote write endpoint |
| `grafana_agent_configure` | `true` | Enable Grafana Agent configuration (set `false` to skip if grafana-cloud-agent role is not used) |
| `grafana_monitor_apimetrics` | `true` | Enable API metrics scraping |
| `grafana_monitor_jmx` | `false` | Enable JMX metrics scraping |
| `grafana_monitor_cadvisor` | `false` | Enable cAdvisor metrics |
| `iptables_configure` | `false` | Enable iptables firewall |
| `iptables_allowed_tcp_ports` | `[22, 443, 30000]` | TCP ports to allow through firewall |
| `fail2_ban_ip_ban_enabled` | `false` | Enable fail2ban IP banning |

## Example Playbook

```yaml
- hosts: radixnode
  become: true
  roles:
    - role: configuration
      vars:
        loki_url: "logs-prod-us-central1.grafana.net/loki/api/v1/push"
        remote_write_url: "https://prometheus-prod-us-central1.grafana.net/api/prom/push"
```

## License

MIT
