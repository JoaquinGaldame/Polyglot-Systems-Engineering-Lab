# Polyglot Systems Engineering Lab

A multi-language backend engineering platform focused on real-world business domains, software architecture, design patterns, API design, persistence strategies, and system design experimentation.

---

## Overview

Polyglot Systems Engineering Lab is a structured backend engineering workspace designed to explore architectural patterns, object-oriented design, distributed systems concepts, and implementation trade-offs across multiple programming languages and frameworks.

Rather than isolated toy examples, each service is built around realistic business domains and concrete backend problems such as reservations, billing, scheduling, availability management, and logistics workflows.

The objective is to design systems the way production software is actually built: with clear domain boundaries, explicit architectural decisions, persistence abstractions, testability, and maintainable service design.

---

## Engineering Approach

This project follows a practical design principle:

> Real business problem → architectural decision → implementation pattern → trade-off analysis

Patterns are introduced only when they solve an actual design problem, not as artificial demonstrations.

Core engineering focus includes:

- Object-oriented design
- Clean Architecture / Hexagonal Architecture
- Design patterns (GoF + backend application patterns)
- API design and service boundaries
- Persistence modeling and repository abstractions
- Testability and maintainability
- Infrastructure orchestration with Docker
- Cross-language implementation comparisons
- Distributed systems experimentation (later phases)

---

## Workspace Structure

```txt
polyglot-systems-engineering-lab/
├── apps/
│   ├── parking-api-nest/
│   ├── billing-api-dotnet/
│   ├── hospital-api-python/
│   └── logistics-api-go/
│
├── packages/
│   └── contracts/
│
├── infra/
│   ├── docker-compose.yml
│   └── postgres/
│       └── init/
│           └── 001-create-databases.sql
│
├── docs/
│   ├── PRE_INITIALIZATION.md
│   ├── API_DOMAINS_AND_PATTERNS.md
│   ├── API_INITIALIZATION_AND_DOCKER_STRATEGY.md
│   ├── DEVELOPMENT_CHECKLIST.md
│   └── ROADMAP.md
│
└── README.md
```

---

## Services
Service	Stack	Primary Focus
Parking API	NestJS / TypeScript	Domain modeling, architectural layering, structural patterns
Billing API	.NET / C#	Object-oriented design, classical design patterns, application architecture
Hospital API	Python / FastAPI	Service orchestration, adapters, pragmatic architecture
Logistics API	Go	Concurrency, lightweight services, performance-oriented backend design

---

## Infrastructure

Initial shared infrastructure:
- PostgreSQL
- pgAdmin
- Redis (optional later phase)

Database isolation strategy:
```text
parking_db
billing_db
hospital_db
logistics_db
```
Each service owns its own persistence boundary.

---

## Design Principles
- Business logic must remain independent from frameworks.
- Persistence concerns should be isolated behind abstractions.
- External integrations should be encapsulated through adapters.
- Architectural decisions must be justifiable.
- Patterns should emerge from design constraints, not from memorization.
- Services must be testable and maintainable.
- Trade-offs are documented explicitly.

---

## Project Evolution

The platform is intentionally built in phases.

Early stages focus on isolated service design and local development.

Later stages may introduce:
- service-to-service communication
- asynchronous workflows
- caching layers
- distributed messaging
- resilience patterns
- observability
- deployment experimentation

---

## Technical Scope

This workspace explores:
- TypeScript / NestJS
- .NET / C#
- Python / FastAPI
- Go
- PostgreSQL
- Docker / Docker Compose
- Redis
- REST API architecture
- design patterns
- backend application patterns
- clean architecture concepts
- distributed systems fundamentals