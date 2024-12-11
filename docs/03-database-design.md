# 🗄️ Database Design

## Overview

This database is designed to support a reservation-based inventory system that must remain consistent under high concurrency.

The main goal is to ensure that stock is never oversold, even when multiple users attempt to purchase the same product simultaneously.

---

## Core Entities

### 1. Product

Represents an item available for purchase.

```sql
id            UUID (PK)
name          TEXT
description   TEXT
price         DECIMAL
createdAt     TIMESTAMP
```

---

### 2. Inventory

Tracks available stock for each product.

```sql
id            UUID (PK)
productId     UUID (FK → Product)
quantity      INT
reserved      INT DEFAULT 0
updatedAt     TIMESTAMP
```

### Key Idea:
- `quantity` = total stock
- `reserved` = temporarily held stock

Available stock = `quantity - reserved`

---

### 3. Reservation

Represents a temporary hold on inventory.

```sql
id            UUID (PK)
productId     UUID (FK → Product)
quantity      INT
status        ENUM('ACTIVE', 'EXPIRED', 'CONFIRMED')
expiresAt     TIMESTAMP
createdAt     TIMESTAMP
```

---

### 4. Order

Final confirmed purchase.

```sql
id            UUID (PK)
userId        UUID
status        ENUM('PENDING', 'PAID', 'FAILED')
totalAmount   DECIMAL
createdAt     TIMESTAMP
```

---

### 5. OrderItem

Links orders to products.

```sql
id            UUID (PK)
orderId       UUID (FK → Order)
productId     UUID (FK → Product)
quantity      INT
price         DECIMAL
```

---

## Concurrency Strategy

To prevent overselling:

### 1. Row-level locking

Inventory rows are locked during reservation:

```sql
SELECT * FROM inventory
WHERE productId = ?
FOR UPDATE;
```

---

### 2. Transactional updates

Reservation + inventory update happen in one transaction:

- lock row
- check stock
- update reserved count
- create reservation

---

### 3. Atomic rule

> A reservation is only valid if it successfully updates inventory inside a transaction.

---

## Why this design works

This design prevents:

- overselling stock
- double reservation issues
- race conditions under concurrent requests

Because:

- inventory updates are atomic
- rows are locked during modification
- reservations are time-bound

---

## Design Decisions

### Why separate Inventory from Product?

To isolate mutable stock data from product metadata.

---

### Why use Reservation table?

To introduce a **controlled state before final purchase**, instead of immediate deduction.

---

### Why not reduce stock directly?

Direct reduction causes:

- race conditions
- inconsistent rollback logic
- difficulty handling payment failure

---

## Data Flow Summary

1. User requests reservation  
2. Inventory row is locked  
3. Stock is checked  
4. Reservation is created  
5. Inventory reserved count increases  
6. On success → converted to order  
7. On failure/timeout → released  

---

## Summary

This database design ensures:

- No overselling under concurrency  
- Predictable inventory state  
- Safe reservation lifecycle  
- Transactional correctness  
