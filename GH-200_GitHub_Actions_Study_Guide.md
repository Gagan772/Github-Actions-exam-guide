# GH-200 — GitHub Actions Certification Study Guide

> **Version:** 2026-02 · **Exam code:** GH-200 · **Passing score:** 70 %  
> **Format:** 60 questions · 120 minutes · Proctored online

---

## How to Use This Guide

1. **Read linearly the first time** — each domain builds on the previous one.
2. **Run every YAML snippet** in a throw-away repo so the syntax sticks.
3. **Mark pitfalls with a highlighter** — they are the exam's favourite traps.
4. **Answer every mini-quiz before scrolling** — active recall beats re-reading.
5. **Use the Quick Sheet** below as your final-hour cheat sheet.
6. **Take the 40-question practice exam** at the end under timed conditions (60 min).
7. **Revisit any section where you score < 70 %** on the mini-quizzes.
8. Domains are weighted differently — spend more time on **Domain 1** and **Domain 4** (20–25 % each).
9. **Scenario Playbook** near the end simulates real-world questions — do these last.
10. Check the **Final Revision Checklist** the night before the exam.

---

## Table of Contents

- [High-Yield Quick Sheet](#high-yield-quick-sheet)
- [Domain 1 — Author and Manage Workflows (20–25 %)](#domain-1--author-and-manage-workflows-2025-)
  - [1.1 Workflow Structure](#11-workflow-structure)
  - [1.2 Triggers](#12-triggers)
  - [1.3 Filters](#13-filters)
  - [1.4 Contexts](#14-contexts)
  - [1.5 Expressions & Functions](#15-expressions--functions)
  - [1.6 Conditionals](#16-conditionals)
  - [1.7 Environment Variables](#17-environment-variables)
  - [1.8 Outputs](#18-outputs)
  - [1.9 Permissions](#19-permissions)
  - [1.10 Job Orchestration (needs)](#110-job-orchestration-needs)
  - [1.11 Strategy Matrix](#111-strategy-matrix)
  - [1.12 Concurrency](#112-concurrency)
  - [1.13 Services](#113-services)
  - [1.14 Timeouts, continue-on-error, defaults](#114-timeouts-continue-on-error-defaults)
  - [1.15 Reusable Workflows](#115-reusable-workflows)
  - [1.16 Artifacts](#116-artifacts)
  - [1.17 Caching](#117-caching)
  - [1.18 Environments](#118-environments)
- [Domain 2 — Consume and Troubleshoot Workflows (15–20 %)](#domain-2--consume-and-troubleshoot-workflows-1520-)
  - [2.1 Reading Logs & Debug Logging](#21-reading-logs--debug-logging)
  - [2.2 Common Failure Patterns](#22-common-failure-patterns)
  - [2.3 Checkout & Token Auth](#23-checkout--token-auth)
  - [2.4 Event Payload Differences](#24-event-payload-differences)
  - [2.5 Workflow Commands](#25-workflow-commands)
  - [2.6 Diagnosing Cache & Artifact Issues](#26-diagnosing-cache--artifact-issues)
  - [2.7 Pinning Action Versions](#27-pinning-action-versions)
- [Domain 3 — Author and Maintain Actions (15–20 %)](#domain-3--author-and-maintain-actions-1520-)
  - [3.1 Action Types Overview](#31-action-types-overview)
  - [3.2 action.yml Anatomy](#32-actionyml-anatomy)
  - [3.3 Composite Actions](#33-composite-actions)
  - [3.4 JavaScript Actions](#34-javascript-actions)
  - [3.5 Docker Container Actions](#35-docker-container-actions)
  - [3.6 Versioning & Publishing](#36-versioning--publishing)
  - [3.7 Testing Actions](#37-testing-actions)
- [Domain 4 — Manage GitHub Actions for the Enterprise (20–25 %)](#domain-4--manage-github-actions-for-the-enterprise-2025-)
  - [4.1 Policies: Allow / Deny Actions](#41-policies-allow--deny-actions)
  - [4.2 Reusable Workflow Governance](#42-reusable-workflow-governance)
  - [4.3 Runner Groups & Access Control](#43-runner-groups--access-control)
  - [4.4 Secrets Scoping](#44-secrets-scoping)
  - [4.5 Audit, Logging & Compliance](#45-audit-logging--compliance)
  - [4.6 Billing, Minutes & Storage](#46-billing-minutes--storage)
- [Domain 5 — Secure and Optimize Automation (10–15 %)](#domain-5--secure-and-optimize-automation-1015-)
  - [5.1 Least Privilege Permissions](#51-least-privilege-permissions)
  - [5.2 Secrets Hygiene](#52-secrets-hygiene)
  - [5.3 OIDC for Cloud Auth](#53-oidc-for-cloud-auth)
  - [5.4 Supply Chain Security](#54-supply-chain-security)
  - [5.5 Optimization Techniques](#55-optimization-techniques)
  - [5.6 Safe Handling of Fork PRs](#56-safe-handling-of-fork-prs)
- [Scenario Playbook (10 Scenarios)](#scenario-playbook-10-scenarios)
- [Practice Exam (40 Questions)](#practice-exam-40-questions)
- [Answer Key & Explanations](#answer-key--explanations)
- [Final Revision Checklist](#final-revision-checklist)

---

## High-Yield Quick Sheet

| Topic | Must-Know Fact |
|---|---|
| **Workflow file location** | `.github/workflows/*.yml` (or `.yaml`) |
| **Default token** | `GITHUB_TOKEN` — scoped to the repo, short-lived |
| **Permissions** | Set at workflow or job level; `permissions: {}` = no perms |
| **Secrets context** | `${{ secrets.MY_SECRET }}` — **masked** in logs automatically |
| **Environment variables** | Precedence: step > job > workflow |
| **Matrix** | `strategy.matrix` — generates parallel jobs; `fail-fast: true` default |
| **Concurrency** | `concurrency: { group: ..., cancel-in-progress: true }` |
| **Reusable workflow trigger** | `on: workflow_call` (callee) / `uses:` at job level (caller) |
| **Composite action** | `runs.using: composite` — steps use `run` or `uses` |
| **JavaScript action** | `runs.using: node20` — bundle with `@vercel/ncc` |
| **Docker action** | `runs.using: docker` — needs `image:` in `action.yml` |
| **Pin actions** | Full SHA > tag > branch (security order) |
| **Cache key miss** | Falls back to `restore-keys` prefixes in order |
| **Artifacts** | `actions/upload-artifact@v4` / `download-artifact@v4` |
| **OIDC** | Use `permissions: id-token: write` + cloud provider trust |
| **Runner types** | GitHub-hosted (ephemeral) vs self-hosted (persistent) |
| **Runner groups** | Enterprise/org level → control which repos can use them |
| **Schedule cron** | UTC only; shortest interval ≈ 5 min; may be delayed |
| **`needs` outputs** | `needs.<job_id>.outputs.<name>` |
| **Step outputs** | `$GITHUB_OUTPUT` file (modern); `echo "key=val" >> $GITHUB_OUTPUT` |
| **Debug logging** | Set secret `ACTIONS_STEP_DEBUG` = `true` |
| **Fork PR restrictions** | Secrets NOT available; `GITHUB_TOKEN` is read-only |
| **Org-level secrets** | Can be scoped to all, private, or selected repos |
| **Billing** | Linux 1×, Windows 2×, macOS 10× minute multiplier |

---

## Domain 1 — Author and Manage Workflows (20–25 %)

### What the Exam Tries to Test Here

The exam verifies you can **write workflows from scratch**, understand **every YAML key**, chain jobs with dependencies, use matrices, handle outputs/secrets/variables, and implement reusable patterns. This is the heaviest domain — nail it.

### Concept Map

```
Workflow Authoring
├── Structure (name, on, jobs, steps)
│   ├── uses (action) vs run (shell)
│   └── Permissions block
├── Triggers
│   ├── push / pull_request / schedule / workflow_dispatch
│   ├── release / workflow_call
│   └── Filters (branches, tags, paths, types)
├── Data Flow
│   ├── Contexts (github, env, secrets, vars, needs, steps, runner, strategy, job)
│   ├── Expressions & Functions
│   ├── Conditionals (if)
│   ├── Environment Variables (env, GITHUB_ENV)
│   └── Outputs (step → job → workflow)
├── Job Orchestration
│   ├── needs / dependency graph
│   ├── strategy.matrix (include/exclude, fail-fast, max-parallel)
│   ├── concurrency groups
│   ├── services
│   ├── timeouts / continue-on-error / defaults
│   └── Reusable workflows (workflow_call)
└── Artifacts & Caching
    ├── upload-artifact / download-artifact
    ├── actions/cache (key / restore-keys)
    └── Environments (protection rules, approvals)
```

---

### 1.1 Workflow Structure

**What it is:** A YAML file in `.github/workflows/` that defines automated processes.

**Why it matters for the exam:** Nearly every question assumes you understand the basic skeleton.

**Syntax:**

```yaml
name: CI                    # Optional display name
on: [push]                  # Trigger event(s)

permissions:                # Optional — restrict GITHUB_TOKEN
  contents: read

jobs:
  build:                    # Job ID (unique within workflow)
    runs-on: ubuntu-latest  # Runner label
    steps:
      - uses: actions/checkout@v4     # Action step
      - run: echo "Hello, World!"     # Shell step
```

**Minimal example — lint on push:**

```yaml
name: Lint
on: push
jobs:
  eslint:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: 20
      - run: npm ci
      - run: npx eslint .
```

**Pitfalls:**
- `uses` and `run` **cannot** appear in the same step.
- The file **must** live under `.github/workflows/`.
- Job IDs and step IDs must contain only alphanumeric characters, `-`, or `_`.

**Mini-Quiz:**

1. Can a single step contain both `uses:` and `run:`? → **No.**
2. What is the minimum required key inside a job? → **`runs-on`** (plus at least one step).
3. Where must workflow files be stored? → **`.github/workflows/`**

---

### 1.2 Triggers

**What it is:** The `on:` key defines which events start a workflow.

**Why it matters:** The exam tests trigger nuances — filtering, inputs, combinations.

**Syntax & Examples:**

```yaml
# Single trigger
on: push

# Multiple triggers
on: [push, pull_request]

# Map form with configuration
on:
  push:
    branches: [main]
  pull_request:
    branches: [main]
```

**workflow_dispatch (manual trigger with inputs):**

```yaml
on:
  workflow_dispatch:
    inputs:
      environment:
        description: "Target environment"
        required: true
        default: "staging"
        type: choice
        options:
          - staging
          - production
```

**schedule (cron):**

```yaml
on:
  schedule:
    - cron: "30 5 * * 1-5"   # 05:30 UTC, Mon–Fri
```

**release:**

```yaml
on:
  release:
    types: [published]
```

**workflow_call (reusable workflow):**

```yaml
on:
  workflow_call:
    inputs:
      node-version:
        required: true
        type: string
    secrets:
      NPM_TOKEN:
        required: true
```

**Pitfalls:**
- `schedule` uses **UTC only** — no timezone setting.
- `pull_request` fires on the **merge commit** (HEAD ref), not the branch HEAD.
- A workflow can have **multiple schedules** (array of cron entries).
- `workflow_call` cannot be combined with other triggers in the same `on:` block — it can coexist but the reusable workflow is invoked only via `workflow_call`.

**Mini-Quiz:**

1. What timezone does `schedule` use? → **UTC.**
2. Can `workflow_dispatch` accept typed inputs? → **Yes** — `string`, `boolean`, `choice`, `number`, `environment`.
3. Which trigger makes a workflow reusable? → **`workflow_call`.**

---

### 1.3 Filters

**What it is:** Narrow when a trigger actually starts a run using `branches`, `tags`, `paths`, and `types`.

**Syntax:**

```yaml
on:
  push:
    branches:
      - main
      - "releases/**"      # glob pattern
    tags:
      - "v*"
    paths:
      - "src/**"
    paths-ignore:
      - "docs/**"
  pull_request:
    types: [opened, synchronize, reopened]
```

**Pitfalls:**
- You **cannot** use both `paths` and `paths-ignore` in the same trigger.
- You **cannot** use both `branches` and `branches-ignore` in the same trigger.
- `tags-ignore` and `tags` are mutually exclusive.

**Mini-Quiz:**

1. Can you combine `paths` and `paths-ignore`? → **No.**
2. What does `"releases/**"` match? → Any branch starting with `releases/`.

---

### 1.4 Contexts

**What it is:** Objects that provide information at runtime: `github`, `env`, `vars`, `secrets`, `job`, `steps`, `runner`, `strategy`, `needs`, `inputs`, `matrix`.

**Why it matters:** The exam asks which context provides a specific piece of data.

| Context | Provides | Example |
|---|---|---|
| `github` | Event info, repo, SHA, ref, actor | `${{ github.sha }}` |
| `env` | Env vars set in workflow | `${{ env.MY_VAR }}` |
| `vars` | Configuration variables (repo/org/env) | `${{ vars.API_URL }}` |
| `secrets` | Encrypted secrets | `${{ secrets.TOKEN }}` |
| `job` | Current job status | `${{ job.status }}` |
| `steps` | Step outputs & outcomes | `${{ steps.build.outputs.version }}` |
| `runner` | Runner details | `${{ runner.os }}` |
| `strategy` | Matrix strategy info | `${{ strategy.fail-fast }}` |
| `needs` | Outputs from dependency jobs | `${{ needs.build.outputs.tag }}` |
| `matrix` | Current matrix values | `${{ matrix.os }}` |
| `inputs` | `workflow_dispatch` / `workflow_call` inputs | `${{ inputs.environment }}` |

**Micro-example — using github context:**

```yaml
- run: echo "Triggered by ${{ github.event_name }} on ${{ github.ref }}"
```

**Pitfalls:**
- `secrets` context is **not available** in `if:` conditions (you can check emptiness but you should not log/print them).
- `vars` are **not** encrypted — use `secrets` for sensitive data.
- `steps.<id>.outcome` is the raw result; `steps.<id>.conclusion` respects `continue-on-error`.

**Mini-Quiz:**

1. Which context gives you the current Git SHA? → **`github.sha`**
2. What is the difference between `outcome` and `conclusion` on a step? → `outcome` is raw; `conclusion` accounts for `continue-on-error`.
3. Are `vars` encrypted? → **No.**

---

### 1.5 Expressions & Functions

**What it is:** `${{ }}` syntax to evaluate expressions and call built-in functions.

**Key functions:**

| Function | Purpose | Example |
|---|---|---|
| `success()` | True if all previous steps succeeded | `if: success()` |
| `failure()` | True if any previous step failed | `if: failure()` |
| `always()` | Always true (runs regardless) | `if: always()` |
| `cancelled()` | True if workflow was cancelled | `if: cancelled()` |
| `contains(search, item)` | Substring / array check | `contains(github.ref, 'main')` |
| `startsWith(str, prefix)` | Prefix check | `startsWith(github.ref, 'refs/tags/')` |
| `endsWith(str, suffix)` | Suffix check | `endsWith(github.ref, '-rc')` |
| `format(str, ...)` | String interpolation | `format('Hello {0}', 'World')` |
| `join(arr, sep)` | Join array | `join(matrix.os, ', ')` |
| `toJSON(value)` | JSON representation | `toJSON(github.event)` |
| `fromJSON(str)` | Parse JSON string | `fromJSON(steps.meta.outputs.json)` |
| `hashFiles(patterns)` | SHA-256 hash of files | `hashFiles('**/package-lock.json')` |

**Micro-example:**

```yaml
- run: echo "Tag build"
  if: startsWith(github.ref, 'refs/tags/')
```

**Pitfalls:**
- In `if:` the `${{ }}` wrapper is **optional** (auto-coercion), but required everywhere else.
- `success()` is the **implicit default** for steps — a step runs only if all prior steps succeeded unless you override `if:`.
- `always()` will run even if the workflow is **cancelled** — be cautious.

**Mini-Quiz:**

1. What is the default `if:` condition for a step? → **`success()`**
2. Does `always()` run on cancellation? → **Yes.**
3. How do you hash lock files for a cache key? → **`hashFiles('**/package-lock.json')`**

---

### 1.6 Conditionals

**What it is:** The `if:` key on jobs or steps controls whether they execute.

**Syntax:**

```yaml
jobs:
  deploy:
    if: github.ref == 'refs/heads/main'
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: echo "Deploying..."
        if: success() && github.event_name == 'push'
```

**Combining conditions:**

```yaml
if: >-
  github.event_name == 'push' &&
  contains(github.ref, 'main') &&
  !cancelled()
```

**Checking job outputs:**

```yaml
jobs:
  check:
    runs-on: ubuntu-latest
    outputs:
      should_deploy: ${{ steps.decide.outputs.deploy }}
    steps:
      - id: decide
        run: echo "deploy=true" >> "$GITHUB_OUTPUT"

  deploy:
    needs: check
    if: needs.check.outputs.should_deploy == 'true'
    runs-on: ubuntu-latest
    steps:
      - run: echo "Deploying!"
```

**Pitfalls:**
- All output values are **strings** — compare with `'true'` not `true`.
- `if:` on a **job** skips the entire job — downstream `needs` jobs see status `skipped`.
- Use `always()` in downstream jobs if you want them to run even when an upstream job is skipped.

**Mini-Quiz:**

1. What type are job outputs? → **String.**
2. If a job is skipped, what status do dependent jobs see? → **`skipped`.**

---

### 1.7 Environment Variables

**What it is:** `env:` sets environment variables at workflow, job, or step scope.

**Precedence:** Step > Job > Workflow (most specific wins).

**Syntax:**

```yaml
env:
  NODE_ENV: production          # workflow level

jobs:
  build:
    env:
      CI: "true"                 # job level
    runs-on: ubuntu-latest
    steps:
      - run: echo "$NODE_ENV $CI"
      - env:
          OVERRIDE: "yes"        # step level
        run: echo "$OVERRIDE"
```

**Setting env vars dynamically (GITHUB_ENV):**

```yaml
- run: echo "VERSION=1.2.3" >> "$GITHUB_ENV"
- run: echo "The version is $VERSION"   # available in subsequent steps
```

**Pitfalls:**
- `GITHUB_ENV` changes are available in **subsequent steps**, not the current one.
- On Windows runners, use `$env:GITHUB_ENV` in PowerShell.
- Default env vars (e.g., `GITHUB_SHA`, `GITHUB_REF`) are **automatically set**.

**Mini-Quiz:**

1. In which step does a `GITHUB_ENV` variable become available? → **The next step.**
2. Which scope wins in env precedence? → **Step level (most specific).**

---

### 1.8 Outputs

**What it is:** Passing data between steps, jobs, and reusable workflows.

**Step outputs (modern approach):**

```yaml
steps:
  - id: version
    run: echo "tag=v1.0.0" >> "$GITHUB_OUTPUT"
  - run: echo "Tag is ${{ steps.version.outputs.tag }}"
```

**Job outputs:**

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    outputs:
      artifact_name: ${{ steps.name.outputs.artifact }}
    steps:
      - id: name
        run: echo "artifact=my-app-v1" >> "$GITHUB_OUTPUT"

  deploy:
    needs: build
    runs-on: ubuntu-latest
    steps:
      - run: echo "${{ needs.build.outputs.artifact_name }}"
```

**Reusable workflow outputs:**

```yaml
# callee.yml
on:
  workflow_call:
    outputs:
      image_tag:
        description: "Docker image tag"
        value: ${{ jobs.build.outputs.tag }}
```

**Pitfalls:**
- The legacy `::set-output` command is **deprecated** — use `$GITHUB_OUTPUT`.
- Job `outputs` must reference step outputs via `steps.<id>.outputs.<key>`.
- All output values are **strings**.

**Mini-Quiz:**

1. How do you set a step output in modern GitHub Actions? → **`echo "key=val" >> "$GITHUB_OUTPUT"`**
2. Where do you declare job outputs? → Under `jobs.<id>.outputs`.

---

### 1.9 Permissions

**What it is:** The `permissions` block restricts what the `GITHUB_TOKEN` can do.

**Why it matters:** The exam heavily tests least privilege and explicit permissions.

**Syntax:**

```yaml
permissions:
  contents: read
  pull-requests: write
  issues: write
```

**Key values:** `read`, `write`, `none`

**Disable all permissions:**

```yaml
permissions: {}
```

**Micro-example — read-only workflow:**

```yaml
name: CI
on: push
permissions:
  contents: read
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: npm test
```

**Pitfalls:**
- Setting `permissions` at workflow level **overrides** all defaults — unlisted permissions become `none`.
- Job-level `permissions` further restrict (cannot escalate beyond workflow level).
- `GITHUB_TOKEN` is **automatically** provided — no need to create it manually.
- For fork PRs, `GITHUB_TOKEN` has **read-only** permissions regardless.

**Mini-Quiz:**

1. What happens to unlisted permissions when you set `permissions:`? → They become `none`.
2. Can a job escalate permissions beyond the workflow level? → **No.**
3. What permissions does `GITHUB_TOKEN` have on fork PRs? → **Read-only.**

---

### 1.10 Job Orchestration (needs)

**What it is:** The `needs` key creates a dependency graph between jobs.

**Syntax:**

```yaml
jobs:
  lint:
    runs-on: ubuntu-latest
    steps:
      - run: echo "Linting..."

  test:
    runs-on: ubuntu-latest
    steps:
      - run: echo "Testing..."

  deploy:
    needs: [lint, test]           # runs after both complete successfully
    runs-on: ubuntu-latest
    steps:
      - run: echo "Deploying..."
```

**Dependency graph:**

```
lint ──┐
       ├──► deploy
test ──┘
```

**Pitfalls:**
- Without `needs`, jobs run **in parallel** by default.
- If any required job fails or is skipped, the dependent job is **skipped** (unless you add `if: always()`).
- Circular dependencies cause a **validation error**.

**Mini-Quiz:**

1. Do jobs run in parallel or sequentially by default? → **Parallel.**
2. What happens to a job if a `needs` job fails? → **It is skipped.**

---

### 1.11 Strategy Matrix

**What it is:** `strategy.matrix` generates multiple job runs from variable combinations.

**Syntax:**

```yaml
jobs:
  test:
    runs-on: ${{ matrix.os }}
    strategy:
      fail-fast: false
      max-parallel: 4
      matrix:
        os: [ubuntu-latest, windows-latest, macos-latest]
        node: [18, 20, 22]
        include:
          - os: ubuntu-latest
            node: 22
            experimental: true
        exclude:
          - os: macos-latest
            node: 18
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: ${{ matrix.node }}
      - run: npm test
```

This generates 3 × 3 = 9 combinations, minus 1 exclusion = **8 jobs**, plus the `include` entry adds a property to an existing combination.

**Pitfalls:**
- `fail-fast` defaults to **`true`** — one failure cancels remaining matrix jobs.
- `include` can **add new combinations** or **add properties** to existing ones.
- `exclude` removes combinations matching **all** specified keys.
- `max-parallel` limits concurrent jobs (useful for rate-limited resources).

**Mini-Quiz:**

1. What is the default value of `fail-fast`? → **`true`.**
2. Can `include` add a completely new matrix combination? → **Yes.**
3. What does `max-parallel` control? → Maximum concurrent matrix jobs.

---

### 1.12 Concurrency

**What it is:** `concurrency` ensures only one workflow/job in a group runs at a time.

**Syntax:**

```yaml
concurrency:
  group: deploy-${{ github.ref }}
  cancel-in-progress: true
```

**Micro-example — prevent concurrent deploys:**

```yaml
name: Deploy
on:
  push:
    branches: [main]
concurrency:
  group: production-deploy
  cancel-in-progress: false    # queue, don't cancel
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - run: echo "Deploying..."
```

**Pitfalls:**
- `cancel-in-progress: true` cancels the **currently running** workflow when a new one is queued.
- Concurrency can be set at **workflow** or **job** level.
- The `group` key should be dynamic (e.g., per branch) to avoid blocking unrelated runs.

**Mini-Quiz:**

1. Which run is cancelled with `cancel-in-progress: true`? → The **in-progress** one.
2. Can concurrency be set at the job level? → **Yes.**

---

### 1.13 Services

**What it is:** Docker containers that run alongside your job (e.g., databases for integration tests).

**Syntax:**

```yaml
jobs:
  integration:
    runs-on: ubuntu-latest
    services:
      postgres:
        image: postgres:16
        env:
          POSTGRES_PASSWORD: testpass
        ports:
          - 5432:5432
        options: >-
          --health-cmd pg_isready
          --health-interval 10s
          --health-timeout 5s
          --health-retries 5
    steps:
      - uses: actions/checkout@v4
      - run: npm test
        env:
          DATABASE_URL: postgres://postgres:testpass@localhost:5432/postgres
```

**Pitfalls:**
- Services run only on **Linux** runners.
- Use `localhost` with port mapping on bare runner; use service label as hostname inside container jobs.
- Health checks prevent steps from starting before the service is ready.

**Mini-Quiz:**

1. Can you use services on Windows runners? → **No.**
2. How do you address a service from a step on a VM runner? → **`localhost:<port>`**

---

### 1.14 Timeouts, continue-on-error, defaults

**Timeouts:**

```yaml
jobs:
  test:
    runs-on: ubuntu-latest
    timeout-minutes: 30         # job timeout (default 360)
    steps:
      - run: long-test
        timeout-minutes: 10     # step timeout
```

**continue-on-error:**

```yaml
steps:
  - run: npm run lint
    continue-on-error: true     # step failure won't fail the job
```

**defaults:**

```yaml
defaults:
  run:
    shell: bash
    working-directory: ./app
```

**Pitfalls:**
- `continue-on-error: true` at job level makes the whole job "soft fail" — downstream `needs` jobs still run.
- The default job timeout is **360 minutes** (6 hours).
- `defaults.run` applies only to `run:` steps, not `uses:` steps.

**Mini-Quiz:**

1. What is the default job timeout? → **360 minutes.**
2. Does `defaults.run.shell` affect `uses:` steps? → **No.**

---

### 1.15 Reusable Workflows

**What it is:** Workflows with `on: workflow_call` that can be called by other workflows.

**Callee (reusable workflow):**

```yaml
# .github/workflows/reusable-ci.yml
name: Reusable CI
on:
  workflow_call:
    inputs:
      node-version:
        required: true
        type: string
    secrets:
      NPM_TOKEN:
        required: false
    outputs:
      test-result:
        description: "Test outcome"
        value: ${{ jobs.test.outputs.result }}

jobs:
  test:
    runs-on: ubuntu-latest
    outputs:
      result: ${{ steps.test.outputs.outcome }}
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: ${{ inputs.node-version }}
      - run: npm ci
      - id: test
        run: |
          npm test && echo "outcome=pass" >> "$GITHUB_OUTPUT" || echo "outcome=fail" >> "$GITHUB_OUTPUT"
```

**Caller (consumer workflow):**

```yaml
# .github/workflows/ci.yml
name: CI
on: push
jobs:
  run-tests:
    uses: ./.github/workflows/reusable-ci.yml
    with:
      node-version: "20"
    secrets:
      NPM_TOKEN: ${{ secrets.NPM_TOKEN }}

  report:
    needs: run-tests
    runs-on: ubuntu-latest
    steps:
      - run: echo "Tests ${{ needs.run-tests.outputs.test-result }}"
```

**Second reusable workflow — deploy:**

```yaml
# .github/workflows/reusable-deploy.yml
name: Reusable Deploy
on:
  workflow_call:
    inputs:
      environment:
        required: true
        type: string
      image-tag:
        required: true
        type: string
    secrets:
      DEPLOY_KEY:
        required: true

jobs:
  deploy:
    runs-on: ubuntu-latest
    environment: ${{ inputs.environment }}
    steps:
      - run: echo "Deploying ${{ inputs.image-tag }} to ${{ inputs.environment }}"
```

**Second caller:**

```yaml
name: Release
on:
  release:
    types: [published]
jobs:
  deploy-staging:
    uses: ./.github/workflows/reusable-deploy.yml
    with:
      environment: staging
      image-tag: ${{ github.event.release.tag_name }}
    secrets:
      DEPLOY_KEY: ${{ secrets.DEPLOY_KEY }}
```

**Pitfalls:**
- Reusable workflows are called at the **job level** (`uses:` under job, not under steps).
- A reusable workflow can call another reusable workflow — up to **4 levels** deep.
- `secrets: inherit` passes **all** caller's secrets without listing them.
- You cannot mix `steps:` and `uses:` in the same job (when calling a reusable workflow).

**Mini-Quiz:**

1. Where does `uses:` go when calling a reusable workflow? → **At the job level.**
2. What is the maximum nesting depth? → **4 levels.**
3. How do you pass all secrets? → **`secrets: inherit`**

---

### 1.16 Artifacts

**What it is:** Files persisted between jobs or after a workflow run.

**Upload:**

```yaml
- uses: actions/upload-artifact@v4
  with:
    name: build-output
    path: dist/
    retention-days: 7
```

**Download:**

```yaml
- uses: actions/download-artifact@v4
  with:
    name: build-output
    path: ./dist
```

**Complete workflow — build and deploy with artifact:**

```yaml
name: Build & Deploy
on:
  push:
    branches: [main]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: npm ci && npm run build
      - uses: actions/upload-artifact@v4
        with:
          name: app-bundle
          path: dist/

  deploy:
    needs: build
    runs-on: ubuntu-latest
    steps:
      - uses: actions/download-artifact@v4
        with:
          name: app-bundle
          path: ./dist
      - run: echo "Deploying dist/ contents..."
```

**Pitfalls:**
- Artifacts v4 requires **unique names** per upload (no overwriting).
- Default retention is **90 days** (configurable at repo/org level).
- Artifacts are **not** shared across workflow runs — use caching for that.

**Mini-Quiz:**

1. What is the default artifact retention? → **90 days.**
2. Can two jobs share an artifact within the same workflow run? → **Yes, via upload/download.**

---

### 1.17 Caching

**What it is:** `actions/cache` persists files across workflow runs to speed up builds.

**Syntax:**

```yaml
- uses: actions/cache@v4
  with:
    path: ~/.npm
    key: npm-${{ runner.os }}-${{ hashFiles('**/package-lock.json') }}
    restore-keys: |
      npm-${{ runner.os }}-
```

**What to cache by ecosystem:**

| Ecosystem | Path | Key File |
|---|---|---|
| npm | `~/.npm` | `package-lock.json` |
| Yarn | `.yarn/cache` | `yarn.lock` |
| pip | `~/.cache/pip` | `requirements.txt` |
| Maven | `~/.m2/repository` | `pom.xml` |
| Gradle | `~/.gradle/caches` | `*.gradle*`, `gradle.properties` |
| Go | `~/go/pkg/mod` | `go.sum` |

**Micro-example — pip caching:**

```yaml
- uses: actions/cache@v4
  with:
    path: ~/.cache/pip
    key: pip-${{ runner.os }}-${{ hashFiles('**/requirements.txt') }}
    restore-keys: |
      pip-${{ runner.os }}-
```

**Pitfalls:**
- Cache size limit: **10 GB** per repo.
- Caches not accessed for **7 days** are evicted.
- `restore-keys` matches by **prefix**, uses the most recent.
- Cache hits on `key` → no new cache saved; hit on `restore-keys` → updated cache saved.

**Mini-Quiz:**

1. What is the cache size limit per repo? → **10 GB.**
2. When is a new cache entry saved? → On a `restore-keys` match (partial hit) but not on exact `key` match.

---

### 1.18 Environments

**What it is:** Named deployment targets (e.g., `staging`, `production`) with optional protection rules.

**Syntax:**

```yaml
jobs:
  deploy:
    runs-on: ubuntu-latest
    environment:
      name: production
      url: https://myapp.example.com
    steps:
      - run: echo "Deploying to production"
```

**Key features:**
- **Required reviewers** — manual approval before job proceeds.
- **Wait timer** — delay deployment by N minutes.
- **Branch protection** — restrict which branches can deploy to an environment.
- **Environment secrets** — scoped to a specific environment.
- **Environment variables** — scoped to a specific environment (via `vars`).

**Pitfalls:**
- Environment protection rules are configured in **repo settings**, not in YAML.
- Environment secrets **override** repo-level secrets of the same name.
- You can reference environment variables with `${{ vars.MY_VAR }}`.

**Mini-Quiz:**

1. Where are environment protection rules configured? → **Repository settings (UI).**
2. Do environment secrets override repo secrets of the same name? → **Yes.**

---

## Domain 2 — Consume and Troubleshoot Workflows (15–20 %)

### What the Exam Tries to Test Here

Can you **read logs**, **diagnose failures**, understand **event differences**, use **workflow commands**, and pin actions correctly? This domain is about consumption and debugging — not writing from scratch.

### Concept Map

```
Troubleshooting
├── Logs & Debugging
│   ├── Step logs, annotations
│   ├── ACTIONS_STEP_DEBUG
│   └── Re-run jobs
├── Common Failures
│   ├── YAML syntax / indentation
│   ├── Expression quoting
│   └── Auth / checkout issues
├── Event Payloads
│   ├── push vs pull_request
│   └── github.event differences
├── Workflow Commands
│   ├── ::notice, ::warning, ::error
│   ├── ::group, ::endgroup
│   └── ::add-mask
└── Pinning Actions
    ├── SHA > tag > branch
    └── Dependabot for action updates
```

---

### 2.1 Reading Logs & Debug Logging

**Step debug logging:**

Set the **secret** `ACTIONS_STEP_DEBUG` to `true` → verbose output for every step.

**Runner diagnostic logging:**

Set the secret `ACTIONS_RUNNER_DEBUG` to `true` → runner process logs.

**Re-running jobs:**
- You can re-run **all** jobs or **failed** jobs only.
- Re-runs use the **same commit SHA** and workflow file.
- Debug logging can be enabled per re-run in the UI.

**Pitfalls:**
- Debug logs may expose **sensitive information** — review before sharing.
- Re-runs respect the **original** workflow file, not current HEAD.

**Mini-Quiz:**

1. Which secret enables verbose step logs? → **`ACTIONS_STEP_DEBUG`**
2. Does re-running a workflow use the latest workflow file? → **No — it uses the original commit's version.**

---

### 2.2 Common Failure Patterns

| Failure | Cause | Fix |
|---|---|---|
| `unexpected value` | YAML indentation error | Check spaces (2-space indent) |
| Expression not evaluated | Missing `${{ }}` in `run:` | Wrap expression properly |
| `Resource not accessible by integration` | Insufficient `GITHUB_TOKEN` permissions | Add `permissions:` block |
| `Process completed with exit code 1` | Command failure | Check command output above |
| `Action not found` | Wrong action reference / typo | Verify `uses:` value |
| Job skipped unexpectedly | `needs` job failed or `if:` condition false | Check dependency chain |

**Micro-example — common quoting trap:**

```yaml
# WRONG — treated as truthy string
if: "true"

# CORRECT — boolean expression
if: true

# CORRECT — compare output
if: steps.check.outputs.result == 'true'
```

---

### 2.3 Checkout & Token Auth

**Default checkout:**

```yaml
- uses: actions/checkout@v4   # uses GITHUB_TOKEN by default
```

**Checkout with PAT (for cross-repo or triggering workflows):**

```yaml
- uses: actions/checkout@v4
  with:
    token: ${{ secrets.PAT }}
```

**Why the PAT matters:** Pushes made with `GITHUB_TOKEN` do **not** trigger new workflow runs (prevents recursion). Use a PAT or GitHub App token if you need to trigger downstream workflows.

**Pitfalls:**
- `actions/checkout` does a **shallow clone** by default (`fetch-depth: 1`).
- For tags, release history, or blame, set `fetch-depth: 0`.
- Submodules are **not** checked out by default — use `submodules: true`.

**Mini-Quiz:**

1. Do pushes with `GITHUB_TOKEN` trigger new workflow runs? → **No.**
2. What is the default `fetch-depth`? → **1 (shallow clone).**

---

### 2.4 Event Payload Differences

| Property | `push` | `pull_request` |
|---|---|---|
| `github.sha` | Commit pushed | Merge commit SHA |
| `github.ref` | `refs/heads/branch` | `refs/pull/N/merge` |
| `github.head_ref` | Not set | Source branch name |
| `github.base_ref` | Not set | Target branch name |
| `github.event.pull_request` | Not available | Full PR object |

**Micro-example — detect PR:**

```yaml
- run: echo "PR #${{ github.event.pull_request.number }}"
  if: github.event_name == 'pull_request'
```

**Pitfalls:**
- `github.ref` on PRs points to `refs/pull/N/merge`, not the source branch.
- To get the PR source branch, use `github.head_ref`.

---

### 2.5 Workflow Commands

**Annotations:**

```yaml
- run: echo "::notice file=app.js,line=1::This is an informational message"
- run: echo "::warning file=app.js,line=10::Deprecated function used"
- run: echo "::error file=app.js,line=20,col=5::Null pointer detected"
```

**Grouping log output:**

```yaml
- run: |
    echo "::group::Install Dependencies"
    npm ci
    echo "::endgroup::"
```

**Masking a value:**

```yaml
- run: |
    MY_SECRET=$(generate-token)
    echo "::add-mask::$MY_SECRET"
    echo "Token generated"
```

**Setting output (modern):**

```yaml
- run: echo "version=1.2.3" >> "$GITHUB_OUTPUT"
```

**Setting env var for subsequent steps:**

```yaml
- run: echo "MY_VAR=hello" >> "$GITHUB_ENV"
```

**Adding to PATH:**

```yaml
- run: echo "/custom/bin" >> "$GITHUB_PATH"
```

**Pitfalls:**
- `::set-output` is **deprecated** — use `$GITHUB_OUTPUT`.
- `::save-state` is **deprecated** — use `$GITHUB_STATE`.
- Masking with `::add-mask::` only masks in **subsequent** log lines.

**Mini-Quiz:**

1. What replaces `::set-output`? → **`$GITHUB_OUTPUT`**
2. How do you mask a value in logs? → **`echo "::add-mask::VALUE"`**
3. Which workflow command adds annotations? → **`::notice`, `::warning`, `::error`**

---

### 2.6 Diagnosing Cache & Artifact Issues

**Cache misses:**

- Key changed (e.g., lock file updated) → new cache entry needed.
- Cache expired (7 days without access).
- Cache size exceeded 10 GB repo limit → oldest evicted.
- Different branch: caches from default branch are available to all, but feature branch caches aren't available to default.

**Artifact issues:**

- Same name uploaded twice in v4 → error (names must be unique).
- Artifact expired (past retention period).
- Large artifacts → slow upload/download, consider caching instead.

**Mini-Quiz:**

1. Are caches from the default branch available to feature branches? → **Yes.**
2. Can a feature branch cache be used on the default branch? → **No** (default branch can only use its own or base branch caches).

---

### 2.7 Pinning Action Versions

**Security order (best to worst):**

```yaml
# BEST — immutable SHA
- uses: actions/checkout@a5ac7e51b41094c92402da3b24376905380afc29  # v4.1.6

# GOOD — major version tag (may change)
- uses: actions/checkout@v4

# RISKY — mutable tag
- uses: actions/checkout@main
```

**Why SHAs?**
- Tags can be **moved** to point to different commits (supply chain attack vector).
- SHAs are **immutable** — the code you audit is the code you run.

**Dependabot for actions:**

```yaml
# .github/dependabot.yml
version: 2
updates:
  - package-ecosystem: "github-actions"
    directory: "/"
    schedule:
      interval: "weekly"
```

**Mini-Quiz:**

1. Why prefer SHA over tag? → Tags can be moved; SHAs are immutable.
2. Where do you configure Dependabot for action updates? → **`.github/dependabot.yml`**

---

## Domain 3 — Author and Maintain Actions (15–20 %)

### What the Exam Tries to Test Here

Understand the **three action types** (composite, JavaScript, Docker), the **action.yml metadata** file, versioning, publishing, and basic toolkits.

### Concept Map

```
Custom Actions
├── action.yml (metadata)
│   ├── name, description
│   ├── inputs / outputs
│   ├── runs (using, steps/main/image)
│   └── branding
├── Composite Actions
│   ├── runs.using: composite
│   └── Multiple run/uses steps
├── JavaScript Actions
│   ├── runs.using: node20
│   ├── @actions/core, @actions/github
│   └── Bundle with ncc
├── Docker Actions
│   ├── runs.using: docker
│   ├── Dockerfile / image reference
│   └── entrypoint, args
└── Versioning & Publishing
    ├── Semantic versioning tags
    ├── Major version tag (v1)
    └── Marketplace listing
```

---

### 3.1 Action Types Overview

| Type | `runs.using` | Use Case | Platform |
|---|---|---|---|
| Composite | `composite` | Combine existing steps | Any runner |
| JavaScript | `node20` | Fast, API-heavy logic | Any runner |
| Docker | `docker` | Custom environment/tools | Linux only |

---

### 3.2 action.yml Anatomy

```yaml
name: "My Action"
description: "Does something useful"
author: "your-name"

branding:
  icon: "check-circle"
  color: "green"

inputs:
  version:
    description: "The version to use"
    required: true
    default: "latest"
  debug:
    description: "Enable debug mode"
    required: false
    default: "false"

outputs:
  result:
    description: "The result of the action"
    value: ${{ steps.run.outputs.result }}   # composite only

runs:
  using: "composite"           # or "node20" or "docker"
  steps:                        # composite
    - run: echo "Hello"
      shell: bash
```

**Pitfalls:**
- `inputs` **default** values are always strings.
- `outputs.value` is only for **composite** actions — JS actions set outputs via `core.setOutput()`.
- `branding` is required for **Marketplace** listing.

---

### 3.3 Composite Actions

**Composite Action 1 — Setup and Lint:**

```
my-lint-action/
├── action.yml
└── scripts/
    └── lint-config.json
```

```yaml
# my-lint-action/action.yml
name: "Lint Code"
description: "Installs dependencies and runs linting"
inputs:
  node-version:
    description: "Node.js version"
    required: false
    default: "20"
outputs:
  lint-status:
    description: "Lint exit code"
    value: ${{ steps.lint.outputs.status }}

runs:
  using: "composite"
  steps:
    - uses: actions/setup-node@v4
      with:
        node-version: ${{ inputs.node-version }}
    - run: npm ci
      shell: bash
    - id: lint
      run: |
        npm run lint && echo "status=pass" >> "$GITHUB_OUTPUT" || echo "status=fail" >> "$GITHUB_OUTPUT"
      shell: bash
```

**Composite Action 2 — Build & Package:**

```
my-build-action/
├── action.yml
```

```yaml
# my-build-action/action.yml
name: "Build and Package"
description: "Build application and prepare artifact"
inputs:
  build-command:
    description: "Build command to run"
    required: false
    default: "npm run build"
  output-dir:
    description: "Build output directory"
    required: false
    default: "dist"
outputs:
  artifact-path:
    description: "Path to built artifact"
    value: ${{ steps.set-path.outputs.path }}

runs:
  using: "composite"
  steps:
    - run: npm ci
      shell: bash
    - run: ${{ inputs.build-command }}
      shell: bash
    - id: set-path
      run: echo "path=${{ inputs.output-dir }}" >> "$GITHUB_OUTPUT"
      shell: bash
```

**Pitfalls:**
- Every `run:` step in a composite action **must** specify `shell:`.
- Composite actions can use `uses:` to call other actions (nested).
- Composite actions share the runner workspace — be mindful of `working-directory`.

---

### 3.4 JavaScript Actions

**File structure:**

```
my-js-action/
├── action.yml
├── index.js
├── package.json
└── dist/
    └── index.js       # bundled via ncc
```

**action.yml:**

```yaml
name: "PR Greeter"
description: "Posts a welcome comment on new PRs"
inputs:
  message:
    description: "Greeting message"
    required: false
    default: "Thanks for the PR!"
outputs:
  comment-id:
    description: "ID of the posted comment"

runs:
  using: "node20"
  main: "dist/index.js"
```

**index.js:**

```javascript
const core = require("@actions/core");
const github = require("@actions/github");

async function run() {
  try {
    const message = core.getInput("message");
    const token = core.getInput("github-token", { required: true });
    const octokit = github.getOctokit(token);
    const { context } = github;

    if (context.payload.pull_request) {
      const { data: comment } = await octokit.rest.issues.createComment({
        owner: context.repo.owner,
        repo: context.repo.repo,
        issue_number: context.payload.pull_request.number,
        body: message,
      });
      core.setOutput("comment-id", comment.id);
    }
  } catch (error) {
    core.setFailed(error.message);
  }
}

run();
```

**Key toolkit methods:**

| Method | Purpose |
|---|---|
| `core.getInput(name)` | Read action input |
| `core.setOutput(name, value)` | Set action output |
| `core.setFailed(message)` | Fail the action |
| `core.warning(message)` | Emit a warning annotation |
| `core.info(message)` | Informational log |
| `core.debug(message)` | Debug log (only with debug enabled) |
| `core.exportVariable(name, val)` | Set env var |
| `core.addPath(path)` | Add to PATH |
| `github.getOctokit(token)` | Authenticated REST/GraphQL client |
| `github.context` | Event context object |

**Pitfalls:**
- Bundle dependencies with `@vercel/ncc` and commit `dist/` — runners don't run `npm install` for actions.
- `core.setFailed()` sets the step to failed **and** logs the error.
- `runs.pre` and `runs.post` allow setup/cleanup scripts.

---

### 3.5 Docker Container Actions

**File structure:**

```
my-docker-action/
├── action.yml
├── Dockerfile
└── entrypoint.sh
```

**action.yml:**

```yaml
name: "Security Scan"
description: "Runs a security scan in a custom container"
inputs:
  scan-target:
    description: "Directory to scan"
    required: true
    default: "."
  severity:
    description: "Minimum severity to report"
    required: false
    default: "HIGH"
outputs:
  report-path:
    description: "Path to the scan report"

runs:
  using: "docker"
  image: "Dockerfile"
  args:
    - ${{ inputs.scan-target }}
    - ${{ inputs.severity }}
  env:
    SCAN_MODE: "full"
```

**Dockerfile:**

```dockerfile
FROM alpine:3.19

RUN apk add --no-cache bash jq curl

COPY entrypoint.sh /entrypoint.sh
RUN chmod +x /entrypoint.sh

ENTRYPOINT ["/entrypoint.sh"]
```

**entrypoint.sh:**

```bash
#!/bin/bash
set -e

SCAN_TARGET="$1"
SEVERITY="$2"

echo "Scanning $SCAN_TARGET at severity $SEVERITY..."
echo "report-path=/tmp/scan-report.json" >> "$GITHUB_OUTPUT"
echo "Scan complete."
```

**Pitfalls:**
- Docker actions run on **Linux only**.
- `image:` can be a `Dockerfile` (built at runtime) or a pre-built image (`image: docker://alpine:3.19`).
- The container runs as **root** by default.
- Inputs are passed as **args** in order — map them in `entrypoint.sh`.
- Each run rebuilds the image if using a Dockerfile (no layer caching by default) — pre-built images are faster.

**Mini-Quiz:**

1. Can Docker actions run on Windows? → **No.**
2. What happens when `image: "Dockerfile"` is used? → The Dockerfile is built at runtime.
3. How are inputs passed to the Docker container? → As `args` (command-line arguments).

---

### 3.6 Versioning & Publishing

**Semantic versioning strategy:**

```
v1.0.0  ← initial release
v1.0.1  ← patch (bug fix)
v1.1.0  ← minor (new feature, backward compatible)
v2.0.0  ← major (breaking change)
v1      ← floating major tag (always points to latest v1.x.x)
```

**Best practice:** Maintain a **floating major tag** (e.g., `v1`) that tracks the latest minor/patch:

```bash
git tag -fa v1 -m "Update v1 tag"
git push origin v1 --force
```

**Marketplace publishing:**
- Action must have a **unique** `name` in `action.yml`.
- Must include `branding` (icon + color).
- Publish via **GitHub Releases** with the "Publish this Action to the GitHub Marketplace" checkbox.

**Mini-Quiz:**

1. What is a floating major tag? → A tag like `v1` that always points to the latest `v1.x.x` release.
2. Is `branding` required for Marketplace? → **Yes.**

---

### 3.7 Testing Actions

**Strategies:**

1. **Unit tests** — test JS logic with Jest/Mocha (for JavaScript actions).
2. **Integration tests** — create a test workflow in the action's repo that runs the action.
3. **Local testing** — use `act` (nektos/act) to run workflows locally.
4. **E2E** — trigger the action in a separate test repo.

**Micro-example — test workflow for an action:**

```yaml
name: Test Action
on: push
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: ./                  # test the local action
        with:
          scan-target: "."
      - run: echo "Action ran successfully"
```

---

## Domain 4 — Manage GitHub Actions for the Enterprise (20–25 %)

### What the Exam Tries to Test Here

Enterprise governance: **policies**, **runner management**, **secrets scoping**, **audit/compliance**, and **billing**. This is about managing Actions at **scale** across many repos and teams.

### Concept Map

```
Enterprise Management
├── Action Policies
│   ├── Allow all / selected / GitHub-authored only
│   ├── Internal actions (private repos in org)
│   └── Marketplace vs verified creators
├── Reusable Workflow Governance
│   ├── Org-level templates
│   └── Required workflows (now "organization rules")
├── Runner Management
│   ├── GitHub-hosted vs self-hosted
│   ├── Runner groups (enterprise/org)
│   ├── Labels and routing
│   └── Scaling (auto-scale basics)
├── Secrets Management
│   ├── Org → repo → environment scoping
│   ├── Visibility controls
│   └── Precedence
├── Audit & Compliance
│   ├── Audit log events
│   ├── Workflow run history
│   └── Organization insights
└── Billing
    ├── Included minutes by plan
    ├── OS multipliers
    ├── Storage for artifacts/caches
    └── Spending limits
```

---

### 4.1 Policies: Allow / Deny Actions

**Enterprise / Org level settings:**

| Policy | Effect |
|---|---|
| **Allow all actions** | Any action from any source |
| **Allow local actions only** | Only `.github/workflows/` and actions in the same org/enterprise |
| **Allow select actions** | Specific list: GitHub-authored, verified creators, or named actions |

**Selecting specific actions:**

```
actions/checkout@*
actions/setup-node@*
my-org/*
```

**Pitfalls:**
- Enterprise policies **override** org policies — orgs can only be more restrictive.
- Internal (private repo) actions can be shared within the org if enabled.
- Forked repos respect the **fork's org** policies.

**Mini-Quiz:**

1. Can an org allow more actions than the enterprise permits? → **No.**
2. Can actions in private repos be shared within an org? → **Yes, if configured.**

---

### 4.2 Reusable Workflow Governance

**Standardization patterns:**

- Create reusable workflows in a **central `.github` repo** or dedicated org repo.
- Enforce via **required workflows** (org-level rulesets) — automatically apply workflows to selected repos.
- Use `workflow_call` inputs to allow customization while enforcing standards.

**Required workflows:**

- Configured in **organization settings → Rulesets**.
- Run automatically on matching repos/branches.
- Cannot be skipped by repo contributors.

**Mini-Quiz:**

1. Where are required workflows configured? → **Organization rulesets.**
2. Can repo contributors skip a required workflow? → **No.**

---

### 4.3 Runner Groups & Access Control

**Runner group hierarchy:**

```
Enterprise
├── Default runner group (all orgs)
└── Custom runner group → assigned to specific orgs
    Organization
    ├── Default runner group (all repos)
    └── Custom runner group → assigned to specific repos
```

**Self-hosted runner considerations:**

| Aspect | GitHub-Hosted | Self-Hosted |
|---|---|---|
| Lifecycle | Ephemeral (fresh each job) | Persistent (shared across jobs) |
| Maintenance | GitHub manages | You manage updates/security |
| Cost | Per-minute billing | Your infrastructure |
| Scaling | Automatic | Manual or auto-scale (ARC) |
| Security | Isolated | Shared environment risk |

**Labels:**

```yaml
runs-on: [self-hosted, linux, x64, gpu]   # must match ALL labels
```

**Pitfalls:**
- Self-hosted runners **persist state** between jobs — security risk for public repos.
- **Never** use self-hosted runners with public repos (untrusted code can access the runner).
- Runner groups at enterprise level can restrict to specific orgs.
- `runs-on` must match **all** specified labels.

**Mini-Quiz:**

1. Are self-hosted runners safe for public repos? → **No — untrusted code can run on them.**
2. How does `runs-on: [self-hosted, linux, gpu]` work? → Runner must have **all three** labels.

---

### 4.4 Secrets Scoping

**Hierarchy:**

```
Organization secrets
├── Visible to: all repos / private repos / selected repos
│
Repository secrets
├── Available to all workflows in the repo
│
Environment secrets
└── Available only in jobs targeting that environment
```

**Precedence (when names collide):**
Environment > Repository > Organization (most specific wins).

**Micro-example — org secret with repo visibility:**

In org settings: Create secret `DEPLOY_KEY` → select "Selected repositories" → choose repos.

**Pitfalls:**
- Org secrets can be **scoped** to all, private-only, or selected repos.
- Environment secrets require the job to declare `environment:`.
- Secrets are **not** passed to fork PRs (security protection).

**Mini-Quiz:**

1. Which secret wins: org, repo, or environment? → **Environment (most specific).**
2. Can you scope org secrets to specific repos? → **Yes.**

---

### 4.5 Audit, Logging & Compliance

**Audit log events for Actions:**

- `workflows.completed_workflow_run`
- `workflows.created_workflow_run`
- `org.update_actions_settings`
- `repo.update_actions_settings`

**Where to find:**
- Organization → Settings → Audit log.
- Enterprise → Settings → Audit log.
- API: `GET /orgs/{org}/audit-log?phrase=action:workflows`

**Workflow run history:**
- Available in the Actions tab per repo.
- Each run shows: trigger, duration, status, logs.
- Logs retained for **90 days**.

**Mini-Quiz:**

1. How long are workflow logs retained? → **90 days.**
2. Where is the org audit log? → **Organization Settings → Audit log.**

---

### 4.6 Billing, Minutes & Storage

**Minute multipliers:**

| Runner OS | Multiplier |
|---|---|
| Linux | 1× |
| Windows | 2× |
| macOS | 10× |

**Plan included minutes (approximate):**

| Plan | Minutes/month |
|---|---|
| Free | 2,000 |
| Team | 3,000 |
| Enterprise | 50,000 |

**Storage:**
- Artifacts and caches count toward **storage** quota.
- Default artifact retention: **90 days**.
- Reducing retention = reducing storage costs.

**Spending limits:**
- Default: **$0** (workflow pauses when minutes exhausted).
- Can set a dollar limit or unlimited.

**Pitfalls:**
- Self-hosted runner minutes are **free** — no billing.
- Larger GitHub-hosted runners have **per-minute pricing** (not included in plan).
- Caches and artifacts share the same storage pool.

**Mini-Quiz:**

1. What is the macOS minute multiplier? → **10×.**
2. Are self-hosted runner minutes billed? → **No.**
3. What happens when included minutes run out and spending limit is $0? → **Workflows pause/fail.**

---

## Domain 5 — Secure and Optimize Automation (10–15 %)

### What the Exam Tries to Test Here

Security best practices (permissions, secrets, OIDC, supply chain) and optimization techniques (caching, concurrency, matrix, path filters). Small domain but high-impact questions.

### Concept Map

```
Security & Optimization
├── Security
│   ├── Least privilege (permissions block)
│   ├── Secrets hygiene (masking, scoping)
│   ├── OIDC (OpenID Connect for cloud auth)
│   ├── Supply chain (SHA pinning, Dependabot)
│   └── Fork PR handling
└── Optimization
    ├── Caching (actions/cache)
    ├── Matrix parallelism
    ├── Concurrency groups
    ├── Path filtering / selective triggers
    └── Composite actions for reuse
```

---

### 5.1 Least Privilege Permissions

**Best practice:** Always declare `permissions` explicitly. Start restrictive, add as needed.

```yaml
permissions:
  contents: read
  pull-requests: write
```

**Available permission scopes:**

| Scope | Values |
|---|---|
| `actions` | read, write, none |
| `checks` | read, write, none |
| `contents` | read, write, none |
| `deployments` | read, write, none |
| `id-token` | write, none |
| `issues` | read, write, none |
| `packages` | read, write, none |
| `pages` | read, write, none |
| `pull-requests` | read, write, none |
| `security-events` | read, write, none |
| `statuses` | read, write, none |

**Mini-Quiz:**

1. What happens to permissions not listed in the `permissions:` block? → They default to `none`.

---

### 5.2 Secrets Hygiene

**Rules:**
- Never echo secrets: `echo ${{ secrets.TOKEN }}` → masked but **risky**.
- Use `::add-mask::` for dynamically generated sensitive values.
- Rotate secrets regularly.
- Scope secrets to environments for production workloads.
- Secrets are **redacted** in logs but can be extracted via encoding tricks — don't rely on masking alone.

**Micro-example — avoid leaking:**

```yaml
# BAD — secret could be logged in error messages
- run: curl -H "Authorization: Bearer ${{ secrets.TOKEN }}" https://api.example.com

# BETTER — use environment variable
- run: curl -H "Authorization: Bearer $TOKEN" https://api.example.com
  env:
    TOKEN: ${{ secrets.TOKEN }}
```

---

### 5.3 OIDC for Cloud Auth

**What it is:** GitHub Actions can request a **short-lived token** from cloud providers (AWS, Azure, GCP) without storing long-lived credentials.

**How it works:**
1. GitHub Actions mints a **JWT** (JSON Web Token) signed by GitHub.
2. Cloud provider's trust policy validates the JWT.
3. Cloud provider issues **temporary credentials**.

**Minimal YAML example (AWS):**

```yaml
permissions:
  id-token: write      # REQUIRED for OIDC
  contents: read

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: aws-actions/configure-aws-credentials@v4
        with:
          role-to-assume: arn:aws:iam::123456789012:role/GitHubActions
          aws-region: us-east-1
      - run: aws s3 ls
```

**Azure OIDC example:**

```yaml
permissions:
  id-token: write
  contents: read

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: azure/login@v2
        with:
          client-id: ${{ secrets.AZURE_CLIENT_ID }}
          tenant-id: ${{ secrets.AZURE_TENANT_ID }}
          subscription-id: ${{ secrets.AZURE_SUBSCRIPTION_ID }}
      - run: az webapp list
```

**Pitfalls:**
- `permissions: id-token: write` is **mandatory** — without it, the OIDC token request fails.
- The trust policy on the cloud side should restrict by repo, branch, and environment.
- OIDC eliminates **long-lived secrets** — the primary security benefit.

**Mini-Quiz:**

1. What permission is required for OIDC? → **`id-token: write`**
2. What does OIDC eliminate? → **Long-lived cloud credentials stored as secrets.**

---

### 5.4 Supply Chain Security

**Pin actions to SHA:**

```yaml
- uses: actions/checkout@a5ac7e51b41094c92402da3b24376905380afc29  # v4.1.6
```

**Dependabot for actions:**

```yaml
# .github/dependabot.yml
version: 2
updates:
  - package-ecosystem: "github-actions"
    directory: "/"
    schedule:
      interval: "weekly"
```

**Other measures:**
- Review third-party action source code before adopting.
- Use actions from **verified creators** or fork/mirror critical actions.
- Enable **code scanning** and **secret scanning** on repos.
- Use `permissions: {}` to disable all permissions when not needed.

---

### 5.5 Optimization Techniques

| Technique | How |
|---|---|
| **Caching** | `actions/cache` for dependencies |
| **Matrix parallelism** | Test across OS/versions concurrently |
| **Concurrency** | Cancel redundant runs |
| **Path filters** | Skip workflows for irrelevant changes |
| **Selective triggers** | Use `paths`, `branches` filters |
| **Composite actions** | Reuse step sequences, reduce duplication |
| **Larger runners** | More CPU/RAM for faster builds (paid) |
| **Artifact scoping** | Lower retention = less storage cost |

**Micro-example — skip CI for docs changes:**

```yaml
on:
  push:
    branches: [main]
    paths-ignore:
      - "docs/**"
      - "*.md"
```

---

### 5.6 Safe Handling of Fork PRs

**Key constraints:**

| Aspect | Fork PR Behavior |
|---|---|
| `GITHUB_TOKEN` | **Read-only** |
| Secrets | **Not available** |
| `pull_request` trigger | Runs on merge commit |
| `pull_request_target` | Runs on **base** branch (has secrets, dangerous) |
| Write permissions | **None** (cannot push, comment, etc. with default token) |

**Safe pattern — `pull_request_target` with explicit checkout:**

```yaml
on: pull_request_target
jobs:
  safe-check:
    runs-on: ubuntu-latest
    permissions:
      pull-requests: write
    steps:
      # Safe — checks out BASE branch (your code)
      - uses: actions/checkout@v4

      # DANGEROUS — if you checked out HEAD (PR code), it could run malicious code
      # Only do this if you're not executing any code from the PR
```

**Pitfalls:**
- `pull_request_target` has **write access** and **secrets** — never execute untrusted PR code in this context.
- Use `pull_request` (not `_target`) for running PR code safely (sandboxed).
- Label-gated workflows can provide a middle ground for fork PRs.

**Mini-Quiz:**

1. Are secrets available in fork PR workflows? → **No** (for `pull_request` trigger).
2. Why is `pull_request_target` dangerous? → It runs with write access and secrets on the base branch, potentially executing untrusted code.

---

## Scenario Playbook (10 Scenarios)

### Scenario 1 — PR CI (Lint + Test) with Cache

**Requirement:** On every PR to `main`, lint and test a Node.js app with npm caching.

```yaml
name: PR CI
on:
  pull_request:
    branches: [main]

permissions:
  contents: read

jobs:
  ci:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: 20
          cache: "npm"
      - run: npm ci
      - run: npm run lint
      - run: npm test
```

**Explanation:** `actions/setup-node` has built-in caching (no separate `actions/cache` needed). Permissions are minimal. The workflow runs only on PRs targeting `main`.

---

### Scenario 2 — Build + Upload Artifact

**Requirement:** Build a production bundle and upload it for later download.

```yaml
name: Build
on:
  push:
    branches: [main]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: 20
          cache: "npm"
      - run: npm ci
      - run: npm run build
      - uses: actions/upload-artifact@v4
        with:
          name: production-build
          path: dist/
          retention-days: 14
```

---

### Scenario 3 — Release Workflow with Environment Approvals

**Requirement:** Deploy to production on a published release, with environment protection.

```yaml
name: Release Deploy
on:
  release:
    types: [published]

permissions:
  contents: read
  id-token: write

jobs:
  deploy:
    runs-on: ubuntu-latest
    environment:
      name: production
      url: https://app.example.com
    concurrency:
      group: production-deploy
      cancel-in-progress: false
    steps:
      - uses: actions/checkout@v4
      - uses: aws-actions/configure-aws-credentials@v4
        with:
          role-to-assume: arn:aws:iam::123456789012:role/deploy
          aws-region: us-east-1
      - run: echo "Deploying ${{ github.event.release.tag_name }}"
```

**Explanation:** `environment: production` triggers approval gates configured in repo settings. Concurrency prevents parallel deploys. OIDC avoids stored credentials.

---

### Scenario 4 — Scheduled Maintenance Workflow

**Requirement:** Run a cleanup job every Sunday at 03:00 UTC.

```yaml
name: Weekly Cleanup
on:
  schedule:
    - cron: "0 3 * * 0"

permissions:
  contents: write

jobs:
  cleanup:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: |
          echo "Running cleanup tasks..."
          find . -name "*.tmp" -delete
          git config user.name "github-actions"
          git config user.email "github-actions@github.com"
          git add -A
          git diff --quiet --cached || git commit -m "chore: weekly cleanup"
          git push
```

---

### Scenario 5 — Manual Dispatch with Inputs

**Requirement:** Manually trigger a deployment with environment and version inputs.

```yaml
name: Manual Deploy
on:
  workflow_dispatch:
    inputs:
      environment:
        description: "Target environment"
        required: true
        type: choice
        options:
          - staging
          - production
      version:
        description: "Version to deploy"
        required: true
        type: string
        default: "latest"

jobs:
  deploy:
    runs-on: ubuntu-latest
    environment: ${{ inputs.environment }}
    steps:
      - run: echo "Deploying ${{ inputs.version }} to ${{ inputs.environment }}"
```

---

### Scenario 6 — Reusable Workflow + Consumer

**Requirement:** Centralized CI that multiple repos consume.

**Reusable (org-central/.github/workflows/node-ci.yml):**

```yaml
name: Node CI
on:
  workflow_call:
    inputs:
      node-version:
        type: string
        default: "20"
      test-command:
        type: string
        default: "npm test"

jobs:
  ci:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: ${{ inputs.node-version }}
          cache: "npm"
      - run: npm ci
      - run: ${{ inputs.test-command }}
```

**Consumer:**

```yaml
name: CI
on: [push, pull_request]
jobs:
  test:
    uses: org-central/.github/workflows/node-ci.yml@v1
    with:
      node-version: "22"
      test-command: "npm run test:ci"
```

---

### Scenario 7 — Matrix Test Across OS + Language Versions

**Requirement:** Test a Python library on multiple OS and Python versions.

```yaml
name: Matrix Tests
on: [push, pull_request]

jobs:
  test:
    runs-on: ${{ matrix.os }}
    strategy:
      fail-fast: false
      matrix:
        os: [ubuntu-latest, windows-latest, macos-latest]
        python: ["3.10", "3.11", "3.12"]
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-python@v5
        with:
          python-version: ${{ matrix.python }}
          cache: "pip"
      - run: pip install -r requirements.txt
      - run: pytest --junitxml=results.xml
      - uses: actions/upload-artifact@v4
        if: always()
        with:
          name: test-results-${{ matrix.os }}-${{ matrix.python }}
          path: results.xml
```

**Explanation:** `fail-fast: false` ensures all combinations run. Unique artifact names prevent collisions.

---

### Scenario 8 — Deployment with Environments and Concurrency

**Requirement:** Deploy to staging automatically, then production with approval.

```yaml
name: Deploy Pipeline
on:
  push:
    branches: [main]

permissions:
  contents: read
  id-token: write

jobs:
  build:
    runs-on: ubuntu-latest
    outputs:
      image-tag: ${{ steps.tag.outputs.tag }}
    steps:
      - uses: actions/checkout@v4
      - id: tag
        run: echo "tag=sha-$(git rev-parse --short HEAD)" >> "$GITHUB_OUTPUT"
      - run: echo "Building image ${{ steps.tag.outputs.tag }}"
      - uses: actions/upload-artifact@v4
        with:
          name: build-manifest
          path: manifest.json

  deploy-staging:
    needs: build
    runs-on: ubuntu-latest
    environment:
      name: staging
      url: https://staging.example.com
    concurrency:
      group: staging-deploy
      cancel-in-progress: true
    steps:
      - run: echo "Deploying ${{ needs.build.outputs.image-tag }} to staging"

  deploy-production:
    needs: [build, deploy-staging]
    runs-on: ubuntu-latest
    environment:
      name: production
      url: https://app.example.com
    concurrency:
      group: production-deploy
      cancel-in-progress: false
    steps:
      - run: echo "Deploying ${{ needs.build.outputs.image-tag }} to production"
```

---

### Scenario 9 — Caching Strategy (Multiple Ecosystems)

**Requirement:** Cache both npm and pip dependencies.

```yaml
name: Multi-Cache
on: push

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - uses: actions/cache@v4
        with:
          path: ~/.npm
          key: npm-${{ runner.os }}-${{ hashFiles('**/package-lock.json') }}
          restore-keys: npm-${{ runner.os }}-

      - uses: actions/cache@v4
        with:
          path: ~/.cache/pip
          key: pip-${{ runner.os }}-${{ hashFiles('**/requirements.txt') }}
          restore-keys: pip-${{ runner.os }}-

      - run: npm ci
      - run: pip install -r requirements.txt
      - run: npm test && pytest
```

---

### Scenario 10 — Action Authoring (Composite)

**Requirement:** Create a reusable action that sets up Node.js, installs, lints, and tests.

```yaml
# .github/actions/node-ci/action.yml
name: "Node CI Steps"
description: "Setup, install, lint, and test a Node project"
inputs:
  node-version:
    description: "Node.js version"
    default: "20"
  lint-command:
    description: "Lint command"
    default: "npm run lint"
  test-command:
    description: "Test command"
    default: "npm test"

runs:
  using: "composite"
  steps:
    - uses: actions/setup-node@v4
      with:
        node-version: ${{ inputs.node-version }}
        cache: "npm"
    - run: npm ci
      shell: bash
    - run: ${{ inputs.lint-command }}
      shell: bash
    - run: ${{ inputs.test-command }}
      shell: bash
```

**Usage:**

```yaml
jobs:
  ci:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: ./.github/actions/node-ci
        with:
          node-version: "22"
```

---

## Practice Exam (40 Questions)

### Multiple Choice Questions

**Q1.** Where must workflow files be stored?  
A) `.github/actions/`  
B) `.github/workflows/`  
C) `.workflows/`  
D) `workflows/`  

**Q2.** What is the default value of `strategy.fail-fast`?  
A) `false`  
B) `true`  
C) `null`  
D) Depends on the runner  

**Q3.** Which command sets a step output in modern GitHub Actions?  
A) `echo "::set-output name=key::value"`  
B) `echo "key=value" >> $GITHUB_OUTPUT`  
C) `export GITHUB_OUTPUT_key=value`  
D) `core.setOutput("key", "value")`  

**Q4.** What permission is required for OIDC token requests?  
A) `contents: write`  
B) `id-token: read`  
C) `id-token: write`  
D) `actions: write`  

**Q5.** Which trigger makes a workflow reusable by other workflows?  
A) `workflow_dispatch`  
B) `workflow_run`  
C) `workflow_call`  
D) `repository_dispatch`  

**Q6.** What happens to permissions not explicitly listed in the `permissions:` block?  
A) They inherit the default  
B) They are set to `read`  
C) They are set to `none`  
D) The workflow fails validation  

**Q7.** Can a single workflow step contain both `uses:` and `run:`?  
A) Yes, `uses:` runs first  
B) Yes, `run:` runs first  
C) No  
D) Only in composite actions  

**Q8.** What is the cache size limit per repository?  
A) 1 GB  
B) 5 GB  
C) 10 GB  
D) Unlimited  

**Q9.** Which context provides the current matrix variable values?  
A) `strategy`  
B) `matrix`  
C) `env`  
D) `job`  

**Q10.** How are secrets handled for pull requests from forks?  
A) All secrets are available  
B) Only org secrets are available  
C) Secrets are not available  
D) Only `GITHUB_TOKEN` is available with write access  

**Q11.** What is the minute multiplier for macOS runners?  
A) 1×  
B) 2×  
C) 5×  
D) 10×  

**Q12.** What does `concurrency.cancel-in-progress: true` cancel?  
A) The queued workflow run  
B) The currently running workflow run  
C) Both queued and running  
D) Neither — it just prevents new runs  

**Q13.** In which timezone does the `schedule` trigger operate?  
A) The repository owner's timezone  
B) EST  
C) UTC  
D) PST  

**Q14.** How do you pass all secrets to a reusable workflow without listing them?  
A) `secrets: all`  
B) `secrets: inherit`  
C) `secrets: *`  
D) `secrets: true`  

