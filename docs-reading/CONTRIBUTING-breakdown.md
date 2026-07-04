# Apache Magpie Contributing Study Notes

## File location in my study repo

Paste/place this file at:

```text
docs-reading/CONTRIBUTING-breakdown.md
```

## Why this document matters

`CONTRIBUTING.md` is not only a setup guide. It acts like the contribution rubric for Apache Magpie.

Before making any PR, I should check my change against this document because it defines:

- how Magpie treats English skill files as code
- which repo layer should be edited for each kind of change
- how skill changes must be tested with evals
- how tool contracts must stay project-agnostic
- how placeholders must be used
- how human confirmation is required before any mutation
- how privacy, sandboxing, and vendor neutrality must be respected
- how commits and PR descriptions should be written

In simple words:

```text
CONTRIBUTING.md tells me how to think before touching the repo.
```

---

## Main purpose

`CONTRIBUTING.md` is the guide for people who want to contribute to the Apache Magpie framework repository.

It explains:

- what kind of project Magpie is
- how the repository is structured
- how skills and tools are organized
- what rules every contribution must follow
- how to set up the repo locally
- how to run checks/tests
- how to open pull requests
- what first contributions are good for beginners

Simple meaning:

```text
README.md = how adopters use Magpie
MISSION.md = why Magpie exists
CONTRIBUTING.md = how contributors work on Magpie itself
```

---

# Opening Paragraph

The file starts by thanking contributors and explaining that Magpie is a generic, project-agnostic framework for agent-assisted repository maintainership.

Important points:

- Magpie is for ASF projects.
- Magpie can also be used by non-ASF open-source communities.
- It is project-agnostic.
- The deeper motivation and commitments are in `MISSION.md`.

Important instruction:

```text
Before sending a patch, read CONTRIBUTING.md end-to-end.
```

Why?

Because Magpie has strong layering rules and safety rules.

A patch can be technically correct but still hard to merge if it violates:

- project-agnostic design
- placeholder discipline
- human-in-the-loop rule
- privacy rules
- skill eval rules
- sandbox/security expectations

---

# English as code

This is the most important idea in the contributing guide.

Magpie says:

```text
English is the primary programming language here.
```

This is not just a metaphor.

In Magpie, many workflows are written as English Markdown files called skills.

These files are executed by agents.

So a `SKILL.md` file should be treated like code.

---

## Why English is treated as code

Traditional code is interpreted by a machine runtime like Python, Go, JavaScript, or Groovy.

Magpie skills are interpreted by AI coding agents.

So the interpreter is not only a compiler. The interpreter is an agent that can read natural language instructions.

Example skill instruction:

```text
Sweep the inbox since last week, classify each message against the six triage classes, and draft a confirmation reply for each message that needs one.
```

A modern agent can execute this type of workflow.

So in Magpie:

```text
English instructions + agent runtime = executable workflow
```

---

## COBOL comparison

The doc compares this to COBOL.

COBOL tried to make programming closer to plain English.

But old compilers could parse syntax, not deep meaning.

Magpie’s idea is that modern agents can now understand English instructions deeply enough for this approach to work.

Simple meaning:

```text
COBOL wanted English-like programming.
Modern agents make that idea more practical.
```

---

## SKILL.md files are programs

The contributing guide says skill files under:

```text
skills/<name>/SKILL.md
```

are programs.

They have:

- inputs
- outputs
- control flow
- error handling
- edge cases
- tests/evals

So changing a skill file is not “just editing docs.”

It is changing program behavior.

Important rule:

```text
A change to SKILL.md is a code change.
```

---

## tool.md files are also code

Tool contracts under `tools/<system>/tool.md` are read by skills at runtime.

So if you change a tool contract, you are changing the API that skills depend on.

Important rule:

```text
A change to tools/<system>/tool.md is also a code change.
```

---

## Traditional code still exists

Magpie still uses normal programming languages for deterministic work.

Examples:

- Python
- Groovy
- Shell scripts

These are used when exact output matters.

Examples:

- CVE JSON generation
- OAuth flow
- archive parsing
- dashboard rendering
- validators
- sandbox scripts

