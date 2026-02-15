Here’s the English content you can insert into the document for Microsoft review. This version is clean, professional, and ready for inclusion in a system design document:

---

🧠 Copilot Core – Task Orchestration Architecture
System Design Document (Execution Layer Only)  
Version: v0.7  
Scope: Excludes Learning Layer

---

1. Purpose

Copilot Core is designed to transform natural language user requests into structured, executable workflows. It functions as an intelligent orchestrator that analyzes intent, decomposes tasks, coordinates across applications and tools, and manages execution with contextual awareness and user feedback.

---

2. Architecture Components

| Component | Description |
|----------|-------------|
| Planner | Parses user input and generates a multi-step execution plan. Each step defines the target app, action, and data flow. |
| Knowledge Layer | Retrieves contextual information from internal or external sources to support LLM reasoning. |
| Tool Connectors | Executable APIs or commands mapped to app capabilities. Each tool defines input/output schemas. |
| Sub-Agents | Lightweight LLM modules for specific tasks (e.g., summarization, tone adjustment). Can be wrapped as tools. |
| Autonomous Triggers | Background conditions that initiate workflows without user input (e.g., file updates, time-based triggers). |
| Control Layer | Policy enforcement, user approvals, and safety checks for sensitive or high-impact actions. |

---

3. Orchestration Flow

1. Intent Parsing  
   Understands the user’s goal and decomposes it into discrete steps.

2. Plan Generation  
   Constructs a structured plan with ordered steps, each mapped to a tool or app.

3. Execution  
   Executes steps sequentially or in parallel, passing outputs between steps.

4. Feedback Loop  
   Captures user feedback and quality signals to trigger retries, plan revisions, or session resets.

---

4. Plan Structure Example

`json
{
  "plan_id": "plan-2026Q1-report",
  "steps": [
    {
      "id": "step1",
      "session_id": "sess-abc123",
      "action": "open_file",
      "target_app": "Excel",
      "params": { "path": "Sales/2026_Q1.xlsx" }
    },
    {
      "id": "step2",
      "session_id": "sess-def456",
      "action": "analyze_data",
      "target_app": "Copilot Core",
      "params": { "input": "step1.output" }
    }
  ]
}
`

---

5. Design Principles

- Plans support sequential and parallel execution  
- Each step has a state: Pending, Running, Success, Failed, Skipped  
- Retry and rollback strategies are built-in  
- User intervention points are explicitly defined  
- Plans are persistable and resumable  
- Session IDs can be reassigned per step for isolation and recovery

---

6. Tool vs Sub-Agent Strategy

| Aspect | Tool | Sub-Agent |
|--------|------|-----------|
| Definition | Executable API or command | LLM-based micro-agent for nuanced tasks |
| Examples | /summarize, /translate | “Summarization agent”, “Tone adjuster” |
| Strengths | Fast, stable, repeatable | Flexible, contextual, creative |
| Weaknesses | Rigid, context-limited | Less predictable, resource-intensive |
| Integration | Direct call from Core | Prompt-based or wrapped as tool |

Strategy: Use tools for deterministic tasks, sub-agents for contextual reasoning. Sub-agents can be wrapped as tools for consistency.

---

7. Session Management & Transition

Session Control UI

| Function | UI Element | Description |
|----------|------------|-------------|
| Start Session | “New Task” button | Begins a new session |
| Refresh Session | “Refresh Session” button | Resets LLM state while preserving context |
| Switch Session | “Switch Session” button or auto-trigger | Creates new session with copied context |
| Restore Session | “Resume Previous” button | Reopens recent sessions |
| Status Display | Session bar | Shows current session ID and state |
| End Session | “End Session” button | Terminates current session |

Session Transition with Context Copy

`json
{
  "session_transition": {
    "from": "sess-abc123",
    "to": "sess-def456",
    "copied_context": [
      "user_goal",
      "plan_id",
      "step_history",
      "relevant_files",
      "prior_outputs"
    ],
    "excluded_context": [
      "error_responses",
      "low-confidence outputs"
    ]
  }
}
`

This ensures continuity while avoiding propagation of flawed reasoning or degraded outputs.

---

Let me know if you want this formatted as a downloadable file, or if you'd like to continue with the learning layer or another system module.
