## Problem

Financial platforms need to decide whether a transaction is legitimate, suspicious, or fraudulent before any money moves.

RiskMesh must return this decision quickly because every millisecond matters during payment authorization. Legitimate users should not experience unnecessary delays, while suspicious or unverified users may require deeper checks before approval.

The system must also explain every decision so analysts can understand why a transaction was allowed, flagged, or blocked. Over time, analyst feedback should improve future risk decisions and reduce both missed fraud and false positives.

## Who Uses It?

RiskMesh is built for companies that process financial transactions or high-risk user actions, such as payment platforms, crypto exchanges, fintech apps, banks, wallets, and online marketplaces.

There are three main user groups:

1. Developers integrate the RiskMesh API into their transaction flow.
2. Fraud analysts review flagged cases, inspect evidence, and resolve investigations.
3. Risk/admin teams manage rules, monitor fraud trends, and evaluate system performance.

## What RiskMesh Decides

RiskMesh decides whether an incoming transaction should be allowed, flagged for review, or blocked before money moves.

For each transaction, RiskMesh returns a risk score, a decision, and an explanation of the main signals behind that decision.

RiskMesh can also mark related entities, such as users, devices, IP addresses, cards, wallets, or merchants, as suspicious when their behavior or connections indicate elevated risk. These risk signals help analysts investigate repeat fraud, account takeover, shared infrastructure and coordinated fraud rings.

## Core Use Cases

- Submit a transaction for risk decision
- Get allow / flag / block result
- Review flagged transactions
- Create and manage fraud rules
- Investigate connected users/devices/IPs
- Resolve cases and feed labels back into the system

## Non-Goals

RiskMesh is not a payment processor. It does not move money, settle payments, or connect directly to banks/card networks.

RiskMesh does not store raw card numbers, CVV, or sensitive payment credentials.

RiskMesh does not perform KYC/identity verification in v1. It can consume identity signals from other systems, but it does not verify documents itself.

RiskMesh does not train its own machine learning model in v1. It uses rules, feature engineering, embeddings, and LLM-assisted investigation.

RiskMesh does not automatically ban user accounts. It provides transaction decisions and entity risk signals; final account-level enforcement remains with the platform using RiskMesh.


## Success Criteria

RiskMesh is successful if it can evaluate transactions in real time, return clear risk decisions, and help analysts investigate suspicious activity with useful evidence.

A successful v1 should meet the following criteria:

- The system accepts transaction risk requests through an authenticated API.
- Each transaction receives an `allow`, `flag`, or `block` decision.
- Low-risk transactions are decided through a fast path without unnecessary delay.
- Suspicious transactions include an explanation with the main risk signals.
- The system computes real-time features such as transaction velocity, new device usage, IP reuse, unusual amount, and entity risk.
- Fraud analysts can view flagged cases, inspect evidence, and resolve cases.
- Analyst resolutions are stored as feedback for future evaluation and model/rule improvement.
- The dashboard shows live decisions, fraud trends, top triggered rules, and case status.
- The system records enough audit data to explain why a decision was made later.
- The production deployment can run on AWS ECS with logs, health checks, and restart behavior.


## Measurable Targets

- Return fast-path decisions in under 200 ms.
- Process at least 100 transactions per minute in the demo environment.
- Generate an explanation for every flagged or blocked transaction.
- Store a complete decision record for 100% of evaluated transactions.
- Create a case for 100% of flagged or blocked transactions.
- Keep API error rate below 1% during demo load tests.
- Show live dashboard updates within 2 seconds of a decision.
- Correctly detects the main simulated fraud scenarios: velocity attack, account takeover, card testing, shared IP abuse, and coordinated fraud ring.

## Comparable Products

RiskMesh is conceptually similar to fraud and risk platforms used by payment companies, fintech apps, marketplaces, and financial institutions.

Comparable products include:

- Stripe Radar: payment fraud detection for Stripe merchants
- Sift: fraud and risk decisioning for digital businesses
- Sardine: fraud, compliance, and risk infrastructure for fintech and crypto
- Feedzai: AI fraud detection for banks and financial institutions
- Featurespace: real-time fraud and financial crime detection
- Forter: fraud prevention for ecommerce transactions
- Riskified: ecommerce fraud decisioning

RiskMesh is not intended to copy these products feature-for-feature. The goal is to build a production-grade fraud detection architecture inspired by the same category of systems, with emphasis on real-time decisions, explainability, entity risk, analyst workflow, and feedback loops.