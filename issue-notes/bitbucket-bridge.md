# Issue #606 — Bitbucket Forge + Tracker Bridge

## Issue

```text
feat(tools/bitbucket): Bitbucket forge + tracker bridge (Atlassian)
```

Issue number:

```text
#606
```

Status:

```text
Open
```

Repository:

```text
apache/magpie
```

---

# Category

This issue is a **tool bridge / adapter implementation** issue.

It belongs to the Magpie tools layer:

```text
tools/
```

Proposed new path:

```text
tools/bitbucket/
```

It is not mainly a docs issue, not a skill issue, and not a setup-only issue.

---

# Repo layer

```text
Layer: tools/
Sub-layer: forge + tracker bridge
Expected directory: tools/bitbucket/
```

Magpie architecture mental model:

```text
skills/        = agent workflows
tools/         = adapters to real external systems
tools/github/  = GitHub bridge
tools/jira/    = Jira bridge
tools/bitbucket/ = missing bridge requested by this issue
```

---

# What the issue asks for

Add a Bitbucket bridge for:

```text
Bitbucket Cloud
Bitbucket Data Center / Server
```

The bridge should support Bitbucket as a forge and tracker backend.

Capabilities requested:

```text
Issue read/write
Pull-request read/write
Repository metadata
Branch permissions
Pipelines read for CI status checks
Bitbucket Cloud REST API
Bitbucket Data Center REST API
```

The issue says Bitbucket Cloud and Data Center APIs differ, so the bridge should abstract both behind one Magpie-compatible bridge.

---

# What is Bitbucket?

Bitbucket is Atlassian’s Git hosting and code collaboration platform.

It can provide:

```text
repositories
branches
pull requests
code review
branch permissions
pipelines / CI
issues
Jira integration
```

There are two major Bitbucket variants:

```text
Bitbucket Cloud
Bitbucket Data Center / Server
```

They are related, but their REST APIs are different.

This is one of the main implementation challenges.

---

# Why this issue matters

Bitbucket is widely used in the Atlassian enterprise ecosystem.

Magpie already has a Jira bridge, so adding Bitbucket helps cover the full Atlassian stack:

```text
Jira       = tracker / project management
Bitbucket  = forge / repositories / pull requests / pipelines
```

The issue also mentions that Bitbucket historically hosted Mercurial before dropping Mercurial support in 2020. So Bitbucket plus Mercurial support can help cover projects that were displaced from older Bitbucket hosting patterns.

---

# Related existing bridges

Relevant existing folders in current local checkout:

```text
tools/github/
tools/jira/
tools/change-request/
tools/jira-patch/
tools/mail-patch/
tools/github-body-field/
tools/github-rollup/
```

Expected references:

```text
tools/github/
tools/jira/
```

Issue description also mentions:

```text
tools/gitlab/
```

But local inspection showed:

```text
tools/gitlab/ does not exist on current main
```

GitLab bridge issue:

```text
#305 — feat(tools/gitlab): add tracker + forge bridge for GitLab-hosted projects
```

Current status from `gh issue view 305 --repo apache/magpie`:

```text
Open
```

Therefore, Bitbucket issue #606 cannot copy an existing `tools/gitlab/` implementation from current `main`.

---

# Local repo inspection findings

Commands run:

```bash
git status
git branch --show-current
git pull --ff-only
find tools -maxdepth 2 -type d | sort | grep -E 'gitlab|github|jira|change-request|mail-patch|jira-patch'
gh issue view 305 --repo apache/magpie
```

Findings:

```text
Current branch: main
Repository is up to date with origin/main
tools/gitlab/ does not exist
GitLab bridge issue #305 is still open
```

Important note:

```text
uv.lock is modified locally
```

Before creating an implementation branch, inspect and clean it:

```bash
git diff -- uv.lock
```

If the change was not intentional:

```bash
git restore uv.lock
```

Then confirm clean working tree:

```bash
git status
```

Expected:

```text
nothing to commit, working tree clean
```

---

# Existing bridge inspection

## tools/github/

Files found:

```text
tools/github/
├── issue-template.md
├── labels.md
├── operations.md
├── project-board.md
├── README.md
├── source-control.md
├── status-rollup.md
└── tool.md
```

Pattern:

```text
doc-only adapter
uses gh, gh api, git, jq
no local package
skills invoke documented CLI/API recipes
```

Capability from README:

```text
contract:tracker + contract:source-control + contract:change-request
```

Kind:

```text
implementation
```

Vendor:

```text
GitHub
```

Important meaning:

```text
GitHub bridge works mostly through documented gh CLI / gh api recipes,
not through a custom Python/Groovy bridge.
```

