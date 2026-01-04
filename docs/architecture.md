# CardSense – System Architecture

---

## 1. Architecture Overview

CardSense is designed using a **Modular Monolith architecture** with clearly defined boundaries between domains.

The system prioritizes:

- Fast MVP development
- Clear separation of concerns
- Ease of debugging and testing
- A smooth path to future scalability

Each module is designed so it can be extracted into a microservice if and when scale requires it.

---

## 2. Architectural Philosophy

### Why Modular Monolith?

CardSense intentionally starts as a modular monolith instead of microservices.

**Reasons:**

- Single codebase is easier to reason about
- Lower infrastructure complexity
- Faster iteration and development speed
- No premature operational overhead

This approach mirrors how many real-world fintech products begin before scaling.

---

## 3. High-Level Architecture

┌──────────────────────────────────────────────┐
│ Client Layer │
├──────────────────────────────────────────────┤
│ Flutter Mobile (Android / iOS) │
│ Flutter Web │
│ React Web (Admin / Fallback) │
└───────────────────────┬──────────────────────┘
│ HTTPS (TLS)
▼
┌──────────────────────────────────────────────┐
│ Backend (NestJS) │
│ Modular Monolith │
│ │
│ - Auth Module │
│ - Users Module │
│ - Cards Module │
│ - Transactions Module │
│ - Categories Module │
│ - Rewards & Benefits Module │
│ - Recommendation Engine │
│ - Notifications & Reminders │
│ │
└───────────────────────┬──────────────────────┘
│
▼
┌──────────────────────────────────────────────┐
│ Data Layer │
│ PostgreSQL (Primary DB) │
│ Redis (Cache, Sessions, Queues) │
└──────────────────────────────────────────────┘

---

## 4. Client Architecture

### Mobile & Web (Flutter)

The Flutter app follows a **feature-based architecture**.

lib/
├── core/
│ ├── network/
│ ├── storage/
│ ├── theme/
│ └── utils/
├── features/
│ ├── auth/
│ ├── dashboard/
│ ├── cards/
│ ├── transactions/
│ ├── recommendations/
│ ├── benefits/
│ └── settings/
├── shared/
│ ├── widgets/
│ ├── models/
│ └── constants/
└── main.dart

**State Management:** Riverpod  
**Local Storage:** Hive / SQLite  
**Security:** Secure storage for tokens (Keychain / Keystore)

---

## 5. Backend Architecture (NestJS)

### Module-Based Design

Each domain is isolated into its own module.

src/
├── modules/
│ ├── auth/
│ ├── users/
│ ├── cards/
│ ├── transactions/
│ ├── categories/
│ ├── rewards/
│ ├── recommendations/
│ └── notifications/
├── common/
│ ├── guards/
│ ├── interceptors/
│ ├── filters/
│ └── decorators/
├── config/
├── database/
└── main.ts

Each module contains:

- Controller (API layer)
- Service (business logic)
- DTOs (validation)
- Entities (ORM models)

---

## 6. Data Architecture

### Primary Database – PostgreSQL

PostgreSQL is chosen for:

- ACID compliance
- Strong relational integrity
- Support for JSONB (flexible benefits data)
- Excellent performance for analytics queries

### Cache & Queue – Redis

Redis is used for:

- Session management
- OTP storage (TTL-based)
- Rate limiting
- Notification queues
- Frequently accessed lookup data

---

## 7. Reward & Recommendation Design (Key Design Decision)

### Data-Driven Reward Rules

CardSense uses a **data-driven reward engine**.

Instead of hardcoding reward logic:

- Reward rules are stored in tables
- Recommendations are computed dynamically
- New cards or benefits require **no code change**

This design allows:

- Easy updates
- Transparent calculations
- Seamless transition to AI-based recommendations later

---

## 8. Security Architecture (High-Level)

- HTTPS enforced across all communication
- JWT-based authentication (access + refresh tokens)
- Password hashing using bcrypt
- Sensitive fields encrypted at rest
- Role-based access control (future-ready)
- No storage of full card numbers

Security is treated as a **first-class concern**, not an afterthought.

---

## 9. Deployment Strategy (Planned)

### MVP Deployment

- Single backend container
- Single PostgreSQL instance
- Redis for cache & sessions

### Production-Ready Path

- AWS ECS with auto-scaling
- RDS PostgreSQL (Multi-AZ)
- ElastiCache (Redis)
- CloudFront for static assets
- Scheduled jobs via AWS Lambda

---

## 10. Scalability Considerations

- Stateless backend services
- Horizontal scaling at the application layer
- Database indexing & query optimization
- Read replicas for analytics-heavy workloads
- Modular design allows service extraction if needed

---

## 11. Trade-offs & Conscious Decisions

### What We Optimized For

- Developer productivity
- Code clarity
- Interview explainability
- Realistic evolution path

### What We Avoided (Initially)

- Microservices complexity
- Event-driven overengineering
- Premature infrastructure scaling
- Distributed transactions

---

## 12. Summary

CardSense’s architecture balances:

- Simplicity for MVP
- Robustness for production
- Flexibility for future growth

It is intentionally designed to evolve — not over-engineered, not under-prepared.
