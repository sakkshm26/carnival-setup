# Carnival - Setup

## Prerequisites

- Docker and Docker Compose installed on your system
- Git installed
- At least 8GB of RAM available for Docker containers
- Ports 3000, 4000, 4001, 4002, 5050, 5432, 6379, 9000, 9001, 9091, and 19530 available

## Quick Setup

### 1. Clone the Repository

```bash
git clone https://github.com/sakkshm26/carnival-setup
cd carnival-setup
```

### 2. Create Environment File

Copy the example environment file and configure your settings:

```bash
cp .env.example .env
```

Edit the `.env` file and update the following required variables:

- `BETTER_AUTH_SECRET`: Generate a strong random string for authentication
- `OPENAI_API_KEY`: Your OpenAI API key for AI functionality
- `GOOGLE_CLIENT_ID` & `GOOGLE_CLIENT_SECRET`: Google OAuth credentials (optional)
- `SLACK_CLIENT_ID` & `SLACK_CLIENT_SECRET`: Slack OAuth credentials (optional)

### 3. Start the Application

Run the following command to start all services:

```bash
docker compose -f docker-compose.yml -p carnival-stack up -d --pull always --force-recreate
```

This command will:
- Pull the latest images for all services
- Create and start all containers in detached mode
- Force recreate containers to ensure clean state
- Use the project name "carnival-stack"

## Services Overview

Once started, the following services will be available:

- **Carnival Client** (http://localhost:3000) - Main web application
- **Carnival Server** (http://localhost:4000) - Backend API server
- **Vector Server** (http://localhost:4001) - Vector database operations
- **Bull Queue UI** (http://localhost:4002) - Queue monitoring dashboard
- **PgAdmin** (http://localhost:5050) - PostgreSQL database admin
- **MinIO Console** (http://localhost:9001) - Object storage admin

### Database Services

- **PostgreSQL**: localhost:5432 (Database)
- **Redis**: localhost:6379 (Cache & Queue)
- **Milvus**: localhost:19530 (Vector Database)

## Default Credentials

### PgAdmin
- Email: admin@example.com
- Password: admin123

### Bull Queue Dashboard
- Username: admin
- Password: admin123

### MinIO
- Access Key: minioadmin
- Secret Key: minioadmin

## Health Checks

The application includes health checks for critical services. You can verify all services are running:

```bash
docker compose -p carnival-stack ps
```

## Stopping the Application

To stop all services:

```bash
docker compose -p carnival-stack down
```

To stop and remove all data volumes:

```bash
docker compose -p carnival-stack down -v
```

## Troubleshooting
### Logs

View logs for specific services:

```bash
# View all logs
docker compose -p carnival-stack logs

# View specific service logs
docker compose -p carnival-stack logs carnival-server

# Follow logs in real-time
docker compose -p carnival-stack logs -f
```

### Rebuilding

If you need to rebuild and restart everything:

```bash
docker compose -p carnival-stack down -v
docker compose -f docker-compose.yml -p carnival-stack up -d --pull always --force-recreate
```

## Configuration

### Environment Variables

All configurable options are available in the `.env` file.

### OAuth Setup

To enable Google or Slack authentication:

1. Create OAuth applications in respective developer consoles
2. Update the client ID, secret, and redirect URIs in your `.env` file
3. Restart the services

## Development

For development purposes, you can mount local directories and modify the docker-compose.yml file accordingly. The application logs are already mounted to `./logs` directory for easy access.
