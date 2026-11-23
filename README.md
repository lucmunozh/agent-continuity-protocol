# Agent Continuity Protocol

**A working protocol for using coding agents on long-running enterprise engineering work — where sessions end, context dies, and a wrong action costs real money.**

---

## The problem

Coding agents are good at tasks and bad at projects.

Inside a single session an agent holds the thread. Between sessions it holds nothing. On a one-afternoon script that does not matter. On a six-month integration project it matters enormously, because the expensive knowledge is not the code — it is everything around it:

- which hypotheses were already tested and ruled out
- which of two contradictory pieces of evidence turned out to be the real one
- which environment is safe to touch and which is not
- what "done" meant for the task you stopped in the middle of

Re-establishing that at the start of every session is the hidden tax on agent-assisted engineering. Most people pay it by pasting a long preamble and hoping. Then, eventually, the agent confidently repeats an experiment that failed three weeks ago, or re-sends a request that was never meant to be sent twice.

In enterprise systems the failure mode is worse than wasted time. The agent is operating near integration endpoints, order pipelines and live-adjacent environments, where an action is not always reversible. An agent that has forgotten the rules is an agent that will eventually break one.

## What this is

Three markdown files and a daily loop. No framework, no dependencies, no tooling to install. It works with any coding agent that can read files in the working directory.

The idea is simple: **make the agent's memory a set of files you own, instead of a context window you don't.**

| File | Answers | Changes |
|---|---|---|
| `AGENTS.md` | What are the standing rules? | Rarely |
| `CURRENT_STATE.md` | Where exactly are we right now? | Every session |
| `CONTINUITY.md` | What happened, what was decided, what is still open? | Appended, never rewritten |

The separation matters. Rules that change every session are not rules. A state file that accumulates history stops being readable. A log that gets edited stops being evidence.

## The daily loop

**Open the session with the same instruction, every time:**

> Read `AGENTS.md` and `CURRENT_STATE.md`. Confirm where we left off before making any changes.

That sentence does three things: it loads the rules, it loads the position, and — most importantly — it forces the agent to state its understanding *before* it is allowed to act. When the agent's summary is wrong, you catch it in the first thirty seconds instead of after it has edited four files on a false premise.

**Close the session by having the agent update the files:**

> Update `CURRENT_STATE.md` and append to `CONTINUITY.md`. Separate what you verified from what you inferred. List what is still open.

Both ends are cheap. The loop only works if you never skip them.

## The four rules that carry the weight

Everything else is detail. These four are what make the protocol hold up under real conditions.

### 1. Read-only by default

The agent investigates, reads, searches and proposes freely. It does not modify configuration, send requests to shared environments, or alter anything with external effects without being asked for that specific action. A session that ends with *"I did not change any configuration or re-send any requests"* is a session you can trust.

### 2. Every claim carries its evidence

A finding is not "the callback was processed." A finding is "the callback was processed — gateway log line 613, 16:44:52, and the order history response." Claims without a pointer are hypotheses, and hypotheses go in a different section than facts.

This is the rule that makes the log worth keeping. Six weeks later you will not remember whether something was observed or assumed.

### 3. Verified and inferred never share a paragraph

Agents blur the line between what they confirmed and what they concluded, and they do it fluently enough that you will not notice. Forcing the separation into the writing is the only reliable defense. In `CONTINUITY.md` they are separate sections, and nothing gets promoted from one to the other without a new piece of evidence.

### 4. One task, one entry, one number

Number your tasks and keep them sequential. It sounds bureaucratic until the first time you need to find "the thing we did about the correlation header," and it takes eight seconds instead of twenty minutes of scrolling.

## Adopting it

```bash
git clone https://github.com/<you>/agent-continuity-protocol.git
cp agent-continuity-protocol/AGENTS.md            your-project/
cp agent-continuity-protocol/templates/*.md       your-project/
```

Then:

1. Edit `AGENTS.md` — fill in your project's scope, environments and prohibitions. This is the one file that is genuinely yours; the template is a starting shape, not an answer.
2. Write the first `CURRENT_STATE.md` by hand. One paragraph is enough.
3. Start your next session with the opening instruction above.

That is the whole setup. The discipline is the product; the files are just where it lives.

## What this is not

- **Not a prompt library.** There are no clever prompts here. The opening and closing instructions are deliberately boring.
- **Not a memory system.** No embeddings, no vector store, no retrieval. Plain files you can read, diff and put in version control — which is the point.
- **Not agent-specific.** It assumes only that the agent can read and write files in the working directory.
- **Not a substitute for judgment.** The protocol makes the agent's reasoning auditable. Auditing it is still your job.

## Where this came from

This protocol was developed in production, not designed in the abstract. It emerged over roughly 265 logged agent-assisted engineering tasks across ten weeks of enterprise telecom integration work — API contract design, data mapping, integration diagnostics and test generation — in an environment where an unintended request against a shared system is not a thing you undo.

Nothing in this repository contains or derives from any client's code, data, configuration or documentation. What is published here is method.

## Contributing

Issues and pull requests are welcome, particularly reports of where the protocol broke down for you. Negative results are more useful than endorsements.

## License

MIT — see [LICENSE](LICENSE).
