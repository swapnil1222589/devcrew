# DevCrew Architecture

```
                    User Request
                         │
                         ▼
              ┌─────────────────────┐
              │   Next.js Frontend  │
              │  (Agent Feed + UI)  │
              └──────────┬──────────┘
                         │ HTTP / SSE
                         ▼
              ┌─────────────────────┐
              │   FastAPI Backend   │
              │  (Orchestrator)     │
              └──────────┬──────────┘
                         │
         ┌───────────────┼───────────────┐
         ▼               ▼               ▼
   ┌─────────┐    ┌─────────┐    ┌────────────┐
   │ Planner │    │  Coder  │    │  Reviewer  │
   │  Agent  │    │  Agent  │    │   Agent    │
   └────┬────┘    └────┬────┘    └─────┬──────┘
        │              │               │
        ▼              ▼               ▼
   Implementation Plan  Code V1     Review
        │              │               │
        │              ▼               │
        │         ┌─────────┐         │
        │         │  Tester │◄────────┘
        │         │  Agent  │
        │         └────┬────┘
        │              │
        │         FAIL│PASS
        │         (1st)│(2nd)
        │              │
        │         Error│
        │              ▼
        │         ┌─────────┐
        │         │  Coder  │ (Fix applied)
        │         │  Agent  │
        │         └────┬────┘
        │              ▼
        │         Code V2
        │              │
        │              ▼
        │         ┌─────────┐
        │         │  Tester │ (Pass)
        │         └────┬────┘
        │              ▼
        │         All Passed
        │              │
        ▼              ▼
   Final Results → UI Updates (Live)
