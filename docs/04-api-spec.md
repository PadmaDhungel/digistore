# API Specification

## GET /products
Returns list of products

## POST /reservation
Creates a reservation

Request:
{
  productId: string,
  quantity: number
}

Response:
{
  reservationId: string,
  expiresAt: string
}

## POST /order
Converts reservation into order