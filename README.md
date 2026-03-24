<div align="center">
  <img src="https://i.ibb.co/y223YKs/Chat-GPT-Image-Mar-24-2026-04-53-10-PM.png" width="100%" alt="Ayush Kumar Banner">
</div>

# Hi there, I'm Ayush Kumar 👋

**Software Engineer | Distributed Systems | Real-Time Architecture**

Building real-time, scalable systems that power collaborative human-AI interaction through distributed state management and event-driven architectures.

---

## System Design Philosophy

I approach software as **distributed systems first**. My thinking centers on:

- **Latency vs. Consistency Tradeoffs** – Understanding when to sacrifice immediate consistency for responsiveness, and vice versa. Real-time systems require explicit tradeoff decisions.
- **Event-Driven Thinking** – Every system is a flow of immutable events processed through independent, scalable services rather than coupled monolithic operations.
- **Distributed State Synchronization** – The core challenge: maintaining eventual consistency across multiple clients/services without creating bottlenecks or race conditions.
- **Scalability at Scale** – Designing for multi-user concurrency from the ground up—not as an afterthought. This shapes architectural choices from day one.
- **Low-Latency Requirements** – Real-time applications demand sub-100ms response times. Architecture must support this through careful service layering and data locality.

This mindset informs every design decision: How do components communicate? What happens during network partitions? Where are the single points of failure?

---

## Core Systems

