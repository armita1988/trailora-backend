# Trailora Backend

**Trailora** is a production travel-booking platform that I independently designed, built, and deployed end-to-end. This repository contains the Node.js/Express REST API and backend services.

**Live:** https://trailora.armita.dev  
**Frontend:** https://github.com/armita1988/trailora-frontend

## Highlights

- Designed REST APIs and MongoDB/Mongoose workflows for tours, users, reviews, and bookings
- Implemented JWT/cookie authentication, protected routes, role-based authorization, and password recovery
- Integrated Stripe Checkout with verified webhooks and duplicate-payment/session protection
- Built reusable filtering, sorting, field selection, and pagination utilities
- Added centralized error handling and API rate limiting
- Built an image pipeline with Multer and Sharp and stored processed images in Amazon S3
- Containerized the backend with Docker and Docker Compose
- Automated release delivery with GitHub Actions, Docker Hub, Amazon EC2, health checks, and commit-SHA image tags

## Tech Stack

**Core:** Node.js 24, Express.js, MongoDB Atlas, Mongoose  
**Security & Payments:** JWT, Cookies, bcryptjs, Role-Based Authorization, Stripe Webhooks  
**Media & Storage:** Multer, Sharp, Amazon S3  
**DevOps:** Docker, Docker Compose, Docker Hub, GitHub Actions, Amazon EC2, Nginx

## Architecture

```text
Client
  ↓
Express Routes
  ↓
Middleware / Authentication / Authorization
  ↓
Controllers / Business Logic
  ↓
Mongoose Models
  ↓
MongoDB Atlas
```

The API uses separated route, controller, middleware, model, and utility layers to keep business logic and infrastructure concerns modular.

## Key Production Workflows

**Booking & payment**

```text
Authenticated user
→ Stripe Checkout Session
→ Stripe-hosted payment
→ Verified webhook
→ Booking stored in MongoDB
```

Booking fulfillment is webhook-driven rather than relying only on the browser success redirect.

**Image processing**

```text
Upload → Multer → Sharp resize/conversion → Amazon S3
```

## Production CI/CD

Pushes to `main` run a GitHub Actions pipeline that:

```text
Install + Lint
→ Build Docker image
→ Push latest + commit-SHA tags to Docker Hub
→ Deploy the exact SHA release to EC2
→ Verify container health and surface logs on failure
```

Commit-SHA image tags make deployed releases traceable to an exact source revision.

## Run Locally

```bash
git clone https://github.com/armita1988/trailora-backend.git
cd trailora-backend
npm install
npm run dev
```

Create the required local environment configuration before starting the API. Secrets and production credentials should never be committed.
