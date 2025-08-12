# Currency Microservices Demo

> A simple microservices demo showcasing service discovery, API Gateway, and inter-service communication  
> Built with Spring Boot, Eureka, and Spring Cloud Gateway

---

## Overview

This project consists of four related microservices demonstrating common patterns:

| Service Name             | Purpose                        | Repo Link                                                                                     |
|-------------------------|-------------------------------|----------------------------------------------------------------------------------------------|
| naming-server           | Service Discovery (Eureka)     | [naming-server](https://github.com/Mehrdad-Ghaderi/naming-server)                            |
| currency-exchange       | Provides currency exchange rates | [currency-exchange-service](https://github.com/Mehrdad-Ghaderi/currency-exchange-service)    |
| currency-conversion     | Converts currency using exchange | [currency-conversion-service](https://github.com/Mehrdad-Ghaderi/currency-conversion-service)|
| api-gateway            | Routes client requests          | [api-gateway](https://github.com/Mehrdad-Ghaderi/api-gateway)                                |

---

## Architecture Diagram

                          [Client]
                             |
                             v
                        [API Gateway] 
                         (port 8765)
                             |
                             v
                   -----------------------
                   |                     |
         [currency-conversion] [currency-exchange]
               (port 8100)         (port 8000)
                             |
                             v
                      [naming-server]
                        (port 8761)


- The API Gateway routes requests to microservices using service discovery from the naming-server (Eureka).
- The currency-conversion service calls the currency-exchange service internally.
- The naming-server acts as a registry for all services.

---

## How to run locally

1. **Clone all repos** under a single parent folder:
    ```bash
    git clone https://github.com/Mehrdad-Ghaderi/naming-server
    git clone https://github.com/Mehrdad-Ghaderi/currency-exchange-service
    git clone https://github.com/Mehrdad-Ghaderi/currency-conversion-service
    git clone https://github.com/Mehrdad-Ghaderi/api-gateway
    ```

2. **Start the naming-server (Eureka)** first:
    ```bash
    cd naming-server
    mvn spring-boot:run
    ```
3. **Start currency-exchange service:**
    ```bash
    cd ../currency-exchange-service
    mvn spring-boot:run
    ```

4. **Start currency-conversion service:**
    ```bash
    cd ../currency-conversion-service
    mvn spring-boot:run
    ```

5. **Start the API gateway:**
    ```bash
    cd ../api-gateway
    mvn spring-boot:run
    ```

6. Open your browser to [http://localhost:8761](http://localhost:8761) to view the Eureka dashboard — all services should be registered.

7. Test the API gateway by hitting a sample endpoint, for example:
    ```bash
    curl "http://localhost:8765/api/v1/currency-conversion/from/USD/to/CAD/quantity/5"
    ```
   ```bash
    curl "http://localhost:8765/api/v1/currency-conversion-feign/from/USD/to/CAD/quantity/6"
    ```
   ```bash
    curl "http://localhost:8765/api/v1/currency-exchange/from/USD/to/CAD/quantity/7"
    ```

---

## Troubleshooting Tips

- If a service does not appear in Eureka, check its `application.properties` for the correct `eureka.client.service-url.defaultZone` value.
- Make sure the naming-server is running before starting other services.
- Check ports if services fail to start due to conflicts.
- Logs can be found in the console where you run `mvn spring-boot:run`.

---

## Next Steps

- Optionally, create Dockerfiles and Docker Compose files to run all services together in containers.
- Add OpenAPI/Swagger docs for easier API exploration.
- Add CI/CD pipelines to automate build and deploy.

# TL;DR

- 4 microservices with separate repos: naming-server, currency-exchange, currency-conversion, api-gateway
- Start services in this order: naming-server → currency-exchange → currency-conversion → api-gateway
- Access Eureka dashboard at [http://localhost:8761](http://localhost:8761)

