# Kafka + Kafdrop + Topic Initializer (Docker Compose)

This setup provides a self-contained Kafka environment using Docker Compose, featuring:

- **Apache Kafka (KRaft mode)**
- **Init container** — automatically creates predefined topics on startup
- **Kafdrop** — a simple web UI for viewing Kafka topics and messages

---

## Services Overview

### kafka
Runs the main Kafka broker in **KRaft mode**.

- Listens on `9092` inside the Docker network
- Auto-creates topics if enabled
- Configured for single-node operation

### init-kafka
A short-lived container that waits until Kafka is ready, then creates required topics.

You can customize the topics by editing `docker/init-kafka.sh`.

Example topics:
- `user-events`
- `user-events-test`

### kafdrop
A web interface for Kafka.

- Runs on [http://localhost:9000](http://localhost:9000)
- Connects to Kafka via the internal Docker hostname `kafka:9092`

---

## Usage

### Start all services

```bash
    docker compose up -d