**Q15.** Which action type can only run on Linux runners?  
A) Composite  
B) JavaScript  
C) Docker  
D) All action types work on all platforms  

**Q16.** What is the maximum nesting depth for reusable workflows?  
A) 2 levels  
B) 3 levels  
C) 4 levels  
D) Unlimited  

**Q17.** What secret enables verbose step debug logging?  
A) `ACTIONS_DEBUG`  
B) `ACTIONS_STEP_DEBUG`  
C) `RUNNER_DEBUG`  
D) `GITHUB_DEBUG`  

**Q18.** What is the default artifact retention period?  
A) 7 days  
B) 30 days  
C) 90 days  
D) 365 days  

**Q19.** Do pushes with `GITHUB_TOKEN` trigger new workflow runs?  
A) Yes, always  
B) Yes, if the workflow has `on: push`  
C) No  
D) Only if `permissions: contents: write` is set  

**Q20.** Which is the most secure way to pin an action version?  
A) Branch name (`@main`)  
B) Major version tag (`@v4`)  
C) Full version tag (`@v4.1.6`)  
D) Full commit SHA  

### Scenario-Based Questions

**Q21.** Your workflow needs to deploy only when code is pushed to `main` AND the push modifies files in `src/`. Which configuration achieves this?  
A) `on: push: branches: [main]` with `paths: [src/**]`  
B) `on: push: branches: [main]` with a separate `if:` condition  
C) `on: push` with `paths: [src/**]` only  
D) Both A and C would work  

