# The two loops

[← Back to course home](README.md)

How one story travels from evidence to a published app: the product loop in OpenCode, the execution loop in Lovable, and the handover and return between them.

| Step | Loop | Where | You type | Skill | Result |
| --- | --- | --- | --- | --- | --- |
| 1–2 | Product | OpenCode | *"Draft product.md from docs/design-thinking"* | `product-management` | `product/product.md` |
| 3–4 | Product | OpenCode | *"Write / refine US-XXX"* | `product-management` | `spec.md`, `status: ready`, `revision: N` |
| Handover | → | OpenCode | `/handover US-XXX` | `lovable-bridge` → `receive-story` | Copies of `product.md` and `spec.md` in Lovable |
| 5 | Execution | Lovable, Plan mode | *"plan US-XXX"*, then *"save the plan"* in Build mode | `plan-story` | `plan.md`, `status: approved` |
| 6 | Execution | Lovable, Build mode | `/build-story US-XXX` | `build-story` | Working slice on the preview, `ACn` self-check table |
| 7a | Execution | Lovable | `/accept-story US-XXX` | `accept-story` | `acceptance.md`, `result: accepted` or `rejected` |
| 7b | Execution | Lovable | `/publish` | `publish` | Live app, release logged in `acceptance.md` |
| Return | ← | OpenCode | `/return US-XXX` | `lovable-bridge`, `feedback-management` | `acceptance.md` copy, `status: done` or new stories |

---

## Where things live

| Artefact | Product repo (OpenCode) | Lovable project |
| --- | --- | --- |
| `docs/design-thinking/` | **original** | – |
| `product/product.md` | **original** | read-only copy |
| `product/stories/<US-XXX-slug>/spec.md` | **original** | read-only copy |
| `product/stories/<US-XXX-slug>/plan.md` | – | **original** |
| `product/stories/<US-XXX-slug>/acceptance.md` | copy, via `/return` | **original** |
| `product/lovable-project.md` | **original** (the link) | – |
| App code, database, RLS | – | **original** |

Do not create other sources of truth: no `backlog.md`, no issue tracker. The backlog is `product/stories/`.

---

## Product loop – steps 1–4 in OpenCode

**The Product Assistant drafts, you decide as PO.** It is the default agent in the repo; it may edit only `product/**` and `docs/design-thinking/**`, and it never builds the app.

1. **Collect** – copy interview notes, personas and problem statements into `docs/design-thinking/`.
2. **Frame** – the Product Assistant drafts `product/product.md` and labels each line as evidence, interpretation, assumption, hypothesis or decision. You approve the framing.
3. **Story** – it allocates the next ID (`US-001`, `US-002`, …) and writes `product/stories/<US-XXX-slug>/spec.md`. Acceptance criteria are written as `- **ACn — Short Title.** <text>`.
4. **Ready** – one question at a time until the story passes the Ready checks. You confirm, and it sets `status: ready` and `revision: 1`.

**The Ready checks**

- [ ] `spec.md` exists, with a story ID
- [ ] Traces to evidence (design-thinking sources and the framing)
- [ ] Value and scope are clear, with an explicit out-of-scope list
- [ ] Testable acceptance criteria in the `ACn` format
- [ ] No blocking product questions left, and the PO confirms Ready

**Revisions.** Any change of meaning in a `ready` story bumps `revision` and needs a new handover. Lovable then plans again.

---

## The handover – `/handover US-XXX`

The Product Assistant checks that the story is `ready` with a `revision` and `ACn` criteria, asks you to confirm, and sends this block with `lovable_send_message`:

```text
/receive-story
=== HANDOVER US-XXX rev N ===
--- FILE product/product.md ---
<full content of product/product.md>
--- FILE product/stories/<US-XXX-slug>/spec.md ---
<full content of spec.md>
=== END HANDOVER ===
```

In Lovable, `receive-story` writes both files verbatim and reports *"Received US-XXX rev N: AC1–ACn"*. Back in OpenCode, the Product Assistant reads both files from Lovable with `lovable_read_file` and compares them with yours. Identical → *"US-XXX rev N received by Lovable"*. Different → it offers to send once more.

**Watch for:** the handover is the only thing OpenCode ever sends Lovable. It never asks Lovable to build, fix or publish.

---

## Execution loop – steps 5–7 in Lovable

**Lovable plans and builds, you control and accept as PO.** Only one story is in flight at a time.

**5. Plan** – in **Plan mode**, type *"plan US-XXX"*. `plan-story` reads the spec and the existing app, and proposes a plan of at most 200 words: approach, components to reuse, files, frontend/backend/database changes (including RLS), risks, and how each `ACn` will be self-checked. Approve it, switch to **Build mode** and say *"save the plan"*. `plan.md` gets `status: approved` and `spec_revision: N`.

**6. Build** – type `/build-story US-XXX`. Lovable builds the smallest complete vertical slice (UI, logic, data, access rules), fixes its own errors, and ends with a table:

| ACn | How it is met | Where to look in the preview |
| --- | --- | --- |

*Watch: the self-check is technical evidence, not acceptance. Deviations from the plan are reported, never applied silently.*

**7a. Accept** – type `/accept-story US-XXX`. Lovable lists what changed, the security scan status and earlier stories worth re-checking, then asks you about **one `ACn` at a time**: PASS or FAIL, and what you observed. At the end it asks for new wishes.

- All PASS and you accept → `result: accepted`.
- Any FAIL → `result: rejected` and a fix prompt that cites the failing `ACn`. Run it, then `/accept-story` again.

**7b. Publish** – type `/publish`. Lovable checks: every released story accepted, nothing half-built in flight, no critical security findings, no secrets in client code, and your explicit go-ahead. **You click Publish** and give it the live URL. It walks you through the live check and logs the release in `acceptance.md`. Rollback is Lovable's version history.

> Anything not in the spec is not built in Lovable. Lovable notes it under **New wishes** in `acceptance.md`, and it travels back with the return.

---

## The return – `/return US-XXX`

The Product Assistant reads `acceptance.md` from Lovable, saves it verbatim in your repo, and summarises the result, the `spec_revision` it was accepted against, FAIL rows, new wishes and limitations.

| Case | Next step |
| --- | --- |
| Accepted against the current `revision` | You confirm → `status: done` in `spec.md` |
| Accepted against an older revision | The current revision is not built yet → `/handover US-XXX` again |
| New wishes, observations or FAIL notes | `feedback-management` classifies them; you decide |

**Feedback categories:** defect · usability issue · changed requirement · new user need · feature idea · invalidated assumption · non-actionable.
**Possible outcomes:** revise an existing story (new `revision`) · create a new story · update `product.md` · take no action.

A `done` story stays done; a later change becomes a new story. Feedback never becomes code without a PO decision.

---

## Definition of Done

A story is **done** when every `ACn` passed on the Lovable preview for the handed-over revision, you accepted it in `/accept-story`, the security scan showed no critical findings, the return is saved in your repo, and you have confirmed `status: done`.
