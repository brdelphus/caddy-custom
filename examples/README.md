# Examples

## Caddy deployment scenarios

| File | Use case |
|---|---|
| [caddy/baremetal.yaml](caddy/baremetal.yaml) | DaemonSet + hostPorts for bare-metal k3s |
| [caddy/loadbalancer.yaml](caddy/loadbalancer.yaml) | Deployment + LoadBalancer for MetalLB / cloud |
| [caddy/mail.yaml](caddy/mail.yaml) | L4 TCP passthrough for SMTP / IMAP (combine with the above) |
| [caddy/full.yaml](caddy/full.yaml) | All optional plugins enabled (WAF, CrowdSec, GeoIP, etc.) |

Combine files with multiple `-f` flags:

```bash
# Bare-metal with mail passthrough
helm install caddy caddy-custom/caddy -n caddy --create-namespace \
  -f examples/caddy/baremetal.yaml \
  -f examples/caddy/mail.yaml

# MetalLB with full security stack
helm install caddy caddy-custom/caddy -n caddy --create-namespace \
  -f examples/caddy/loadbalancer.yaml \
  -f examples/caddy/full.yaml
```

## App values

Each file is a drop-in values override for the app's official Helm chart.
Set `ingressClassName: caddy-custom` and the relevant `caddy.ingress/` annotations.

| File | Chart | Helm repo |
|---|---|---|
| [apps/nextcloud.yaml](apps/nextcloud.yaml) | `nextcloud/nextcloud` | `https://nextcloud.github.io/helm` |
| [apps/mailu.yaml](apps/mailu.yaml) | `mailu/mailu` | `https://mailu.github.io/helm-charts` |
| [apps/gitea.yaml](apps/gitea.yaml) | `gitea-charts/gitea` | `https://dl.gitea.com/charts` |
| [apps/grafana.yaml](apps/grafana.yaml) | `grafana/grafana` | `https://grafana.github.io/helm-charts` |
| [apps/vaultwarden.yaml](apps/vaultwarden.yaml) | `vaultwarden/vaultwarden` | `https://guerzon.github.io/vaultwarden` |
| [apps/jellyfin.yaml](apps/jellyfin.yaml) | `jellyfin/jellyfin` | `https://jellyfin.github.io/jellyfin-helm` |
| [apps/authelia.yaml](apps/authelia.yaml) | `authelia/authelia` | `https://charts.authelia.com` |
| [apps/azuracast.yaml](apps/azuracast.yaml) | manual Ingress | — |

### Usage pattern

```bash
# 1. Add the chart repo
helm repo add nextcloud https://nextcloud.github.io/helm
helm repo update

# 2. Install with the example values as a base, then override what you need
helm install nextcloud nextcloud/nextcloud \
  -n nextcloud --create-namespace \
  -f examples/apps/nextcloud.yaml \
  --set nextcloud.host=cloud.yourdomain.com \
  --set nextcloud.password=changeme
```

> These examples are starting points — review and adjust hostnames, resource limits,
> and storage settings for your environment before deploying.
