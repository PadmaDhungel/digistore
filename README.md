# Resilient Commerce Platform

A full-stack e-commerce system built to explore real-world backend challenges, especially **safe inventory handling under high concurrency** using a reservation-based approach.

---

## Tech Stack

- **Frontend:** Next.js (App Router)  
- **Backend:** NestJS (modular structure)  
- **Database:** PostgreSQL  
- **ORM:** Prisma  
- **Monorepo:** Turborepo  
- **Package Manager:** pnpm  

---

##  Problem Statement

Modern e-commerce systems tend to break down under load, especially when multiple users try to buy the same limited stock at the same time.

Typical issues include:

- Overselling when requests hit at the same time  
- Race conditions leading to incorrect inventory state  
- Cart abandonment leaving stock locked unnecessarily  
- UI showing outdated or inconsistent availability  

This project tries to address those problems using a **reservation-first inventory model**, where stock is not immediately deducted.

---

##  System Goal

The main goal is simple: **inventory should always stay correct, even under concurrent usage**.

To achieve that, the system leans toward:

- Keeping data consistency as the priority  
- Letting the backend fully control inventory state  
- Using database transactions instead of optimistic assumptions  

---

##  Core Features

- Product catalog browsing  
- Real-time stock visibility  
- Reservation-based checkout flow  
- Automatic expiration of reservations  
- Order creation from valid reservations  
- Safe inventory updates under concurrency  

---

##  System Architecture

```text
┌────────────────────┐
│   Next.js (UI)     │
└────────┬───────────┘
         │ HTTP
         ▼
┌────────────────────┐
│   NestJS API       │
│ (Business Logic)   │
└────────┬───────────┘
         │ Transactions
         ▼
┌────────────────────┐
│ PostgreSQL DB      │
│ (Source of Truth)  │
└────────────────────┘
```

### Optional Extensions

- Redis → caching and real-time updates  
- Queue system (BullMQ) → handling reservation expiry jobs  
- WebSockets → live inventory updates on the frontend  

---

##  Core System Flow

### 1. Product Browsing
- User opens product list  
- System returns current available stock  

---

### 2. Reservation Flow
- User starts checkout  
- Backend checks stock availability  
- A reservation is created inside a database transaction  
- Stock is held temporarily (not deducted yet)  
- Reservation gets an expiration timestamp  

---

### 3. Order Completion

If payment succeeds:
- reservation is converted into an order  
- stock is finalized  

If payment fails or times out:
- reservation expires  
- stock is released back into inventory  

---

##  Key Design Principles

- Database is treated as the single source of truth  
- Inventory updates are always atomic  
- Frontend state is never trusted for stock accuracy  
- Concurrency is handled at the database/service layer  
- Every stock change must be deterministic  

---

##  Core Domain Model: Reservation System

Instead of immediately reducing stock:

### Traditional Flow (❌)

- User clicks “Buy”  
- Stock is reduced immediately  
- Failures can leave system inconsistent  

---

### Reservation Flow (✅)

- User starts checkout  
- System creates a reservation  
- Stock is temporarily held  
- Reservation expires if not completed  
- On success, it becomes an order  

---

### Why this works better

- Prevents overselling  
- Handles concurrent requests safely  
- Makes rollback predictable  
- Keeps inventory state consistent  

---

##  Getting Started

### Install dependencies

```bash
pnpm install
```

### Start development

```bash
pnpm dev
```

---

##  Monorepo Structure

```text
apps/
  web        → Next.js frontend
  api        → NestJS backend

packages/
  db         → Prisma schema + database client
```

---

##  Development Phases

### Phase 1 — Foundation (Current)
- Setting up monorepo (Turborepo)  
- Defining system architecture  
- Designing database schema  

---

### Phase 2 — Inventory System
- Implement reservation logic  
- Add transaction-safe stock updates  
- Handle concurrency properly  

---

### Phase 3 — Order System
- Checkout flow  
- Order lifecycle handling  
- Failure recovery cases  

---

### Phase 4 — Real-time System
- WebSocket-based inventory updates  
- UI sync improvements  
- Reservation countdown tracking  

---

### Phase 5 — Production Hardening
- Logging and observability  
- Rate limiting  
- Idempotency handling  
- Deployment setup  

---

##  Engineering Focus

The system is built with a simple priority order:

> Correctness first, then consistency, then scalability, then performance.

---

##  Documentation

More detailed design notes live in:

```
/docs
```

They cover:

- system design  
- database schema  
- API structure  
- concurrency handling  
- edge cases and failures  

---

##  Scope (Phase 1)

Right now, the focus is only on:

- system design  
- database modeling  
- backend architecture setup  
- inventory + reservation logic  

Not included yet:

- payment integration  
- UI polishing  
- deployment optimization  

---

##  What This Project Shows

- Ability to design backend systems  
- Understanding of concurrency issues  
- Practical database transaction usage  
- Monorepo structuring skills  
- Real-world thinking in e-commerce systems  

---

##  License

Educational / portfolio project