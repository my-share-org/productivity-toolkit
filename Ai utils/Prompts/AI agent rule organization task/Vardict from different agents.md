# Prompt:
You are reviewing a documentation-architecture problem for AI agent rules. Give a verdict. Do not rewrite the rule files. Do not agree by default. If the proposed system is weak, say where it fails and what to do instead.

## How the rules work today

I keep AI agent rules in one folder:

- `Global-rule.md` — 113 lines. Added to the agent's general memory. It applies to every project and every session, on every stack.
- `django-rule.md` — 328 lines. Django/DRF backend.
- `fastapi-rule.md` — 380 lines. FastAPI backend. It is written as a mirror of the Django file (same section order, plus a Django→FastAPI naming map and two FastAPI-only sections on async and anti-patterns).
- `frontend-rule.md` — 246 lines. React/Next.js.
- `n8n-rule.md` — 188 lines. n8n workflows.

That is 1,255 lines in total. A normal session loads the global file plus one stack file: about 441 lines for Django, 493 for FastAPI, 359 for frontend, 301 for n8n.

The global file already covers architecture, code quality, security, error handling, performance, testing, configuration, DevOps, git, and agent behavior.

The Django file then covers, in this order: a reference project tree (about 80 lines), app organization, API versioning, models, serializers, views, rate limiting, database queries, error handling, auth/OTP/email/notifications, domain patterns (payments, ledgers, callbacks, dashboards, metering, debug routes — about 44 lines), logging, shared utilities, Celery, Docker, Bruno API collections (about 28 lines), docs, testing (about 7 lines), Django security, and a pre-write checklist (about 34 lines). FastAPI repeats that shape. Frontend and n8n repeat the same kinds of concerns in their own words: structure, errors, security, testing, rate limits, domain patterns, and a pre-write checklist.

## The problem

If an agent reads a full stack file at the start of a session, it pays for every section even when the task needs one of them. Testing rules matter when writing tests. API-building rules matter when building an API. Payment, Bruno, Docker, and Celery rules do not matter for either of those tasks. The files are too big for "load everything, every time."

## What I am trying to do

Two changes:

1. Across stacks: compare the stack files, take the rules that are actually common, and move those into a common rules folder. Each stack folder then keeps only what is specific to that stack, including that stack's project structure and file layout.
2. Within a stack: split what remains so a session does not read the whole stack file. A short README maps each rule file to the task it is for. The agent reads that README, picks the file that matches the task, and reads only that file.

Global rules still apply to every session. Common stack rules would apply when doing that kind of work on any stack. Stack files would apply only for that stack's structure and its genuinely different rules.

## What I want from you

1. Verdict on the problem. Is the cost real, and is "the agent loads a whole stack file per session" the right diagnosis?
2. Verdict on the proposed system. Does "common folder + stack-specific folder + a README that points at one task file" hold up? Say what breaks it: rules that look common but are not, rules that are common but still must be visible while writing stack-specific code, a README that becomes a second copy of the rules, or a loader that still attaches the whole folder.
3. One framework or pathway to follow. Short enough to act on. Name the folders, what is allowed in each, how an agent chooses files for a session, and how a rule stays in one place when stacks would otherwise repeat it. Do not design the full file tree section by section, and do not rewrite the rules.
---

