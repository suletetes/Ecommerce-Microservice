# Ecommerce-Microservice

A full-stack, cloud-native e-commerce platform built with a microservices architecture. This monorepo contains multiple
services (Node.js/TypeScript backends, a Next.js frontend, and shared libraries) orchestrated with Docker and
Kubernetes, using NATS for event-driven communication.

---

## Table of Contents

- [Architecture Overview](#architecture-overview)
- [Services](#services)
- [Development Setup](#development-setup)
- [Build & Deployment](#build--deployment)
- [Folder Structure](#folder-structure)
- [Testing](#testing)
- [Contributing](#contributing)

---

## Architecture Overview

- **Microservices**: Each domain (auth, orders, tickets) is a separate Node.js/TypeScript service.
- **Frontend**: Next.js React app for the user interface.
- **Shared Library**: Common code (errors, events, middlewares) in `common` package.
- **Event Bus**: NATS Streaming for inter-service communication.
- **Database**: Each service uses its own MongoDB instance.
- **Orchestration**: Docker for containerization, Kubernetes for deployment, Skaffold for local dev automation.

---

## Services

- **auth/**: Handles authentication (signup, signin, signout, current user). Uses JWT, Express, and Mongoose.
- **orders/**: Manages order creation, status, and event publishing. Depends on tickets and auth services.
- **tickets/**: Ticket CRUD operations. Publishes events on ticket changes.
- **client/**: Next.js frontend for user interaction. Communicates with backend services via API routes.
- **common/**: Shared TypeScript code (errors, events, middlewares) published as `@rallycoding/common`.
- **nats-test/**: Utilities for testing NATS event publishing and listening.
- **infra/k8s/**: Kubernetes manifests for all services, MongoDB, NATS, and ingress.

---

## Development Setup

### Prerequisites

- [Docker](https://www.docker.com/)
- [Kubernetes (minikube or Docker Desktop)](https://kubernetes.io/)
- [Skaffold](https://skaffold.dev/)
- [Node.js](https://nodejs.org/) (for local dev)

### Steps

1. **Install dependencies**
    - Run `npm install` in each service and the `common` package if developing locally.
2. **Start Kubernetes cluster**
    - Use minikube or Docker Desktop.
3. **Configure environment variables**
    - Each service may require environment variables (e.g., JWT_KEY, MONGO_URI, NATS_URL). See respective service docs
      or deployment manifests.
4. **Run with Skaffold**
    - From the project root, run:
      ```sh
      skaffold dev
      ```
    - Skaffold will build Docker images, deploy to Kubernetes, and watch for changes.

---

## Build & Deployment

- **Docker**: Each service has its own Dockerfile.
- **Kubernetes**: Manifests in `infra/k8s/` deploy all services, databases, NATS, and ingress.
- **Skaffold**: Automates build and deployment for local development.

---

## Folder Structure

```
Ecommerce-Microservice/
├── auth/         # Auth service (Node.js/TypeScript)
├── client/       # Next.js frontend
├── common/       # Shared TypeScript code
├── infra/
│   └── k8s/      # Kubernetes manifests
├── nats-test/    # NATS event test utilities
├── orders/       # Orders service (Node.js/TypeScript)
├── tickets/      # Tickets service (Node.js/TypeScript)
├── skaffold.yaml # Skaffold config
└── README.md     # This file
```

---

## Testing

- Each backend service uses Jest for unit/integration tests.
- Run tests in a service directory with:
  ```sh
  npm test
  ```
- The `client` app can be tested with Next.js/React testing tools.

---

## Contributing

1. Fork and clone the repo.
2. Create a new branch for your feature/fix.
3. Make changes and add tests as needed.
4. Run tests locally before submitting a PR.
5. Open a pull request with a clear description.

---

## Credits

Inspired by microservices best practices and event-driven architectures.
