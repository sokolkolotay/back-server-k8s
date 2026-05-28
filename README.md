# ⚙️ Back Server K8s - Helm Charts & Kubernetes

Инфраструктурный репозиторий для деплоя Notes API стека на Kubernetes (k3s) с использованием Helm.

## 🏗️ Архитектура
Telegram Bot → Kafka (KRaft) → Ktor API → PostgreSQL
↓
Prometheus → Grafana
GitHub push → Actions → копирование charts на VPS → helm upgrade

## ⚙️ Технологии

| Слой | Технология |
|---|---|
| Оркестрация | Kubernetes (k3s) |
| Пакетный менеджер | Helm 3 |
| Message Broker | Apache Kafka 3.7 (KRaft, без ZooKeeper) |
| CI/CD | GitHub Actions |
| Инфраструктура | VPS 4 CPU / 2GB RAM |

## 📦 Helm Charts

| Chart | Описание |
|---|---|
| `charts/postgres` | PostgreSQL 15 с PVC |
| `charts/kafka` | Apache Kafka в KRaft режиме |
| `charts/ktor-api` | Notes API с Kafka consumer |
| `charts/tg-bot` | Telegram бот с Kafka producer |

## 🚀 Деплой

```bash
# Установить все сервисы
helm upgrade --install postgres ./charts/postgres
helm upgrade --install kafka ./charts/kafka
helm upgrade --install ktor-api ./charts/ktor-api
helm upgrade --install tg-bot ./charts/tg-bot

# Проверить статус
kubectl get pods
kubectl get services
```

## 🔄 CI/CD Pipeline

При каждом пуше в `main`:
1. GitHub Actions копирует Helm charts на VPS через SCP
2. Выполняет `helm upgrade --install` для каждого сервиса
3. Kubernetes автоматически обновляет поды

## 🔐 Секреты

Токен Telegram бота хранится в Kubernetes Secret
