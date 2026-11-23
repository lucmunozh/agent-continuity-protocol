# Operations

> Procedures that are **proven to work in this environment** — not the ones in the vendor docs.
>
> The gap between documented and actual is where days disappear. Every time you work out how
> to do something the hard way, it goes here, in enough detail that it works the second time
> without the rediscovery.
>
> Rule for this file: a procedure is only written down **after it has worked end to end**.
> An untested procedure here is worse than no procedure, because the next reader trusts it.
>
> Never put credentials, tokens, keystores or private keys in this file. Reference where they
> live, not what they are.

---

## OP-`<n>` — `<Short imperative title: "Deploy a build to <env>">`

**Last verified:** `<YYYY-MM-DD>` · **Environment:** `<which one>`

### When you need this

`<The situation that sends someone to this entry.>`

### Prerequisites

- `<Access, tooling, configuration that must already be in place>`

### Steps

1. `<Step, concrete enough to follow without thinking>`
2. `<...>`

### How you know it worked

`<The observable confirmation — the status code, the log line, the UI state. Not "it should work".>`
`<If you cannot name the confirmation, the procedure is not proven yet.>`

### Known failure modes

| Symptom | Cause | Fix |
|---|---|---|
| `<what you see>` | `<what it actually is>` | `<what to do>` |

### Notes

`<Anything surprising. Especially where the official documentation is wrong or incomplete —>`
`<that is exactly the knowledge that is expensive to rediscover and impossible to search for.>`

---

## OP-`<n-1>` — `<...>`

`<...>`
