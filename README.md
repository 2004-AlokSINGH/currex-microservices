# currex-microservices

# 🧾 Currency Microservices with Spring Boot & Docker

This project demonstrates a cloud-native microservices architecture using **Spring Boot**, **Docker**, **Eureka**, **Spring Cloud Gateway**, and **Zipkin** for distributed tracing. The services are containerized and managed via **Docker Compose**.

---

## 🛠️ Services Overview

| Service                    | Port  | Docker Image                                                       |
|---------------------------|-------|---------------------------------------------------------------------|
| Currency Exchange Service | 8000  | `singhalok2024/mmv3-currency-exchange-service:0.0.1-SNAPSHOT`      |
| Currency Conversion       | 8100  | `singhalok2024/mmv3-currency-conversion-service:0.0.1-SNAPSHOT`    |
| Eureka Naming Server      | 8761  | `singhalok2024/mmv3-naming-server:0.0.1-SNAPSHOT`                  |
| API Gateway               | 8765  | `singhalok2024/mmv3-api-gateway:0.0.1-SNAPSHOT`                    |
| Zipkin                    | 9411  | `openzipkin/zipkin:2.23`                                           |

---

## 📦 Docker Compose Setup (Sample Configuration)

```yaml
version: '3.8'
services:

  naming-server:
    image: singhalok2024/mmv3-naming-server:0.0.1-SNAPSHOT
    ports:
      - "8761:8761"
    networks:
      - currency-network

  currency-exchange:
    image: singhalok2024/mmv3-currency-exchange-service:0.0.1-SNAPSHOT
    mem_limit: 700m
    ports:
      - "8000:8000"
    networks:
      - currency-network
    depends_on:
      - naming-server
    environment:
      EUREKA.CLIENT.SERVICEURL.DEFAULTZONE: http://naming-server:8761/eureka
      MANAGEMENT.ZIPKIN.TRACING.ENDPOINT: http://zipkin-server:9411/api/v2/spans

  currency-conversion:
    image: singhalok2024/mmv3-currency-conversion-service:0.0.1-SNAPSHOT
    mem_limit: 700m
    ports:
      - "8100:8100"
    networks:
      - currency-network
    depends_on:
      - naming-server
      - currency-exchange
    environment:
      EUREKA.CLIENT.SERVICEURL.DEFAULTZONE: http://naming-server:8761/eureka
      MANAGEMENT.ZIPKIN.TRACING.ENDPOINT: http://zipkin-server:9411/api/v2/spans

  api-gateway:
    image: singhalok2024/mmv3-api-gateway:0.0.1-SNAPSHOT
    mem_limit: 700m
    ports:
      - "8765:8765"
    networks:
      - currency-network
    depends_on:
      - naming-server
      - currency-conversion
      - currency-exchange
    environment:
      EUREKA.CLIENT.SERVICEURL.DEFAULTZONE: http://naming-server:8761/eureka
      MANAGEMENT.ZIPKIN.TRACING.ENDPOINT: http://zipkin-server:9411/api/v2/spans

  zipkin-server:
    image: openzipkin/zipkin:2.23
    ports:
      - "9411:9411"
    networks:
      - currency-network

networks:
  currency-network:
