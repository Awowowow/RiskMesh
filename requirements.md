# RiskMesh Requirements

## Purpose

RiskMesh is a real-time fraud decisioning platform for financial transactions and high-risk user actions.

The system helps financial platforms decide whether an incoming transaction should be allowed, flagged for review, or blocked before money moves. It must return decisions quickly, explain the evidence behind risky decisions, and improve over time using analyst feedback.

## Functional Requirements

### Transaction Evaluation

- The system must allow external platforms to submit transactions for risk evaluation.
- Each submitted transaction must receive one of three decisions: `allow`, `flag`, or `block`.
- Each transaction must receive a numeric risk score.
- Each transaction decision must include enough context to explain why the decision was made.
- The system must support a fast path for low-risk transactions.
- The system must support deeper checks for suspicious, unusual, or unverified activity.

### Risk Signals

- The system must compute real-time transaction risk features.
- The system must track user-level behavior such as transaction frequency, amount patterns, and historical risk.
- The system must track device-level behavior such as new device usage and shared devices.
- The system must track IP-level behavior such as repeated usage, shared IPs, and unusual access patterns.
- The system must support entity-level risk signals for users, devices, IP addresses, cards, wallets, merchants, and related entities.
- The system must detect suspicious relationships between entities, such as multiple users sharing the same device or IP.

### Rules Engine

- The system must allow fraud rules to be created, updated, enabled, and disabled.
- Rules must be evaluated against transaction features.
- Rules must support actions such as `allow`, `flag`, `block`, and `review`.
- The system must record which rules fired for each transaction.
- Rule changes should take effect without requiring a full service restart.
- The system should support backtesting rules against historical transaction data.

### Decisioning

- The system must combine transaction features, rules, historical behavior, and entity risk into a final decision.
- The system must be able to block clearly high-risk transactions before money moves.
- The system must be able to flag uncertain transactions for manual review.
- The system must minimize friction for low-risk users.
- The system must not automatically ban user accounts as a primary decision. It may produce entity-level risk signals or recommendations, but final account enforcement belongs to the platform using RiskMesh.

### Case Management

- The system must create review cases for flagged or blocked transactions.
- Fraud analysts must be able to view a list of open cases.
- Fraud analysts must be able to inspect transaction details, risk signals, fired rules, and decision explanations.
- Fraud analysts must be able to claim and resolve cases.
- Case resolutions must include outcomes such as `confirmed_fraud`, `false_positive`, and `insufficient_evidence`.
- Analyst case resolutions must be stored as feedback for future evaluation and improvement.

### Explainability

- Every flagged or blocked transaction must include a human-readable explanation.
- Explanations must identify the main risk signals behind the decision.
- Explanations should be useful to fraud analysts during review.
- The system must keep a decision record so that a transaction can be audited later.

### Feedback Loop

- Analyst resolutions must be stored as labels.
- The system must use analyst labels to evaluate future decision quality.
- The system should update risk profiles when cases are resolved.
- Confirmed fraud cases should increase risk for connected suspicious entities.
- False positives should be tracked so rules and scoring logic can be improved.

### Dashboard

- The dashboard must show live transaction decisions.
- The dashboard must show flagged and blocked transactions.
- The dashboard must provide a case queue for analysts.
- The dashboard must show decision explanations and fired rules.
- The dashboard must show entity relationships for suspicious users, devices, IPs, cards, wallets, and merchants.
- The dashboard must show high-level fraud trends and system performance.

### Simulator

- The system should include a transaction simulator for development and demos.
- The simulator should generate normal transactions.
- The simulator should generate known fraud scenarios such as velocity attacks, account takeover, card testing, shared IP abuse, and coordinated fraud rings.
- The simulator should make it possible to test whether RiskMesh detects expected fraud patterns.

## Non-Functional Requirements

### Performance

- Fast-path transaction decisions should return in under 200 ms.
- The system should avoid unnecessary delay for low-risk transactions.
- Suspicious transactions may use deeper checks when needed.
- Dashboard updates should appear shortly after decisions are made.

### Reliability

- The system must handle duplicate transaction submissions safely.
- The system must continue operating if non-critical AI functionality is unavailable.
- If AI-based investigation fails, the system should fall back to rules and deterministic scoring.
- Background jobs must be retryable.
- Webhook delivery, if supported, must be retryable and idempotent.

### Security

- External APIs must require authentication.
- Merchant API keys or secrets must not be stored in plain text.
- Sensitive data must not be logged.
- Raw card numbers, CVV, and sensitive payment credentials must not be stored.
- Access to analyst/admin functionality must be role-based.

### Observability

- The system must produce structured logs.
- The system must expose health checks for services.
- The system should collect metrics for latency, decision counts, errors, and queue health.
- The system should support tracing a transaction across the decision flow.
- Decision records must include enough audit information to reconstruct what happened.

### Scalability

- The system should be designed so transaction ingestion, feature computation, rules evaluation, decisioning, case management, and analytics can scale independently.
- The system should support asynchronous processing where immediate synchronous response is not required.
- The system should be able to process demo load without data loss.

## Data Requirements

- The system must store merchants or client platforms.
- The system must store submitted transactions.
- The system must store computed transaction features.
- The system must store rules and rule evaluation logs.
- The system must store final transaction decisions.
- The system must store decision explanations.
- The system must store fraud cases and analyst resolutions.
- The system must store entity relationships and risk signals.
- The system must store audit events for important transaction lifecycle steps.

## Integration Requirements

- External systems must be able to submit transactions through an authenticated API.
- External systems must be able to retrieve transaction decision status.
- The system should support webhooks for sending completed decisions back to merchants.
- The dashboard must receive live decision updates.
- The system should expose APIs for rules, cases, transactions, analytics, and entity graphs.

## AI Requirements

- AI should assist with investigation and explanation, not replace deterministic decision controls.
- AI-generated outputs must be structured and stored with the decision record.
- AI failures must not stop the core fraud decision flow.
- AI explanations should reference concrete risk signals, not vague guesses.
- The system should support comparing AI recommendations against analyst outcomes.

## Machine Learning Requirements

- The first version may operate without training a custom machine learning model.
- If custom ML is added, the model must be trained from labeled historical transactions.
- Training labels must come from analyst case resolutions or trusted historical fraud outcomes.
- Model evaluation must use fraud-relevant metrics such as precision, recall, F1 score, false positive rate, and false negative rate.
- Model output should be a fraud probability or risk score that can be combined with rules and other risk signals.
- Model predictions must be explainable using feature importance, reason codes, or similar techniques.

## Success Criteria

RiskMesh is successful if it can evaluate transactions in real time, return clear risk decisions, and help analysts investigate suspicious activity with useful evidence.

A successful first production-grade version should meet these criteria:

- The system accepts transaction risk requests through an authenticated API.
- Each transaction receives an `allow`, `flag`, or `block` decision.
- Low-risk transactions are decided through a fast path.
- Suspicious transactions include explanations with the main risk signals.
- The system computes features such as transaction velocity, new device usage, IP reuse, unusual amount, and entity risk.
- The system correctly detects the main simulated fraud scenarios.
- Fraud analysts can view flagged cases, inspect evidence, and resolve cases.
- Analyst resolutions are stored as feedback for future evaluation and improvement.
- The dashboard shows live decisions, fraud trends, top triggered rules, and case status.
- The system records enough audit data to explain why a decision was made later.
- The production deployment can run with health checks, logs, restart behavior, and basic monitoring.

## Out of Scope

- RiskMesh is not a payment processor.
- RiskMesh does not move money, settle payments, or connect directly to banks or card networks.
- RiskMesh does not store raw card numbers, CVV, or sensitive payment credentials.
- RiskMesh does not perform full KYC or document verification in the first version.
- RiskMesh does not automatically ban user accounts as its primary responsibility.
- RiskMesh does not need to train its own machine learning model in the first version, although the architecture should allow one to be added later.