Simple distinction:

```text
English skills = judgement-heavy workflows
Python/Groovy/Shell = deterministic helper logic
```

---

# Practical consequences

The guide gives three practical consequences.

## 1. Skill changes need tests/evals

If you change a skill, run the eval suite.

You should think about:

- edge cases
- regression cases
- expected output
- boundary behavior

If you fix a bug in skill behavior, add an eval fixture like a regression test.

## 2. Tool contract changes are API changes

Changing wording in a `tool.md` can change how skills understand and use the tool.

So tool contract changes must be reviewed carefully.

## 3. Author both layers agentically

Whether you are editing:

- a skill file
- a tool contract
- Python bridge code
- docs

the recommended workflow is agentic:

```text
state intent
iterate
probe edges
test
review diff
```

The feedback differs:

```text
English skill layer → eval suite
Python layer → pytest, mypy, ruff
```

---

# What this framework is

The guide repeats Magpie’s four work streams:

1. Security-issue handling
2. Issue and PR triage/management
3. Conversational contributor mentoring
4. Development-cycle skills for contributors and committers

All four are built on a shared base:

```text
skill + tool + sandbox + HITL substrate
```

HITL means:

```text
Human-in-the-loop
```

Simple meaning:

```text
Agents help, but humans stay in control.
```

---

# Project-agnostic framework

Magpie must not contain project-specific assumptions.

Adopters configure their own project identity, rosters, canned responses, security model, release trains, etc. in their own adopter-side config.

That config does not live in the Magpie framework repo.

Important understanding:

```text
Magpie framework repo = generic reusable framework
Adopter repo/config = project-specific details
```

---

# Normative commitments

The guide says Magpie’s main principles are codified in:

```text
RFC-AI-0004
```

It lists six principles:

- HITL
- sandbox
- vendor neutrality
- conversational + correctable
- write-access discipline
- privacy by design

Very important:

```text
Every contribution must respect these principles.
```

---

# Repository layout

The repo has four main layers.

## 1. Root docs

Root docs contain cross-cutting rules.

Examples:

- `README.md`
- `MISSION.md`
- `AGENTS.md`
- `CONTRIBUTING.md`
- `docs/rfcs/`

Purpose:

```text
Define project-wide rules, vision, conventions, and governance.
```

## 2. Skills

Skills live under:

```text
skills/
```

Each skill contains a `SKILL.md`.

Skills must use placeholders like:

```text
<PROJECT>
<tracker>
<upstream>
<security-list>
```

They must not contain project-specific strings.

## 3. Tools

Tools live under:

```text
tools/
```

Each tool subtree connects Magpie skills to an external system.

Examples:

- GitHub
- Jira
- Gmail
- PonyMail
- Vulnogram
- privacy LLM
- sandbox
- validators

Each tool must be project-agnostic.

Project-specific variables are filled by adopter configuration.

## 4. Project template

Project template lives under:

```text
projects/_template/
```

This is the scaffold for adopter project configuration.

Adopters copy/fill this template in their own tracker repo.

---

# Directory tree understanding

## Root files

```text
README.md
MISSION.md
AGENTS.md
CONTRIBUTING.md
```

Meaning:

- `README.md` = overview and adoption
- `MISSION.md` = why project exists
- `AGENTS.md` = editorial and agent rules
- `CONTRIBUTING.md` = contributor workflow

## docs/

Important subfolders:

```text
docs/rfcs/
docs/setup/
docs/security/
docs/issue-management/
docs/pr-management/
docs/mentoring/
docs/modes.md
docs/mode-economics.md
```

Meaning:

- `docs/rfcs/` = normative rules
- `docs/setup/` = adoption/sandbox/privacy setup
- `docs/security/` = security workflow docs
- `docs/issue-management/` = issue skills docs
- `docs/pr-management/` = PR skills docs
- `docs/mentoring/` = mentoring docs
- `docs/modes.md` = automation mode model
- `docs/mode-economics.md` = token/cost calibration

## .claude/skills/

The doc shows `.claude/skills/` as the current reference skill location for Claude Code runtime.