# Vardict from different AI agents: 
## Antigravity (Gemini Flash 3.8, effort-high):
```markdown
## Part 1: Verdict on the Problem

### 1. Is the cost real?
**Financially and technically regarding token limits, no.** 
450 lines of Markdown is roughly 2,500–3,500 tokens. In modern context windows (128k–1M+), 3,500 tokens costs fractions of a cent and takes <1% of the window. If the concern is token billing or filling the context window, it is a non-issue.

**Cognitively for the model, yes.**
The real problem is **instruction dilution and cross-task interference**. When an LLM is given 450 lines of mixed constraints, rules compete for attention. Feeding Celery retry policies, Bruno collection formats, and payment ledger idempotency rules to an agent tasked with fixing a CSS layout or writing a simple Pydantic validator increases hallucination, causes rule misses, and prompts premature over-engineering.

### 2. Is "the agent loads a whole stack file per session" the right diagnosis?
**No. The right diagnosis is "the stack file mixes invariant baseline conventions with episodic domain recipes."**

Your file is not failing because it is 400 lines long; it is failing because it conflates two fundamentally different kinds of information:
- **Baseline Invariants (Always needed):** Naming conventions, project structure, error response envelope shape, dependency injection style, ORM query rules (e.g., N+1 prevention).
- **Episodic Recipes (Task-specific):** How to structure Celery tasks, how to write Bruno test scripts, payment callback flows, Docker configurations, and test runners.

---

## Part 2: Verdict on the Proposed System

The proposed system (**Common folder + Stack folder + README router to one task file**) is **fragile and will degrade output quality**. Here is why:

### 1. Cross-Cutting Blind Spots (The Fatal Flaw)
Software engineering tasks are rarely isolated to a single file boundary. If an agent is tasked with *"Add an endpoint to record an order payment,"* what file does it pick from your README?
- If it picks `api.md`, it forgets your `models.md` / query safety rules (`select_related`, locking).
- If it picks `payments.md`, it forgets the standard error response envelope format from `errors.md`.
- If it picks `views.md`, it misses rate limiting.

Breaking rules by micro-tasks assumes tasks are orthogonal. In reality, writing any feature touches API design, data modeling, error handling, and security simultaneously.

### 2. The Multi-Hop Agent Failure
Relying on an agent to autonomously read a README, decide which file it needs, and invoke a tool to read that file before doing work adds a point of failure at step zero. Agents frequently:
- Skip reading the README entirely and proceed based on default training memory.
- Misdiagnose the task category.
- Read only the selected file and ignore critical surrounding context.

### 3. The "False Commonality" Abstraction Trap
Rules that appear common across Django, FastAPI, React, and n8n usually sound like: *"Handle errors gracefully,"* *"Follow domain-driven design,"* or *"Validate input at the boundary."*
- When moved to a `common/` folder, they become so abstract that they are useless for steering an LLM's concrete code output.
- The value of an agent rule lies in framework-specific idiom enforcement (e.g., `raise HTTPException(...)` vs `return Response(status=...)`, or Pydantic schemas vs DRF serializers). A shared "common backend" layer creates a diluted middle tier that adds friction without providing concrete constraints.

### 4. README Drift
A README mapping tasks to files inevitably either becomes:
- Too sparse (causing the agent to pick the wrong file), or
- A duplicate summary of the rules themselves, creating two sources of truth that drift out of sync.

---

## Part 3: The Framework to Follow

Replace the **"micro-split by task with README routing"** model with a **Two-Tier System: Invariants (Core) vs. Recipes (On-Demand)**.

### 1. Folder Structure

```text
rules/
├── global.md                   # Stack-agnostic baseline (Repo/Git, Security invariants, Agent behavior)
├── django/                     # (or fastapi/, frontend/, n8n/)
│   ├── core.md                 # Framework Invariants (Always loaded for that stack: ~100-140 lines)
│   └── recipes/                # Episodic Reference Files (Loaded ONLY when relevant)
│       ├── testing.md          # Fixtures, test factories, mocking conventions
│       ├── background-jobs.md  # Celery/workers, task signatures, retry logic
│       ├── domain-patterns.md  # Ledgers, metering, payment webhooks
│       └── tooling.md          # Docker setup, Bruno API specs
```

### 2. What Is Allowed Where (Strict Boundaries)

*   **`global.md` (Universal Invariants):** Only rules that apply regardless of whether the developer is writing Terraform, Bash, Python, or TypeScript (e.g., git branching rules, secret handling, general tone, safe command execution).
*   **`<stack>/core.md` (Framework Invariants):** Only the non-negotiable patterns needed for *any* standard edit in that codebase.
    *   Directory tree layout.
    *   Standard response envelope / error schema.
    *   Data layer rules (e.g., indexing, N+1 query prevention).
    *   Auth context injection.
    *   *Target size: 100–150 lines.*
*   **`<stack>/recipes/*.md` (Episodic Guides):** Specialized, deep domain guidance.
    *   Must contain implementation details, templates, or edge-case workflows.
    *   Never contains basic code style or core project layout.

### 3. How the Agent Selects Files (Deterministic Index, No README)

Instead of a separate README router, place a **deterministic trigger index** directly at the bottom of `<stack>/core.md`:

```markdown
### On-Demand Context
Before modifying or creating features in these areas, read the corresponding file:
- Celery / Async Workers: `rules/django/recipes/background-jobs.md`
- Tests / Fixtures / Mocks: `rules/django/recipes/testing.md`
- Payments / Billing / Ledgers: `rules/django/recipes/domain-patterns.md`
- Bruno Collections / Docker: `rules/django/recipes/tooling.md`
```

**Why this works:**
- The agent gets `global.md` + `<stack>/core.md` on every run. This ensures it **never** violates your project structure, ORM best practices, or error formats.
- The router is inline with the core instructions, requiring zero preliminary tool-calling hops just to understand what rules exist.
- When an agent sees a prompt mentioning *"Write tests for Celery payments"*, it reads the core invariants immediately and knows explicitly to pull in `testing.md`, `background-jobs.md`, and `domain-patterns.md`.

### 4. Single-Source of Truth Rule (Eliminating Duplication)

**Ban the `common/` stack folder.** Use the **"Abstract in Global, Concrete in Stack"** principle:
- If a rule cannot be expressed with framework-specific syntax or concrete examples, it belongs in `global.md` (e.g., *"Never commit raw secrets; mock external HTTP calls"*).
- If it requires code or framework terminology (even if Django and FastAPI solve it similarly), define it **independently in each stack's `core.md`** using that framework's native idioms. Duplicating 8 lines of concrete implementation guidance between Django and FastAPI is strictly better than maintaining an abstract shared abstraction layer.
```

## Chat GPT (Default - Thinking):
```markdown
# Verdict: The problem is real, but the solution needs one important correction

I agree with the goal of reducing irrelevant rule loading. I would not implement the proposed system exactly as described. The strongest design is a layered, task-selective rules architecture with explicit loading behavior—not simply a common folder, several stack folders, and a README that tells the agent what to read.

Your current files have two separate problems:

1. Content duplication: the same engineering principles are expressed in the global file and again in stack files, and Django and FastAPI mirror one another.
2. Loading granularity: a task-specific agent session receives rules that are unrelated to its current work.

These problems require different solutions. Moving common rules addresses duplication; splitting and selectively loading rules addresses relevance. Doing only one leaves the other problem intact.

## 1. Is the loading cost real?

Yes, but line count alone is not the right measure.

| Issue                                            | Verdict                                                                           |
| ------------------------------------------------ | --------------------------------------------------------------------------------- |
| Loading 328–380 lines for every backend task     | Often unnecessarily broad                                                         |
| Loading a global file plus a complete stack file | Simple and reliable, but potentially wasteful                                     |
| Paying for irrelevant rules                      | Real in terms of context consumption, attention, and instruction competition      |
| Splitting everything into tiny files             | Not automatically better; discovery and loading overhead can outweigh the savings |
| Assuming every irrelevant line has an equal cost | Incorrect                                                                         |

The important cost is not just tokens. It is the chance that relevant requirements get buried among unrelated instructions, that the agent follows a rule outside its intended context, or that conflicting instructions compete for attention.

For example, a Django task that only adds serializer validation may not need payment-ledger patterns, Celery conventions, Bruno collection maintenance, or Docker deployment instructions. Loading those sections adds little value.

However, a serializer change might affect database queries, authorization, error responses, or tests. Those adjacent rules may still matter even if the task is not explicitly a testing or database task.

### Is “the whole stack file loads every session” the correct diagnosis?

It is correct if your actual agent configuration attaches the entire stack file to every session. Your description establishes that as the current behavior.

But do not assume that splitting files will fix it until you inspect the loader. Some agent systems automatically load every file in a configured rules directory, import all referenced files, or attach a complete folder as context. In those systems, creating 20 files and a README may change nothing.

The correct diagnosis is therefore:

> The current loading policy is too broad, and the rules are not organized into independently selectable units.

Measure the actual behavior using loaded file lists, token usage if available, and a few representative tasks. That establishes whether the bottleneck is file size, loader behavior, duplication, or all three.

## 2. Does the proposed architecture hold up?

Yes, with four conditions. The folder structure is reasonable, but the rules need clear ownership, scope, and loading semantics.

### Where the proposal can fail

A. Rules that look common but are not

“Use consistent error handling” sounds universal. The implementation is not. Django REST Framework may use exception handlers and serializer errors; FastAPI may use exception handlers, Pydantic validation, and dependency injection. React has yet another error-handling model.

Fix: Extract the invariant principle, not the framework-specific implementation. Keep implementation details in the owning stack file.

B. Common rules that must remain visible during stack-specific work

A shared security rule is still relevant when writing a Django view. A shared testing principle still applies when writing a FastAPI test. If the agent loads only the task file, it may miss those constraints.

Fix: Separate always-applicable rules from conditionally applicable rules. Every session loads the global rules and the applicable common baseline. Task-specific files add detail; they do not replace that baseline.

C. The README becomes a second rulebook

If the README starts explaining how to write serializers, secure endpoints, or test APIs, it becomes a duplicate source of truth. The two versions will eventually disagree.

Fix: Make the README an index and routing guide only. It should state when to load each file, not restate the rules inside it.

D. The loader still attaches the whole folder

A perfect file structure is useless if the agent automatically reads every file under the rules directory. A README does not guarantee selective loading.

Fix: Verify the actual loader's behavior. Use explicit file references or a supported conditional-loading mechanism. If the tool cannot load files dynamically, use its supported rule scoping or separate rule entry points.

### The additional risk: splitting too aggressively

There is a trade-off between loading too much and making the agent navigate too many files.

If a normal API task requires the agent to discover and read seven small files, the savings may be marginal, and the chance of missing a relevant rule increases.

Use task-sized modules, not one file per topic or individual rule. A single API implementation module may need conventions for views, validation, permissions, responses, and related queries. Keep those together when they are commonly used together.

Also, don't automatically classify all of your existing global rules as universal. A global file containing architecture, DevOps, testing, and security may itself be too broad. Universal principles belong there; detailed procedures should be moved to the appropriate scope.

## 3. The framework I recommend

Use a four-layer, single-source-of-truth architecture, with explicit loading at each layer.

Layer 1 — Global

Always load

Universal agent behavior, cross-project principles, and rules that genuinely apply to every task and stack.

Layer 2 — Common engineering

Conditional

Stack-independent rules for security, testing, API design, error handling, database safety, and other shared engineering concerns. Load the relevant modules for the task.

Layer 3 — Stack-specific

Conditional

Django, FastAPI, React/Next.js, and n8n conventions, including project structure, framework-specific implementations, and stack-specific exceptions.

Layer 4 — Task-specific

On demand

Specialized guidance for payments, OTP, Celery, Bruno collections, Docker operations, migrations, or other areas touched by the current task.

### Folder ownership

Use a layout along these lines, without trying to redesign every file now:

```
rules/
├── Global-rule.md
├── common/
│   ├── README.md
│   └── <shared rule modules>
├── django/
│   ├── README.md
│   └── <Django-specific modules>
├── fastapi/
│   ├── README.md
│   └── <FastAPI-specific modules>
├── frontend/
│   ├── README.md
│   └── <frontend-specific modules>
└── n8n/
    ├── README.md
    └── <n8n-specific modules>
```

The placeholders are intentional: decide module boundaries after comparing the existing rules, rather than mechanically splitting every section.

### How the agent chooses rules for a session

1. Load the global rules. These apply regardless of the task.
2. Identify the stack or stacks involved. Inspect the relevant project structure when necessary; do not assume every task uses only one stack.
3. Read the relevant stack README. Treat it as a routing index, not as a rule source.
4. Load the applicable common modules and stack-specific modules. Select by what the task changes and what those changes could affect.
5. Load specialized modules when needed. A change involving payment callbacks may require API, security, database, and payment guidance—not merely the payment file.
6. Check for missing dependencies or conflicts. If a selected rule references another module, either load that dependency or make the dependency explicit and enforceable.

The key distinction: select rules by the work being performed and its impact, not just by the task's label.

For instance, “add a serializer field” might need API conventions, data-model constraints, authorization, and tests. It would not normally need Docker deployment instructions unless deployment configuration is also changing.

### How to keep each rule in one place

Adopt a strict ownership policy:

- One canonical source per rule. Do not maintain separate copies of the same requirement in Django and FastAPI.
- Common principles live in `common/`. Stack files reference those principles rather than paraphrasing them.
- Stack-specific implementation lives with the stack. A shared rule may link to a Django-specific or FastAPI-specific implementation, but it should not contain competing implementations.
- Exceptions belong to the stack that requires them. Document the exception and its scope explicitly; don't weaken the common rule to accommodate one framework.
- The README owns navigation only. It lists each module's purpose, applicability, and dependencies.
- Avoid circular references. Common modules must not depend on stack modules in ways that create confusing or recursive loading.

For example, common security guidance can own the requirement to validate and authorize requests. Django owns how that is implemented with its permission and request mechanisms; FastAPI owns its corresponding mechanisms. The principle is written once, and the implementation is written where it belongs.

## 4. How I would migrate your existing rules

Don't split all 1,255 lines in one pass. Use a controlled extraction process.

1. Inventory and classify. Compare the Django and FastAPI files by meaning, not section headings. Mark each rule as universal, shared principle, stack-specific implementation, domain-specific, or obsolete/duplicated.
2. Extract the genuinely shared rules. Start with the clearest duplicates. Preserve framework-specific examples, differences, and exceptions rather than forcing artificial symmetry.
3. Split the stack files by task-sized modules. Separate common workflows from specialized concerns. Keep each stack's project structure and file-layout conventions in that stack's scope.
4. Implement and verify selective loading. Confirm that a normal task reads only the intended files. If the agent cannot dynamically select modules, use the smallest supported entry-point configuration instead.
5. Test against representative tasks. Include a simple API change, a test-writing task, a payment or background-job task, and a cross-stack task. Check for missing instructions, conflicts, unnecessary loading, and repeated rules.

Track four outcomes: files actually loaded, approximate tokens consumed, missed applicable rules, and contradictory or duplicated instructions. A smaller context that produces weaker code is not an improvement.

## Final recommendation

Proceed with your proposed direction, but make selective loading a verified mechanism rather than a README convention. Build the shared layer around semantic ownership, not matching section titles, and resist splitting files so finely that navigation becomes another burden.

The goal is not the fewest lines or the most files. It is the smallest sufficient set of authoritative rules for each task, with no loss of applicable constraints.
```

## Claude (sonnet 5.5 Eport-medium):
```markdown
# Verdict

## 1. The problem

**The cost is real, but smaller and different from how you've framed it.**

441 lines is roughly 5-8k tokens. That is not a context-window crisis. The real costs are:

- **Dilution.** Dead rules (Bruno, payments, Docker) sit next to the task and compete for attention. Models follow a rule less reliably when it is buried among irrelevant ones.
- **Drift.** Five files restating the same concerns in their own words will diverge. The FastAPI "mirror of Django" is the worst case: every edit to one needs a manual port to the other.
- **Duplication with Global.** Global already covers security, errors, testing, and so on. Each stack file restates these, so the same rule exists in two or three places with slightly different wording.

**"The agent loads a whole stack file per session" is the right symptom, wrong root cause.** The root cause is that the files are organized by stack first and by concern second, with no rule owning a single location. Splitting files without fixing ownership just gives you more files to drift.

Also check whether your loader attaches the whole folder or only files the agent reads on demand. If your tool auto-attaches everything in the folder (many rule systems do), the entire plan fails regardless of design. Verify this first.

## 2. The proposed system

**Mostly sound, with four failure points.**

**Common rules that aren't common.** Rules that read the same ("validate input at the boundary," "paginate list endpoints") usually have stack-specific mechanics (DRF serializers vs Pydantic, `select_related` vs `selectinload`). If you hoist the sentence and leave the mechanics in the stack, you get a vague common rule plus a stack rule that must still be read. Test for common: *could an agent follow this rule correctly without knowing the stack?* If not, it's stack-specific, or it splits into a principle (common) and an implementation (stack).

**Common rules that must be visible while writing stack code.** A common `testing.md` is useless if the agent is in `django/` and never opens `common/`. Common rules fail silently when nobody routes to them. The README has to route to both layers per task, not just to one file.

**README becoming a second copy of the rules.** It will, the moment someone adds "remember: always use X" to it. A router should only hold task → file(s) mappings and nothing normative.

**Loader still attaches the folder.** Covered above. Also: if the agent reads the README and then picks a file itself, that depends on the agent following the protocol. Make the README's instruction short and unambiguous, and keep a few always-needed rules in Global so a skipped README doesn't cause serious damage.

**Two additional weaknesses:**
- "Pick the file that matches the task" fails on multi-concern tasks ("add a payment endpoint with tests"). The README must allow several files per task, not one.
- Global at 113 lines is the only guaranteed layer. Anything safety-critical (secrets, auth, destructive operations) must stay there, not move into a task file that might not get read.

## 3. Framework to follow

**Folders**

- `global/` is always loaded and kept small. Holds only rules that apply to every task on every stack, plus agent behavior and the safety-critical invariants. No stack-specific mechanics, no examples beyond one line.
- `common/` holds one file per concern (testing, API design, security, errors, git, configuration, and so on). Principles and decisions that hold across stacks, written stack-neutral. No framework names, no code.
- `<stack>/` holds only what common can't say: project tree and file layout, framework mechanics, stack-only patterns. Each file opens with a one-line pointer to its common counterpart and states only the delta.
- `README.md` at the root is the router and nothing else. A table of task → files, always listing common and stack files together.

**Placement rule (decides where every rule lives)**

1. True on every stack and every task → `global/`
2. True on every stack for one kind of work → `common/<concern>.md`
3. Needs the framework to be applied correctly → `<stack>/<concern>.md`
4. Principle is shared but mechanics differ → principle in common, mechanics in stack, never both

A rule has exactly one home. If you're tempted to write it twice, it's in the wrong layer.

**How an agent picks files in a session**

1. Global is already loaded.
2. Read the README. Identify the stack and the task(s).
3. Load every file the README lists for those tasks, common and stack together. Multiple tasks mean a union of files.
4. Do not load files the README doesn't list for the task.

Keep the README to one line per task, e.g. "Writing tests → common/testing.md + django/testing.md." Add a separate "Always read before writing code" row for the short pre-write checklist, which should be one small file per stack, not 34 lines embedded in a big one.

**Keeping a rule in one place**

- Domain patterns (payments, ledgers, metering) are task-specific and should be their own files. If two stacks need them, they live in `common/` with the stack mechanics split out.
- The Django→FastAPI naming map stays as the only cross-stack mirror file. Everything else should reference common instead of mirroring.
- Do the migration as an audit: for each section in the five files, label it global / common / stack / delete. Expect a surprising amount to be duplicate of Global or low value (a 7-line testing section in Django is probably just Global's testing rule restated).

**Success check:** after the split, a typical session should load Global plus 2-3 small files (roughly 150-250 lines) instead of 441. If a typical task still needs five or more files, your task boundaries in the README are too fine, so merge them.
```

## Cursor (grok 4.7 effort-high)

**Problem.** Real, and the diagnosis is right. The waste is not that 1,255 lines exist. The waste is that a session loads `Global-rule.md` (113 lines, already in memory) plus one entire stack file. Django is 328 lines. About 80 of those are the project tree, about 44 are domain patterns, about 28 are Bruno, about 34 are the pre-write checklist, and testing is about 7. A testing session still pays for payments, Docker, and Celery. FastAPI (380) copies that shape, so the same session cost exists there (about 493 lines with Global). Frontend (246) and n8n (188) repeat the same kinds of concerns.

**Proposed system.** Common folder + stack folder + a README that points at one task file is the right system. It fails if any of these are skipped:

- A shared topic is not a shared rule. Security, errors, testing, and rate limits appear in every file, and Global already states the principle. The stack file states the mechanism: DRF throttles and a Redis cache, the Django error JSON, `check --deploy`, n8n webhook checks. Move a bullet to common only when it can be pasted into every stack unchanged. If the wording has to change, it stays in the stack. If Global already says it, delete the copy. Do not relocate it.
- The pre-write checks (ownership, secrets, pagination, webhook verification) have to sit in the file the agent reads while building the endpoint. A separate "security" or "common" file will be skipped, because the README correctly told the agent not to open it.
- The README is a map: task, file, one sentence. If it restates the rules, it is the new monolith. If the loader attaches the whole folder, the split saves nothing. `django-rule.md`, `fastapi-rule.md`, `frontend-rule.md`, and `n8n-rule.md` have to become pointers, or agents will keep opening the long file.
- Do not split by the old section numbers. Versioning, serializers, views, and the error contract are one task. Domain patterns, Bruno, Docker, Celery, and tests are separate tasks. Four tiny API files make a normal endpoint open four files, and the saving disappears.

**Framework.**

- Always loaded: `Global-rule.md` only. No second always-on file.
- `common/`: one file per task, and only bullets that are identical on every stack and absent from Global. No project trees. No framework API names.
- `django/`, `fastapi/`, `frontend/`, `n8n/`: a `README.md` (the only stack file read for every task), a `structure.md` (tree and file placement, opened only when placing files), and one file per specific task. `api.md` starts with the short pre-write checklist. Each line names the extra file to open only when that line matches.
- Session: Global is already loaded. Read `<stack>/README.md`. Read the one file it names. Read a second file only when that file says this task matches. Do not read the rest.
- One home: already in Global → delete. Identical on two or more stacks → one file in `common/`, and the stack README points at it. Same concern, different mechanism → that stack file only.
- Migration: label each section once as delete, move to `common/`, or keep in the stack. FastAPI stops mirroring Django section by section. Keep the naming map. Point shared behavior at `common/`.

Done when a normal task loads the stack README plus one task file, a test task does not load the tree, Bruno, or payments, and no rule text appears in two files.