# Food Delivery Management System

## Project Description

A microservices-based platform for ordering food from restaurants with clear, real-time status updates. Restaurants manage menus, customers place orders, and delivery workers fulfill them. Services remain independent and scalable, using REST for synchronous calls and Kafka for asynchronous events like order status changes and notifications.

Core capabilities:
- Manage restaurant menus
- Create and update orders with status steps
- Assign delivery workers and track progress
- Send notifications to customers and workers

## Tech Stack

- Node.js (services, e.g., Express)
- PostgreSQL (data storage)
- Apache Kafka (event streaming & notifications)
- Docker & Docker Compose (local development & orchestration)