Skill families include:

- security skills
- PR management skills
- issue skills
- setup skills
- contributor nomination
- write-skill
- list-skills

Note: README also talked about `.agents/skills/` as the canonical adopter path. This contributing guide is describing the framework repo’s current/reference runtime layout.

## tools/

Important tool areas:

- `tools/github/`
- `tools/jira/`
- `tools/gmail/`
- `tools/ponymail/`
- `tools/mail-source/`
- `tools/vulnogram/`
- `tools/cve-org/`
- `tools/privacy-llm/`
- `tools/agent-isolation/`
- `tools/sandbox-lint/`
- `tools/skill-and-tool-validator/`
- `tools/skill-evals/`
- `tools/dashboard-generator/`
- `tools/pr-management-stats/`
- `tools/security-tracker-stats-dashboard/`
- `tools/probe-templates/`
- `tools/dev/`

Simple meaning:

```text
skills describe workflows
tools connect those workflows to real systems
```

---

# Skill families

A skill is one workflow the agent runs end-to-end.

Each skill usually has:

```text
SKILL.md
step-detail files
helper scripts
fixtures/evals
```

The guide lists these families.

## security-*

Purpose:

```text
Full security report lifecycle
```

Examples:

- import security issue
- triage security issue
- deduplicate
- sync status
- allocate CVE
- fix issue
- invalidate issue

This is high-risk and maintainer-sensitive.

As a beginner contributor, read it carefully but avoid making deep changes early.

## pr-management-*

Purpose:

```text
Maintainer-side PR queue management
```

Examples:

- triage PRs
- code review
- mentor contributors
- PR stats

This is a good future study area for us.

## issue-*

Purpose:

```text
Issue tracker triage and fix workflow
```

Examples:

- issue triage
- issue fix workflow
- reproducer
- reassess old backlog
- stats

This is also a good area for beginner-friendly contributions.

## setup-*

Purpose:

```text
Framework adoption, sandbox install, verify, update, diagnostics, override promotion, shared config sync
```

This is very important because README adoption flow depends on setup.

We should study setup skills deeply later.

## contributor-nomination

Purpose:

```text
Build evidence brief for committer or PMC nomination.
```

This is related to contributor growth and Apache governance.

## meta-skills

Examples:

- `write-skill`
- `list-skills`

Purpose:

```text
Help create new skills and list available skills.
```

---

# Frontmatter description is router contract

Each `SKILL.md` has YAML frontmatter.

The `description` field is very important because the agent router uses it to decide whether to load a skill.

If the description is vague, the wrong skill may load or the right skill may be missed.

Important rule:

```text
Skill description must be precise.
```

The validator catches common bad description patterns.

---

# Skill anatomy

A skill directory usually contains:

## 1. SKILL.md

Main workflow file.

Contains:

- YAML frontmatter
- skill name
- description
- license
- procedure

## 2. Step-detail files

For long workflows, each major step can have its own file.

Example:

```text
step-1-...
step-2-...
step-3-...
```

## 3. Bundled scripts

Some deterministic helper scripts can be included.

These do not need an LLM.

## 4. Fixtures + evals

Skill tests live under:

```text
tools/skill-evals/evals/<skill>/
```

Each eval case may contain:

```text
input.json
expected.json
README.md
```

---

# Proposal → Confirm → Apply pattern

This is one of the most important rules.

For any mutation, skills must follow:

```text
proposal → confirm → apply
```

Meaning:

1. Agent proposes the action.
2. Agent shows the diff or intended change.
3. Maintainer explicitly confirms.
4. Agent applies the change.

This is required for anything that writes state.

Examples requiring confirmation:

- posting a comment
- sending an email
- changing a label
- writing to tracker
- modifying filesystem
- opening a PR
- sending a mailing-list post

Read-only inspection is okay without confirmation.

Simple rule:

```text
Read-only = okay without confirmation
Write/change = explicit human confirmation required
```

---

# Tool families

Tools are project-agnostic adapters to external systems.

Each tool subtree should have:

```text
tool.md or README.md
adapter surface
variables adopter must provide
implementation if needed
```

