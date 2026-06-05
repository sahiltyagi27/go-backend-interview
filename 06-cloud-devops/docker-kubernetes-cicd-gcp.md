# Docker, Kubernetes, CI/CD, and GCP

## Docker

Image:

> Packaged filesystem and metadata used to create containers.

Container:

> Running instance of an image.

Dockerfile for Go:

```dockerfile
FROM golang:1.22 AS build
WORKDIR /app
COPY go.mod go.sum ./
RUN go mod download
COPY . .
RUN CGO_ENABLED=0 go build -o server ./cmd/server

FROM gcr.io/distroless/static
COPY --from=build /app/server /server
ENTRYPOINT ["/server"]
```

Docker Compose is useful for local dependencies:

```yaml
services:
  api:
    build: .
    ports:
      - "8080:8080"
  redis:
    image: redis:7
  postgres:
    image: postgres:16
```

## Kubernetes

Pod:

> Smallest deployable unit.

Deployment:

> Manages replicas and rolling updates.

Service:

> Stable network endpoint for pods.

Ingress:

> Routes external HTTP traffic into the cluster.

ConfigMap:

> Non-secret configuration.

Secret:

> Sensitive configuration.

HPA:

> Horizontal Pod Autoscaler scales pods based on metrics.

## CI/CD

Typical pipeline:

```text
checkout
go test ./...
go vet ./...
lint
build Docker image
push image
deploy
rollback if needed
```

## GCP Basics

| Need | GCP Service |
|---|---|
| VMs | Compute Engine |
| Containers | GKE / Cloud Run |
| Object storage | Cloud Storage |
| Async messaging | Pub/Sub |
| Wide-column NoSQL | Bigtable |
| Logs/metrics | Cloud Logging / Cloud Monitoring |
| Permissions | IAM |

IAM principle:

> Grant the least privilege needed for the workload.

