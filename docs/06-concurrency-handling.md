# Concurrency Handling

## Problem

Multiple users can try to purchase the last item at the same time.

## Solution

- Use database transactions
- Lock inventory rows during reservation
- Ensure atomic updates

## Strategy

- SELECT ... FOR UPDATE
- Transaction wrapping in service layer
- Reject if stock is already reserved