# Spring Boot Kotlin + StatsD Metrics Example on Clever Cloud
[![Clever Cloud - PaaS](https://img.shields.io/badge/Clever%20Cloud-PaaS-orange)](https://clever-cloud.com)

This is a Spring Boot application developed with Kotlin that demonstrates how to build a REST API with JPA persistence and StatsD metrics export, and deploy it to Clever Cloud.

## About the Application

This application provides a REST API to manage customers, backed by an embedded H2 database. It uses [Micrometer](https://micrometer.io/) to export application metrics to a StatsD server.

### REST Endpoints

- `GET /customers` - List all customers
- `GET /customers/{lastName}` - Find customers by last name

### Customer Model

```json
{
    "id": 1,
    "firstName": "...",
    "lastName": "..."
}
```

### StatsD Metrics

The application exports the following custom metrics via StatsD:
- `bootstrap` - Counter incremented on application startup
- `views.customer` - Counter incremented on each `/customers` request
- `gaugeSample` - Random gauge value set on each `/customers` request
- Automatic HTTP request timing via `@Timed`

## Technology Stack

- [Spring Boot 3.4](https://spring.io/projects/spring-boot) - Application framework
- [Spring Data JPA](https://spring.io/projects/spring-data-jpa) - Database access
- [Micrometer + StatsD](https://micrometer.io/docs/registry/statsD) - Metrics export
- Kotlin 2.1
- Java 21
- Gradle 8.13
- H2 (embedded database)

## Prerequisites

- JDK 21+
- Gradle 8.13+ (or use the included Gradle Wrapper)

## Running the Application Locally

### Development Mode

```bash
./gradlew bootRun
```

The application will be accessible at http://localhost:8080.

### Running Tests

```bash
./gradlew test
```

## Deploying on Clever Cloud

You have two options to deploy your Spring Boot application on Clever Cloud: using the Web Console or using the Clever Tools CLI.

### Option 1: Deploy using the Web Console

#### 1. Create an account on Clever Cloud

If you don't already have an account, go to the [Clever Cloud console](https://console.clever-cloud.com/) and follow the registration instructions.

#### 2. Set up your application on Clever Cloud

1. Log in to the [Clever Cloud console](https://console.clever-cloud.com/)
2. Click on "Create" and select "An application"
3. Choose "Java + Gradle" as the runtime environment
4. Configure your application settings (name, region, etc.)

#### 3. Configure Environment Variables

Add the following environment variable in the Clever Cloud console:

| Variable | Value | Description |
|----------|-------|-------------|
| `CC_JAVA_VERSION` | `21` | Specifies to use Java 21 |

#### 4. Deploy Your Application

You can deploy your application using Git:

```bash
# Add Clever Cloud as a remote repository
git remote add clever git+ssh://git@push-par-clevercloud-customers.services.clever-cloud.com/app_<your-app-id>.git

# Push your code to deploy
git push clever master
```

### Option 2: Deploy using Clever Tools CLI

#### 1. Install Clever Tools

Install the Clever Tools CLI following the [official documentation](https://www.clever-cloud.com/doc/clever-tools/getting_started/):

```bash
# Using npm
npm install -g clever-tools

# Or using Homebrew (macOS)
brew install clever-tools
```

#### 2. Log in to your Clever Cloud account

```bash
clever login
```

#### 3. Create a new application

```bash
# Initialize the current directory as a Clever Cloud application
clever create --type gradle <YOUR_APP_NAME>

# Set the required environment variables
clever env set CC_JAVA_VERSION 21
```

#### 4. Deploy your application

```bash
clever deploy
```

#### 5. Open your application in a browser

Once deployed, you can access your application at the URL provided by Clever Cloud.

```bash
# List all customers
curl https://<your-domain>/customers

# Find customers by last name
curl https://<your-domain>/customers/Bauer
```

### StatsD on Clever Cloud

Clever Cloud provides a built-in StatsD endpoint at `localhost:8125` for all applications. The application is pre-configured to send metrics to this address — no additional setup is required.

### Monitoring Your Application

Once deployed, you can monitor your application through:

- **Web Console**: The Clever Cloud console provides logs, metrics, and other tools to help you manage your application.
- **CLI**: Use `clever logs` to view application logs and `clever status` to check the status of your application.

## Additional Resources

- [Spring Boot Documentation](https://docs.spring.io/spring-boot/reference/)
- [Micrometer StatsD Registry](https://micrometer.io/docs/registry/statsD)
- [Clever Cloud Documentation](https://www.clever-cloud.com/doc/)
