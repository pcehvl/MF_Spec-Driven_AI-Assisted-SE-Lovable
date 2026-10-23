# Setup Guide – MF-MyMVP-Lovable-Template

[← Back to course home](README.md)

Everything you need to set up before you build your MVP with the Lovable harness: OpenCode for the product loop, Lovable for the execution loop, and the MCP link between them.

**Contents:** [1. Before you start](#1-before-you-start) · [2. Software](#2-the-software-at-a-glance) · [3. Accounts](#3-create-your-accounts) · [4. Install OpenCode](#4-install-git-and-opencode) · [5. Lovable workspace](#5-lovable-workspace-skills-and-knowledge-once) · [6. Product repo](#6-your-product-repo-and-the-mcp-link) · [7. Lovable project](#7-your-lovable-project) · [8. Checklist](#8-ready-checklist) · [9. Security](#9-security-rules) · [10. Troubleshooting](#10-troubleshooting)

## 1. Before you start

Plan about 45 minutes. You install much less than in the main track: Lovable hosts the app, the database and the published site, so you need no Node.js, Supabase CLI or Vercel CLI on your machine.

MyMVP for Lovable is a **harness**, not a product. It has two halves:

| Half | Lives in | Contains |
| --- | --- | --- |
| Product workspace | Your GitHub repo, opened in **OpenCode** | `AGENTS.md`, the **Product Assistant** agent, three skills (`product-management`, `feedback-management`, `lovable-bridge`), the commands `/handover` and `/return`, `product/` and `docs/design-thinking/` |
| Execution workspace | Your **Lovable** workspace and project | Six Lovable skills (`receive-story`, `plan-story`, `harness-init`, `build-story`, `accept-story`, `publish`) and the workspace knowledge |

You act as Product Owner (PO) in both. The agents draft and build; you decide and accept.

**What you need**

- A laptop with macOS 13+, Windows 10/11 or a recent Linux, and a modern browser.
- A terminal you are comfortable in (Terminal on macOS, PowerShell or WSL on Windows).

**The order of work**

1. Create accounts
2. Install Git and OpenCode, and connect OpenCode to a model
3. Set up your Lovable workspace once: skills and knowledge
4. Create your product repo from the template, open it in OpenCode and sign in to Lovable's MCP
5. Create a Lovable project and run `/harness-init`
6. Tick off the ready checklist

## 2. The software at a glance

| Tool | Version | What it is for | When |
| --- | --- | --- | --- |
| Git | 2.40+ | Your product repo; the Product Assistant commits each story change | Day 1 |
| GitHub CLI (`gh`) | 2.x | Optional: create and clone your repo from the terminal | Day 1 |
| OpenCode | 2.x | Runs the Product Assistant and the Lovable MCP link | Day 1 |
| A language model provider | – | The model behind the Product Assistant | Day 1 |
| VS Code (or another editor) | latest | Reading `product.md`, specs and returned `acceptance.md` files | Day 1 |
| Lovable | web | Plans, builds, previews and publishes the app | Day 1 |

## 3. Create your accounts

| Account | Where | Cost | Notes |
| --- | --- | --- | --- |
| GitHub | [github.com/signup](https://github.com/signup) | Free | Apply for the [GitHub Student Developer Pack](https://education.github.com/pack) with your student email. |
| OpenCode | [opencode.ai](https://opencode.ai) | Free with free models | Sign in when you run `/connect` in OpenCode. Paid models are optional and at your own cost. See below. |
| Lovable | [lovable.dev](https://lovable.dev) | Free plan, or a paid plan for more credits | Every message to Lovable costs credits, including each handover. |

**Use a free model (recommended).** OpenCode gives you access to free models through OpenCode Zen, with no API key or payment needed. We recommend **MiMo-V2.6-Flash** or **Nemotron 3.5**. Free models may have rate limits, and their availability can change.

Other providers (GitHub Copilot, Anthropic, OpenAI, OpenCode Zen / Go) also work, at your own cost; set a spending limit on any API key you create.

## 4. Install Git and OpenCode

> **The harness needs OpenCode 2.** The agent and permission files use the OpenCode 2 format. If you have OpenCode 1 from npm, Homebrew or Chocolatey, uninstall it first.

### macOS

```bash
# 1. Apple command line tools (gives you git)
xcode-select --install

# 2. Homebrew – see https://brew.sh, then:
brew install git gh
brew install --cask visual-studio-code

# 3. OpenCode 2
curl -fsSL https://opencode.ai/v2/install | bash
```

### Windows

Recommended: install WSL 2 (`wsl --install -d Ubuntu` in PowerShell as Administrator), then follow the Linux block inside Ubuntu, and keep your repo under `~/code`, not `/mnt/c`.

Without WSL: `winget install Git.Git GitHub.cli Microsoft.VisualStudioCode`, and the OpenCode 2 Windows app from [opencode.ai/download](https://opencode.ai/download). Use Git Bash as your terminal.

### Linux and WSL (Ubuntu/Debian)

```bash
sudo apt update && sudo apt install -y git curl gh
curl -fsSL https://opencode.ai/v2/install | bash
```

### Check the installation

```bash
git --version        # 2.40 or newer
gh --version         # 2.x (optional)
opencode --version   # 2.x
```

### Connect OpenCode to a model

```bash
opencode
```

1. Type `/connect`, choose **OpenCode**, and sign in in the browser window that opens.
2. Type `/models` and pick **MiMo-V2.6-Flash** or **Nemotron 3.5** (both are marked Free).

## 5. Lovable workspace: skills and knowledge (once)

Do this once per Lovable workspace. Every project in the workspace then knows the harness.

**Step 1 – Download the skills.** Open the [template's releases](https://github.com/pcehvl/MF-MyMVP-Lovable-Template/releases) and download the six ZIPs from the newest release.

**Step 2 – Import them.** In Lovable, open **Workspace settings → Skills** and import each ZIP. Set **Auto-use** like this:

| Skill | Auto-use | Runs when |
| --- | --- | --- |
| `receive-story` | ON | A handover block arrives from OpenCode |
| `plan-story` | ON | You ask *"plan US-XXX"* |
| `harness-init` | OFF | You type `/harness-init` (once per project) |
| `build-story` | OFF | You type `/build-story US-XXX` |
| `accept-story` | OFF | You type `/accept-story US-XXX` |
| `publish` | OFF | You type `/publish` |

The four OFF skills must run only when you call them. Building, accepting and publishing are your decisions.

If the importer expects `SKILL.md` at the top level of the ZIP, unzip it and zip the folder's contents instead.

**Step 3 – Add the workspace knowledge.** Copy the full text of [`lovable/workspace-knowledge.md`](https://github.com/pcehvl/MF-MyMVP-Lovable-Template/blob/main/lovable/workspace-knowledge.md) and paste it into Lovable's workspace knowledge. It holds the rules every project follows: the spec is the contract, one question at a time, RLS, secrets and credits.

## 6. Your product repo and the MCP link

**Step 1 – Git identity and GitHub login (once per machine)**

```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"   # same email as GitHub
git config --global init.defaultBranch main
gh auth login && gh auth setup-git                 # optional, but makes push easy
```

**Step 2 – Create your repo from the template**

1. Open [github.com/pcehvl/MF-MyMVP-Lovable-Template](https://github.com/pcehvl/MF-MyMVP-Lovable-Template).
2. Click **Use this template → Create a new repository**. Name it after your product, for example `studymatch-product`.
3. Click **Create repository**.

Or in one command: `gh repo create studymatch-product --template pcehvl/MF-MyMVP-Lovable-Template --private --clone`

**Step 3 – Clone it and open it in OpenCode**

```bash
mkdir -p ~/code && cd ~/code
gh repo clone <your-username>/<your-repo>
cd <your-repo>
opencode
```

Inside the repo, OpenCode loads `AGENTS.md` and `opencode.json`. The **Product Assistant** is the default agent, so you talk to it directly; there is no Planner or Coder here.

**Step 4 – Sign in to Lovable's MCP server**

Type `/mcps` in OpenCode and sign in to **lovable** in the browser window that opens. The server is configured in `opencode.json` (`https://mcp.lovable.dev`).

What the link may do is restricted by the harness:

| Lovable tool | Permission | Used for |
| --- | --- | --- |
| `lovable_list_workspaces`, `lovable_list_projects`, `lovable_get_project` | allow | Finding and linking your project |
| `lovable_list_files`, `lovable_read_file` | allow | Checking a handover arrived; fetching `acceptance.md` |
| `lovable_get_message`, `lovable_list_messages` | allow | Checking a sent handover |
| `lovable_send_message` | **ask** | Sending a handover (costs credits) |
| everything else (deploy, database, create project …) | deny | Never used from OpenCode |

## 7. Your Lovable project

1. In Lovable, create a new project for your MVP.
2. Switch to **Build** mode and type `/harness-init`. Lovable copies `AGENTS.md` and `product/stories/` into the project and stops. It does not create `product.md` or any story; those arrive with the first handover.
3. The first time you run `/handover` in OpenCode, the Product Assistant asks which Lovable project belongs to your product and saves the link in `product/lovable-project.md`.

You are ready. Start with playbook step 1: put your design-thinking material in `docs/design-thinking/` and ask the Product Assistant to draft `product/product.md`. See [The two loops](workflow.md) for every step after that.

## 8. Ready checklist

**Day 1**

- [ ] GitHub account (and Student Developer Pack applied for)
- [ ] `git` and `opencode` 2 installed and showing versions
- [ ] OpenCode connected with `/connect`, **MiMo-V2.6-Flash** or **Nemotron 3.5** selected in `/models`
- [ ] Lovable account created
- [ ] Six Lovable skills imported, with Auto-use OFF for `harness-init`, `build-story`, `accept-story` and `publish`
- [ ] Workspace knowledge pasted into Lovable
- [ ] Your own product repo created from the template and cloned
- [ ] OpenCode started in the repo, with the Product Assistant as the active agent
- [ ] `/mcps` shows **lovable** as connected
- [ ] Lovable project created and `/harness-init` run in Build mode

**Before the first handover**

- [ ] `product/product.md` framed and approved by you
- [ ] One story with `status: ready`, a `revision` and `ACn` acceptance criteria

## 9. Security rules

- Never paste keys, passwords or secrets into OpenCode or the Lovable chat.
- In Lovable, secrets belong in the backend's secret storage, never in client code.
- User-owned data must be protected with Row Level Security (RLS). Ask Lovable to show the policies when you accept a story.
- Never publish while Lovable's security scan shows critical findings; `/publish` checks this.
- Read every `lovable_send_message` request before you approve it. It sends a handover and costs credits.
- Set a spending limit on any paid model API key, and keep an eye on your Lovable credits.

## 10. Troubleshooting

| Symptom | Likely cause | Fix |
| --- | --- | --- |
| `command not found` right after installing | Terminal opened before the install | Close and reopen the terminal; on Linux/WSL run `source ~/.bashrc` |
| Agents or permissions behave oddly | OpenCode 1, or started outside the repo | Check `opencode --version` is 2.x; run `opencode` from the repo folder |
| `/handover` says the Lovable tools are unavailable | Not signed in to the MCP server | Run `/mcps` and sign in to **lovable**; or use the manual fallback below |
| The handover arrives but `receive-story` does not run | Skill not imported, or Auto-use OFF | Check **Workspace settings → Skills**; `receive-story` must be ON |
| `/handover` reports that files differ after sending | Lovable changed or did not save the copy | Accept the offer to send once more; if it still differs, use the manual fallback |
| Lovable builds without you asking | `build-story` (or `accept-story`, `publish`) has Auto-use ON | Turn Auto-use OFF for those four skills |
| `/harness-init` says it cannot write files | You are in Plan mode | Switch to Build mode and run it again |
| `/build-story` refuses to start | No approved `plan.md` for the current revision, or another story in flight | Plan the story again, or accept the story in flight first |
| `/return` says the story is not accepted yet | No `acceptance.md` in Lovable | Run `/accept-story US-XXX` in Lovable first |
| Lovable wants to build something not in the spec | A new wish | Note it under "New wishes" in `/accept-story`; change the spec in OpenCode |
| Out of Lovable credits | Credit limit reached | Wait for the reset or upgrade; batch your requests |

**Manual fallback (no MCP).** Ask the Product Assistant for the handover block, copy it and paste it into the Lovable chat in Build mode. For a return, open `acceptance.md` in Lovable's Code view, copy it and paste it into OpenCode.

Still stuck? Email Per Christian Engdal at [pcen@hvl.no](mailto:pcen@hvl.no) with the exact step, the full error text and a screenshot.

## Sources

- `README.md`, `AGENTS.md`, `opencode.json`, `.opencode/` and `lovable/` in the [MF-MyMVP-Lovable-Template](https://github.com/pcehvl/MF-MyMVP-Lovable-Template) repository, and its [release notes](https://github.com/pcehvl/MF-MyMVP-Lovable-Template/releases)
- [OpenCode 2 documentation](https://opencode.ai/v2/docs)
- [Main course Setup Guide](https://pcehvl.github.io/MF_Spec-Driven_AI-Assisted-SE/setup/SETUP.html)