### System 1: Real-Time AI Arbitration Engine
**The Socratic Arena** | [github.com/Ayush-Kumar0207/The_Socratic_Arena](https://github.com/Ayush-Kumar0207/The_Socratic_Arena)

**Problem:** Moderate real-time competitive debates with AI-driven scoring while maintaining consistent rankings across concurrent matches.

**Architecture:**  
Client (React) ↓ WebSocket ↓ Message Queue / Broker ↓ Score Pipeline (AI Evaluation) ↓ Leaderboard State Machine ↓ Database (Event Log + Current State)

**Key Challenges:**
- **AI Latency Boundary:** Gemini API calls (500-2000ms) cannot block real-time message delivery. Solution: Async scoring pipeline with eventual consistency for leaderboard updates.
- **Concurrent Match State:** Multiple debates running simultaneously. Centralized state machine prevents race conditions in Elo calculations.
- **Debate Ordering Guarantee:** Messages must be processed in order within a debate, but debates themselves are independent. Partitioned event log per debate.

**Tech Stack:** Node.js, Socket.IO, Supabase (PostgreSQL), Gemini API, React, TailwindCSS

---

### System 2: Collaborative Code Execution Platform
**CodeVerse** | [github.com/Ayush-Kumar0207/codeverse](https://github.com/Ayush-Kumar0207/codeverse)

**Problem:** Enable real-time collaborative coding with live preview and multi-language execution without state divergence between clients.

**Architecture:**  
Client A (Editor State) ←→ WebSocket ←→ Client B (Editor State) ↓ Operational Transform / CRDT Layer ↓ Canonical Document State ↓ Code Execution Service (isolated runtime) Output → Broadcast to all clients

**Key Challenges:**
- **Concurrent Edits:** Three clients editing simultaneously. Without CRDT/OT, edits conflict. Solution: Implement operation-based merging to ensure convergence.
- **Execution Isolation:** Running untrusted code (Python, C++, Java) safely. Each execution is containerized with resource limits and timeouts.
- **Latency Hiding:** Execution takes 200-500ms. UI remains responsive through optimistic rendering while awaiting server-side results.
- **Multi-Language Support:** Backend abstraction over language runtimes. Each language has separate execution handler that returns standardized output.

**Tech Stack:** Next.js, Node.js, Socket.IO, TypeScript, Docker (execution), Express, Vercel

---

### System 3: Algorithm Visualization & Tracking System
**AlgoVista** | [github.com/Ayush-Kumar0207/algovista](https://github.com/Ayush-Kumar0207/algovista)

**Problem:** Visualize algorithm execution state in real-time while maintaining step-wise consistency and enabling progress tracking across sessions.

**Architecture:**  
Algorithm Executor (step-by-step iterator) ↓ (yields state at each step) State Manager (immutable snapshots) ↓ Visualization Renderer (React components) ↓ (also persists to DB) Streak & Progress Tracker

**Key Challenges:**
- **Execution State Complexity:** Algorithms involve multiple data structures changing simultaneously. Each step is an atomic state transition. State must be serializable for session resumption.
- **Rendering Efficiency:** Redraw visualization on every step (can be 100+ steps). Solution: Memoization + incremental DOM updates. Only changed elements re-render.
- **Progress Consistency:** User pauses, closes browser, returns later. State snapshot on database allows resumption at exact step. No desync between visual state and persisted state.
- **Streak Gamification:** Track consecutive days without breaking state consistency. Distributed timestamp validation prevents clock-skew exploits.

**Tech Stack:** TypeScript, React, Algorithm visualization library, Supabase, Streak tracking service

---

## System Design Capabilities

| Capability                        | Application |
|-----------------------------------|-------------|
| **Distributed Systems Design**    | Multi-service architectures with async communication patterns |
| **Event-Driven Architectures**    | Decoupled systems communicating through immutable events and message brokers |
| **Real-Time Synchronization**     | WebSocket-based state propagation with consistency guarantees |
| **Consistency Models**            | Strong, Eventual, Causal, and Weak consistency tradeoff analysis |
| **Scalable Backend Design**       | Horizontal scaling through partitioning, caching, and load balancing |
| **AI Integration Pipelines**      | Non-blocking LLM calls with fallback and degradation strategies |
| **Concurrency & State Management**| Race condition prevention, distributed locks, atomic operations |
| **Low-Latency System Design**     | Optimizing for p50, p95, p99 latency SLOs |

---

## Technology Stack (System Layers)

**Computation Layer**  
TypeScript · JavaScript · Python · C++ · Java

**Interface Layer**  
React · Next.js · TailwindCSS

**Service Layer**  
Node.js · Express · Socket.IO

**Real-Time Synchronization Layer**  
WebSocket (Socket.IO) · Event-driven message passing

**Data Layer**  
PostgreSQL (Supabase) · Event sourcing for auditability

**Execution Layer**  
Docker · Containerized runtimes with resource limits

**Intelligence Layer**  
Gemini API · Async AI evaluation pipelines

**Infrastructure Layer**  
Vercel · Cloud deployment · GitHub OAuth

---

## System Design Lens

**What I Optimize For:**

1. **Latency Budget Allocation** – Every millisecond counts in real-time systems. I design with explicit latency budgets per service. When Gemini API hits the budget, the system has a fallback strategy rather than blocking.
2. **Consistency Under Concurrency** – Multi-user systems are inherently chaotic. I use event sourcing, CRDT algorithms, or distributed locks depending on consistency requirements and failure modes.
3. **Scalability Through Partitioning** – Vertical scaling hits limits fast. Instead, I partition horizontally: debates partitioned by debate ID, users by region, code execution by language runtime.
4. **Failure Mode Design** – Systems fail. I design for graceful degradation: cache misses don't crash the system, AI timeouts don't block live editing, network partitions preserve data through event logs.
5. **Observability First** – Distributed systems are opaque. Every component emits structured logs, metrics, and traces. Debugging production requires comprehensive observability.
6. **State Machine Discipline** – Complex systems need explicit state machines. Transitions are validated, race conditions are eliminated through centralized orchestration.

---

## What I'm Solving For

- Sub-100ms round-trip latency in collaborative editing with concurrent multi-user edits
- Consistency of AI-generated rankings across distributed evaluation pipeline without blocking realtime updates
- State synchronization when clients reconnect after network failures (no data loss)
- Horizontal scalability to support thousands of simultaneous concurrent editors/debaters
- Safe execution of arbitrary user code without resource exhaustion or security vulnerabilities
- Recovery guarantees through immutable event logs and incremental state snapshots
- Race condition elimination in financial state (Elo rankings, scores) under high concurrency

---

## Ongoing Research & Optimization

- Operational Transform vs. CRDT tradeoffs for documents >100MB with 100+ concurrent editors
- Sub-50ms AI inference latency for real-time scoring without accuracy degradation
- Distributed consensus algorithms for multi-region state consistency
- Observability & tracing patterns for diagnosing latency tail in distributed systems
- Event sourcing retention policies and snapshot compression for long-running systems

---

## Architecture Decision Log

Recent optimization decisions:

- **WebSocket over HTTP polling** – Latency budget required <100ms, polling adds 500-1000ms overhead. WebSocket reduced to 50-80ms p95.
- **Async AI scoring** – Blocking on Gemini API blocked UI. Async pipeline with eventual consistency maintains <100ms message delivery.
- **Partition debates by ID** – Single global state machine became bottleneck at 500+ concurrent debates. Partitioned event log per debate, single leaderboard update queue.
- **CRDT over operational transform** – OT requires operational history replay. CRDT enables tombstone-based deletion without history replay. Reduced memory overhead 60%.

---

## Career Focus

I'm optimizing for problems where:
- **Distributed systems thinking** is critical to solution quality
- **Real-time constraints** drive architectural decisions
- **Scalability** is non-negotiable from day one
- **Consistency under concurrency** requires careful tradeoff analysis
- **AI integration** must not compromise latency requirements

---

## Connect

Available for discussion on system design, distributed architecture, real-time systems, and building resilient high-scale platforms.

**GitHub:** [Ayush-Kumar0207](https://github.com/Ayush-Kumar0207)
