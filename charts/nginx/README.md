# nginx

A Helm chart that deploys [NGINX](https://nginx.org) on Kubernetes.

Scaffolded with `helm create` and tailored for nginx: it pins the official
`nginx` image, and can optionally serve custom content or a custom server
block from a ConfigMap — no custom image required.

## Install

```bash
helm install my-nginx ./charts/nginx
```

## Uninstall

```bash
helm uninstall my-nginx
```

## Key configuration

| Value | Default | Description |
|-------|---------|-------------|
| `replicaCount` | `1` | Number of nginx replicas |
| `image.repository` | `nginx` | Image repository |
| `image.tag` | `""` | Image tag (defaults to `Chart.appVersion`) |
| `containerPort` | `80` | Port nginx listens on in the container |
| `service.type` | `ClusterIP` | Service type |
| `service.port` | `80` | Service port |
| `ingress.enabled` | `false` | Enable an Ingress |
| `autoscaling.enabled` | `false` | Enable a HorizontalPodAutoscaler |
| `nginx.staticContent` | sample HTML | Custom `index.html` served at `/`. Set to `null` to keep the image default |
| `nginx.serverBlock` | `""` | Custom server config mounted at `/etc/nginx/conf.d/default.conf` |

When either `nginx.staticContent` or `nginx.serverBlock` is set, the chart
renders a ConfigMap and mounts the relevant files into the container.

### Examples

Serve your own landing page:

```bash
helm install my-nginx ./charts/nginx \
  --set-string nginx.staticContent='<h1>Hello world</h1>'
```

Use the stock nginx welcome page (no custom content):

```bash
helm install my-nginx ./charts/nginx --set nginx.staticContent=null
```

## Install from the published repo

Releases are published to GitHub Pages by `helm/chart-releaser-action` (see
`.github/workflows/release.yaml`), triggered by pushing a git tag:

```bash
helm repo add nginx-helm https://ucabvas.github.io/nginx-helm
helm repo update
helm install my-nginx nginx-helm/nginx
```

## Development

```bash
helm lint charts/nginx
helm template charts/nginx
```