**Q22.** A matrix job with 3 OS × 3 Node versions has `fail-fast: true`. One combination fails. What happens?  
A) All 9 jobs continue running  
B) Only the failed job stops  
C) All remaining in-progress matrix jobs are cancelled  
D) The workflow waits for all to finish, then marks as failed  

**Q23.** You need a step to always run (even on failure or cancellation) to upload test results. What `if:` should you use?  
A) `if: failure()`  
B) `if: always()`  
C) `if: success() || failure()`  
D) `if: !cancelled()`  

**Q24.** Your organization wants to ensure all repos run a security scan. What GitHub feature enforces this?  
A) Branch protection rules  
B) Required workflows (organization rulesets)  
C) CODEOWNERS file  
D) Repository templates  

**Q25.** A developer reports that `${{ secrets.API_KEY }}` shows as empty in a fork PR. Why?  
A) The secret name is wrong  
B) Secrets are not available in fork PRs  
C) The `permissions` block is missing  
D) The secret expired  

**Q26.** You want Job B to run after Job A, even if Job A is skipped. What do you add to Job B?  
A) `needs: [a]` only  
B) `needs: [a]` and `if: always()`  
C) Remove the `needs` key  
D) `needs: [a]` and `if: success()`  

**Q27.** A workflow sets `env: FOO=bar` at the workflow level and `env: FOO=baz` at the step level. What value does `$FOO` have in that step?  
A) `bar`  
B) `baz`  
C) Empty  
D) Error — duplicate env names aren't allowed  