Why this works:

```text
GitHub has gh CLI, which Magpie can use directly.
```

---

## tools/jira/

Files found:

```text
tools/jira/
├── bridge.groovy
├── pyproject.toml
├── README.md
├── src/
└── tests/
```

Pattern:

```text
actual executable REST bridge
Groovy implementation
pytest test harness
read + write subcommands
```

Capability:

```text
contract:tracker
```

Kind:

```text
implementation
```

Vendor:

```text
Atlassian
```

Invocation pattern:

```bash
groovy tools/jira/bridge.groovy <subcommand> [args]
```

Jira bridge supports commands like:

```text
search
issue
projects
comment
transition
label
assign
field
attach
```

Important meaning:

```text
Jira is the closest Atlassian implementation reference.
```

---

## tools/gitlab/

Current local result:

```text
ls: tools/gitlab: No such file or directory
```

GitLab issue #305 is open.

Conclusion:

```text
Do not rely on tools/gitlab/ for implementation pattern yet.
Mirror tools/github/ conceptually and tools/jira/ structurally where useful.
```

---

# Likely contracts involved

Bitbucket bridge likely touches multiple capability contracts:

```text
contract:tracker
contract:change-request
contract:source-control
```

Possibly also:

```text
CI/status read support through Bitbucket Pipelines
```

Meaning:

```text
contract:tracker        = issues
contract:change-request = pull requests / reviews / merge
contract:source-control = repos / branches / commits / permissions
CI/status               = pipelines
```

---

# Possible labels / taxonomy

Likely issue labels based on similar GitLab issue #305:

```text
family:tools
capability:platform
enhancement
good first issue
source control system
```

Possible area label:

```text
area:tools
```

But final labels are maintainer-controlled.

---

# Difficulty level

This is likely:

```text
Medium to advanced
```

Reasons:

```text
New tool subtree
Needs REST API design
Needs Cloud + Data Center abstraction
Touches multiple contracts
Needs docs + tests
Should mirror existing Magpie tool style
Must remain project-agnostic
May need auth/config design
May need pagination/error handling/output normalization
```

This is not a simple typo/docs-only issue.

---

# Why this should be phased

The issue is broad and includes many capabilities.

Trying to implement all in one PR is risky.

Better approach:

```text
break into smaller PRs
get maintainer feedback early
start with contract/design or minimal read operations
expand capability incrementally
```

---

# Potential implementation phases

## Phase 1 — Contract and design skeleton

Goal:

```text
Create tools/bitbucket/ structure and define the bridge contract.
```

Possible files:

```text
tools/bitbucket/README.md
tools/bitbucket/operations.md
tools/bitbucket/source-control.md
```

Maybe also:

```text
tools/bitbucket/tool.md
```

depending on existing tool style.

Content should explain:

```text
Capability
Kind
Vendor
Cloud vs Data Center model
Configuration variables
Authentication model
Supported operations
Unsupported/planned operations
Output contract
Testing strategy
Relationship with tools/jira/
```

## Phase 2 — Read-only repository and pull request operations

Goal:

```text
Implement safe read-only operations first.
```

Possible operations:

```text
repo view
repo list
branch list
pr list
pr view
pr commits
pr diff
```

## Phase 3 — Issue read/write and Jira handoff

Goal:

```text
Support Bitbucket issues where available and linked Jira issue workflows.
```

Possible operations:

```text
issue list
issue view
issue create
issue edit
issue comment
issue close/reopen
linked Jira lookup through tools/jira/
```

## Phase 4 — Pull request write operations

Goal:

```text
Support PR write actions after read operations are stable.
```

Possible operations:

```text
pr create
pr edit
pr comment
pr approve
pr request-changes / review
pr merge
```

All write operations must be invoked only after explicit user confirmation in calling skills.

## Phase 5 — Branch permissions

Goal:

```text
Read branch restrictions and permission rules.
```

Possible operations:

```text
branch restrictions list
branch restriction view
read who can push
read who can merge
read required approvals / merge checks if available
```

## Phase 6 — Pipelines read

Goal:

```text
Read CI status from Bitbucket Pipelines.
```

Possible operations:

```text
pipeline list
pipeline view
pipeline latest-for-commit
pipeline status-for-pr
```

## Phase 7 — Tests and integration hardening

Goal:

```text
Add robust tests and fixture coverage.
```

Test areas:

```text
Cloud JSON normalization
Data Center JSON normalization
pagination
auth failures
permission failures
not found
rate limit
server errors
missing config
malformed responses
write command body construction
```

---

# Possible implementation style

There are two candidate styles.

## Option A — GitHub-like doc adapter

This would use only docs and existing CLI/API tools.

