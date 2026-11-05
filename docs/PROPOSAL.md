# AlgoArena — Formal System Specification & Graduation Project Proposal

> **Project Title:** *A Gamified Competitive Coding Platform with AI-Generated, Validated Challenges*  
> **Academic Advisor:** Dr. Cherry Ahmed  
> **Domains:** EdTech · Artificial Intelligence · Distributed Systems · Real-Time Web Engineering · Gamification  
> **Primary Deliverable:** Production-grade web platform featuring an automated 10-stage AI problem validation pipeline, isolated Judge0 sandbox execution, and real-time Glicko-2 ranked 1v1 competitive battles.

---

## Table of Contents

1. [Abstract](#1-abstract)
2. [Problem Statement & Motivation](#2-problem-statement--motivation)
3. [Project Objectives](#3-project-objectives)
4. [Related Work & System Novelty](#4-related-work--system-novelty)
5. [Scope & Phased Delivery Plan](#5-scope--phased-delivery-plan)
6. [Core Functional Modules](#6-core-functional-modules)
7. [Multi-Stage AI Problem Validation Pipeline](#7-multi-stage-ai-problem-validation-pipeline)
8. [Fair-Play & Anti-Cheating Architecture](#8-fair-play--anti-cheating-architecture)
9. [System Architecture & Technology Stack](#9-system-architecture--technology-stack)
10. [Non-Functional Requirements (NFRs)](#10-non-functional-requirements-nfrs)
11. [Evaluation Methodology](#11-evaluation-methodology)
12. [Risk Analysis & Mitigation Matrix](#12-risk-analysis--mitigation-matrix)
13. [Implementation Roadmap & Team Workstreams](#13-implementation-roadmap--team-workstreams)
14. [Infrastructure & Operational Cost Model](#14-infrastructure--operational-cost-model)
15. [Expected Outcomes & Deliverables](#15-expected-outcomes--deliverables)

---

## 1. Abstract

Popular coding-practice platforms such as LeetCode, HackerRank, and Codeforces are effective for building technical skills, but many learners disengage because practice is solitary, relies on fixed question banks, and offers limited explanation of why a solution fails.

**AlgoArena** introduces a gamified, competitive coding platform that combines LLM-generated algorithmic challenges with real-time multiplayer play. Every AI-generated problem is produced together with a reference solution, a brute-force baseline solution, and a comprehensive test suite, and is accepted into the active problem bank only after passing a rigorous, automated multi-stage validation pipeline inside an isolated execution sandbox.

Players practice solo on adaptively selected problems calibrated to their topic weaknesses, receive tiered AI hints and structured code reviews, and compete in real-time rated 1v1 matches powered by **Glicko-2 matchmaking**, server-authoritative synchronized clocks, XP progression, daily streaks, and achievement badges. To preserve competitive integrity, the platform enforces strict anti-cheating controls in ranked play. System effectiveness is evaluated across problem-validation pass rates, execution and synchronization latency, matchmaking fairness, sandbox security, and user engagement studies.

---

## 2. Problem Statement & Motivation

1. **Static Question Banks & Solution Memorization:** Fixed problem catalogs encourage memorizing canonical patterns rather than developing true algorithmic adaptability. Furthermore, widely published solutions make static problems trivial to look up during live competitions.
2. **Binary Verdict Feedback:** Traditional online judges return coarse verdicts (`Accepted`, `Wrong Answer`, `Time Limit Exceeded`, `Runtime Error`) and occasionally a raw failing input, but rarely explain the underlying logical flaw or guide the learner toward improved time and space complexity.
3. **High Learner Drop-Off & Isolation:** Solitary problem grinding lacks immediate social feedback loops, leading to declining motivation once initial novelty fades.
4. **Unverified AI Generation in Existing Tools:** While standalone AI coding tutors exist, they are disconnected from rated head-to-head competition and do not mathematically or empirically verify that AI-generated problem statements, constraints, and test suites are unambiguous, solvable, and complexity-accurate.

---

## 3. Project Objectives

1. **Automated Problem Verification Pipeline:** Engineer a multi-stage validation pipeline that automatically verifies candidate LLM problems—enforcing schema validity, answer uniqueness, reference vs. brute-force cross-checks, independent solver agreement, ambiguity auditing, and empirical calibration—with a target validation pass rate of **$\ge 70\%$** of generated candidates and **zero unverified problems** ever served in ranked matches.
2. **Sandboxed Multi-Language Execution IDE:** Deliver a browser-based Monaco IDE supporting **Python, C++, and Java**, backed by an isolated self-hosted **Judge0** worker enforcing strict per-language CPU, memory, process-count, and network-disabled constraints.
3. **Real-Time Ranked 1v1 Competitive Engine:** Implement server-authoritative 1v1 coding battles with live opponent progress via WebSockets (`Socket.io`), **Glicko-2** skill ratings (rating, deviation, volatility), widening matchmaking queues with fallback practice bots, and a **60-second disconnect grace window**.
4. **Tiered AI Mentorship (Practice Mode Only):** Provide an incremental AI hint system (`Concept Nudge → Approach Hint → Targeted Code Feedback`) and post-submission code reviewer hardened against prompt injection.
5. **Comprehensive Empirical Evaluation:** Benchmark the platform across problem correctness, match synchronization latency ($<300\text{ ms}$), judging latency ($<5\text{ s}$ median), rating convergence, sandbox containment, and learner engagement.

---

## 4. Related Work & System Novelty

Existing platforms fall into three distinct categories:
- **Classic Online Judges** (*LeetCode, HackerRank, Codeforces, Codewars*): Feature large static banks and batch contests, but offer limited real-time 1v1 pacing or adaptive AI tutoring.
- **Gamified Coding Platforms** (*CodinGame, CodeCombat*): Emphasize visual competition and engagement, but rely on manually curated problem sets.
- **Standalone AI Coding Assistants**: Offer conversational hints and code explanations, but lack sandbox verification and cannot be safely integrated into rated competitive environments.

### Core Novelty of AlgoArena
AlgoArena bridges these three domains by combining:
1. **Multi-Stage Sandboxed Verification of LLM Problems:** Ensuring every generated problem is provably solvable, unambiguous, and complexity-verified before reaching users.
2. **Solo-Practice Quarantine & Empirical Calibration:** New AI problems must accumulate $N \ge 20$ real solo attempts to calibrate actual difficulty before promotion to ranked 1v1 pools.
3. **Fair-Play-Aware Competitive Modes:** Strictly isolating AI mentorship to solo practice while equipping ranked 1v1 battles with telemetry signals and post-match token similarity checks.

---

## 5. Scope & Phased Delivery Plan

To guarantee high engineering quality and reliable delivery, the system is structured into three sequential phases:

### Phase 1 — Foundation (Committed Core Deliverable)
- User registration, JWT authentication, and persistent player profiles.
- Self-hosted Judge0 sandbox integration and browser-based Monaco IDE supporting **Python, C++, and Java**.
- **10-Stage AI Problem Validation Pipeline** (schema validation, uniqueness, cross-check, multi-LLM solve check, critic audit, edge-case & complexity enforcement, similarity filtering, and quarantine calibration).
- Solo practice mode with bank-first serving and empirical calibration funnel ($N \ge 20$ attempts).
- Core gamification mechanics: XP calculation, player levels, daily coding streaks, and initial achievement badges.

### Phase 2 — Ranked 1v1 & AI Mentorship (Committed MVP Completion)
- Real-time Ranked 1v1 matchmaking queue with widening rating search windows and fallback practice bots.
- Server-authoritative match timers, live opponent test-case progress over `Socket.io`, and a 60-second disconnect grace window.
- **Glicko-2** rating engine ($R$, $RD$, $\sigma$) and Redis Sorted-Set global leaderboards.
- Fair-play & anti-cheating telemetry layer (AI lockout in ranked play, focus-loss/paste telemetry, token-based similarity checks).
- Tiered 3-level AI Hint System (`Concept Nudge → Approach Hint → Targeted Feedback`) in solo practice mode.

### Phase 3 — Stretch Goals
- Post-submission AI Code Reviewer with complexity and style critique.
- Adaptive difficulty engine targeting per-topic player weaknesses (e.g., Dynamic Programming, Graphs, Greedy).
- Additional game modes: **Spectator Mode**, **Debug-the-Code**, and **Timed Sprints**.
- Multi-stage **Boss Problems** as tier-advancement milestone trials.
- **Team Battles**, bracket tournaments, and collaborative **Pair Programming** powered by CRDTs (`Yjs`).

---

## 6. Core Functional Modules

### 6.1 AI Generation & Mentorship Layer
- **Structured Challenge Generation:** Produces a complete problem artifact—problem statement, input/output specification, mathematical constraints, optimal reference solution, brute-force solution, and deterministic test generator/suite—strictly conforming to a Zod JSON schema.
- **Tiered Hint & Review Engine:** Analyzes failed practice submissions and delivers progressive guidance without leaking full solutions:
  1. *Tier 1 — Concept Nudge:* Identifies the algorithmic paradigm or data structure required.
  2. *Tier 2 — Approach Hint:* Outlines the high-level transition or invariant without code.
  3. *Tier 3 — Targeted Code Feedback:* Pinpoints the specific logical bug, off-by-one error, or complexity bottleneck in the user's submission.
  - All user code and comments are wrapped as untrusted data blocks to neutralize prompt-injection attempts.
- **Adaptive Topic Selection:** Tracks per-tag solve rates and routes solo players toward problems addressing their weakest algorithmic categories.

### 6.2 Real-Time Competitive Layer
- **Deterministic Win Condition:** The first player to pass $100\%$ of hidden and public test cases wins the duel. If neither player passes all tests before the server clock expires, ties are broken by:
  1. Highest percentage of test cases passed,
  2. Fewest incorrect submission penalties,
  3. Earliest timestamp of the highest-scoring submission.
- **Matchmaking & Rating Engine:** Pairs players by Glicko-2 rating proximity using an expanding search window over time, seamlessly assigning calibrated practice bots during off-peak hours.
- **Disconnect Resilience:** Maintains match state in Redis; if a player drops connection, a 60-second grace timer allows state recovery before declaring a forfeit.

### 6.3 Gamification Engine
- **Progression Loop:** Awards XP scaled by problem difficulty, solve speed, and match outcomes; tracks consecutive daily activity streaks; and unlocks milestone badges.

---

## 7. Multi-Stage AI Problem Validation Pipeline

Because LLM-generated problems can suffer from hallucinated constraints, ambiguous wording, or incorrect reference code, **no problem enters the platform until it passes all 10 validation stages**:

```
[Stage 1: Structured Generation]
       │
       ▼
[Stage 2: Zod Schema Validation] ──(Malformed)──► Retry / Discard
       │
       ▼
[Stage 3: Answer-Uniqueness Check] ──(Non-deterministic Output)──► Reject
       │
       ▼
[Stage 4: Reference vs. Brute-Force Sandbox Cross-Check] ──(Output Mismatch)──► Reject
       │
       ▼
[Stage 5: Independent Multi-LLM Solve Verification] ──(Unintended Failure)──► Reject
       │
       ▼
[Stage 6: Critic LLM Ambiguity & Constraint Audit] ──(Ambiguous Spec)──► Reject
       │
       ▼
[Stage 7: Edge-Case Coverage Verification] ──(Missing Boundary Tests)──► Regenerate Tests
       │
       ▼
[Stage 8: Complexity Separation Check (TLE Enforcement)] ──(Brute-Force Passes)──► Reject
       │
       ▼
[Stage 9: Duplicate & Cosine/Token Similarity Check] ──(Near-Duplicate)──► Reject
       │
       ▼
[Stage 10: Solo-Practice Quarantine Calibration (N >= 20)] ──(Verified)──► Ranked 1v1 Bank
```

1. **Structured Candidate Generation:** LLM outputs statement, I/O format, constraints, optimal reference solution, brute-force solution, and test suite/generator.
2. **Schema Validation (`Zod`):** Enforces strict type, field, and constraint bounds; malformed JSON payloads are automatically retried or discarded.
3. **Answer-Uniqueness Verification:** Confirms that the problem requires a single canonical output per test input (or provides a deterministic custom comparator) so valid alternative outputs are never falsely rejected.
4. **Reference vs. Brute-Force Cross-Check:** Executes both the optimal reference solution and the brute-force solution inside the Judge0 sandbox across all small and medium test cases. Any output discrepancy immediately rejects the candidate.
5. **Independent Multi-LLM Solve Verification:** A secondary, independent LLM solver attempts the problem purely from the generated statement and constraints (without seeing the reference solution). If the independent solver fails or uncovers contradictions, the problem is flagged.
6. **Critic LLM Ambiguity Audit:** A dedicated Critic prompt audits the problem statement for unstated assumptions, input-bound contradictions, or formatting ambiguities.
7. **Edge-Case Coverage Enforcement:** Verifies that the test suite includes minimum/maximum bounds, empty or single-element collections, duplicate values, negative numbers, and worst-case structural topologies (e.g., skewed trees, dense graphs).
8. **Asymptotic Complexity Separation Check:** Runs the brute-force solution against maximum-scale stress tests in Judge0. The brute-force solution **must fail with Time Limit Exceeded (TLE)** while the optimal reference solution **must pass comfortably within the language time limit**, proving the problem genuinely tests the target time complexity.
9. **Duplicate & Similarity Filtering:** Computes semantic and structural similarity against existing bank entries and canonical public problems to prevent near-duplicates.
10. **Solo-Practice Quarantine Calibration ($N \ge 20$ Attempts):** Accepted problems initially enter the **Solo Practice Quarantine Pool**. Only after accumulating at least $N \ge 20$ real user attempts—confirming empirical solve rates, median solve times, and zero abnormal failure spikes—is the problem promoted to the **Ranked 1v1 Pool**.

> **Bank-First Serving Architecture:** Because multi-stage generation and sandbox verification take 10–30+ seconds, ranked 1v1 matches **always** draw instantaneously from the pre-validated, calibrated problem bank.

---

## 8. Fair-Play & Anti-Cheating Architecture

1. **Strict Ranked AI Lockout:** All AI hint endpoints and AI code review services are hard-disabled at the API guard level whenever a user has an active ranked match session in Redis.
2. **Just-in-Time Problem Reveal:** Ranked match problems are selected server-side at match start from the calibrated bank and revealed simultaneously to both competitors.
3. **Client-Side Integrity Telemetry:** The Monaco IDE client records non-intrusive fair-play telemetry during ranked matches:
   - Window/tab focus-loss events (`visibilitychange` / `blur` duration and frequency),
   - Large external clipboard paste bursts exceeding threshold character counts,
   - Anomalously rapid solve times relative to problem difficulty.
4. **Post-Match Token Similarity Detection:** Submissions from ranked matches undergo token-level structural fingerprinting (normalizing identifiers and whitespace) to detect copy-pasted or plagiarized code.
5. **Rating Quarantine:** Flagged matches are held for administrative review and excluded from Glicko-2 rating updates if a fair-play violation is confirmed.

---

## 9. System Architecture & Technology Stack

To maximize developer velocity and end-to-end type safety, AlgoArena uses a unified **TypeScript** architecture across the client, API server, real-time WebSocket gateway, and background validation workers.

| Architectural Layer | Technology Choice | Engineering Rationale |
|---|---|---|
| **Primary Language** | **TypeScript (Full-Stack)** | Unified static typing and shared Zod schemas across frontend, backend APIs, WebSocket events, and AI workers |
| **Supported Sandbox Languages** | **Python 3, C++ (GCC), Java (OpenJDK)** | Covers the top three competitive programming languages with calibrated per-language CPU and memory multipliers |
| **Frontend Client** | **Next.js (React), Monaco Editor, Tailwind CSS, Zustand** | App-router SSR/CSR performance, VS Code-grade browser IDE experience, utility styling, and low-overhead reactive match state |
| **Backend API & Real-Time Gateway** | **NestJS + Socket.io** | Modular dependency-injected architecture pairing REST controllers with room-based WebSocket namespaces for live 1v1 sync |
| **AI & Validation Layer** | **Provider-Agnostic TypeScript LLM Interface + Zod Structured Outputs** | Decouples validation logic from any single vendor (Google Gemini, OpenAI, Anthropic, or OpenRouter); enforces strict JSON schemas |
| **Code Execution Sandbox** | **Self-Hosted Judge0 (`isolate` Linux Sandbox) on Dedicated Worker Host** | Industry-standard competitive judge enforcing Linux namespaces, `cgroups`, CPU time, memory, process limits, and zero network egress |
| **Relational Database** | **PostgreSQL** | ACID persistence for users, Glicko-2 rating histories, validated problem banks, test suites, and match audit logs |
| **In-Memory Cache & Queues** | **Redis** | Sub-millisecond matchmaking queues, ephemeral live 1v1 match clocks/states, Sorted-Set (`ZSET`) global leaderboards, and rate limiting |
| **DevOps & Infrastructure** | **Git, GitHub Actions, Docker, Vercel + Cloud VM** | Automated CI pipelines, containerized Judge0 workers isolated from the core database, and zero-downtime web deployments |

### Security Isolation of Untrusted Code
Judge0 relies on Linux kernel isolation (`isolate` with `cgroups` and namespaces). Consequently, the Judge0 execution worker runs on a **dedicated, isolated host/VM** holding zero application secrets, zero access to the PostgreSQL/Redis internal network, disabled outbound network egress for user programs, and strict per-submission resource caps.

---

## 10. Non-Functional Requirements (NFRs)

| Requirement Category | Target Metric | Architectural Mechanism |
|---|---|---|
| **Concurrency Capacity** | **100+ simultaneous active users** / **20+ concurrent 1v1 matches** | Stateless NestJS API horizontality + Redis-backed WebSocket state |
| **Match State Sync Latency** | **$< 300\text{ ms}$** end-to-end opponent progress updates | Event-driven `Socket.io` room broadcasts backed by in-memory Redis |
| **Code Judging Latency** | **$< 5\text{ s}$ median** turnaround for multi-test submissions | Pre-warmed self-hosted Judge0 worker pool with batched test execution |
| **Matchmaking Queue Time** | **$< 60\text{ s}$ median** wait time at target concurrency | Dynamic Glicko-2 rating window expansion + calibrated bot fallback |
| **Sandbox Resource Limits** | Enforced per-language CPU time, RAM cap, process limit (`ulimit`), output byte cap, and **network disabled** | Judge0 `isolate` sandbox profiles per language (Python / C++ / Java) |
| **Fault Tolerance & Availability** | **Zero ranked match disruption** during external LLM outages | Bank-first problem serving + asynchronous background generation queues |

---

## 11. Evaluation Methodology

1. **Validation Pipeline Yield & Precision:** Measure the percentage of LLM-generated candidates passing each of the 10 validation stages (targeting $\ge 70\%$ overall yield after prompt tuning) alongside blind human expert review of a random stratified sample for statement clarity and constraint soundness.
2. **System Latency & Throughput Benchmarks:** Conduct automated load testing simulating 100 concurrent users and 20 active matches to verify $<300\text{ ms}$ WebSocket broadcast latency and $<5\text{ s}$ median Judge0 execution latency.
3. **Glicko-2 Matchmaking Fairness:** Analyze win-rate distributions across rating differentials and verify rating deviation ($RD$) convergence over simulated and live matches.
4. **Pedagogical Quality of AI Hints:** Evaluate a sample of Tier 1–3 hints and code reviews against a blind rubric measuring diagnostic accuracy, clarity, and strict non-leakage of complete solutions.
5. **Comparative User Engagement Study:** Conduct a structured study with 15–20 computer science students comparing AlgoArena against a baseline static judge workflow across session duration, voluntary return rate, and post-session motivation surveys.
6. **Adversarial Sandbox Penetration Testing:** Execute adversarial payloads—including fork bombs (`:(){ :|:& };:`), infinite loops, heap/stack exhaustion, `/etc/passwd` reads, and raw socket creation—to verify $100\%$ containment within Judge0.

---

## 12. Risk Analysis & Mitigation Matrix

| Identified Risk | Impact | Engineering Mitigation |
|---|---|---|
| **LLM generates unsolvable, ambiguous, or incorrect problems** | High | 10-stage automated validation pipeline (Section 7) + $N \ge 20$ solo-practice quarantine before ranked promotion |
| **Players use external AI tools during ranked 1v1 matches** | Medium | Hard lockout of platform AI in ranked mode + focus-loss/paste telemetry + post-match token similarity analysis (Section 8) |
| **LLM API rate limits or token cost spikes** | Medium | Offline batch pre-generation into PostgreSQL bank, aggressive caching, and per-user daily hint quotas |
| **Malicious code attempts sandbox escape or DoS** | Critical | Dedicated isolated Judge0 worker VM, network-disabled containers, strict `cgroups`/process limits |
| **Prompt injection via malicious code comments in AI Reviewer** | Medium | Delimit user code as non-executable data blocks, strip system-override instructions, and enforce Zod output schemas |
| **Matchmaking cold-start (empty queue at off-peak hours)** | Medium | Widening Glicko-2 search window + calibrated fallback practice bots with realistic solve-time distributions |
| **Player network drop mid-battle** | Low | Server-authoritative Redis match state with a 60-second reconnection grace window |

---

## 13. Implementation Roadmap & Team Workstreams

### 16-Week Execution Schedule

| Weeks | Phase | Key Engineering Milestones |
|---|---|---|
| **Weeks 1–2** | Architecture & Setup | Monorepo scaffolding, TypeScript/NestJS/Next.js setup, PostgreSQL & Redis schemas, Dockerized Judge0 worker, CI/CD pipelines |
| **Weeks 3–5** | Core Execution & IDE | JWT Auth, user profiles, Monaco Editor integration (Python/C++/Java), Judge0 submission pipeline, and sandbox resource hardening |
| **Weeks 6–8** | Validation Pipeline & Solo MVP | 10-stage AI Problem Validation Pipeline, solo practice loop, quarantine calibration ($N \ge 20$), XP, levels, streaks, and badges (**Phase 1 Complete**) |
| **Weeks 9–11** | Ranked 1v1 & Matchmaking | Socket.io battle gateway, server-authoritative timers, widening matchmaking queue + fallback bots, Glicko-2 rating engine, and Redis global leaderboards |
| **Weeks 12–13** | AI Mentorship & Fair Play | Tiered AI hint engine, prompt-injection defenses, ranked telemetry signals, and token-based similarity detector (**Phase 2 Complete**) |
| **Week 14** | Load & Security Testing | Concurrency stress testing (100 users / 20 matches) and adversarial Judge0 sandbox security audit |
| **Week 15** | Empirical Evaluation | Student user study, validation yield analysis, and latency benchmarking |
| **Week 16** | Final Polish & Stretch Goals | Selected Phase 3 stretch features, final technical report, and live demonstration |

### Core Engineering Workstreams
- **Frontend & Interactive UX:** Next.js App Router, Monaco IDE workspace, live 1v1 battle HUD, Zustand state synchronization, and gamification animations.
- **Backend & Real-Time Distributed Systems:** NestJS API modules, Socket.io match rooms, server-authoritative clocks, Glicko-2 rating service, and Redis leaderboards.
- **AI Orchestration & Validation Pipeline:** Provider-agnostic LLM client, Zod schemas, 10-stage automated problem verifier, and tiered hint/review agents.
- **Sandbox Security, DevOps & QA:** Self-hosted Judge0 worker isolation, Docker Compose orchestration, GitHub Actions CI/CD, load testing, and sandbox security verification.

---

## 14. Infrastructure & Operational Cost Model

1. **LLM Token Consumption:**
   - *Batch Problem Generation:* Amortized one-time cost per validated bank problem $(\text{Candidates Generated} \times \text{Tokens per Stage} \times \text{Unit Token Price})$, minimized by bank-first serving.
   - *Practice Mode Hints & Reviews:* Bounded by per-user daily hint quotas and routed to cost-efficient fast reasoning models.
2. **Cloud Compute & Storage:**
   - **Frontend Hosting:** Vercel (Zero-cost / Hobby tier).
   - **Application & Database Host:** Single cloud VM running NestJS, PostgreSQL, and Redis containers.
   - **Dedicated Sandbox Host:** Separate isolated Linux VM running the Judge0 worker and `isolate` runtime, funded via academic/student cloud credits.

---

## 15. Expected Outcomes & Deliverables

Upon completion of the committed phases, **AlgoArena** delivers:
1. **Verified AI Problem Bank & Pipeline:** An automated 10-stage generation and sandbox validation engine producing ambiguity-free, complexity-verified algorithmic challenges.
2. **Browser-Based Multi-Language IDE:** Secure execution of **Python, C++, and Java** submissions via an isolated Judge0 cluster.
3. **Real-Time Ranked 1v1 Arena:** Low-latency ($<300\text{ ms}$) head-to-head battles with synchronized timers, Glicko-2 ratings, fallback bots, and a 60-second disconnect grace window.
4. **Context-Aware AI Tutor:** Tiered practice-mode hints and code reviews protected against prompt injection and locked out during ranked matches.
5. **Empirical Benchmark & Security Report:** Full evaluation metrics covering validation pass rates, system latency under load, matchmaking fairness, sandbox security verification, and user engagement findings.
