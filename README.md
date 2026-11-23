# Agent Continuity Protocol

**A working protocol for using coding agents on long-running enterprise engineering work — where sessions end, context dies, and a wrong action costs real money.**

---

## Why this exists

I lost two months of work in a single afternoon.

Not the code — the code was in git. What I lost was everything around it: which approaches we had already tried and ruled out, why a particular design decision was made, which of two contradictory pieces of evidence turned out to be the real one, and the procedure we had painstakingly worked out for getting a build deployed to a stubborn environment.

All of it lived in one long agent conversation. The conversation broke. The knowledge went with it.

That was the expensive lesson: **the chat cannot be the project's memory.** Not because the tooling is bad, but because a conversation is the wrong container for knowledge that has to survive longer than the conversation does.

## The problem

Coding agents are good at tasks and bad at projects.

Inside a single session an agent holds the thread. Between sessions it holds nothing. On a one-afternoon script that does not matter. On a six-month integration project it matters enormously, because the expensive knowledge is not the code:

- which hypotheses were already tested and ruled out
- why the design is the way it is, and what was rejected to get there
- which procedures actually work in this environment, as opposed to the documented ones
- which of two contradictory observations is the real one
- what "done" meant for the task you stopped in the middle of

Re-establishing that at the start of every session is the hidden tax on agent-assisted engineering. Most people pay it by pasting a long preamble and hoping. Then, eventually, the agent confidently repeats an experiment that failed three weeks ago, or re-sends a request that was never meant to be sent twice.

In enterprise systems the failure mode is worse than wasted time. The agent is operating near integration endpoints, order pipelines and live-adjacent environments, where an action is not always reversible. An agent that has forgotten the rules is an agent that will eventually break one.

## What this is

Five markdown files and a daily loop. No framework, no dependencies, no tooling to install. It works with any coding agent that can read files in the working directory.

The idea is simple: **make the agent's memory a set of files you own, instead of a context window you don't.**

| File | Answers | Shape |
|---|---|---|
| `AGENTS.md` | What are the standing rules? | Rarely changes |
| `CURRENT_STATE.md` | Where exactly are we right now? | Rewritten every session |
| `DECISIONS.md` | What did we decide, and why? | Appended; rarely revised |
| `OPERATIONS.md` | How do I actually do things here? | Grows as procedures are proven |
| `CONTINUITY.md` | What happened, with what evidence? | Append-only, never edited |

The separation matters, and it took a while to get right. The first version of this had three files, and `CURRENT_STATE.md` kept swelling because decisions and procedures had nowhere else to go. Splitting them out is what made the state file stay readable.

Each file fails in a specific way when merged with another:

- **Decisions in the log** get buried. Three months later you need *why*, and you are scrolling through two hundred dated entries to find it.
- **Procedures in the state file** make it unreadable. State should be one screen; a working deployment procedure is a page on its own.
- **State in the log** rots. A snapshot that accumulates history stops being a snapshot.
- **A log that gets edited** stops being evidence, which is the only reason to keep it.

## The daily loop

**Open the session with the same instruction, every time:**

> Read `AGENTS.md` and `CURRENT_STATE.md`. Confirm where we left off before making any changes.

That sentence does three things: it loads the rules, it loads the position, and — most importantly — it forces the agent to state its understanding *before* it is allowed to act. When the agent's summary is wrong, you catch it in the first thirty seconds instead of after it has edited four files on a false premise.

**Close the session, or any milestone, with:**

> Save a continuity point: changes made, evidence, decisions, open items and next step. Separate what you verified from what you inferred.

Both ends are cheap. The loop only works if you never skip them.

Do not wait for the end of the day. A deployment, a significant fix, or a surprising result is a continuity point. The two-month loss happened because there was nothing to save to.

## The four rules that carry the weight

Everything else is detail. These four are what make the protocol hold up under real conditions.

### 1. Read-only by default

The agent investigates, reads, searches and proposes freely. It does not modify configuration, send requests to shared environments, or alter anything with external effects without being asked for that specific action.

A session that ends with *"I backed up both files before modifying them. I did not change the server, did not restart the application, and did not disable TLS"* is a session you can trust. That sentence is not politeness — it is the deliverable.

### 2. Every claim carries its evidence

A finding is not "the callback was processed." A finding is "the callback was processed — gateway log line 613, 16:44:52, and the order history response." Claims without a pointer are hypotheses, and hypotheses go in a different section than facts.

This is the rule that makes the log worth keeping. Six weeks later you will not remember whether something was observed or assumed.

### 3. Verified and inferred never share a paragraph

Agents blur the line between what they confirmed and what they concluded, and they do it fluently enough that you will not notice. Forcing the separation into the writing is the only reliable defense.

The corollary is the hardest habit to build: **a problem is not closed until it is confirmed in the place that matters.** The fix compiling is not the fix working. The log line appearing is not the user-facing behavior being correct. *"That confirmation is still missing before we call this closed"* is a complete and acceptable end to a session.

### 4. One task, one entry, one number

Number your tasks and keep them sequential. It sounds bureaucratic until the first time you need to find "the thing we did about the correlation header," and it takes eight seconds instead of twenty minutes of scrolling.

## Adopting it

```bash
git clone https://github.com/lucmunozh/agent-continuity-protocol.git
cp agent-continuity-protocol/AGENTS.md      your-project/
cp agent-continuity-protocol/templates/*.md your-project/
```

Then:

1. Edit `AGENTS.md` — fill in your project's scope, environments and prohibitions. This is the one file that is genuinely yours; the template is a starting shape, not an answer.
2. Write the first `CURRENT_STATE.md` by hand. One paragraph is enough.
3. Leave `DECISIONS.md`, `OPERATIONS.md` and `CONTINUITY.md` empty. They fill themselves as you work.
4. Start your next session with the opening instruction above.

That is the whole setup. The discipline is the product; the files are just where it lives.

## What this is not

- **Not a prompt library.** There are no clever prompts here. The opening and closing instructions are deliberately boring.
- **Not a memory system.** No embeddings, no vector store, no retrieval. Plain files you can read, diff and put in version control — which is the point.
- **Not agent-specific.** It assumes only that the agent can read and write files in the working directory.
- **Not a substitute for judgment.** The protocol makes the agent's reasoning auditable. Auditing it is still your job.

## Where this came from

This protocol was developed in production, not designed in the abstract. It emerged over roughly 450 logged agent-assisted engineering tasks across four months of enterprise telecom integration work — API contract design, data mapping, integration diagnostics and test generation — in an environment where an unintended request against a shared system is not a thing you undo.

It started as three files after the loss described above, and grew to five as the gaps showed themselves.

Nothing in this repository contains or derives from any client's code, data, configuration or documentation. What is published here is method.

## Contributing

Issues and pull requests are welcome, particularly reports of where the protocol broke down for you. Negative results are more useful than endorsements.

## License

MIT — see [LICENSE](LICENSE).
