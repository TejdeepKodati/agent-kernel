# AgentKernel: Autonomous Agent Runtime & Distributed Task Queue

[![TypeScript](https://img.shields.io/badge/TypeScript-5.6-blue.svg)](https://www.typescriptlang.org/)
[![React](https://img.shields.io/badge/React-18.3-cyan.svg)](https://react.dev/)
[![Tailwind CSS](https://img.shields.io/badge/Tailwind-3.4-38bdf8.svg)](https://tailwindcss.com/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

> Distributed agent execution runtime featuring DAG-based multi-step task planning, asynchronous worker pools with priority scheduling, execution sandboxing, company memory grounding, and automated transactional rollback.

Built by **[Tejdeep Kodati](https://github.com/TejdeepKodati)**.

---

## 🌐 Live Deployment & Source Code
- **Live Vercel Application**: [https://agent-kernel.vercel.app](https://agent-kernel.vercel.app)
- **GitHub Repository**: [https://github.com/TejdeepKodati/agent-kernel](https://github.com/TejdeepKodati/agent-kernel)

---

## 🏛️ Architectural Overview

AgentKernel serves as the underlying distributed runtime for enterprise AI agents, orchestrating goals from natural language decomposition down to deterministic tool execution:

```
                  ┌────────────────────────────────────────┐
                  │    Agent Task Decomposition Engine     │
                  └───────────────────┬────────────────────┘
                                      │
                         [ Kahn's Algorithm O(V+E) ]
                                      ▼
                  ┌────────────────────────────────────────┐
                  │       Topological Execution DAG        │
                  │   [Layer 0] ──► [Layer 1] ──► [Layer 2]│
                  └───────────────────┬────────────────────┘
                                      │
                 ┌────────────────────┴────────────────────┐
                 ▼                                         ▼
   ┌───────────────────────────┐             ┌───────────────────────────┐
   │ Priority Task Queue (Heap)│             │ Transactional Checkpointer│
   │ • Critical / High / Norm  │             │ • O(1) Rollback Stack     │
   │ • Exponential Backoff     │             │ • Invariant Preserved     │
   │ • Dead Letter Queue (DLQ) │             └─────────────┬─────────────┘
   └─────────────┬─────────────┘                           │
                 │                                         │
                 ▼                                         │
   ┌───────────────────────────┐                           │
   │  Execution Sandbox Runner │                           │
   │  • Parameter Sanitization │                           │
   │  • AST Syntax Validation  │                           │
   └─────────────┬─────────────┘                           │
                 │                                         │
                 ▼                                         │
   ┌───────────────────────────┐                           │
   │  Invariant Verification   │                           │
   │  • Pre/Post Conditions    │                           │
   │  • Credential Leak Guard  │ ──[ Violation ]───────────┘
   │  • Output Schema Checks   │
   └─────────────┬─────────────┘
                 │ [ Passed ]
                 ▼
   ┌───────────────────────────────────────────────────────┐
   │             Hierarchical Company Memory               │
   │  • Working Context (LRU Cache Buffer)                 │
   │  • Policy Ontology (Prefix Trie O(L))                 │
   │  • Semantic Knowledge Base (Dense Vector RAG k-NN)    │
   └───────────────────────────────────────────────────────┘
```

---

## 🚀 Key Technical Highlights

### 1. Directed Acyclic Graph (DAG) Task Scheduling
- Decomposes multi-step agent plans into topologically sorted layers using **Kahn's Algorithm** ($O(V+E)$).
- Resolves parallel batches so independent tool invocations execute concurrently across worker threads while strictly respecting prerequisites.
- Deterministic cycle detection prevents infinite loops in agent reasoning chains.

### 2. Distributed Priority Task Queue & Workers
- Four-tiered priority heap scheduling (`Critical`, `High`, `Normal`, `Low`).
- Automated exponential backoff retry policy with jitter for transient API and downstream tool errors.
- Dead Letter Queue (DLQ) for inspecting poisoned jobs and enabling manual operator replay.

### 3. Execution Sandboxing & Invariant Verification
- Tool parameters and outputs are strictly validated against schema invariants.
- Security filters prevent secret leakage (API tokens, AWS keys, database passwords) and block unapproved destructive mutations.
- State machine snapshots active context variables; on invariant failure, an $O(1)$ rollback stack restores clean system state.

### 4. Hierarchical Company Memory
- **L1 Working Cache**: High-throughput LRU buffer for active multi-turn context.
- **L2 Company Ontology**: Prefix Trie for instantaneous $O(L)$ departmental policy resolution.
- **L3 Semantic Vector Memory**: Cosine similarity $k$-NN search over enterprise SOP manuals and past execution runbooks.

---

## 🛠️ Local Development

```bash
# Clone the repository
git clone https://github.com/TejdeepKodati/agent-kernel.git
cd agent-kernel

# Install dependencies
npm install

# Run local development server
npm run dev

# Build production bundle
npm run build
```

---

## 📄 License
MIT © Tejdeep Kodati