**Q28.** You're authoring a composite action and a step fails with "shell is required." What did you forget?  
A) The `runs.using` value  
B) The `shell:` property on a `run:` step  
C) The `action.yml` file  
D) The `steps:` key  

**Q29.** An enterprise admin sets Actions policy to "Allow local actions only." Can repos in the org use `actions/checkout@v4`?  
A) Yes — it's a GitHub-authored action  
B) No — only actions defined in the org/enterprise are allowed  
C) Only if the repo forks the action  
D) Only with an org admin override  

**Q30.** Your cache key is `npm-Linux-abc123` and `restore-keys` is `npm-Linux-`. The lock file changed. What happens?  
A) Cache miss, no restore  
B) Exact match on key  
C) Prefix match on restore-keys, cache restored, new cache saved after job  
D) Error — cache keys cannot contain hashes  

**Q31.** You configure `permissions: {}` at the workflow level. What permissions does `GITHUB_TOKEN` have?  
A) Default read/write  
B) Read-only  
C) No permissions at all  
D) Error — empty permissions block is invalid  

**Q32.** A reusable workflow is called with `uses: my-org/shared/.github/workflows/ci.yml@v1`. Where is the `uses:` placed?  
A) Under `steps:`  
B) Under `jobs.<id>` (job level)  
C) Under `on:`  
D) Under `env:`  

