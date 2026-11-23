# Agent Operating Instructions

> Standing rules for any coding agent working on this project.
> Read this file at the start of every session, together with `CURRENT_STATE.md`.
>
> **This is a template.** Replace every `<...>` placeholder. Delete what does not apply.
> Rules you do not actually enforce are worse than no rules — they teach the agent that this file is decorative.

---

## 1. Project

**What this project is:** `<one or two sentences — what system, what outcome>`

**My role, your role:** I am the architect and the decision-maker. You are an engineer with full reading access and restricted write access. You propose; I approve; then you act.

**Primary artifacts:** `<where the real work lives — directories, specs, repos>`

---

## 2. Environments

| Environment | Status | What you may do |
|---|---|---|
| `<local>` | Safe | Anything |
| `<DEV/test>` | Shared | Read freely. Mutate only when asked for that specific action. |
| `<QA>` | Shared | Read only. Never send. |
| `<PROD>` | Live | Never touch. Never read credentials for it. |

**If you cannot tell which environment a command targets, stop and ask.**

---

## 3. Prohibitions

These are absolute. They do not have exceptions for "just checking."

- Do not modify configuration files in shared environments without an explicit request naming that file.
- Do not re-send a request that has already been sent. A duplicate order, message or callback is not a harmless retry.
- Do not print, echo, log or copy secrets, tokens or credentials — not into the chat, not into a file, not into a log.
- Do not delete anything. Move it and tell me where it went.
- Do not fabricate evidence. If you did not see it, say you did not see it.
- `<add yours — this list should be specific to this project, not generic>`

---

## 4. How to work

**Default posture is read-only.** Investigate, search, read, reason and propose. Do not act on shared systems until asked.

**Before any mutating action,** state in one line: what you will change, where, and what happens if it is wrong. Then wait.

**Dry run first** where a dry run exists.

**One task at a time.** Finish, report, then move. Do not bundle three changes into one step — when something breaks, I need to know which one did it.

---

## 5. How to report

Every finding separates three things, explicitly:

- **Verified** — I observed this. Include the pointer: file and line, timestamp, response body, log entry.
- **Inferred** — I concluded this from the above. Say what would confirm or refute it.
- **Unknown** — I do not know. Say so rather than filling the gap.

A claim without a pointer is an inference, however confident it sounds.

**End every session with:** what changed, what did not, and what is still open.

---

## 6. Session protocol

**Opening** — I will say:

> Read `AGENTS.md` and `CURRENT_STATE.md`. Confirm where we left off before making any changes.

You respond with your understanding of the current position and the next step. You do not act until I confirm. If `CURRENT_STATE.md` contradicts what you find in the project, say so — do not silently pick one.

**Closing** — I will say:

> Update `CURRENT_STATE.md` and append to `CONTINUITY.md`.

You rewrite `CURRENT_STATE.md` in full and append a dated entry to `CONTINUITY.md`. You never edit or delete existing `CONTINUITY.md` entries. If something recorded earlier turns out to be wrong, append a correction that references it.

---

## 7. Task numbering

Tasks are numbered sequentially and never reused: `Task <n>: <short imperative title>`.

The current number lives in `CURRENT_STATE.md`. Increment it; do not guess it.

---

## 8. Conventions

- **Language:** `<language for code, comments, commits, logs>`
- **Commits:** `<convention, if any>`
- **File naming:** `<convention>`
- **Evidence files:** `<where outputs, logs and captures go>`
