# Inventory Reservation System

## Concept

Inventory is not immediately deducted. Instead, it is reserved temporarily.

## Flow

1. Check stock availability
2. Create reservation
3. Hold stock
4. Wait for checkout completion
5. Convert reservation into order OR release stock

## States

- AVAILABLE
- RESERVED
- SOLD