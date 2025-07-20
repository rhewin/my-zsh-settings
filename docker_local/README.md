# Docker Local Development Environment

A comprehensive Docker Compose setup for local development with essential services including databases, message queues, email testing, and database management tools.

## Services Overview

### Databases
- **MySQL 8.0** - Relational database
- **PostgreSQL 15** - Advanced relational database
- **MongoDB 7** - NoSQL document database

### Message Queue
- **RabbitMQ 3.12** - Message broker with management interface

### Development Tools
- **Adminer 4.8.1** - Web-based database management interface
- **Mailpit** - Email testing and SMTP server

## Quick Start

### Prerequisites
- Docker
- Docker Compose
- Make (optional, for convenience commands)

### Start All Services
```bash
# Start all services in background
make up
# or
docker-compose up -d

# View logs
make logs
# or
docker-compose logs -f

# Stop all services
make down
# or
docker-compose down
```

## Service Connections

### MySQL
- **Host:** localhost
- **Port:** 3306
- **Username:** root
- **Password:** See `services/mysql/.env`
- **CLI Access:** `make mysql-cli`

### PostgreSQL
- **Host:** localhost
- **Port:** 5432
- **Username:** See `services/postgresql/.env`
- **Password:** See `services/postgresql/.env`
- **Database:** See `services/postgresql/.env`
- **CLI Access:** `make psql-cli`

### MongoDB
- **Host:** localhost
- **Port:** 27017
- **Username:** See `services/mongodb/.env`
- **Password:** See `services/mongodb/.env`
- **Connection String:** `mongodb://username:password@localhost:27017`

### RabbitMQ
- **AMQP Port:** 5672
- **Management Port:** 15672
- **Username:** admin
- **Password:** test1234
- **Virtual Host:** /
- **Connection URL:** `amqp://admin:test1234@localhost:5672/`
- **CLI Status:** `make rabbitmq-cli`

### Mailpit (Email Testing)
- **SMTP Port:** 1025
- **Web Interface:** 8025
- **SMTP Settings:** 
  - Host: localhost
  - Port: 1025
  - No authentication required

## Web Interfaces

### RabbitMQ Management
- **URL:** http://localhost:15672
- **Username:** admin
- **Password:** test1234
- **Quick Access:** `make rabbitmq-web`
- **Features:** Queue management, exchange configuration, message monitoring

### Adminer (Database Management)
- **URL:** http://localhost:8080
- **Quick Access:** `make adminer-web`
- **Supports:** MySQL, PostgreSQL, MongoDB
- **Features:** Query editor, table browser, import/export

### Mailpit (Email Testing)
- **URL:** http://localhost:8025
- **Quick Access:** `make mailpit-web`
- **Features:** Email preview, SMTP testing, message inspection

## Make Commands

| Command             | Description                           |
|---------------------|---------------------------------------|
| `make up`           | Start all services                    |
| `make down`         | Stop all services                     |
| `make restart`      | Restart all services                  |
| `make logs`         | View service logs                     |
| `make mysql-cli`    | Access MySQL CLI                      |
| `make psql-cli`     | Access PostgreSQL CLI                 |
| `make rabbitmq-cli` | Check RabbitMQ status                 |
| `make rabbitmq-web` | Open RabbitMQ management UI           |
| `make adminer-web`  | Open Adminer database UI              |
| `make mailpit-web`  | Open Mailpit email UI                 |
| `make cleanup`      | Remove containers and cleanup system  |

## Directory Structure

```
docker_local/
|- docker-compose.yml          # Main compose configuration
|- Makefile                    # Convenience commands
|- README.md                   # This documentation
|- services/                   # Service configurations
    |- mongodb/
    |   |-  .env               # MongoDB credentials
    |- mysql/
    |   |-  .env               # MySQL credentials
    |- postgresql/
    |   |-  .env               # PostgreSQL credentials
    |- rabbitmq/
    |   |-  .env               # RabbitMQ credentials
    |- mailpit/
    |   |-  .env               # Mailpit configuration
```

## Configuration

### Environment Files
Each service has its own `.env` file in the `services/` directory:
- **mysql/.env** - MySQL root password and database settings
- **postgresql/.env** - PostgreSQL user, password, and database
- **mongodb/.env** - MongoDB credentials and settings
- **rabbitmq/.env** - RabbitMQ admin credentials
- **mailpit/.env** - Mailpit SMTP configuration

### Data Persistence
All services use Docker volumes for data persistence:
- `mysql_data` - MySQL data
- `pg_data` - PostgreSQL data
- `mongo_data` - MongoDB data
- `rabbit_data` - RabbitMQ data
- `mailpit_data` - Mailpit email data

## Development Usage Examples

### Connecting from Application Code

#### Node.js/JavaScript
```javascript
// MySQL
const mysql = require('mysql2');
const connection = mysql.createConnection({
  host: 'localhost',
  port: 3306,
  user: 'root',
  password: 'your_password',
  database: 'your_database'
});

// PostgreSQL
const { Pool } = require('pg');
const pool = new Pool({
  host: 'localhost',
  port: 5432,
  user: 'your_user',
  password: 'your_password',
  database: 'your_database'
});

// MongoDB
const { MongoClient } = require('mongodb');
const client = new MongoClient('mongodb://username:password@localhost:27017');

// RabbitMQ
const amqp = require('amqplib');
const connection = await amqp.connect('amqp://admin:test1234@localhost:5672/');

// Email via Mailpit
const nodemailer = require('nodemailer');
const transporter = nodemailer.createTransporter({
  host: 'localhost',
  port: 1025,
  secure: false // No SSL/TLS
});
```

#### Python
```python
# MySQL
import mysql.connector
conn = mysql.connector.connect(
    host='localhost',
    port=3306,
    user='root',
    password='your_password',
    database='your_database'
)

# PostgreSQL
import psycopg2
conn = psycopg2.connect(
    host='localhost',
    port=5432,
    user='your_user',
    password='your_password',
    database='your_database'
)

# MongoDB
from pymongo import MongoClient
client = MongoClient('mongodb://username:password@localhost:27017')

# Email via Mailpit
import smtplib
server = smtplib.SMTP('localhost', 1025)
```

## Troubleshooting

### Common Issues

**Port conflicts:**
```bash
# Check if ports are in use
netstat -tlnp | grep :3306
netstat -tlnp | grep :5432
netstat -tlnp | grep :27017
```

**Service not starting:**
```bash
# Check service logs
docker-compose logs [service_name]
make logs
```

**Data persistence issues:**
```bash
# List volumes
docker volume ls

# Remove volumes (WARNING: This deletes all data)
make cleanup
```

**Permission issues:**
```bash
# Recreate containers
docker-compose down
docker-compose up -d --force-recreate
```

## Security Notes

- **Default passwords** are used for development only
- **Change credentials** in production environments
- **Environment files** contain sensitive data - never commit to version control
- **Firewall rules** should restrict access to localhost only

## Monitoring

### Health Checks
```bash
# Check all container status
docker-compose ps

# Individual service health
docker exec local-mysql mysqladmin ping
docker exec local-postgresql pg_isready
docker exec local-rabbitmq rabbitmqctl status
```

### Resource Usage
```bash
# Container resource usage
docker stats

# Volume disk usage
docker system df
```

---

**Happy coding!**

For issues or improvements, check the service logs and container status first.
