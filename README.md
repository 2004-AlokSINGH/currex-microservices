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
  currency-network

# 🚀 Spring Boot Microservices with Docker & Docker Compose

This project demonstrates a microservices architecture using **Spring Boot**, **Docker**, and **Docker Compose**. It features service discovery with **Eureka**, API routing with **Spring Cloud Gateway**, inter-service communication using **Feign**, and **Zipkin** for distributed tracing.

---

## 🔗 Application URLs

### 🏦 Currency Exchange Service  
> Handles currency exchange rates  
**URL:**  
`http://localhost:8000/currency-exchange/from/USD/to/INR`

---

### 💱 Currency Conversion Service  
> Converts currency using REST and Feign clients  
**URLs:**  
- `http://localhost:8100/currency-conversion/from/USD/to/INR/quantity/10`  
- `http://localhost:8100/currency-conversion-feign/from/USD/to/INR/quantity/10`

---

### 📘 Eureka Naming Server  
> Service discovery and registration  
**URL:**  
`http://localhost:8761/`

---

### 🚪 API Gateway (Spring Cloud Gateway)  
> Routes all client requests to appropriate microservices  
**URLs:**  
- `http://localhost:8765/currency-exchange/from/USD/to/INR`  
- `http://localhost:8765/currency-conversion/from/USD/to/INR/quantity/10`  
- `http://localhost:8765/currency-conversion-feign/from/USD/to/INR/quantity/10`  
- `http://localhost:8765/currency-conversion-new/from/USD/to/INR/quantity/10`

---

### 📈 Zipkin for Distributed Tracing  
> View request flow between microservices  
**URL:**  
`http://localhost:9411/`

---

## 🐳 Docker Commands

```bash
# 🧪 Run Zipkin manually (if not using Docker Compose)
docker run -p 9411:9411 openzipkin/zipkin:2.23

# 📤 Push Docker images to Docker Hub
docker push singhalok2024/mmv3-currency-exchange-service:0.0.1-SNAPSHOT
docker push singhalok2024/mmv3-currency-conversion-service:0.0.1-SNAPSHOT
docker push singhalok2024/mmv3-naming-server:0.0.1-SNAPSHOT
docker push singhalok2024/mmv3-api-gateway:0.0.1-SNAPSHOT

# 🔧 Check Docker Compose version
docker-compose --version

# ▶️ Start all services using Docker Compose
docker-compose up

# 🔁 Monitor a sample API continuously
watch -n 0.1 curl http://localhost:8000/sample-api
```

---

## 🧠 Concepts Covered

- ✅ Spring Boot Microservices
- 🧭 Eureka Discovery Server
- 🌉 Spring Cloud Gateway
- 🔗 Feign REST Clients
- 📊 Centralized Logging & Zipkin Distributed Tracing
- 🐳 Docker & Docker Compose

---

## 👨‍💻 Maintainer

**Alok Singh**  
Docker Hub: [singhalok2024](https://hub.docker.com/u/singhalok2024)

---

