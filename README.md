# BookStoreApp-Distributed-Application

<hr>

## About this project
This is an Ecommerce project currently in development, where users can add books to a cart and complete purchases.

The application is developed using Java, Spring, and React. It utilizes Spring Cloud Microservices and the Spring Boot Framework extensively to create a distributed architecture.

<hr>

## Frontend Checkout Flow
![CheckOutFlow](https://user-images.githubusercontent.com/14878408/103235826-06d5ca00-4969-11eb-87c8-ce618034b4f3.gif)

## Architecture
All microservices are developed using Spring Boot. These applications register with a Eureka discovery server.

The Frontend React App makes requests to an NGINX server which acts as a reverse proxy. The NGINX server redirects requests to the Zuul API Gateway.

Zuul routes requests to the appropriate microservice based on the URL route. Zuul also registers with Eureka to retrieve the IP or domain for the microservice while routing the request.

<hr>

## Run this project in Local Machine

> Frontend App

Navigate to the "bookstore-frontend-react-app" folder. Run the below commands to start the Frontend React Application:

```
yarn install
yarn start
```

> Backend Services

To start backend services, follow the steps below using an IDE (IntelliJ/Eclipse) or the Command Line.

Import this project into your IDE and run all Spring Boot projects, or build all jars by running the "mvn clean install" command in the root parent pom. All services will be available on the ports mentioned below.

Note: Running services individually does not provide full monitoring metrics. To see metrics like JVM memory, Tomcat error counts, and other data, use the Docker deployment method.

> Using Docker (Recommended)

1. Start the Docker Engine on your machine.
2. Run "mvn clean install" at the root of the project to build all microservice jars.
3. Run "docker-compose up --build" to start all containers.

Use the "Postman Api collection" in the Postman directory to make requests to various services.

Services will be exposed on these ports:

```
Api Gateway Service       : 8765
Eureka Discovery Service  : 8761
Consul Discovery          : 8500
Account Service           : 4001
Billing Service           : 5001
Catalog Service           : 6001
Order Service             : 7001
Payment Service           : 8001
```

<hr>

### Service Discovery
This project uses Eureka or Consul as the discovery service.

When running services locally, Eureka is used for service discovery. When running via Docker, Consul is used as the service discovery mechanism.

Consul is preferred in the Docker environment due to its robust feature set. Eureka is used for individual local runs to avoid the overhead of managing a local Consul agent.

<hr>

### Troubleshooting

If you encounter issues starting services or if an API fails, it may be due to schema changes in the database.

In such cases, clear or drop the "bookstore_db" database and restart the services. If issues persist, please raise an issue in the repository for assistance.

<hr>

## Deployment
The intended cloud provider for this project is AWS.

The project is designed to be deployed across multiple Regions and Availability Zones.

- The React App, Zuul, and Eureka serve as public-facing services located in a public subnet.
- Microservices are packaged into Docker containers and deployed in AWS ECS within a private subnet.
- Private subnets use a NAT Gateway for external internet requests.
- A Bastion host is used for SSH access into private subnet microservices.

The AWS Architecture diagram below provides a visual overview:

![Bookstore Final](https://user-images.githubusercontent.com/14878408/65784998-000e4500-e171-11e9-96d7-b7c199e74c4c.jpg)

<hr>

## Monitoring
There are two setups available for monitoring:

1. Prometheus and Grafana.
2. TICK stack monitoring.

Prometheus operates on a pull model. We use Consul discovery to provide target hosts dynamically, ensuring that new service instances are automatically monitored without manual configuration.

The TICK (Telegraf, InfluxDB, Chronograf, Kapacitor) stack uses both push and pull models. Services push metrics to InfluxDB, while Telegraf pulls metrics from specified targets. Chronograf or Grafana can be used for visualization, and Kapacitor handles alarm configurations.

The "docker-compose" configuration manages all monitoring containers.

Dashboards are available at these ports:

```
Grafana   : 3030
Zipkin     : 9411
Prometheus : 9090
Telegraf   : 8125
InfluxDb   : 8086
Chronograf : 8888
Kapacitor  : 9092 
```

For the first-time login to Grafana, use the following credentials:
Username: admin
Password: admin

<hr>

**Screenshots of Tracing in Zipkin.**

<img alt="Zipkin" src="https://user-images.githubusercontent.com/14878408/65939069-6b426a80-e442-11e9-90fd-d54b60786d41.png">
<hr>
<img alt="Zipkin" src="https://user-images.githubusercontent.com/14878408/65939165-bb213180-e442-11e9-9ad7-5cfd4fa121ef.png">

<hr>

**Screenshots of Monitoring in Grafana.**

<img width="1680" alt="Screen Shot 2019-10-16 at 9 16 21 PM" src="https://user-images.githubusercontent.com/14878408/66936473-65ac6d80-f05b-11e9-9e7d-9652059438cd.png">

<img width="1680" alt="Screen Shot 2019-10-16 at 9 16 12 PM" src="https://user-images.githubusercontent.com/14878408/66936524-79f06a80-f05b-11e9-8898-1002813aad8e.png">

<hr>

**Screenshots of Monitoring in Chronograf (TICK).**

![Screen Shot 2019-10-16 at 12 44 20 PM](https://user-images.githubusercontent.com/14878408/66934353-f8e3a400-f057-11e9-82ab-eda7a230c09d.png)

![Screen Shot 2019-10-16 at 12 52 08 PM](https://user-images.githubusercontent.com/14878408/66934482-2e888d00-f058-11e9-8dea-f1f275765265.png)

<hr>

> Account Service

To obtain an "access_token" for a user, you require a "clientId" and "clientSecret".

```
clientId : '93ed453e-b7ac-4192-a6d4-c45fae0d99ac'
clientSecret : 'client.devd123'
```

Current users in the system:

```
Admin 
userName: 'admin.admin'
password: 'admin.devd123'
```

```
Normal User 
userName: 'devd.cores'
password: 'cores.devd123'
```

To get the accessToken for the Admin User:
"curl 93ed453e-b7ac-4192-a6d4-c45fae0d99ac:client.devd123@localhost:4001/oauth/token -d grant_type=password -d username=admin.admin -d password=admin.devd123"

<hr>

## About the Developer
Ruchira Dayanand Patil is a Software Developer with over 5 years of professional experience in full-stack development and distributed systems. Specializing in Java, Spring, and modern cloud architectures, Ruchira maintains this project to demonstrate scalable microservice patterns and robust monitoring implementations.

**Contact Information:**
- Email: ruchirapatil2000@gmail.com
- LinkedIn: https://www.linkedin.com/in/ruchira-patil-1662181b8