## Forge / tracker tools

Examples:

- `github/`
- `jira/`

Purpose:

```text
Connect Magpie to GitHub Issues, PRs, and Jira.
```

GitHub is full read/write.

Jira is currently read-only Groovy, with write path tracked separately.

## Mail tools

Examples:

- `gmail/`
- `ponymail/`
- `mail-source/imap/`
- `mail-source/mbox/`

Purpose:

```text
Read/write mail or read archives.
```

Gmail is full read/write/drafts.

PonyMail is read-only.

IMAP and mbox are stubs/alternate backend paths.

## CVE workflow tools

Examples:

- `vulnogram/`
- `cve-org/`

Purpose:

```text
CVE allocation and publication/checking.
```

This relates to security workflows.

## Runtime / safety tools

Examples:

- `agent-isolation/`
- `privacy-llm/`
- `sandbox-lint/`

Purpose:

```text
Sandbox, privacy checks, redaction, and safety validation.
```

## Dev loop tools

Examples:

- `skill-and-tool-validator/`
- `skill-evals/`
- `dev/`

Purpose:

```text
Validate skills, run behavioral evals, run local checks.
```

## Reporting tools

Examples:

- `dashboard-generator/`
- `pr-management-stats/`
- `security-tracker-stats-dashboard/`

Purpose:

```text
Generate dashboards for maintainers/security workflows.
```

## Authoring tools

Example:

```text
probe-templates/
```

Purpose:

```text
Boilerplate for sandbox probe scripts.
```

---

# Cross-cutting concerns

Every change must respect six principles from RFC-AI-0004.

## 1. Human-in-the-loop on every state change

Every write/action/change needs human confirmation.

Examples:

- comments
- labels
- emails
- tracker transitions
- filesystem mutations

Read-only inspection is fine.

A skill that auto-applies without confirmation is a bug.

## 2. Secure sandbox by default

Agent processes run inside a sandbox.

The sandbox uses:

- bubblewrap
- network allowlist
- tool permission rules

Skills should not assume unrestricted access to the machine.

Tools that need network access must declare required hosts.

## 3. Vendor neutrality

Skills should be Markdown + YAML, not tied to one vendor.

Do not write skills that only work with one model/provider.

Use placeholders instead of project-specific names:

```text
<PROJECT>
<tracker>
<upstream>
<security-list>
<private-list>
<default-branch>
```

## 4. Conversational, correctable

Maintainers can override skill behavior without forking the framework.

Override files live in adopter repos:

```text
.apache-magpie-overrides/<skill>.md
```

Useful overrides can later be promoted back into the framework.

## 5. Write-access discipline

Outbound messages are drafted for review.

Examples:

- PR comments
- reporter emails
- mailing-list posts

They are not sent autonomously.

## 6. Privacy by design

Private data goes only to approved LLMs.

Privacy tools include:

- redactor
- checker
- per-mailing-list gating contract

Important:

```text
Private content must not be sent to an unapproved model.
```

---

# Placeholder discipline

The guide says hardcoded project names are not allowed inside generic skills/tools.

Bad:

```text
Apache Airflow
apache/airflow
```

Good:

```text
<PROJECT>
<upstream>
```

Why?

Because Magpie must be project-agnostic.

The placeholder checker fails commits if hardcoded project names appear inside skills/tools.

---

# Agent harnesses

The current reference runtime is:

```text
Claude Code
```

But Magpie wants vendor neutrality.

Other runtimes are tracked as future/partial ports:

- Codex CLI
- Gemini CLI
- local LLMs
- Cursor
- Aider
- GitHub Copilot CLI
- Goose
- Amazon Q
- JetBrains Junie
- OpenHands

Simple understanding:

```text
Claude Code works today.
Other runtimes are part of Magpie’s vendor-neutral roadmap.
```

This is relevant to beginner issues because runtime adapters are good future contribution areas.

---

# MCP servers

MCP servers used by Claude Code runtime include:

- Slack
- Gmail
- Google Calendar
- Google Drive
- framework-internal servers

MCP-compatible harnesses may use these once skill-format adapters are ready.

