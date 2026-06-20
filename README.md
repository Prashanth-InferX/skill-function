# Skill Function

**A new primitive in AI. Callable expertise in the cloud.**

> *Not a file you download. A function you call.*

---

## What is a Skill Function?

A **Skill Function** is a protected AI capability hosted as a callable endpoint in the cloud. You send input, you receive expert output. The underlying instructions never leave the platform.

```bash
POST api.inferx.net/skills/saas-pricing

{
  "input": "B2B SaaS, $50 ACV, PLG motion, 3 tiers"
}

→ Expert pricing strategy. 195ms. Instructions never exposed.
```

Think of it like calling Stripe's API — you don't know how their fraud detection works. You just get the result.

---

## The Problem with Local Skills

Today, AI skills are SKILL.md files. You download them locally, they load into your agent's context window, and force everything through one expensive model.

Three structural failures:

**1. The monolithic model tax**
One flagship model handles everything — summarizing a routine email and analyzing complex legal contracts pay the same price. 80% of context is consumed by tool definitions and history, not your actual task.

**2. Security risk**
Local skills run with full system privileges. They can read files, invoke shell commands, and access credentials. 26.1% of publicly available skills contain at least one security vulnerability.

**3. Context bottleneck**
Complex workflows cannot fit in a single context window. The more skills you add, the slower and less accurate your agent gets.

---

## The Skill Function Architecture

Skill Function moves each skill to the cloud as an independent callable service.

```
Agent
  └── calls Skill Function via MCP
        └── runs in isolated cloud context
              └── right model for the task
                    └── returns expert output
```

### Right Model for Each Skill

Every Skill Function is bound to the optimal model for its task:

| Task complexity | Model tier | Cost vs flagship |
|---|---|---|
| Simple classification | 7B–9B | 95–99% cheaper |
| Data extraction | 14B–35B | 80–90% cheaper |
| Code generation | 35B–70B | 60–80% cheaper |
| Complex reasoning | Flagship | Same |

**Result: 70–90% lower inference cost for mixed workloads.**

### Dedicated Clean Context

Each Skill Function runs in its own isolated context window:
- No unrelated tool definitions
- No cross-skill history
- No accumulated conversation pollution
- Performance does not degrade with every turn

### Skills Call Other Skills

```
orchestrator
  ├── legal-reviewer      ← if legal task
  ├── pricing-strategist  ← if pricing task  
  └── code-reviewer       ← if code task
```

One call from your agent. The orchestrator routes automatically. Complex workflows decompose into a graph of skill calls — no single context limit.

### Zero Local Execution

A Skill Function cannot access local files, invoke shell commands, or read credentials. Security is architectural — not bolted on.

Even a successful prompt injection can only influence the skill's output text. It cannot trigger system-level actions.

---

## MCP-Native Discovery

Skill Function exposes a standard MCP tool-calling interface.

```json
{
  "mcpServers": {
    "inferx-skills": {
      "url": "https://api.inferx.net/mcp",
      "transport": "http"
    }
  }
}
```

Subscribe to a skill → it auto-appears in your agent's MCP tool list. Works with Claude Code, Cursor, OpenClaw, and any MCP-compatible agent.

No local deployment. No environment variables. No manual configuration.

---

## Comparison

| | SKILL.md | MCP Tool | Skill Function |
|---|---|---|---|
| Execution | Local | Local/Remote | Cloud isolated |
| Model | Agent's primary | Agent's primary | Per-skill optimal |
| Context | Shared window | Shared window | Dedicated isolated |
| Security | Full local privileges | Varies | Zero local execution |
| Composition | File references | Agent mediated | Direct function calls |
| Discovery | Manual install | Manual config | MCP auto-discovery |
| Context limit | Single window | Single window | Unlimited via composition |

---

## Use Cases

### Navigation-Based RAG

Replace traditional RAG with a skill graph:

```
index-skill (holds summaries)
  ├── section-1-skill  ← full text, dedicated context
  ├── section-2-skill  ← full text, dedicated context
  └── section-N-skill  ← full text, dedicated context
```

No "lost in the middle" problem. Scales to arbitrarily large documents.

### Parallel Orchestration

```
product-launch-planner
  ├── ad-strategy      ← runs in parallel
  └── pricing-model    ← runs in parallel

→ Aggregated result returned to agent
```

Latency = max of parallel calls, not sum. Context stays clean.

### Email Routing

```
email-orchestrator (3B model)
  ├── simple-summarizer (7B)   ← routine emails
  └── complex-summarizer (70B) ← legal/multi-topic emails
```

Cost: 1/50th of using a flagship model for everything.

---

## Getting Started

**Try 50+ curated Skill Functions free:**

```bash
# Via MCP config
{
  "mcpServers": {
    "inferx-skills": {
      "url": "https://api.inferx.net/mcp"
    }
  }
}
```

Or call directly via API:

```bash
curl -X POST https://api.inferx.net/skills/saas-pricing \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"input": "B2B SaaS product, $50 ACV target"}'
```

**Import your own SKILL.md:**

Upload any SKILL.md file to InferX and it runs as a protected callable endpoint. Your skill, your property, private by default.

→ **[inferx.net](https://inferx.net)** — free to start

---

## Technical White Paper

Three papers covering the full architecture:

1. **Cloud-Native Skill as a Service** — architecture and comparison
2. **Right Model for Right Skill** — cost reduction and model routing
3. **Zero-Authority Reasoning** — security architecture

→ [Read the white paper](#) *(https://inferx.net/skill-function-whitepaper)*

---

## Contributing

This repository defines the **Skill Function** primitive and specification.

We believe Skill Function should become an open standard — the way MCP standardized tool calling and SKILL.md standardized skill format.

Contributions welcome:
- Skill Function specification improvements
- Reference implementations
- Adapters for other agent frameworks
- Security research

---

## License

MIT

---

## Contact

**prashanth velidandi** — prashanth@inferx.net 
**Twitter/X** — [@InferXai](https://x.com/InferXai)  
**Website** — [inferx.net](https://inferx.net/skill-function)

---

*The model is just the runtime. The skill is the product.*
