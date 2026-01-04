# CardSense – Product & Engineering Roadmap

This roadmap outlines how CardSense evolves from an MVP into a scalable, intelligent card decision platform.

The guiding principle:
**Build value early, add intelligence progressively, avoid over-engineering.**

---

## 1. Roadmap Philosophy

CardSense follows a **phased delivery model**:

- MVP focuses on **clarity, correctness, and usefulness**
- Phase 2 adds **automation and intelligence**
- AI is introduced **only when enough structured data exists**

Each phase is independently valuable and deployable.

---

## 2. Phase 0 – Foundation (Week 0–1)

### Objective

Prepare the project for clean development and collaboration.

### Deliverables

- Public GitHub repository
- Folder structure (backend, frontend, docs)
- Product vision document
- Architecture overview
- Database ERD & schema
- API contracts

### Outcome

A clear blueprint that can be reviewed, discussed, and extended.

---

## 3. Phase 1 – MVP (Core Product) ⭐

### Timeline

**4–6 weeks**

### Product Goals

- Solve the real problem: managing multiple cards
- Provide reliable, rule-based recommendations
- Keep everything transparent and manual-first

---

### 3.1 MVP Features (Must-Have)

#### Authentication & User Management

- Email/password login
- OTP-based login (phone)
- JWT-based sessions
- User preferences (language, reminders)

#### Card Management

- Add credit & debit cards
- Store billing cycle and due dates
- Card activation/deactivation
- Credit utilization calculation

#### Transactions

- Manual transaction entry
- Category assignment
- Filters (date, card, category)
- Monthly spending summaries

#### Categories

- Predefined categories
- User-defined custom categories
- Parent–child hierarchy

#### Card Benefits

- Pre-populated card benefits database
- User-added custom benefits
- Read-only shared benefits

#### Recommendation Engine (Rule-Based)

- “Which card should I use?”
- Category + amount based
- Powered by reward rules (data-driven)
- Fully explainable output

#### Reminders & Notifications

- Payment due reminders
- Mark bill as paid
- Notification history

---

### 3.2 MVP Engineering Scope

#### Backend

- NestJS modular monolith
- PostgreSQL as primary database
- Redis (optional, minimal use)
- REST APIs (v1)
- Swagger documentation

#### Frontend

- Flutter (Android + iOS)
- Clean, minimal UI
- Manual-first workflows

#### DevOps

- Local Docker setup
- Separate env configs (dev/staging/prod)
- Basic logging

---

### MVP Exit Criteria

- User can manage multiple cards reliably
- Recommendation engine works end-to-end
- No critical feature relies on AI or automation

---

## 4. Phase 2 – Smart Automation (Post-MVP)

### Timeline

**After MVP stability**

### Product Goals

- Reduce manual effort
- Increase insight density
- Add intelligence without losing control

---

### 4.1 Phase 2 Features

#### SMS-Based Transaction Parsing

- Read bank SMS (Android)
- Regex-based extraction (amount, merchant, card)
- User confirmation before saving

#### Enhanced Analytics

- Category-wise trends
- Month-over-month comparisons
- Credit utilization alerts

#### Benefits Optimization

- Detect unused benefits
- Highlight expiring milestones
- Spend-to-unlock suggestions

#### Performance Improvements

- Redis caching
- Pre-computed analytics
- Faster dashboards

---

### 4.2 Engineering Enhancements

- Background jobs (queues)
- Better indexing & query tuning
- Improved notification delivery
- Admin panel (internal) for benefits management

---

## 5. Phase 3 – AI-Assisted Intelligence 🤖

### Key Principle

**AI enhances decisions — it does not replace rules blindly.**

AI is introduced only after:

- Sufficient transaction history
- Clean reward rules
- Stable user behavior patterns

---

### 5.1 AI Use Cases

#### Smart Categorization

- Predict transaction categories
- Learn from user corrections

#### Intelligent Recommendations

- Combine:
  - Reward rules
  - User spending habits
  - Past card usage
- Rank cards more contextually

#### Personalized Insights

- “You could have earned ₹X more last month”
- “Switch card for dining to improve rewards”

---

### 5.2 AI Architecture (Future)

- Python-based ML service (separate)
- Offline model training
- Online inference via API
- Explainability retained (why a card was chosen)

---

## 6. What We Build First vs Later

### Build First (Now)

- Core card & transaction management
- Rule-based recommendation engine
- Clear, correct data model
- Simple UI

### Build Later (Intentionally)

- AI models
- Auto-import from banks
- Payment execution
- Financial product integrations

This avoids premature complexity and regulatory risk.

---

## 7. Success Metrics by Phase

### MVP

- Users add and actively track multiple cards
- Recommendation feature usage
- Zero missed reminders (self-use test)

### Phase 2

- Reduction in manual entry
- Increased engagement
- Faster decision-making

### Phase 3

- Accuracy of AI recommendations
- Trust and explainability
- User retention

---

## 8. Summary

CardSense evolves in **layers**:

1. **Reliable data + rules**
2. **Automation and insights**
3. **AI-assisted intelligence**

Each phase delivers value on its own.

This roadmap ensures:

- Fast MVP delivery
- Strong architectural foundation
- Credible AI story without hype