MCP means tools/data exposed to agents in a standard way.

---

# Code in this repo

Although English is the primary language, deterministic code exists under `tools/`.

## Python

Python packages are managed with:

```text
uv
```

Each Python package has its own `pyproject.toml`.

The repo root has:

```text
uv.lock
```

for reproducible dependency locking.

Important Python packages include:

- CVE JSON generator
- Vulnogram OAuth helper
- Gmail OAuth helper
- skill validator
- skill evals
- sandbox linter
- privacy checker
- PII redactor

Common commands:

```bash
uv run pytest
uv run ruff check
uv run ruff format
uv run mypy
```

## skill-evals

`skill-evals` is special.

It is pure Python standard library.

No `uv` needed.

Example command:

```bash
PYTHONPATH=tools/skill-evals/src python3 -m skill_evals.runner \
    tools/skill-evals/evals/security-issue-import/
```

## Groovy

Groovy tools include:

- Jira bridge
- dashboard generator

Groovy is used for deterministic CLI tools where JVM + JSON/HTTP handling is useful.

Install Groovy 3.x or newer if working on these tools.

## Shell scripts

Shell scripts live mainly under:

- `tools/agent-isolation/`
- `tools/dev/`

They handle:

- sandbox wiring
- status line helpers
- placeholder checks
- local pre-commit checks

Shell scripts are deterministic and tested by running them.

---

# Getting set up

Required tools:

- `uv`
- `prek`
- `gh` CLI
- `groovy` if working on Groovy tools

First-time clone:

```bash
git clone git@github.com:apache/magpie.git
cd magpie
uv tool install prek
prek install
prek run --all-files
```

If hooks path conflicts:

```bash
git config --local core.hooksPath .git/hooks
prek install
```

Important:

```text
CI runs the same checks, so run them locally before PR.
```

---

# Running skills against adopter projects

If you want to run framework skills against an adopter project, you need the sandbox setup.

Use:

```text
setup-isolated-setup-install
setup-isolated-setup-verify
```

The adoption tutorial is in:

```text
docs/setup/install-recipes.md
```

We will study and reproduce this later.

---

# Lightening the agent context

Many runtime skills are not needed when editing Magpie itself.

They can crowd the agent context.

So the guide suggests disabling some skills locally using:

```text
.claude/settings.local.json
```

This file is:

- local only
- gitignored
- per-user
- per-clone

You can turn skills off/on depending on what you are working on.

Simple meaning:

```text
Disable unrelated skills locally so the agent focuses better.
```

---

# Making changes

Before editing, decide which layer the change belongs to.

## If changing purpose/scope

Edit:

```text
MISSION.md
```

## If changing normative principles

Edit:

```text
docs/rfcs/
```

## If changing editorial/confidentiality/placeholder rules

Edit:

```text
AGENTS.md
```

## If changing a skill workflow

Edit:

```text
skills/<name>/SKILL.md
```

## If changing an external adapter

Edit:

```text
tools/<system>/
```

## If changing adopter project template

Edit:

```text
projects/_template/
```

## If changing sandbox/privacy/dev-loop infra

Edit:

```text
tools/agent-isolation/
tools/privacy-llm/
tools/dev/
```

## If changing project-specific behavior

Do not edit Magpie framework repo.

That belongs in adopter project config.

---

# Important rules while making changes

## Root docs, skills, and tool adapters are project-agnostic

Do not paste concrete project names into generic files.

Use placeholders.

## Skills never mutate without user confirmation

Any new action must follow:

```text
proposal → confirm → apply
```

## Tool adapters declare adopter-side variables

If something differs per project, the tool declares the variable.

The active project config fills it in.

The adapter does not directly read project config.

The calling skill resolves variables and passes them through environment.

## Skill behavior changes require eval updates

If skill behavior changes, update the matching eval cases under:

```text
tools/skill-evals/evals/<skill>/
```

---

# Authoring with an agent

The guide says most contributions are authored agentically.

That means you work with a coding agent through conversation.

Important mindset:

```text
You are author/editor.
Agent is typist with opinions.
```

