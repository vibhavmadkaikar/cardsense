# CardSense – Database ERD & Schema Design

---

## 1. Design Goals

The CardSense database is designed with the following goals:

- Strong relational integrity
- Clear ownership of data
- Support for analytics and recommendations
- Flexibility for evolving reward rules
- Security-first handling of sensitive data
- Easy future migration to scale (read replicas, partitioning)

PostgreSQL is used as the primary database due to its reliability, ACID guarantees, and JSONB support.

---

## 2. Core Entities Overview

At a high level, the system revolves around **Users**, **Cards**, and **Transactions**.

User → Cards → Transactions → Reminders → Payments

Everything is ultimately scoped to a **user**, ensuring data isolation and security.

---

## 3. Entity Relationship Diagram (Conceptual)

Users
├── UserPreferences (1:1)
├── Cards (1:N)
│ ├── Transactions (1:N)
│ ├── Reminders (1:N)
│ ├── PaymentHistory (1:N)
│ └── UserCardBenefits (1:N)
├── Categories (1:N, custom)
├── Sessions (1:N)
├── OTPVerifications (1:N)
├── NotificationLogs (1:N)
└── AuditLogs (1:N)

`CardBenefits` exists as a **shared reference table** and is not owned by users.

---

## 4. Table-by-Table Explanation

### 4.1 Users

**Purpose:**  
Stores authentication and core identity information.

**Key Decisions:**

- UUID primary key for security and scalability
- Email and phone stored separately with verification flags
- No plaintext passwords (bcrypt hash only)

This table is the **root of ownership** for almost all other data.

---

### 4.2 User Preferences

**Purpose:**  
Stores user-specific application settings.

**Why separate from users?**

- Keeps the `users` table lean
- Preferences change more frequently
- Easy to extend without touching auth logic

This enforces a clean **1:1 relationship**.

---

### 4.3 Cards

**Purpose:**  
Represents both credit and debit cards owned by a user.

**Key Fields:**

- `billing_cycle_day` and `payment_due_day` support reminders
- `credit_limit` and `current_balance` enable utilization tracking
- Only **last four digits** are stored (encrypted if needed)

**Important:**  
Full card numbers are **never stored**, aligning with security best practices.

---

### 4.4 Categories

**Purpose:**  
Classifies transactions for analytics and recommendations.

**Design Highlights:**

- Supports fixed (system) and custom (user) categories
- Self-referencing parent-child structure
- Enables hierarchical analytics (e.g., Food → Dining)

This makes reporting and future ML classification easier.

---

### 4.5 Transactions

**Purpose:**  
Stores all spending activity.

**Key Design Choices:**

- Linked to both `user` and `card`
- Category is optional to allow uncategorized imports
- SMS metadata stored only for traceability
- Designed to be append-heavy and query-efficient

Indexes are added on:

- user_id
- card_id
- transaction_date

---

### 4.6 Card Benefits (Reference Data)

**Purpose:**  
Pre-populated knowledge base of card benefits.

**Why JSONB fields?**

- Reward structures vary widely
- Milestone and category-based rewards are dynamic
- Avoids schema churn for every new benefit rule

This table enables **data-driven reward logic**, not hardcoded rules.

---

### 4.7 User Card Benefits

**Purpose:**  
Allows users to override or add custom benefits for their cards.

**Why this table exists:**

- Real-world card benefits may differ per user
- Promotions, retention offers, or custom waivers

This keeps global benefits clean while allowing personalization.

---

### 4.8 Reminders

**Purpose:**  
Tracks billing and payment reminders.

**Design Notes:**

- Reminder date is calculated from due date
- Flags track sent and paid states
- Supports future automation and retries

This table powers notification workflows.

---

### 4.9 Payment History

**Purpose:**  
Stores historical bill payments.

**Why separate from reminders?**

- One reminder can lead to multiple payments
- Supports partial payments
- Clean audit trail for financial history

---

### 4.10 Sessions

**Purpose:**  
Manages refresh tokens and device sessions.

**Security Design:**

- Tokens stored hashed
- Supports logout from specific devices
- Enables session expiry and revocation

---

### 4.11 OTP Verifications

**Purpose:**  
Handles email/phone verification and password resets.

**Key Features:**

- Time-bound (TTL-based)
- Short-lived, disposable records
- Can be moved to Redis in future for performance

---

### 4.12 Notification Logs

**Purpose:**  
Audits all outgoing notifications.

**Why important:**

- Debug failed reminders
- Compliance and traceability
- Analytics on notification effectiveness

---

### 4.13 Audit Logs

**Purpose:**  
Tracks sensitive actions across the system.

**Tracked Events:**

- Login attempts
- Card creation/deletion
- Transaction modifications

Audit logs are append-only and immutable.

---

## 5. Analytics & Performance Strategy

### Spending Analytics Table

Instead of running heavy aggregation queries repeatedly:

- Monthly spending is pre-computed
- Optimized for dashboards
- Can be refreshed via scheduled jobs

This pattern improves performance and scalability.

---

## 6. Indexing Strategy

Indexes are created on:

- Foreign keys
- Date-based filters
- High-frequency query fields

This balances:

- Fast reads
- Acceptable write performance

---

## 7. Security Considerations

- UUIDs prevent enumeration attacks
- Sensitive fields encrypted at rest
- No financial credentials stored
- Clear separation of auth, business, and audit data

---

## 8. Scalability & Future Readiness

The schema supports:

- Read replicas for analytics
- Partitioning transactions by user or month
- Extracting analytics into a separate service later
- Migrating benefits logic to AI/ML engines

---

## 9. Summary

The CardSense database is:

- Normalized where correctness matters
- Flexible where business rules evolve
- Secure by default
- Optimized for real-world fintech use cases

It intentionally balances **clarity, performance, and extensibility**.