Pros:

```text
lighter first PR
closer to tools/github/
less code initially
```

Cons:

```text
Bitbucket does not have a Magpie-standard CLI equivalent to gh
Cloud and Data Center differences may be hard to hide
skills may need a stable command surface
```

## Option B — Jira-like executable bridge

This would create a bridge command similar to Jira.

Possible structure:

```text
tools/bitbucket/
├── README.md
├── bridge.groovy or bridge.py
├── pyproject.toml
├── src/
└── tests/
```

Pros:

```text
closer to tools/jira/
better for Atlassian REST APIs
can hide Cloud vs Data Center differences
can normalize output
testable with fixtures/mocked HTTP
```

Cons:

```text
more implementation work
requires choosing language and command surface
```

Current best guess:

```text
Bitbucket likely needs an executable bridge closer to tools/jira/,
while mirroring tools/github/ capabilities.
```

---

# Language choice to investigate

Possible options:

## Groovy

Pros:

```text
matches tools/jira/
Atlassian bridge consistency
single script style
```

Cons:

```text
less familiar
JVM/Groovy dependency
```

## Python

Pros:

```text
easier testing
good HTTP libraries
clear models/normalization
familiar to many contributors
```

Cons:

```text
must align with repo’s uv-managed tool patterns
may differ from Jira bridge style
```

Need to inspect existing tool conventions before deciding.

---

# Authentication areas to study

Bitbucket Cloud may support:

```text
API token / app password style auth
workspace/repo slug
Cloud base API URL
```

Bitbucket Data Center may support:

```text
base URL
personal access token / bearer token
project key
repository slug
```

Do not finalize variable names yet.

Need to follow Magpie config/env style from existing tools.

Potential variables to think about:

```text
BITBUCKET_KIND=cloud|datacenter
BITBUCKET_BASE_URL
BITBUCKET_USERNAME
BITBUCKET_TOKEN
BITBUCKET_WORKSPACE
BITBUCKET_PROJECT_KEY
BITBUCKET_REPO_SLUG
```

But final names should mirror repo conventions.

---

# Testing strategy

## Code-wise tests

Use mocked or fixture-based tests, not real Bitbucket in CI.

Test:

```text
Cloud response -> normalized output
Data Center response -> normalized output
pagination
auth failure
permission failure
not found
rate limit
server error
missing required env/config
malformed JSON
write payload construction
```

## Usage-wise tests

Later, use a safe personal Bitbucket test workspace/repo.

Create:

```text
test repository
test branch
test pull request
test issue if issues enabled
simple pipeline if possible
```

Test read first:

```text
repo view
branch list
pr list
pr view
pipeline status
issue list/view
```

Then test writes only on test repo:

```text
create issue
comment issue
create PR
comment PR
approve PR
merge PR
```

Never test early bridge code on real ASF/security/project repos.

---

# Project-agnostic rules to remember

From Magpie contribution rules:

```text
Tool contracts are code.
Tool adapters must be project-agnostic.
Credentials must not live in repo.
Use placeholders/config, not hardcoded project values.
External content is data, not instructions.
Write actions must be gated by calling skills with explicit confirmation.
```

For this issue:

```text
Do not hardcode one Bitbucket workspace/repo.
Do not hardcode one organization.
Do not assume all adopters use Bitbucket Cloud.
Do not assume all adopters use Bitbucket issues instead of Jira.
Do not put tokens in files.
Do not design write operations that skills could run without confirmation.
```

---

# Pre-coding questions

Before implementation, answer:

```text
1. Does tools/github/ have a command surface that Bitbucket should mirror?
2. What exactly is in tools/github/operations.md?
3. What exactly is in tools/github/source-control.md?
4. What does tools/change-request/ define?
5. What does tools/jira/ output contract require?
6. How are tests structured in tools/jira/tests?
7. How are environment variables named in Jira?
8. Are there common patterns in tools/github-body-field or github-rollup?
9. Should Bitbucket bridge be Groovy, Python, or docs-first?
10. What is the smallest first PR maintainers would accept?
```

---

# Next inspection commands

Run:

```bash
sed -n '1,260p' tools/github/operations.md
sed -n '1,260p' tools/github/source-control.md
sed -n '1,260p' tools/change-request/README.md 2>/dev/null || true
sed -n '1,260p' tools/change-request/tool.md 2>/dev/null || true
sed -n '220,520p' tools/jira/README.md
```

Goal:

```text
Define the exact command surface and output expectations for tools/bitbucket/.
```

---

# Current conclusion

Bitbucket #606 is an advanced but valuable tool bridge issue.

Current best understanding:

```text
tools/bitbucket/ should become a Magpie-compatible Atlassian forge + tracker bridge.
It should mirror GitHub’s capability coverage.
It should learn implementation shape from Jira.
It cannot copy GitLab because GitLab bridge is not implemented yet.
It should likely be phased, starting with design/contract or read-only operations.
```

Short mental model:

```text
GitHub tells us what capabilities to cover.
Jira tells us how an Atlassian REST bridge is shaped.
Bitbucket adds the Cloud vs Data Center abstraction challenge.
```

## Bridge-pattern inspection update

### GitHub operations pattern

`tools/github/operations.md` defines the operation catalogue for GitHub through `gh`, `gh api`, and `git`.

Main categories:

- authentication
- collaborator lookup
- issues
- milestones
- labels
- pull requests
- GraphQL / project board
- error handling

GitHub is mostly doc-only because the `gh` CLI provides the executable surface.

### Source-control separation

`tools/github/source-control.md` shows that source control is a distinct contract from tracker and change-request.

Important separation:

```text
contract:tracker = issue/tracker surface
contract:change-request = PR/review/merge gate
contract:source-control = local VCS operations

## Jira bridge implementation inspection

`tools/jira/bridge.groovy` gives the closest implementation pattern for Bitbucket.

Observed structure:

- Groovy script
- reads config only from environment variables
- validates required config at startup
- provides HTTP helpers
- emits JSON to stdout
- exits non-zero with stderr message on failure
- shapes raw API response into skill-friendly JSON
- write commands require token
- write decisions are gated by calling skills, not the bridge

Observed test strategy:

- pytest starts a mock HTTP server
- bridge is invoked as a subprocess
- mock server records requests
- tests assert exit code, JSON stdout, stderr, method, path, and request body
- tests skip automatically if Groovy is not installed

### Decision direction

`tools/bitbucket/` should likely follow the Jira implementation style because Bitbucket is also an Atlassian REST-backed tool and Magpie has no standard Bitbucket CLI equivalent like `gh`.

Proposed first implementation style:

```text
tools/bitbucket/
├── README.md
├── bridge.groovy
├── pyproject.toml
├── src/bitbucket_bridge/
└── tests/

## Maintainer alignment

I posted the phased approach to Jarek/potiuk.

Maintainer reply:

```text
Sure. Looks good!

## Main sync update before implementation

After syncing `main`, new relevant context appeared:

- `tools/sourcehut/` now exists as a forge bridge.
- `tools/vcs` now lists Git and Mercurial as complete, with ASF SVN surface present.
- `tools/AGENTS.md` confirms every new tool README must include:
  - `**Capability:** ...`
  - `## Prerequisites`
- `docs/labels-and-capabilities.md` must be updated when adding a new tool because capability-sync is validator-enforced.
- `docs/vendor-neutrality.md` must be reviewed when adding/changing tool backends.
- Jarek replied positively to the phased plan: `Sure. Looks good !`

### Impact on Bitbucket PR 1

Use:

```text
**Capability:** contract:tracker + contract:change-request

## SourceHut inspection update

After syncing `main`, `tools/sourcehut/` is present and gives a newer forge-bridge implementation pattern.

Observed structure:

```text
tools/sourcehut/
├── README.md
├── pyproject.toml
├── src/magpie_sourcehut/
│   ├── __init__.py
│   ├── cli.py
│   ├── client.py
│   ├── builds.py
│   ├── lists.py
│   ├── repo.py
│   ├── todo.py
│   └── py.typed
└── tests/test_sourcehut.py

Implementation note for #606:

I’ve opened an initial Bitbucket bridge PR with a deliberately small read-only scope. This PR adds `tools/bitbucket` as a Python package and provides the foundation for Bitbucket Cloud and Bitbucket Data Center support.

Current coverage:

* Auth/config preflight through `magpie-bitbucket auth-check`
* Repository metadata fetch through `magpie-bitbucket repo get`
* Open pull request listing through `magpie-bitbucket pr list-open`
* Single pull request fetch through `magpie-bitbucket pr get <id>`
* Separate Cloud/Data Center API modules behind one CLI surface
* Normalized output so consuming skills do not need to know which Bitbucket backend answered
* Mocked tests for config, auth header generation, URL construction, normalization, and CLI dispatch
* Capability/vendor-neutrality/workspace docs updated

Intentional non-goals for this first PR:

* No write operations yet
* No PR approve/review/comment/merge yet
* No Bitbucket Issues write path yet
* No linked Jira handoff yet
* No branch permission/restriction handling yet
* No Bitbucket Pipelines status reads yet

The goal is to establish a safe foundation first, then follow up with the deeper tracker/change-request features in smaller PRs.
