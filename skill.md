You are a GitHub Actions Certification Instructor and Technical Writer.

GOAL
Create ONE high-quality Markdown file: "GH-200_GitHub_Actions_Study_Guide.md" that fully prepares me for the GitHub Actions certification exam (GH-200). It must be exam-focused, but also build conceptual clarity.

AUDIENCE
I’m preparing for the certification exam. Assume I know basic Git and CI/CD ideas but need GitHub Actions mastery.

NON-NEGOTIABLE OUTPUT RULES
1) Output ONLY the final Markdown content for a single .md file (no extra commentary).
2) Include a clean Table of Contents with working anchor links.
3) Use clear headings, bullet points, and many short code examples.
4) For EVERY concept: include (a) what it is, (b) why it matters for the exam, (c) syntax, (d) a minimal example, (e) common mistakes/pitfalls, (f) 1–3 quick check questions.
5) Include at least:
   - 20 “micro-examples” (tiny YAML snippets)
   - 8 complete workflows (end-to-end examples)
   - 2 reusable workflows (workflow_call) + 2 call-site examples
   - 2 composite actions + 1 JavaScript action + 1 Docker container action (with file structures)
   - 40 practice questions total (mix MCQ + scenario-based), with answers + brief explanations
6) Keep examples realistic: lint/test/build, artifacts, caching, deploy via environment, PR checks, release workflow.

EXAM BLUEPRINT (FOLLOW THIS STRUCTURE + WEIGHTING)
Create major sections exactly in this order (use these headings):
1) Author and manage workflows (20–25%)
2) Consume and troubleshoot workflows (15–20%)
3) Author and maintain actions (15–20%)
4) Manage GitHub Actions for the enterprise (20–25%)
5) Secure and optimize automation (10–15%)

Within each section, break into subtopics and cover the must-know details.

CONTENT REQUIREMENTS (MUST COVER)
A) Core workflow authoring
- Workflow structure: name, on, jobs, steps, uses vs run
- Triggers: push, pull_request, workflow_dispatch, schedule, release, workflow_call
- Filters: branches/tags/paths, types, inputs
- Contexts: github, env, vars, secrets, job, steps, runner, strategy, needs
- Expressions: ${{ }}, functions (success(), failure(), always(), cancelled()), contains, startsWith, format, join
- Conditionals: if at job and step level, short-circuiting, needs.* outputs
- Environment variables: env at workflow/job/step; GITHUB_ENV; precedence rules
- Outputs: step outputs, job outputs, reusable workflow outputs
- Permissions: permissions block, least privilege, GITHUB_TOKEN basics

B) Job orchestration & advanced YAML
- needs dependency graphs
- strategy.matrix (include/exclude), fail-fast, max-parallel
- concurrency groups + cancel-in-progress
- services (databases) for integration tests
- timeouts, continue-on-error, defaults.run.shell & working-directory
- reusable workflows (workflow_call) and organization standards
- artifacts: upload/download, retention concepts
- caching: actions/cache, key/restore-keys, what to cache (npm/maven/gradle/pip)
- environments: protection rules conceptually, approvals, environment secrets

C) Runners & execution
- GitHub-hosted vs self-hosted runners
- runner labels, runner groups, scaling basics
- OS differences in shell/path handling
- secrets and environment behavior on forks/PRs (conceptually)
- troubleshooting runner failures (permissions, checkout, auth, missing tools)

D) Consuming & troubleshooting workflows
- Reading workflow logs, step debug logging, re-run jobs
- Common failure patterns (yaml indentation, quoting, expression mistakes)
- Checkout, token auth issues, permissions issues
- Understanding event payload differences (push vs PR)
- Using workflow commands: ::notice ::warning ::error, grouping, masking, set-output replacement patterns
- Diagnosing cache misses, artifact not found, matrix issues
- Pinning action versions (tags vs SHAs) and why it matters

E) Authoring & maintaining actions
- Types: composite, JavaScript, Docker
- action.yml anatomy (name, description, inputs, outputs, runs, branding)
- Versioning: tags, releases, semantic versioning strategy
- Publishing and reuse (including Marketplace conceptually)
- Toolkits: @actions/core, @actions/github (high level)
- Docker action considerations (entrypoint, args, permissions, caching layers)
- Testing actions (basic strategies) and troubleshooting

F) Enterprise/Org management
- Policies to allow/deny actions (GitHub-hosted vs Marketplace vs internal)
- Reusable workflow governance and standardization
- Runner groups access control (conceptual)
- Secrets: org/repo/environment scopes; how scoping affects usage
- Audit/logging concepts; managing Actions at scale
- Billing/minutes/storage basics (conceptual), artifact retention impacts

G) Security & optimization (must be strong)
- Least privilege permissions + explicit permissions blocks
- Secrets hygiene: avoid echoing, masking, environment protection
- OIDC overview for cloud auth (conceptual + minimal YAML example)
- Pin actions to commit SHA, supply chain basics
- Dependabot/updates conceptually (if relevant)
- Optimize: caching, matrix parallelism, concurrency, skipping paths, selective triggers, composite actions reuse
- Safe handling of PRs from forks (conceptual constraints)

FORMAT REQUIREMENTS (VERY IMPORTANT)
1) Start with:
   - A 10–15 line “How to use this guide” section
   - A “High-yield Quick Sheet” table (1-page feel) with the most testable items.
2) For each main domain section:
   - Begin with a “What the exam tries to test here” subsection
   - Then “Concept map (text-based)” using an ASCII tree
   - Then subtopics with: Definition → Syntax → Example → Pitfalls → Mini-quiz
3) Include “Scenario Playbook” near the end:
   - 10 scenarios (e.g., PR checks, release deploy, matrix tests, caching strategy, reusable workflow, action authoring)
   - Each scenario: requirement → best workflow snippet → explanation
4) End with:
   - Practice exam (40 questions) + answer key + brief explanations
   - Final revision checklist (tick-box style)

EXAMPLE QUALITY BAR
- All YAML must be valid and consistent.
- Use modern best practices (e.g., permissions, pinning guidance, outputs approach).
- Include multiple realistic workflows:
   1) PR CI (lint+test) with cache
   2) Build + upload artifact
   3) Release workflow with environment approvals concept
   4) Scheduled maintenance workflow
   5) Manual dispatch with inputs
   6) Reusable workflow + consumer workflow
   7) Matrix test across OS + language versions
   8) Deployment-style workflow (generic) with environments and concurrency

Now generate the complete Markdown file.