You are still responsible for correctness.

## Lead with intent, not code

Good prompt style:

```text
I want X behavior to change.
Why: Y.
Find the right file/step and propose the change.
```

Bad prompt style:

```text
Paste this code exactly here.
```

Because the agent should help reason about the correct place, edge cases, and related files.

## Iterate, do not dictate

Workflow:

```text
agent proposes
you read
you correct specifically
agent revises
tests run
repeat
```

Good correction:

```text
This needs an eval guardrail case.
```

Bad correction:

```text
Make it better.
```

Specific feedback converges faster.

## Probe boundary conditions

After a first draft, ask:

```text
What happens if field is empty?
What happens if JSON is malformed?
What happens if API returns 503?
What is the smallest input that breaks this?
What existing tool/skill behaves similarly?
```

This helps create robust changes.

## Agent is not the reviewer

Even if the agent helped write the change, you own the PR.

You are responsible for:

- scope
- diff
- commit message
- PR description
- tests
- correctness
- review responses

---

# Concrete first moves

The guide gives common contribution entry points.

## New skill

Use:

```text
/magpie-write-skill
```

This helps scaffold the skill with correct structure.

## Modify existing skill

Open `SKILL.md` in context.

Explain:

```text
what behavior should change
why it should change
```

The agent should propose diff + eval updates.

## New tool bridge

Start from:

```text
tools/<system>/tool.md
```

or the closest existing bridge.

First draft the contract surface.

Then implement.

Then document.

## Documentation change

Quote existing prose.

Say what feels wrong.

Ask the agent to propose a rewrite.

This is the fastest beginner loop.

---

# Running the dev loop

Before PR, run:

```bash
prek run --all-files
```

CI runs the same config.

The hook set includes:

- doctoc
- end-of-file fixer
- trailing whitespace
- mixed line ending check
- merge conflict check
- private key detection
- markdownlint
- typos
- placeholder checker
- Python checks
- lychee link checker

Important:

```text
A broken link or anchor can block merge.
```

## CI workflows

Main CI checks include:

- `pre-commit.yml`
- `zizmor.yml`
- lychee link check through prek
- pytest matrix

Simple meaning:

```text
Run local checks before pushing to avoid CI failures.
```

---

# Opening a pull request

## Base branch

Use:

```text
main
```

Do not open PRs against another branch unless coordinated.

## Scope

One concern per PR.

Examples:

Good:

```text
One docs clarification PR
One tool adapter PR
One skill behavior PR
```

Bad:

```text
Docs + skill refactor + new tool bridge + unrelated typo fixes in one PR
```

## Commit message

Use Conventional Commits style.

Examples:

```text
feat(skills): add missing guardrail for issue triage
fix(tools/github): handle empty PR body
docs(setup): clarify install recipe
chore(deps): bump validator dependency
```

Subject should be:

- imperative-present
- 72 characters or less
- clear

## PR description

Use:

```text
## Summary
- what changed
- why it changed

## Test plan
- how you verified it
```

## Reviews

At least one repo collaborator approval is needed.

Extra care for changes to:

- `AGENTS.md`
- RFCs
- skill files

Because these affect future behavior broadly.

## Rebase before push

Recommended flow:

```bash
git fetch origin main
git rebase origin/main
prek run --all-files
git push
```

---

# Your first contribution

Good beginner paths:

## 1. Good first issue

There are good first issues around:

- tool bridges
- runtime adapters

Important:

```text
Comment on the issue before starting so maintainers can assign it.
```

This prevents duplicate work.

## 2. Tool bridges

Examples mentioned:

- Jira write path
- Bugzilla
- IMAP/mbox wiring
- GitLab
- Mailman 3/Hyperkitty
- Discourse
- Zulip
- Matrix
- Forgejo
- OSV.dev
- Pagure

These are rewarding but harder.

## 3. Agent CLI runtime adapters

Examples:

- Codex
- Gemini
- local LLM
- Cursor
- Aider
- GitHub Copilot
- Goose
- Amazon Q
- Junie
- OpenHands

These relate to vendor neutrality.

## 4. Documentation improvements

