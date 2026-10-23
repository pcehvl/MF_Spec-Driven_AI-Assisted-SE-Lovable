[← MakersForge 2026 – all tracks](https://pcehvl.github.io/MakersForge-2026/)

# Beyond Vibe Coding – From Concept to MVP with Lovable

**Spec-driven, AI-assisted software engineering with agents – the Lovable track.**
Course material for the lecture by Asst. Prof. Per Christian Engdal, Western Norway University of Applied Sciences (HVL), 2026.

This track uses the same method as the [main course page](https://pcehvl.github.io/MF_Spec-Driven_AI-Assisted-SE/), but with a different harness: **OpenCode runs the product loop, Lovable runs the execution loop**, and the two are connected through Lovable's MCP server. You write specs in your own repository; Lovable plans, builds, helps you accept and publishes the app.

> **Found something that doesn't match what you see?** Let me know – email Per Christian Engdal at [pcen@hvl.no](mailto:pcen@hvl.no) with the step and a screenshot.

---

## Start here

1. **Set up** → follow the [Setup Guide](setup.md) (about 45 minutes).
2. **Create your product repo** → open [MF-MyMVP-Lovable-Template](https://github.com/pcehvl/MF-MyMVP-Lovable-Template) and click **Use this template**.
3. **Learn the loop** → read [The two loops](workflow.md), then follow the [MVP Playbook](#the-mvp-playbook-in-seven-steps) below.

---

## Course material

| What | Link | Use it for |
| --- | --- | --- |
| Setup Guide | [setup.md](setup.md) | Accounts, OpenCode, the Lovable workspace, your product repo, the MCP link and your Lovable project |
| The two loops | [workflow.md](workflow.md) | Commands, artefacts, the handover and the return, step by step |
| Template | [MF-MyMVP-Lovable-Template](https://github.com/pcehvl/MF-MyMVP-Lovable-Template) | The harness you build your own MVP with |
| Lovable skills | [Releases](https://github.com/pcehvl/MF-MyMVP-Lovable-Template/releases) | One ZIP per Lovable skill, to import into your Lovable workspace |
| Slides, notes, videos | [Main course page](https://pcehvl.github.io/MF_Spec-Driven_AI-Assisted-SE/) | The lecture itself. Parts 01–04 apply unchanged; part 05 and demos 3–4 show the local OpenCode execution loop, which Lovable replaces here. Part 06 shows the take-home harness for both tracks |

---

## What is different from the main track

| | Main track ([MF-MyMVP-Template](https://github.com/pcehvl/MF-MyMVP-Template)) | Lovable track ([MF-MyMVP-Lovable-Template](https://github.com/pcehvl/MF-MyMVP-Lovable-Template)) |
| --- | --- | --- |
| Product loop | OpenCode, Product Assistant | OpenCode, Product Assistant (unchanged) |
| Execution loop | OpenCode, Planner and Coder, on your machine | Lovable, driven by you in the Lovable chat |
| Link between the loops | Same repository | Lovable MCP: `/handover` and `/return` |
| App code and database | Your repo, Supabase | Your Lovable project and its backend |
| Quality gate | `npm run verify`, Pull Request | Self-check per `ACn`, `/accept-story` on the preview, security scan |
| Deploy | Vercel CLI | **Publish** in Lovable, via `/publish` |
| Installed locally | Git, gh, Node.js, OpenCode, Supabase CLI, Vercel CLI | Git and OpenCode |

The rule is the same in both: **the spec is the contract**. Lovable builds only what a handed-over `spec.md` says, and every change of behaviour starts as a spec change in your product repo.

---

## The two loops

```text
PRODUCT LOOP (your repo, OpenCode)
Evidence → Framing → Backlog → Story → Refinement → ready (rev N)
  → /handover US-XXX ─────────────────────────────┐
                                                  ▼
EXECUTION LOOP (Lovable, driven by the PO in the Lovable chat)
receive → plan → approve → build → accept on preview → publish
  ┌───────────────────────────────────────────────┘
  ▼
/return US-XXX → done, or feedback → Story revision or new Story
```

Only two things cross between the loops: a **handover** (`product.md` + one ready `spec.md`) and a **return** (that Story's `acceptance.md`). Details in [The two loops](workflow.md).

---

## The MVP playbook in seven steps

One story at a time, through both loops. The same seven steps as in the main track; only the tools in the execution loop differ.

**Product loop – in OpenCode. The Product Assistant drafts, you decide as Product Owner**

1. **Collect** – add your design-thinking material to `docs/design-thinking/`.
2. **Frame** – the Product Assistant drafts `product/product.md`; you decide as PO.
3. **Story** – the Product Assistant writes one `spec.md` with testable acceptance criteria (`ACn`).
4. **Ready** – refine one question at a time until the spec is `ready` with a `revision`, then run `/handover US-XXX`.

**Execution loop – in Lovable. Lovable plans and builds, you control and accept**

5. **Plan** – in Plan mode, ask *"plan US-XXX"*; approve the plan and say *"save the plan"* in Build mode.
6. **Build** – run `/build-story US-XXX`. Lovable builds the smallest vertical slice and self-checks each `ACn` on the preview.
7. **Accept, publish, learn** – run `/accept-story US-XXX` and answer PASS or FAIL per `ACn`, then `/publish`. Back in OpenCode, run `/return US-XXX`: the story becomes `done`, and feedback becomes new or revised stories.

> Generated code alone is never Done. The PO accepts in Lovable, and the Story is marked `done` in the product repo only after the return.

---

## Your take-home harness

A product repo in OpenCode, the app in Lovable. Start from [MF-MyMVP-Lovable-Template](https://github.com/pcehvl/MF-MyMVP-Lovable-Template).

```text
your-mvp/
├─ AGENTS.md · opencode.json
├─ product/
│  ├─ product.md
│  └─ stories/US-XXX-<slug>/   spec.md · acceptance.md
├─ docs/
│  └─ design-thinking/
├─ .opencode/                  agents/ · skills/
└─ lovable/
   ├─ skills/
   └─ workspace-knowledge.md
```

App code lives in the Lovable project, not in this repo.

| Where | Loop | What it holds |
| --- | --- | --- |
| `product/` | Product (shared) | `product.md` and one folder per story: `spec.md` goes out, `acceptance.md` comes back. Plan and verification stay in Lovable |
| `.opencode/` | Product, in OpenCode | The Product Assistant drafts the specs. `/handover` sends `spec.md` to Lovable, `/return` brings `acceptance.md` back |
| `lovable/skills/` | Execution, in Lovable | `/harness-init`, `/build-story`, `/accept-story` and `/publish`, imported once per workspace |
| `lovable/workspace-knowledge.md` | Rules for Lovable | Pasted into workspace knowledge, so Lovable follows the same rules. It builds and checks each `ACn` on the preview |

---

## Getting help

- Check **Troubleshooting** at the end of the [Setup Guide](setup.md#10-troubleshooting) first.
- Still stuck? Email Per Christian Engdal at [pcen@hvl.no](mailto:pcen@hvl.no) with the exact step, the full error text and a screenshot of the Lovable chat.
