# Domain docs

Domain documentation layout for this repo.

<!-- Layout: single-context (CONTEXT.md + docs/adr/ at repo root) -->

## Layout

This repo uses a **single-context** layout:

```
CONTEXT.md          # domain language for the whole repo
docs/adr/           # architectural decision records
```

These docs use the domain's language. When you read code, prefer the terms defined here over ad-hoc naming.

## Architectural decisions

Past architectural decisions are recorded as ADRs under:

```
docs/adr/
```

Each ADR captures one decision: its context, the decision itself, and its consequences. Consult them before reversing or contradicting a past decision.

## Consumer rules

- `improve-codebase-architecture` reads `CONTEXT.md` for domain language and `docs/adr/` for past decisions.
- `diagnose` reads `CONTEXT.md` to use correct domain terms when describing bugs.
- `tdd` reads `CONTEXT.md` to name tests and assertions in domain language.

When a decision changes the domain language, update `CONTEXT.md`. When a decision is architectural, add an ADR.
