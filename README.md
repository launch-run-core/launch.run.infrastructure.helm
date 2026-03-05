# launch.run.infrastructure.helm

Kubernetes manifests and Docker Compose configurations for the Launch Run platform.

## Structure

```
staging/                    Kubernetes manifests for staging environment
  namespace.yaml            Namespace definition
  configmap.yaml            Shared ConfigMap
  secret.yaml               Secret references
  ingress.yaml              Ingress controller configuration
  pdb.yaml                  PodDisruptionBudgets
  monitoring.yaml           Prometheus/Grafana/Loki stack
  kustomization.yaml        Kustomize configuration
  <service>.yaml            Individual service Deployments + Services
docker-compose.yml          Infrastructure services (databases, queues, observability)
docker-compose.override.yml Application services (Go microservices, Python AI service)
```

## Kubernetes Services

The staging environment includes manifests for all microservices:

| Category | Services |
|----------|----------|
| Core | gateway, listing-service, search-service, user-service, auth-service |
| Transactions | transaction-service, payment-service, auction-service, rental-service |
| Communication | notification-service, notification-hub, messaging-service |
| Analytics | analytics-service, intelligence-service, recommendation-service |
| Content | media-service, media-processing, review-service, feed-service |
| Commerce | pricing-service, coupon-service, campaign-service, loyalty-service, subscription-service |
| Trust | verification-service, moderation-service, kyc-service, dispute-service, escrow-service |
| Infrastructure | category-service, attribute-service, geolocation-service, search-ranking |
| Platform | experiment-service, feature-flag-service, scheduler-service, workflow-service |
| Data | report-service, data-export-service, event-store-service, audit-service |
| Finance | finance-service, insurance-service, tax-service, invoice-service |
| Other | document-service, consent-service, translation-service, shipping-service, webhook-service, graph-service, vector-service, leaderboard-service, scraper-service, rate-limiter-service |
| AI | ai-service |

## Local Development with Docker Compose

### Start infrastructure only

```bash
docker compose up -d
```

This starts: PostgreSQL, Redis (cache/sessions/pubsub), Kafka, ClickHouse, Meilisearch, MinIO, MongoDB, Elasticsearch, Neo4j, Qdrant, TimescaleDB, ScyllaDB, Temporal, Prometheus, Grafana, Loki.

### Start infrastructure + application services

```bash
docker compose up -d
```

Docker Compose automatically merges `docker-compose.override.yml`.

### Start a specific service

```bash
docker compose up -d gateway
```

## Deploying to Kubernetes (staging)

### Apply with kubectl

```bash
kubectl apply -k staging/
```

### Apply with Kustomize

```bash
kustomize build staging/ | kubectl apply -f -
```

## CI/CD

GitHub Actions runs on every PR:
1. **kubeconform** -- validates K8s manifests against the Kubernetes API schema
2. **docker compose config** -- validates Docker Compose files parse correctly
3. **yamllint** -- YAML syntax linting

## Port Mapping

| Service | gRPC Port |
|---------|-----------|
| listing-service | 9001 |
| search-service | 9002 |
| user-service | 9003 |
| transaction-service | 9004 |
| analytics-service | 9005 |
| intelligence-service | 9006 |
| review-service | 9007 |
| notification-service | 9008 |
| media-service | 9009 |
| ai-service | 9011 |
| auth-service | 9012 |
| messaging-service | 9014 |
| geolocation-service | 9015 |
| category-service | 9016 |
| attribute-service | 9017 |
| pricing-service | 9018 |
| auction-service | 9020 |
| rental-service | 9021 |
| payment-service | 9022 |
| verification-service | 9023 |
| moderation-service | 9024 |
| experiment-service | 9025 |
| feature-flag-service | 9026 |
| campaign-service | 9027 |
| coupon-service | 9028 |
| loyalty-service | 9029 |
| subscription-service | 9030 |
| dispute-service | 9031 |
| escrow-service | 9032 |
| shipping-service | 9033 |
| document-service | 9034 |
| scheduler-service | 9035 |
| workflow-service | 9036 |
| feed-service | 9037 |
| audit-service | 9038 |
| consent-service | 9039 |
| translation-service | 9040 |
| tax-service | 9041 |
| invoice-service | 9042 |
| kyc-service | 9043 |
| webhook-service | 9044 |
| graph-service | 9045 |
| recommendation-service | 9046 |
| leaderboard-service | 9047 |
| report-service | 9048 |
| scraper-service | 9049 |
| rate-limiter-service | 9050 |
| vector-service | 9051 |
| event-store-service | 9052 |
| notification-hub | 9053 |
| media-processing | 9054 |
| search-ranking | 9055 |
| data-export-service | 9056 |
| finance-service | 9060 |
| insurance-service | 9061 |

Gateway REST API: port 8080
