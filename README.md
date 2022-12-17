# BookStore Deployment

The bookstore application is implemented using multiple independently deployable services.
Each service can be implemented using any language/framework of your choice.
Also, each service has it's own data persistence mechanism and any database shouldn't be shared by multiple services.
However, a message broker will be used to communicate order events to other services and decided to use Kafka for this purpose.

## How to run the application using docker-compose?

### 1. I want to work on only a specific service
Each service implementation will provide a **docker-compose.yaml** file which defines all the dependent services.
You should be able to run `docker-compose up -d` to start these containers and run your application from IDE or terminal.

For example, if you want to run `catalog-service-java-springboot` service then you do following:

```shell
$ git clone https://github.com/sivalabs-bookstore/catalog-service-java-springboot.git
$ cd catalog-service-java-springboot
$ docker-compose up -d
$ ./mvnw spring-boot:run (or run the application from your IDE)
```

### 2. I just want to run one or more services
Each service implementation will be published as a Docker image to DockerHub.
So, if you just want to run one or more services then you can use docker-compose manifest files in https://github.com/sivalabs-bookstore/bookstore-deployment/tree/main/docker-compose

> **IMPORTANT NOTE**
>
> Due to the memory limitations of your computer, sometimes services may not start properly.
> Please check the container logs to make sure the services started correctly.

If you want to just run **catalog-service-java-springboot** and **payment-service-java-springboot** services then you can do the following:

```shell
$ git clone https://github.com/sivalabs-bookstore/bookstore-deployment.git
$ cd bookstore-deployment/docker-compose
$ docker-compose -f payment-service-java-springboot.yaml -f catalog-service-java-springboot.yaml up -d
```

The following services depends on **Kafka**:
* order-service
* delivery-service

If you want to run any service that depends on Kafka then you should include **common-infra.yaml** to the list of YAML files.

For example if you want to run **payment-service-java-springboot** and **order-service-java-springboot** then you can do the following:

```shell  
docker-compose -f common-infra.yaml  -f payment-service-java-springboot.yaml -f order-service-java-springboot.yaml up -d
```
