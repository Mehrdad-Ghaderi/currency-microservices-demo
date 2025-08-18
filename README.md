# Currency Microservices Demo

> A simple microservices demo showcasing service discovery, API Gateway, and inter-service communication  
> Built with Spring Boot, Eureka, Spring Cloud Gateway, and Docker

---

## Overview

This project consists of **four related microservices** demonstrating common patterns:

| Service Name        | Purpose                                                          | Repo Link                                                                                     |
|---------------------|------------------------------------------------------------------|-----------------------------------------------------------------------------------------------|
| naming-server       | Service Discovery (Eureka)                                       | [naming-server](https://github.com/Mehrdad-Ghaderi/naming-server)                             |
| currency-exchange   | Provides currency exchange rates(DB & live from Frankfurter API) | [currency-exchange-service](https://github.com/Mehrdad-Ghaderi/currency-exchange-service)     |
| currency-conversion | Converts currency using exchange                                 | [currency-conversion-service](https://github.com/Mehrdad-Ghaderi/currency-conversion-service) |
| api-gateway         | Routes client requests                                           | [api-gateway](https://github.com/Mehrdad-Ghaderi/api-gateway)                                 |

- The currency-exchange service fetches live currency exchange rates from an external API, and in case the fetching
  fails, it uses the local H2 database.

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

- naming-server: central registry for all microservices; allows dynamic discovery without hardcoding URLs
- api-gateway: exposes a single entry point for clients, routing requests and applying cross-cutting concerns like
  logging or security.
- currency-exchange: provides exchange rates; now enhanced to fetch live rates for more realistic scenarios.
- currency-conversion: demonstrates inter-service communication by converting currency amounts using exchange rates from
  currency-exchange.
- Zipkin collects distributed tracing data.

---

## Run with Docker Compose

1. Clone this umbrella repo, which provides only a README and a docker-compose.yaml file:

```bash
git clone https://github.com/Mehrdad-Ghaderi/currency-microservices-demo.git
cd currency-microservices-demo
```

2. Start all service with Docker Compose:

```
docker-compose up -d
```

3. Access services:

- Eureka dashboard: http://localhost:8761
- Zipkin dashboard: http://localhost:9411
- API Gateway sample endpoints:

``` bash
  curl "http://localhost:8765/api/v1/currency-exchange/from/USD/to/CAD""
  ```

``` bash
  curl "http://localhost:8765/api/v1/currency-conversion/from/USD/to/CAD/quantity/5"
  ```

 ``` bash
  curl "http://localhost:8765/api/v1/currency-conversion-feign/from/USD/to/CAD/quantity/6"
```

## Notes

- Environment variables for Eureka and Zipkin are already configured in docker-compose.yaml.
- This setup works on any machine with Docker and Docker Compose installed.

---

## To access the services directly:

   ``` bash
    curl "http://localhost:8000/api/v1/currency-exchange/from/USD/to/CAD"
   ```

   ``` bash
    curl "http://localhost:8100/api/v1/currency-conversion/from/USD/to/CAD/quantity/5"
   ```

``` bash
 curl "http://localhost:8100/api/v1/currency-conversion-feign/from/USD/to/CAD/quantity/6"
 ```

## Troubleshooting Tips

- If a service does not appear in Eureka, check its `application.properties` for the
  correct `eureka.client.service-url.defaultZone` value.
- Make sure the naming-server is running before starting other services.
- Check ports if services fail to start due to conflicts.
- Logs can be found in the console where you run `mvn spring-boot:run`.

---

# TL;DR

- 4 microservices with separate repos: naming-server, currency-exchange, currency-conversion, api-gateway
- Start services in this order: naming-server → currency-exchange → currency-conversion → api-gateway
- Access Eureka dashboard at [http://localhost:8761](http://localhost:8761)
- Access Zipkin dashboard at [http://localhost:9411](http://localhost:9411)
- Access exchange service through Eureka
  at [http://localhost:8765/api/v1/currency-exchange/from/USD/to/CAD](http://localhost:8765/api/v1/currency-exchange/from/USD/to/CAD)
- Access conversion service through Eureka
  at [http://localhost:8765/api/v1/currency-conversion-feign/from/USD/to/CAD/quantity/6](http://localhost:8765/api/v1/currency-conversion-feign/from/USD/to/CAD/quantity/6)
- Access exchange service
  directly [http://localhost:8000/api/v1/currency-exchange/from/USD/to/CAD](http://localhost:8000/api/v1/currency-exchange/from/USD/to/CAD)
- Access conversion service
  directly [http://localhost:8100/api/v1/currency-conversion-feign/from/USD/to/CAD/quantity/6](http://localhost:8100/api/v1/currency-conversion-feign/from/USD/to/CAD/quantity/6)
