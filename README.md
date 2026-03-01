# Financial Control Center
A production-grade financial SaaS engineered to simulate real-world subscription billing, recurring job processing, and reliability patterns.

---

## Overview

Financial Control Center is a full-stack SaaS application designed to model production-level backend architecture within a personal finance domain.

Rather than focusing on feature breadth, this project emphasizes:

- Clean relational data modeling
- Idempotent recurring processing
- Background job orchestration
- Failure recovery strategies
- Observability and auditability
- Horizontal scaling considerations

The system is intentionally designed to resemble real-world SaaS backend infrastructure.

---

## Core Features

### Authentication & Authorization
- JWT-based authentication
- Refresh token rotation
- Role-based access control (RBAC)
- Secure password hashing
- Token expiration handling

### Financial Domain
- Expense tracking
- Budget allocation and enforcement
- Subscription management (recurring billing simulation)
- Financial task tracking tied to subscriptions

### Recurring Processing Engine
- Background job worker
- Scheduled subscription processing
- Automatic expense generation
- Retry with exponential backoff
- Dead-letter queue handling
- Idempotency protection

### Observability
- Structured logging
- Request ID tracing
- Job lifecycle tracking
- Audit logs for financial mutations
- Failed job monitoring dashboard

---

## System Architecture

High-Level Flow:

Client  
  ↓  
API Server  
  ↓  
Relational Database  
  ↓  
Background Worker  
  ↓  
Retry Queue / Dead Letter Queue  

### Components

**API Layer**
- Handles HTTP requests
- Performs validation and authorization
- Manages transactions
- Emits structured logs

**Database Layer**
- Relational schema with normalized entities
- Indexed for performance
- Transaction-safe writes

**Background Worker**
- Polls job table
- Processes recurring subscriptions
- Implements retry + backoff
- Moves failed jobs to dead-letter state

---

## Data Modeling Principles

Entities include:

- Users
- Roles
- Expenses
- Budgets
- Subscriptions
- RecurringSchedules
- FinancialTasks
- Jobs
- AuditLogs

### Indexing Strategy

Indexes are created on:
- Foreign keys
- Frequently queried fields (user_id, next_run_at, status)
- Composite indexes for common filtering patterns

Goals:
- Avoid full table scans
- Maintain predictable query performance
- Optimize recurring job polling

---

## Idempotency Strategy

Recurring billing must never duplicate charges.

This system ensures idempotency via:
- Unique constraints on generated expense references
- Idempotency keys for recurring runs
- Transactional safeguards
- Defensive job state updates

---

## Failure Handling

The system is designed to gracefully handle:

- Worker crashes mid-job
- Database write failures
- Duplicate processing attempts
- Token expiration
- Partial transaction failures

Retry logic:
- Exponential backoff
- Max retry threshold
- Dead-letter escalation

---

## Scaling Considerations

While this project runs on a single-node deployment, it is architected with horizontal scaling in mind.

### API Scaling
- Stateless server design
- Horizontal scaling behind load balancer
- Centralized token verification

### Worker Scaling
- Multiple worker instances supported
- Database row locking to prevent duplicate job execution

### Database Scaling
- Read replica strategy (future)
- Caching layer placement (future)
- Partitioning strategy for large expense tables (future)

---

## Tradeoffs

### Relational Database vs NoSQL
Relational database chosen for:
- Strong consistency guarantees
- Transactional integrity
- Structured financial data modeling

### Polling Worker vs Event Streaming
Polling approach chosen for:
- Simplicity
- Predictable state transitions
- Lower operational complexity

### Synchronous vs Asynchronous Processing
- User-triggered financial writes are synchronous for immediate feedback.
- Recurring subscription processing is asynchronous for resilience.

---

## Security Considerations

- Password hashing with strong algorithm
- Token expiration enforcement
- Role-based permission checks
- Input validation and sanitization
- Error response classification (4xx vs 5xx)

---

## Deployment

- Environment-based configuration
- Production-ready build
- Logging configured per environment
- Database migrations supported

Deployment target: (To be defined)

---

## Future Improvements

- Rate limiting middleware
- Distributed locking improvements
- Caching layer integration
- Metrics dashboard
- Circuit breaker pattern for worker resilience

---

## Engineering Focus

This project prioritizes:

- Correctness over feature quantity
- Predictable performance
- Explicit tradeoffs
- Failure-aware design
- Production-oriented thinking

It is intentionally built to serve as a system design discussion artifact in technical interviews.

---

## Status

🚧 In Active Development

Planned Completion: (Add timeline)

