# System Design

## Architecture

Client → Next.js → NestJS API → PostgreSQL

## Core Services

- Product Service
- Inventory Service
- Reservation Service
- Order Service

## Request Flow

1. User requests product
2. API checks inventory
3. Reservation is created
4. Stock is held
5. Order is created after confirmation