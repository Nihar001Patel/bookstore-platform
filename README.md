## Local Development Setup
* Install Java 21. Recommend using [SDKMAN](https://sdkman.io/) for managing Java versions.
* Install [Docker Desktop](https://www.docker.com/products/docker-desktop/)
* Install [IntelliJ IDEA](https://www.jetbrains.com/idea) or any of your favorite IDE
* Install [Taskfile](https://taskfile.dev/) utility
* Install [Postman](https://www.postman.com/) or any REST Client

```shell
$ curl -s "https://get.sdkman.io" | bash
$ source "$HOME/.sdkman/bin/sdkman-init.sh"
$ sdk install java 21.0.1-tem
$ sdk install maven
$ brew install go-task
(or)
$ go install github.com/go-task/task/v3/cmd/task@latest

```

Verify the prerequisites

```shell
$ java -version
$ docker info
$ docker compose version
$ task --version
```

### How to run the application locally?

```shell
# Clone your repository:
$ git clone https://github.com/nihar001patel/bookstore-platform.git
$ cd bookstore-platform
```

#### Option 1: Start the infra components using Docker Compose and run microservices from IDE

1. **Start all the required services such as PostgreSQL, RabbitMQ, Keycloak, etc.:** `$ task start_infra`

2. **Start individual microservices:**
  You can start individual microservices by running their respective main entrypoint classes from IDE: `ApiGatewayApplication`, `CatalogServiceApplication`, `OrderServiceApplication`, `NotificationServiceApplication`, `BookstoreWebappApplication`

3. **Access the application** at http://localhost:8080

* Catalog Service PostgreSQL DB: `jdbc:postgresql://localhost:15432/postgres` with credentials `postgres/postgres`
* Order Service PostgreSQL DB: `jdbc:postgresql://localhost:25433/postgres` with credentials `postgres/postgres`
* RabbitMQ: `http://localhost:15672` with credentials `guest/guest`
* Keycloak: `http://localhost:9191` with credentials `admin/admin1234`
* MailHog: `http://localhost:8025`

#### Option 2: Working on individual microservices

Each microservice has Testcontainers based configuration to start the required services such as PostgreSQL, RabbitMQ, Keycloak, etc automatically.

You can start individual microservices by running their respective Test main entrypoint classes from IDE: `TestCatalogServiceApplication`, `TestOrderServiceApplication`, `TestNotificationServiceApplication`, `ApiGatewayApplication`, `BookstoreWebappApplication`.

#### Option 3: Run all the infra components and applications using Docker Compose

1. **Start all:** `$ task start`

2. **Access the application** at http://localhost:8080


## Run the application with Observability Stack

1. Start Grafana, Tempo, Loki, Prometheus using `$ task start_monitoring`
2. Set `MANAGEMENT_TRACING_ENABLED=true` in `deployment/docker-compose/.env` file
3. Restart the application using `$ task restart`

Now you can access the observability stack using the following URLs:

* Access Grafana at http://localhost:3000
* Access Prometheus at http://localhost:9090
