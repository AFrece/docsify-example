# Getting started

Petrol Cart Service provides a REST API for cart related services such as creating a cart, adding and removing items from cart, creating order from cart and order fulfillments.

# Prerequisites
* JDK 11
* Maven 3.6.0+
* git
* Postman

# Building
```
mvn clean package
```
# Database migration
> [!WARNING]
>PostgreSQL 9.6.X is required.

You can start the database using `docker-compose.yml` sample with 

```bash
docker-compose up -d cart-service-pg
```
Once the database is running, modify `models/src/main/resources/db/main.properties` with correct connection info. 

> [!NOTE]
> This step is necessary in case you are not using database instance started using docker-compose, or if you have modified docker-compose configuration file.

Run the migration using:

```bash
mvn install
cd models
mvn liquibase:update
```


# Running

<!-- tabs:start -->

#### ** Command line **

Modify the configuration `api/src/main/resources/config.yml`.

Run:
```bash
mvn clean package
cd v1/api
java -Djava.util.logging.manager=org.apache.logging.log4j.jul.LogManager -cp "target/classes;target/dependency/*" com.kumuluz.ee.EeApplication
```

#### ** Docker **
> [!DANGER]
> Docker must already be installed!

> [!TIP]
> An example of `.ci/docker-compose.yml` is included in the root of the project.

Before running, you need to build all the necessary images and set proper config in compose.

**Buiding the service image**

```bash
mvn clean package
cd v1
docker build -t cart-service .
```

**Run with compose**

Once all images are built and environment variables in `docker-compose.yml` properly configured, start all the services with 
```bash
docker-compose up -d
```

<!-- tabs:end -->

