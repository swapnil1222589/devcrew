# DevCrew — Multi-Agent AI Software Engineering Team.

A polished hackathon prototype demonstrating a collaborative multi-agent AI engineering platform.

## Concept  

DevCrew is a team of specialized AI agents that collaborate on real software engineering tasks:

- **Planner Agent** — Decomposes requirements
- **Coder Agent** — Writes and fixes code
- **Reviewer Agent** — Reviews against requirements
- **Tester Agent** — Runs real Python tests in a safe sandbox
- **Orchestrator** — Manages the Plan → Code → Review → Test → Fix → Verify loop

The prototype intentionally demonstrates a failure-and-fix cycle so judges can see real agent collaboration in action.

## Architecture

```
Frontend (Next.js + React + Tailwind)
      │
      ▼
FastAPI Backend (Agent Orchestration + SSE Streaming)
      │
      ├── Planner Agent
      ├── Coder Agent
      ├── Reviewer Agent
      ├── Tester Agent
      └── Safe Python Sandbox
```

## Quick Start

### 1. Clone & Install

```bash
git clone https://github.com/your-user/devcrew.git
cd devcrew
npm install
```

### 2. Environment

```bash
cp .env.example .env
```

Edit `.env` as needed. The default backend URL is `http://localhost:8000`.

### 3. Start Backend

```bash
# In a new terminal
pip install -r backend/requirements.txt
python -m uvicorn backend.main:app --host 0.0.0.0 --port 8000 --reload
```

### 4. Start Frontend

```bash
npm run dev
```

Visit `http://localhost:3000`.

## Demo Mode

Click **Demo Mode** in the sidebar, or click the **🚀 Start DevCrew** button with the default request. The demo automatically runs the classic average-function scenario:

1. Planner creates tasks
2. Coder generates initial code (with an intentional empty-list edge-case bug)
3. Reviewer finds the issue
4. Tester fails (`ZeroDivisionError`)
5. Coder applies the fix
6. Tester passes all 5/5 tests
7. Reviewer approves

This proves real agent collaboration rather than a single-shot code generator.

## Agent Communication

Agent events stream live to the frontend via Server-Sent Events (`/api/tasks/{id}/events`). The timeline shows:

- Agent name and icon
- Status (Analyzing, Implementing, Reviewing, Testing, Fixing, Completed)
- Action description
- Timestamp
- Errors (shown honestly when a test fails)

## Technology Stack

- **Frontend:** Next.js 16, React 19, TypeScript, Tailwind CSS v4, Framer Motion (optional), Lucide icons
- **Backend:** Python 3.11+, FastAPI, Pydantic, Uvicorn
- **Agents:** Deterministic simulated agent logic for reliable demo; architecture supports real LLM providers (OpenAI-compatible or Groq-compatible APIs)
- **Sandbox:** Controlled Python execution with timeouts and resource limits
- **Database:** SQLite (in-memory for prototype); Drizzle ORM available

## Important Design Decisions

### Controlled Failure Scenario

The prototype uses a reliable, deterministic agent flow so the demo never breaks. The initial `Coder` output intentionally omits the empty-list guard (`if not numbers: return 0`) so the `Tester` can fail with `ZeroDivisionError`, demonstrating the full collaboration loop. After the first failure, the `Coder` applies the fix and the `Tester` passes.

### Real Code Execution

The `Tester Agent` runs predefined Python tests against the generated code. The sandbox uses temporary directories and controlled subprocess execution with timeouts, not arbitrary host-level commands.

### LLM Configurability

The agent architecture supports real LLM APIs configured via environment variables (`OPENAI_API_KEY`, `GROQ_API_KEY`, etc.). For this prototype, deterministic simulation ensures the hackathon demo is bulletproof.

## File Structure

```
├── src/app/page.tsx          # Main UI (Agent feed, input, results)
├── src/app/globals.css        # Dark premium theme
├── src/components/icons.tsx  # Custom SVG icons
├── backend/
│   ├── main.py               # FastAPI server + SSE endpoints
│   ├── agents.py             # Agent implementations
│   ├── sandbox.py            # Safe Python execution
│   └── requirements.txt
├── public/
│   └── images/
├── .env.example
└── README.md
```

## Deployment

### Production Build

```bash
npm run build
npm start
```

### Docker (optional)

A `Dockerfile` for the backend and `docker-compose.yml` can be added to containerize the Python agent service separately from the Next.js frontend.

## Security & Safety

- The Python sandbox executes only predefined, controlled tasks
- No arbitrary user commands are allowed at the host level
- Execution timeout: 5 seconds
- Memory limits applied where practical
- Restricted temporary filesystem access only

## License

MIT — Hackathon prototype. Build something amazing with it.
