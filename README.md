<div align="center">

# ⚔️ AlgoArena — A Gamified Competitive Coding Platform with AI-Generated, Validated Challenges

**Multi-Stage Sandboxed AI Challenge Validation · Real-Time Glicko-2 Ranked 1v1 Battles · Isolated Judge0 Execution · Tiered AI Mentorship**

[![Status](https://img.shields.io/badge/Status-Phase_1_%E2%80%94_In_Active_Development-F59E0B?style=for-the-badge)](./docs/PROPOSAL.md)
[![Advisor](https://img.shields.io/badge/Academic_Advisor-Dr._Cherry_Ahmed-4F46E5?style=for-the-badge)](./docs/PROPOSAL.md)

[![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=for-the-badge&logo=typescript&logoColor=white)](https://www.typescriptlang.org/)
[![Next.js](https://img.shields.io/badge/Next.js-000000?style=for-the-badge&logo=nextdotjs&logoColor=white)](https://nextjs.org/)
[![React](https://img.shields.io/badge/React-20232A?style=for-the-badge&logo=react&logoColor=61DAFB)](https://react.dev/)
[![Tailwind CSS](https://img.shields.io/badge/Tailwind_CSS-06B6D4?style=for-the-badge&logo=tailwindcss&logoColor=white)](https://tailwindcss.com/)
[![Zustand](https://img.shields.io/badge/Zustand-443E38?style=for-the-badge&logo=react&logoColor=white)](https://zustand-demo.pmnd.rs/)
[![NestJS](https://img.shields.io/badge/NestJS-E0234E?style=for-the-badge&logo=nestjs&logoColor=white)](https://nestjs.com/)
[![Socket.io](https://img.shields.io/badge/Socket.io-010101?style=for-the-badge&logo=socketdotio&logoColor=white)](https://socket.io/)
[![Judge0](https://img.shields.io/badge/Judge0-Sandbox_Engine-1E293B?style=for-the-badge&logo=linux&logoColor=white)](https://judge0.com/)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4169E1?style=for-the-badge&logo=postgresql&logoColor=white)](https://www.postgresql.org/)
[![Redis](https://img.shields.io/badge/Redis-DC382D?style=for-the-badge&logo=redis&logoColor=white)](https://redis.io/)
[![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)](https://www.docker.com/)
[![GitHub Actions](https://img.shields.io/badge/GitHub_Actions-2088FF?style=for-the-badge&logo=githubactions&logoColor=white)](https://github.com/features/actions)

📄 **[Read the Full Formal System Specification & Proposal (`docs/PROPOSAL.md`)](./docs/PROPOSAL.md)**

</div>

---

## Overview

Traditional competitive programming and interview-prep platforms (*LeetCode, HackerRank, Codeforces*) rely on static question banks that encourage pattern memorization, return binary pass/fail verdicts without pedagogical feedback, and isolate learners in solitary grinding loops.

**AlgoArena** transforms algorithmic practice into a fair, dynamic, and competitive multiplayer experience by uniting **automated multi-stage LLM problem validation**, **real-time Glicko-2 ranked 1v1 duels**, **isolated Judge0 code execution**, and **prompt-injection-hardened AI mentorship** (restricted strictly to practice mode).

---

## Key Architecture & Technical Novelty

### 1. 🔬 10-Stage AI Problem Validation Pipeline
Unverified LLM-generated coding challenges frequently suffer from ambiguous statements, broken constraints, or faulty reference code. In **AlgoArena**, no AI-generated problem ever reaches a player until it passes a **10-stage automated verification funnel** (targeting a $\ge 70\%$ candidate validation yield and **zero unverified problems** in ranked play):

```
[1. LLM Structured Generation] ──► [2. Zod Schema Validation] ──► [3. Answer-Uniqueness Check]
                                                                               │
[6. Critic LLM Ambiguity Audit] ◄── [5. Independent Multi-LLM Solve] ◄── [4. Ref vs. Brute-Force Cross-Check]
               │
               ▼
[7. Edge-Case Coverage Check] ──► [8. Complexity Separation (TLE)] ──► [9. Duplicate/Similarity Filter]
                                                                               │
                                                                               ▼
                                                   [10. Solo-Practice Quarantine Calibration (N >= 20)]
                                                                               │
                                                                               ▼
                                                                  🏆 Promoted to Ranked 1v1 Bank
```

1. **Structured Candidate Generation:** Produces statement, I/O spec, constraints, optimal reference solution, brute-force baseline solution, and deterministic test suite/generator.
2. **Schema Validation (`Zod`):** Enforces strict JSON schema types and constraint bounds; malformed outputs are retried or discarded.
3. **Answer-Uniqueness Check:** Verifies single canonical output per input (or attaches a deterministic custom validator).
4. **Reference vs. Brute-Force Cross-Check:** Executes both solutions inside the Judge0 sandbox across all baseline tests; any output mismatch immediately rejects the candidate.
5. **Independent Multi-LLM Solve Verification:** A secondary, independent LLM solver must solve the problem strictly from the statement without access to the reference solution.
6. **Critic LLM Ambiguity Audit:** Audits the problem text for unstated assumptions, edge-case contradictions, or I/O formatting ambiguity.
7. **Edge-Case Coverage Enforcement:** Guarantees inclusion of minimum/maximum bounds, empty/single-element inputs, duplicates, and worst-case topologies.
8. **Complexity Separation Enforcement:** Runs the brute-force solution on maximum-scale tests in Judge0—confirming the brute-force solution fails with **Time Limit Exceeded (TLE)** while the reference solution passes within limits.
9. **Similarity & Duplicate Check:** Rejects candidates too similar to existing bank problems or well-known public benchmarks.
10. **Solo-Practice Quarantine Calibration ($N \ge 20$ Attempts):** Validated problems enter solo practice first. Only after accumulating **$N \ge 20$ real solo attempts** with healthy empirical solve rates and zero abnormal failure spikes are they promoted to the **Ranked 1v1 Bank** (*bank-first serving*).

---

### 2. ⚔️ Real-Time Ranked 1v1 Mode
- **Server-Authoritative Synchronized Clocks:** Match timers and win conditions are managed authoritatively on the NestJS + Redis backend to prevent client-side clock tampering.
- **Live Opponent Progress via `Socket.io`:** Real-time WebSocket broadcasts (`< 300 ms` target sync latency) stream opponent submission attempts and test-case pass ratios without leaking code.
- **Glicko-2 Rating System:** Tracks player skill rating ($R$), rating deviation ($RD$), and rating volatility ($\sigma$) for fairer pairings and rapid new-player convergence, backed by Redis Sorted-Set (`ZSET`) global leaderboards.
- **Widening Matchmaking Queue & Practice Bots:** Pairs competitors by Glicko-2 proximity with a dynamically expanding search window (`< 60 s` median wait), seamlessly falling back to calibrated practice bots when queues are sparse.
- **60-Second Disconnect Grace Window:** Ephemeral match state in Redis preserves active duels during brief network drops, allowing players to reconnect within 60 seconds before forfeiting.

---

### 3. 🛡️ Sandboxed Code Execution (`Judge0` + `isolate`)
- **Multi-Language Browser IDE:** Powered by **Monaco Editor** supporting **Python, C++, and Java**.
- **Strict Kernel Isolation:** Code runs on a dedicated, self-hosted **Judge0** worker VM using Linux namespaces and `cgroups` (`isolate`) with strict per-language CPU time, memory ceilings, process count caps (`ulimit`), output size caps, and **outbound network access completely disabled**.
- **Low-Latency Turnaround:** Pre-warmed worker containers deliver a **$< 5\text{ s}$ median judging latency** across full test suites.

---

### 4. 🤖 AI Mentorship (Practice Mode Only)
- **Tiered 3-Step Hint System:** Guides learners through failed submissions without spoiling the answer:
  1. **Tier 1 — Concept Nudge:** Points toward the relevant data structure or algorithmic paradigm.
  2. **Tier 2 — Approach Hint:** Explains the state transition or invariant conceptually.
  3. **Tier 3 — Targeted Code Feedback:** Identifies the exact logical flaw, boundary bug, or complexity bottleneck in the player's code.
- **Prompt-Injection Defenses:** User source code and comments are isolated as non-executable data blocks and validated against strict output schemas so adversarial comments cannot hijack the AI reviewer into leaking solutions.

---

### 5. 🔒 Fair-Play & Anti-Cheating Layer
- **Hard Ranked AI Lockout:** All AI hints and code review endpoints are strictly disabled during active ranked 1v1 sessions.
- **Client Integrity Telemetry:** Tracks window/tab focus-loss events (`blur`/`visibilitychange`) and large external clipboard paste bursts during ranked duels.
- **Post-Match Token Similarity Detection:** Runs token-normalized structural comparisons across submissions to flag suspected plagiarism and hold flagged matches for review before applying rating changes.

---

## System Architecture & Technology Stack

| Layer | Choice | Engineering Rationale |
|---|---|---|
| **Languages** | **TypeScript (Full-Stack)** | Unified language and shared `Zod` types across frontend, backend, WebSocket events, and AI workers |
| **Frontend** | **Next.js (React), Monaco Editor, Tailwind CSS, Zustand** | Fast SSR/CSR routing, VS Code-grade browser IDE, utility styling, and lightweight reactive match state |
| **Backend & Real-Time** | **NestJS + Socket.io** | Structured modular REST APIs paired with low-latency WebSocket rooms for live 1v1 match state and leaderboards |
| **AI Services** | **Provider-Agnostic TypeScript LLM Layer + JSON Schema / Zod Validation** | Swappable model providers (Gemini / OpenAI / Claude) using native structured outputs without heavy wrapper lock-in |
| **Code Execution** | **Self-Hosted Judge0 (`isolate` sandbox) on Dedicated Worker Host** | Battle-tested judge enforcing CPU, memory, process, and network-disabled limits; isolated from core app secrets and DB |
| **Primary Database** | **PostgreSQL** | Relational integrity for user profiles, Glicko-2 rating histories, validated problem banks, and match records |
| **Cache & Queues** | **Redis** | Matchmaking queues, server-authoritative live match state, Sorted-Set (`ZSET`) leaderboards, and rate limiting |
| **DevOps & CI/CD** | **Git, GitHub Actions, Docker, Vercel + Cloud VM** | Automated testing and builds; Vercel for Next.js frontend and isolated Dockerized cloud hosts for NestJS and Judge0 |

---

## Non-Functional Performance Targets

| Metric | Target |
|---|---|
| **Concurrency** | `100` simultaneous users / `20` concurrent live 1v1 matches |
| **Match State Sync Latency** | `< 300 ms` for live opponent progress updates via `Socket.io` |
| **Judging Latency** | `< 5 s` median turnaround for multi-test Judge0 submissions |
| **Matchmaking Queue Wait** | `< 60 s` median wait time with widening Glicko-2 search window |
| **Problem Validation Pass Rate** | `>= 70%` candidate validation yield; `0` unverified problems served in ranked matches |

---

## Phased Roadmap

### Phase 1: Foundation (Committed Core Deliverable)
- [ ] **Authentication & Player Profiles:** JWT registration/login, persistent user profiles, and session management.
- [ ] **Judge0 Sandbox & Monaco IDE:** Browser-based IDE supporting **Python, C++, and Java** with per-language CPU, memory, process, and network-disabled sandbox limits.
- [ ] **10-Stage Problem Validation Pipeline:** Automated LLM generation, Zod schema check, answer-uniqueness check, reference vs. brute-force cross-check, independent multi-LLM solve check, critic LLM ambiguity audit, edge-case & complexity (TLE) verification, and similarity filtering.
- [ ] **Solo Practice & Quarantine Calibration Funnel:** Bank-first solo practice mode with empirical calibration quarantine ($N \ge 20$ attempts before ranked promotion).
- [ ] **Core Gamification Engine:** Experience points (XP), player levels, daily coding streaks, and achievement badges.

### Phase 2: Ranked 1v1 & AI Mentorship (Completes MVP)
- [ ] **Matchmaking Queue & Fallback Bots:** Rating-proximity matchmaking queue in Redis with widening search windows and calibrated practice bots for cold-start queues.
- [ ] **Real-Time 1v1 Arena (`Socket.io`):** Server-authoritative match timers, live opponent test-case progress indicators, deterministic tie-breakers, and a 60-second disconnect grace window.
- [ ] **Glicko-2 Rating System & Global Leaderboard:** Full Glicko-2 ($R, RD, \sigma$) rating updates and real-time Redis Sorted-Set leaderboards.
- [ ] **Fair-Play & Anti-Cheating Layer:** Ranked AI lockout, tab focus-loss and large-paste telemetry, and post-match token-based code similarity detection.
- [ ] **Tiered AI Hint System (Practice Mode):** 3-tier incremental hints (`Concept Nudge → Approach Hint → Targeted Feedback`) hardened against prompt injection.

### Phase 3: Stretch Goals
- [ ] **AI Code Reviewer:** Deep post-submission complexity and code-quality reviews in solo practice.
- [ ] **Adaptive Difficulty by Topic Weakness:** Dynamic problem selection targeting each player's weakest algorithmic categories (e.g., DP, Graphs, Greedy).
- [ ] **Custom Game Modes:** Live **Spectator Mode**, **Debug-the-Code**, and **Timed Sprint** modes.
- [ ] **Boss Problems:** Multi-stage milestone algorithmic trials at tier boundaries.
- [ ] **Team Battles & Collaborative Pair Programming:** Multi-player bracket tournaments and real-time collaborative coding via CRDTs (`Yjs`).

---

## Documentation & Evaluation Plan

For full details on the **10-stage validation pipeline**, **Glicko-2 matchmaking math**, **sandbox security testing** (fork bombs, infinite loops, memory exhaustion, network isolation), **16-week implementation schedule**, and **user study methodology**, see:

👉 **[`docs/PROPOSAL.md` — Formal Graduation Project Proposal & System Specification](./docs/PROPOSAL.md)**
