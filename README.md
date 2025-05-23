# Real-Time Data Streaming Pipeline

A complete real-time data streaming solution that integrates Apache Airflow, Apache Kafka, Apache Spark, and Apache Cassandra to process and store user data from an external API.

## Architecture

![Architecture Diagram](https://github.com/nlhtungg/real-time-data-streaming/blob/master/architecture.png)

This project implements the following data pipeline:

1. **Data Source**: Random user data from the [Random User API](https://randomuser.me/api/)
2. **Data Orchestration**: Apache Airflow for task scheduling and orchestration
3. **Message Queue**: Apache Kafka for real-time data streaming
4. **Data Processing**: Apache Spark for stream processing
5. **Data Storage**: Apache Cassandra for storing processed user data

## Components

- **Apache Airflow**: Task scheduling and workflow management
- **Apache Kafka**: Distributed streaming platform
- **Apache Spark**: Distributed data processing
- **Apache Cassandra**: NoSQL database for storing processed data
- **Docker & Docker Compose**: Containerization and orchestration

## Prerequisites

- Docker and Docker Compose
- Python 3.9+
- Git

## Getting Started

### Clone the Repository

```bash
git clone <repository-url>
cd real-time-data-streaming
```

### Start the Services

```bash
docker-compose up -d
```

This will start all necessary services:
- Zookeeper & Kafka
- Schema Registry
- Control Center
- Airflow (Webserver, Scheduler, Postgres)
- Spark (Master and Worker)
- Cassandra

### Access Services

- **Airflow UI**: http://localhost:8080 (username: `admin`, password: `admin`)
- **Kafka Control Center**: http://localhost:9021
- **Spark Master UI**: http://localhost:9090

## How It Works

1. **Data Generation**: Airflow DAG (`kafka_stream.py`) fetches random user data from the Random User API
2. **Data Streaming**: The DAG formats the data and sends it to Kafka topic `users_created`
3. **Data Processing**: Spark Streaming job (`spark_stream.py`) processes the data from Kafka
4. **Data Storage**: Processed data is stored in Cassandra database `spark_streams.created_users`

## File Structure

- `docker-compose.yml`: Defines all services and their configurations
- `requirements.txt`: Python dependencies
- `spark_stream.py`: Spark streaming application
- `dags/kafka_stream.py`: Airflow DAG for data generation and Kafka production
- `script/entrypoint.sh`: Initialization script for Airflow

## Running the Pipeline

1. Start all services with `docker-compose up -d`
2. Access Airflow UI at http://localhost:8080
3. Enable and trigger the `user_automation` DAG
4. The DAG will fetch user data and publish to Kafka for 1 minute
5. The Spark application will process the data and store it in Cassandra
6. Check data in Cassandra with:

```bash
docker exec -it cassandra cqlsh -e "SELECT * FROM spark_streams.created_users LIMIT 10;"
```

## Configuration

The system is configured with:
- 1 Kafka broker
- 1 Spark master and 1 worker
- 1 Cassandra node
- Airflow with PostgreSQL as backend

## Extending the Project

- Add more data sources by creating additional Airflow DAGs
- Process more complex data with custom Spark transformations
- Create visualization with a frontend application

## Troubleshooting

If you encounter issues:

1. Check service status: `docker-compose ps`
2. Check logs: `docker-compose logs <service-name>`
3. Restart services: `docker-compose restart <service-name>`
4. Start from scratch: `docker-compose down -v && docker-compose up -d`

## License

[MIT License](LICENSE)
