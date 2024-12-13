# Failure Scenarios

## Payment Failure

Reservation is released and stock is restored.

## Reservation Expiry

If user does not complete checkout, reservation is automatically cleared.

## Concurrent Requests

Handled using database transactions to avoid overselling.

## System Crash

Database remains source of truth; incomplete transactions are rolled back.