# ByteBank Infra

Repositório de infraestrutura do ecossistema ByteBank. Contém o Docker Compose com todos os serviços de suporte (Redis, RabbitMQ, Kafka, Zipkin, Prometheus, Grafana, pgAdmin) e as configurações de observabilidade.

---

## Serviços de Infraestrutura

| Serviço | Imagem | Porta |
|---------|--------|-------|
| Redis | `redis:7-alpine` | 6379 |
| RabbitMQ | `rabbitmq:3-management-alpine` | 5672 / 15672 |
| Zipkin | `openzipkin/zipkin` | 9411 |
| Prometheus | `prom/prometheus` | 9090 |
| Grafana | `grafana/grafana` | 3000 |
| pgAdmin | `dpage/pgadmin4` | 5050 |

---

## Como Executar

### Pré-requisitos

- Docker e Docker Compose instalados
- Rede Docker `bytebank-net` criada:

```bash
docker network create bytebank-net
```

### Subindo a infraestrutura

```bash
docker compose -f infra_docker/docker-compose.dev.yml up -d
```

Suba a infraestrutura **antes** de qualquer microsserviço.

---

## Observabilidade

**Prometheus**

Configurado com Service Discovery via Eureka — coleta métricas automaticamente de todos os serviços registrados, sem precisar adicionar cada um manualmente no `prometheus.yml`. O scrape interval é de 15 segundos.

```yaml
eureka_sd_configs:
  - server: 'http://eureka-server.bytebank:8761/eureka'
```

**Grafana**

Pré-configurado com Prometheus como datasource padrão via provisionamento automático — ao subir o container já está conectado, sem precisar configurar manualmente pela UI.

Acesso: `http://localhost:3000` — usuário/senha padrão: `admin/admin`

Em produção, acessível via domínio próprio: [bytebank.thalesf.dev/grafana](https://bytebank.thalesf.dev/grafana)

**Zipkin**

Rastreamento distribuído de requisições entre os microsserviços. Acesso em `http://localhost:9411`.

---

## RabbitMQ

Management UI: `http://localhost:15672`

---

## pgAdmin

Ferramenta visual para gerenciar os bancos PostgreSQL de cada microsserviço.

- Acesso: `http://localhost:5050`

Cada microsserviço tem seu próprio banco PostgreSQL configurado no respectivo `docker-compose.yml`.

---

## Estrutura

```
bytebank-infra/
├── grafana/
│   └── provisioning/
│       └── datasources/
│           └── datasource.yml    ← Prometheus como datasource padrão
├── prometheus/
│   ├── Dockerfile
│   └── prometheus.yml            ← Scrape via Eureka Service Discovery
└── infra_docker/
    └── docker-compose.dev.yml    ← Todos os serviços de infraestrutura
```

---

## Autor

**Thales Fernandes**

[![GitHub](https://img.shields.io/badge/GitHub-ThalesF93-181717?style=flat&logo=github)](https://github.com/ThalesF93)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Thales_Fernandes-0A66C2?style=flat&logo=linkedin)](https://www.linkedin.com/in/thales-fernandes-24418126a/)
