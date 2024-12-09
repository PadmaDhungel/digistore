# Overview

This project is a full-stack e-commerce platform designed to explore safe inventory handling under high concurrency.

The main focus is solving real-world issues such as overselling and inconsistent stock updates using a reservation-based system.

## Key Idea

Instead of directly reducing stock on purchase, the system reserves inventory first and only finalizes it upon successful checkout.

## Stack

- Next.js (Frontend)
- NestJS (Backend)
- PostgreSQL (Database)
- Prisma (ORM)

## High-Level Architecture

Client → API → Database