**Q33.** A service container `redis` is defined in a job running on `ubuntu-latest` (VM, not container job). How do you connect to Redis from a step?  
A) `redis:6379`  
B) `localhost:6379`  
C) `127.0.0.1:6380`  
D) `services.redis.host:6379`  

**Q34.** You want to set an environment variable dynamically in Step 1 and use it in Step 2. Which approach works?  
A) `export MY_VAR=hello` in Step 1  
B) `echo "MY_VAR=hello" >> $GITHUB_ENV` in Step 1  
C) `env: MY_VAR: hello` in Step 1  
D) Both A and C  

**Q35.** A workflow uses `pull_request_target` and checks out the PR head ref. Why is this dangerous?  
A) It uses too many minutes  
B) Untrusted PR code executes with write access and secrets  
C) The checkout fails on forks  
D) The PR cannot be merged  

**Q36.** Which of these is NOT a valid `workflow_dispatch` input type?  
A) `string`  
B) `boolean`  
C) `choice`  
D) `array`  

**Q37.** You need to prevent a self-hosted runner from being used by every repo in the org. What do you create?  
A) A branch protection rule  
B) A runner group with selected repo access  
C) An environment protection rule  
D) A CODEOWNERS restriction  

**Q38.** Your job has `continue-on-error: true` on a step that fails. What is the job's final status?  
A) `failure`  
B) `success`  
C) `skipped`  
D) `cancelled`  