Best beginner path.

Read a doc or skill, find a gap, fix it.

Good areas:

- `docs/setup/`
- `docs/security/`
- `docs/pr-management/`

For us, this is the best first path.

## 5. Eval fixtures

There are already many eval cases, but more edge cases are welcome.

A fixture usually includes:

```text
input.json
expected.json
README.md
```

This is easier than changing skill behavior.

## 6. New skill

Use:

```text
/magpie-write-skill
```

Better after understanding existing skills.

## 7. Tool bridge implementation

Harder but valuable.

Good if comfortable with API design.

This matches our possible long-term Bitbucket bridge direction.

---

# Confidentiality

Even though `apache/magpie` is public, many skills deal with private/security workflows at runtime.

Important rules:

- never paste real reporter PII into fixtures/docs
- never paste real security tracker URLs
- never paste pre-publication CVE URLs/IDs
- use synthetic examples
- reporter-supplied CVSS scores are not authoritative
- per-user config stays gitignored

Simple rule:

```text
Use placeholders and synthetic data in public framework docs/tests.
```

---

# Authoritative references

If docs disagree, layer-specific docs win.

Important references:

- `MISSION.md`
- `README.md`
- `AGENTS.md`
- `docs/rfcs/RFC-AI-0004.md`
- `docs/setup/`
- `skills/<name>/SKILL.md`
- `tools/<name>/`
- `tools/skill-evals/README.md`
- `tools/skill-and-tool-validator/README.md`

Simple meaning:

```text
Read the specific layer doc before changing that layer.
```

---

# Personal PR checklist from CONTRIBUTING.md

Before opening any PR, check:

- [ ] Is this change in the correct layer?
- [ ] Is the change project-agnostic?
- [ ] Did I avoid hardcoded project names like Apache Airflow or apache/airflow?
- [ ] Did I use placeholders like `<PROJECT>`, `<tracker>`, `<upstream>`, `<security-list>` where needed?
- [ ] If I changed a skill, did I treat it as code, not documentation?
- [ ] If I changed skill behavior, did I update or add eval fixtures?
- [ ] If the skill mutates anything, does it follow proposal → confirm → apply?
- [ ] If I changed a tool contract, did I treat it like an API change?
- [ ] Did I avoid real PII, real security tracker links, and private CVE details?
- [ ] Did I run `prek run --all-files`?
- [ ] If Python code changed, did I run `uv run pytest`, `ruff`, and `mypy` for that package?
- [ ] Is the PR focused on one concern only?
- [ ] Is the commit message in Conventional Commits style?
- [ ] Does the PR description include Summary and Test plan?

---

# My mental model

Magpie has two kinds of code:

```text
English code:
- skills
- tool contracts
- RFC workflows
- agent instructions

Traditional code:
- Python
- Groovy
- Shell scripts
```

Both need tests/checks.

For English code, the test style is mostly evals and validator checks.

For traditional code, the test style is pytest, ruff, mypy, shell execution, and CI hooks.

---

# Final Understanding of CONTRIBUTING.md

`CONTRIBUTING.md` tells us that Magpie contributions are different from normal repos because much of the “code” is English.

Core ideas:

```text
English skills are code.
Tool contracts are code.
Every change must remain project-agnostic.
Every state-changing action needs human confirmation.
Skills require evals.
Private data must stay private.
Agentic authoring is expected.
You still own the final PR.
```

---

# What this means for us

Best early contribution path:

```text
1. Read docs deeply.
2. Create study notes.
3. Reproduce setup.
4. Run local dev loop.
5. Pick documentation improvements.
6. Add small clarity fixes.
7. Study issue/pr-management skills.
8. Then move to tool bridge work.
```

Best first contribution types for us:

- docs clarification
- broken link fix
- terminology clarification
- setup reproduction notes
- beginner-friendly README improvement
- issue/pr-management doc improvement
- eval fixture after understanding skill behavior
- tool bridge contract review later

Avoid early:

- private security workflows
- autonomous merge changes
- RFC changes without deep discussion
- privacy/LLM routing internals
- broad refactors
- changing skill behavior without evals
