# MultiTier-App-Containerization

This project demonstrates how to containerize and run a **5-tier Java web application architecture** using **Docker** and **Docker Compose**.

The application is divided into multiple services, where each service runs inside its own Docker container.

## Architecture

The project contains 5 running containers:

| Service | Container Name | Technology | Purpose |
|---|---|---|---|
| Web Server | `vproweb` | Nginx | Reverse proxy for the application |
| Application Server | `vproapp` | Tomcat 10 + Java 21 | Runs the Java web application |
| Database | `vprodb` | MySQL 8.0.33 | Stores application data |
| Cache | `vprocache01` | Memcached | Provides caching support |
| Message Broker | `vpromq01` | RabbitMQ | Handles message queuing |

## Project Structure

```bash
MultiTier-App-Containerization/
├── Docker-files/
│   ├── app/
│   │   └── Dockerfile
│   ├── db/
│   │   ├── Dockerfile
│   │   └── db_backup.sql
│   └── web/
│       ├── Dockerfile
│       └── nginvproapp.conf
├── src/
├── vagrant/
├── compose.yml
└── .gitignore
```

## Docker Images Used

### Custom Built Images

These images are built using custom Dockerfiles and pushed to Docker Hub:

```bash
vish2002dil/vprofileapp
vish2002dil/vprofiledb
vish2002dil/vprofileweb
```

### Official Docker Hub Images

```bash
memcached
rabbitmq
```

## Prerequisites

Before running this project, make sure you have installed:

```bash
docker --version
docker compose version
git --version
```

Required tools:

- Docker
- Docker Compose
- Git

## How to Run the Project

### 1. Clone the Repository

```bash
git clone https://github.com/VishishtaDilsara/MultiTier-App-Containerization.git
cd MultiTier-App-Containerization
```

### 2. Build and Start All Containers

```bash
docker compose -f compose.yml up -d --build
```

This command will:

- Build the custom Docker images
- Create containers for all services
- Start the complete application stack
- Create required Docker volumes
- Connect all services through the Docker network

### 3. Check Running Containers

```bash
docker ps
```

You should see 5 containers running:

```bash
vproweb
vproapp
vprodb
vprocache01
vpromq01
```

### 4. Access the Application

Open your browser and visit:

```bash
http://localhost
```

You can also access the Tomcat application directly using:

```bash
http://localhost:8080
```

## Service Ports

| Service | Port |
|---|---|
| Nginx Web Server | `80` |
| Tomcat App Server | `8080` |
| MySQL Database | `3306` |
| Memcached | `11211` |
| RabbitMQ | `5672` |

## Useful Docker Commands

### View Logs

```bash
docker compose -f compose.yml logs -f
```

### View Logs of a Specific Service

```bash
docker logs vproapp
docker logs vproweb
docker logs vprodb
```

### Stop All Containers

```bash
docker compose -f compose.yml down
```

### Stop and Remove Containers with Volumes

```bash
docker compose -f compose.yml down -v
```

### Rebuild Containers

```bash
docker compose -f compose.yml up -d --build
```

### Check Docker Volumes

```bash
docker volume ls
```

## Persistent Volumes

This project uses Docker volumes to persist data:

```bash
vprodbdata
vproappdata
```

`vprodbdata` stores MySQL database data.

`vproappdata` stores Tomcat web application data.

## Application Flow

1. User accesses the application through Nginx on port `80`.
2. Nginx forwards the request to the Tomcat application container on port `8080`.
3. Tomcat connects with:
   - MySQL for database operations
   - Memcached for caching
   - RabbitMQ for messaging
4. The response is returned back to the user through Nginx.

## Docker Compose Services

The project is managed using `compose.yml`, which defines:

- `vprodb`
- `vprocache01`
- `vpromq01`
- `vproapp`
- `vproweb`

## Docker Hub Push Commands

After building the images, they can be pushed to Docker Hub using:

```bash
docker push vish2002dil/vprofileapp
docker push vish2002dil/vprofiledb
docker push vish2002dil/vprofileweb
```

## Troubleshooting

### Check if containers are running

```bash
docker ps -a
```

### Restart a container

```bash
docker restart vproapp
```

### Remove unused Docker resources

```bash
docker system prune
```

### Check container network

```bash
docker network ls
```

## Author

**Vishishta Dilsara**

## Project Topic

**Containerize 5-Tier Architecture Application**

## Technologies Used

- Docker
- Docker Compose
- Java
- Maven
- Apache Tomcat
- MySQL
- Nginx
- Memcached
- RabbitMQ
- GitHub
- Docker Hub