**Q39.** Which file configures Dependabot to automatically update GitHub Actions versions?  
A) `.github/workflows/dependabot.yml`  
B) `.github/dependabot.yml`  
C) `dependabot.yml` (root)  
D) `.github/actions/dependabot.yml`  

**Q40.** A workflow has jobs: `build`, `test` (needs build), `deploy` (needs test). Build succeeds, test fails. What happens to deploy?  
A) Deploy runs with `failure()` status  
B) Deploy is skipped  
C) Deploy runs but fails  
D) Deploy waits for test to be re-run  

---

## Answer Key & Explanations

| # | Answer | Explanation |
|---|---|---|
| 1 | **B** | Workflow files must be in `.github/workflows/`. |
| 2 | **B** | `fail-fast` defaults to `true` — one failure cancels the rest. |
| 3 | **B** | Modern approach: `echo "key=value" >> $GITHUB_OUTPUT`. Option D is for JS actions only. |
| 4 | **C** | `id-token: write` is required for OIDC. There is no `id-token: read`. |
| 5 | **C** | `workflow_call` makes a workflow reusable. |
| 6 | **C** | Unlisted permissions default to `none` when `permissions:` is set. |
| 7 | **C** | A step cannot have both `uses:` and `run:`. |
| 8 | **C** | Cache limit is 10 GB per repository. |
| 9 | **B** | `matrix` context provides current matrix variable values. |
| 10 | **C** | Secrets are not available for fork PRs (security measure). |
| 11 | **D** | macOS runners use a 10× minute multiplier. |
| 12 | **B** | `cancel-in-progress: true` cancels the currently running workflow. |
| 13 | **C** | Schedule triggers use UTC exclusively. |
| 14 | **B** | `secrets: inherit` passes all caller's secrets. |
| 15 | **C** | Docker actions can only run on Linux runners. |
| 16 | **C** | Reusable workflows can nest up to 4 levels deep. |
| 17 | **B** | `ACTIONS_STEP_DEBUG` secret enables verbose step logging. |
| 18 | **C** | Default artifact retention is 90 days. |
| 19 | **C** | Pushes with `GITHUB_TOKEN` do not trigger new workflow runs (prevents recursion). |
| 20 | **D** | Full commit SHA is immutable and most secure. |
| 21 | **A** | Combining `branches` and `paths` filters correctly narrows the trigger. |
| 22 | **C** | `fail-fast: true` cancels remaining in-progress matrix jobs on any failure. |
| 23 | **B** | `always()` runs even on failure or cancellation. Note: `success() \|\| failure()` skips on cancellation. |
| 24 | **B** | Required workflows (org rulesets) enforce workflows across repos. |
| 25 | **B** | Secrets are not passed to workflows triggered by fork PRs. |
| 26 | **B** | `if: always()` makes the job run regardless of upstream status (including skipped). |
| 27 | **B** | Step-level env overrides workflow-level (most specific wins). |
| 28 | **B** | Composite action `run:` steps must include `shell:`. |
| 29 | **B** | "Allow local actions only" blocks all external actions, including GitHub-authored ones. |
| 30 | **C** | `restore-keys` prefix match restores the most recent matching cache; a new cache is saved post-job. |
| 31 | **C** | `permissions: {}` grants no permissions to `GITHUB_TOKEN`. |
| 32 | **B** | Reusable workflows are called at the job level with `uses:`. |
| 33 | **B** | On VM runners (not container jobs), services are accessed via `localhost:<port>`. |
| 34 | **B** | `$GITHUB_ENV` persists env vars across steps; `export` only affects the current shell. |
| 35 | **B** | `pull_request_target` runs with write access/secrets; checking out untrusted PR code is dangerous. |
| 36 | **D** | `array` is not a valid `workflow_dispatch` input type. Valid: string, boolean, choice, number, environment. |
| 37 | **B** | Runner groups with selected repo access control which repos can use specific runners. |
| 38 | **B** | `continue-on-error: true` makes the step's failure non-fatal — the job still succeeds. |
| 39 | **B** | Dependabot config lives at `.github/dependabot.yml`. |
| 40 | **B** | Deploy is skipped because its dependency (test) failed. |

