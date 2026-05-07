# Configuration Repository for E-Commerce Microservices

This repository contains centralized configuration files for all microservices in the e-commerce application.

## Configuration Files

### Overview
- **api-gateway-dev.properties** - API Gateway configuration (port 8080)
- **order-service-dev.properties** - Order Service configuration (port 9001)
- **product-service-dev.properties** - Product Service configuration (port 9002)
- **user-service-dev.properties** - User Service configuration (port 9003)

### Naming Convention
Configuration files follow the pattern: `{service-name}-{profile}.properties`

Where:
- `{service-name}` = Spring application name (e.g., api-gateway, order-service)
- `{profile}` = Environment profile (dev, prod, staging)

## How It Works

1. **Config Server** (Port 8888) reads these files from this Git repository
2. **Client Services** fetch their configuration from the Config Server at startup
3. Changes can be pushed to this repo, and services can refresh configs without restarting

## Configuration Server URL
```
http://localhost:8888
```

## Fetching Configuration

To fetch configuration for a specific service:
```
GET http://localhost:8888/{service-name}/{profile}
```

Example:
```
GET http://localhost:8888/order-service/dev
```

## Key Services and Ports

| Service | Port | Profile |
|---------|------|---------|
| Eureka Server | 8761 | - |
| Config Server | 8888 | - |
| API Gateway | 8080 | dev |
| Order Service | 9001 | dev |
| Product Service | 9002 | dev |
| User Service | 9003 | dev |

## Important Notes

1. **Redis**: Configured at `localhost:6379` for API Gateway rate limiting
2. **Database**: Using H2 in-memory database for development
3. **Security**: JWT tokens are configured in user-service and api-gateway
4. **Eureka**: All services register with Eureka at `http://localhost:8761/eureka/`

## How to Add New Configuration

1. Create a new file: `{service-name}-{profile}.properties`
2. Add your configuration properties
3. Commit and push to this repository
4. Services will automatically pick up changes on refresh or restart

## Example: Refreshing Configuration at Runtime

Send a POST request with actuator endpoint:
```bash
curl -X POST http://localhost:{service-port}/actuator/refresh
```

(Requires spring-boot-starter-actuator dependency)

## Security Recommendations

1. Use `.gitignore` to exclude sensitive files
2. Store secrets in environment variables, not in config files
3. Use different profiles for dev, staging, and production
4. Enable authentication on Config Server in production
5. Use SSH for Git repository access

---

Last Updated: May 7, 2026

