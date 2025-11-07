# Food Delivery Management System

## Problem Statement

Customers place food orders from local restaurants and expect clear updates from order placement to delivery. Restaurants and delivery workers need a system that handles menus, orders, and delivery tasks without manual coordination. The system must support independent services with reliable communication.

## Objectives

- Receive and update food orders through clear status steps.
- Assign delivery workers and update delivery progress.
- Send notifications to customers and delivery workers at key events.
- Keep services independent and scalable.
- Manage restaurant menus and availability.

## Proposed Architecture

The system consists of eight microservices communicating via REST for synchronous calls, with Kafka handling asynchronous event messaging:

1. User Service — manages user data (profiles, identities).
2. Product Service — manages restaurant products/menu items and availability.
3. Cart Service — maintains user carts prior to checkout.
4. Order Service — processes orders and updates order status.
5. Payment Service — handles payment transactions and confirmations.
6. Delivery Service — assigns drivers and tracks shipments/delivery progress.
7. Notification Service — sends real-time updates to customers and workers.
8. Analytics Service — aggregates events and operational metrics for insights.

Architecture sketch link: (add link here)

### Architecture Diagram

```mermaid
flowchart LR
	subgraph Clients
		CUST[Customer App]
	end

	subgraph Edge
		NGINX[Nginx / Ingress<br/>Load Balancer]
	end

	CUST --> NGINX

	subgraph Core[Core Services]
		USER[User Service]
		PRODUCT[Product Service]
		CART[Cart Service]
		ORDER[Order Service]
		PAYMENT[Payment Service]
		DELIVERY[Delivery Service]
		NOTIFY[Notification Service]
		ANALYTICS[Analytics Service]
	end

	subgraph Data[Data Stores]
		MYSQL[(MySQL Cluster)]
		REDIS[(Redis Cache)]
	end

	subgraph Infra[Messaging & Orchestration]
		KAFKA[(Kafka Broker)]
		K8S[Kubernetes]
	end

	NGINX --> USER
	NGINX --> PRODUCT
	NGINX --> CART
	NGINX --> ORDER
	NGINX --> PAYMENT
	NGINX --> DELIVERY

	USER --> MYSQL
	PRODUCT --> MYSQL
	CART --> REDIS
	ORDER --> MYSQL
	PAYMENT --> MYSQL
	DELIVERY --> MYSQL
	NOTIFY --> MYSQL
	ANALYTICS --> MYSQL

	CART --> ORDER
	USER --> ORDER
	PRODUCT --> CART
	ORDER --> PAYMENT
	PAYMENT --> ORDER
	ORDER --> DELIVERY
	DELIVERY --> ORDER

	ORDER -- events --> KAFKA
	PAYMENT -- events --> KAFKA
	DELIVERY -- events --> KAFKA
	NOTIFY -- consumes --> KAFKA
	ANALYTICS -- consumes --> KAFKA

	K8S --- USER
	K8S --- PRODUCT
	K8S --- CART
	K8S --- ORDER
	K8S --- PAYMENT
	K8S --- DELIVERY
	K8S --- NOTIFY
	K8S --- ANALYTICS
```

## Key Technologies

- Language: Node.js
- Database: MySQL
- Message Broker: Apache Kafka
- Cache: Redis
- API Documentation: Swagger / OpenAPI
- CI/CD: GitHub Actions
- Repository Structure: Monorepo
- Load Balancing & Rate Limiting: Nginx
- Containerization: Docker
- Orchestration: Kubernetes

## Risks and Mitigation

- Service failure: Use retries and request timeouts; implement health checks and circuit breakers.
- Message loss: Use Kafka acknowledgments and durable logging.
- Team coordination: Use clear role assignments, shared conventions, and frequent commits.