---

## Final Revision Checklist

- [ ] I can write a workflow file from scratch (name, on, jobs, steps)
- [ ] I know all major triggers and their filters (branches, paths, types)
- [ ] I understand the difference between `uses:` and `run:`
- [ ] I can set up a strategy matrix with include/exclude
- [ ] I know `fail-fast` defaults to `true`
- [ ] I can chain jobs with `needs` and pass outputs between them
- [ ] I understand env var precedence (step > job > workflow)
- [ ] I can use `$GITHUB_OUTPUT` and `$GITHUB_ENV` correctly
- [ ] I know the permissions block and least privilege principle
- [ ] I can author and consume reusable workflows (`workflow_call`)
- [ ] I know `secrets: inherit` passes all secrets
- [ ] I understand artifacts (upload/download, unique names in v4)
- [ ] I can configure caching with proper keys and restore-keys
- [ ] I understand environments, protection rules, and approvals
- [ ] I can read workflow logs and enable debug logging
- [ ] I know common failure patterns and how to fix them
- [ ] I understand push vs pull_request event payload differences
- [ ] I can use workflow commands (::notice, ::error, ::add-mask)
- [ ] I know the three action types (composite, JavaScript, Docker)
- [ ] I can write a valid `action.yml` with inputs/outputs
- [ ] I remember `shell:` is required in composite action `run:` steps
- [ ] I understand Docker actions are Linux-only
- [ ] I know versioning strategy (SHA > tag > branch)
- [ ] I understand enterprise action policies and runner groups
- [ ] I know secrets scoping: environment > repo > org
- [ ] I know billing multipliers: Linux 1×, Windows 2×, macOS 10×
- [ ] I understand OIDC and `id-token: write` requirement
- [ ] I know fork PR restrictions (no secrets, read-only token)
- [ ] I know `pull_request_target` risks
- [ ] I can configure Dependabot for action updates
- [ ] I understand cache limits (10 GB) and eviction (7 days)
- [ ] I know `concurrency.cancel-in-progress` cancels the running job
- [ ] I can set up services for integration tests (Linux only)
- [ ] I know default job timeout is 360 minutes
- [ ] I've taken the practice exam and scored ≥ 70 %

---

> **Good luck on the GH-200 exam!** 🎯  
> Review any section where you feel unsure, run the YAML snippets, and trust your preparation.
