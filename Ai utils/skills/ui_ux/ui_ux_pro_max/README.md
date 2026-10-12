# UI/UX Pro Max and Full-Stack Engineering Skills Master Handbook and AI Agent Router

> Quick Orientation: This repository directory houses a specialized collection of 15 production-grade engineering skills combining UI/UX design intelligence, design systems, modern React and Next.js frontend architecture, FastAPI Python backend engineering, automated testing, and Bruno API client lifecycle management. All individual skills reside directly within this folder and its designated sub-packages (`./ui-ux-pro-max/`, `./bruno-api-clint/`, and root-level pattern directories). This master handbook functions as the primary decision-routing engine and technical manual for AI agents, automated development workflows, and software engineers.

```yaml
group_name: UI/UX Pro Max and Full-Stack Engineering Skills
source_repositories:
  - https://github.com/affaan-m/ecc
  - https://github.com/bruno-collections/bruno-agent-skills
  - https://github.com/claudekit/skills
source_license: MIT / Apache-2.0
total_skills: 15
skills_location: ./
coverage: UI/UX Design Intelligence, Design Systems, shadcn/ui & Tailwind CSS, Brand Identity & Banners, HTML Pitch Decks, React 18/19, Next.js Turbopack, Web Performance Optimization, FastAPI REST Architecture, Pytest TDD, React Testing Library, Bruno API Client Scaffolding & Testing
```

## Source and Provenance

This skill group synthesizes specialized capabilities from three primary upstream sources:

- Upstream Repository 1: [https://github.com/affaan-m/ecc](https://github.com/affaan-m/ecc)
  - Project Name: Everyday Claude Code (ECC)
  - Upstream Skill Path: `.agents/skills/`
  - License: MIT
  - Synced Skills (6): [`fastapi-patterns`](./fastapi-patterns/SKILL.md), [`nextjs-turbopack`](./nextjs-turbopack/SKILL.md), [`python-testing`](./python-testing/SKILL.md), [`react-patterns`](./react-patterns/SKILL.md), [`react-performance`](./react-performance/SKILL.md), [`react-testing`](./react-testing/SKILL.md)
  - Local Target Path: `./`

- Upstream Repository 2: [https://github.com/claudekit/skills](https://github.com/claudekit/skills)
  - Project Name: ClaudeKit UI/UX Pro Max & Creative Suite
  - Upstream Skill Path: `skills/`
  - License: MIT / Apache-2.0
  - Synced Skills (7): [`banner-design`](./ui-ux-pro-max/banner-design/SKILL.md), [`brand`](./ui-ux-pro-max/brand/SKILL.md), [`design`](./ui-ux-pro-max/design/SKILL.md), [`design-system`](./ui-ux-pro-max/design-system/SKILL.md), [`slides`](./ui-ux-pro-max/slides/SKILL.md), [`ui-styling`](./ui-ux-pro-max/ui-styling/SKILL.md), [`ui-ux-pro-max`](./ui-ux-pro-max/ui-ux-pro-max/SKILL.md)
  - Local Target Path: `./ui-ux-pro-max/`

- Upstream Repository 3: [https://github.com/bruno-collections/bruno-agent-skills](https://github.com/bruno-collections/bruno-agent-skills)
  - Project Name: Bruno Agent Skills
  - Upstream Skill Path: `skills/` or repository root
  - License: MIT
  - Synced Skills (2): [`bruno-collection-generator`](./bruno-api-clint/bruno-collection-generator/SKILL.md), [`bruno-test-writer`](./bruno-api-clint/bruno-test-writer/SKILL.md)
  - Local Target Path: `./bruno-api-clint/`

## Update Procedure for AI Agents and Developers

To synchronize these skills with upstream releases without overwriting local enhancements, follow this reproducible update procedure:

### Automated Update via CLI / Python Script

```bash
# Step 1: Clone upstream repositories into temporary directories
git clone --depth 1 https://github.com/affaan-m/ecc.git temp_ecc_sync
git clone --depth 1 https://github.com/bruno-collections/bruno-agent-skills.git temp_bruno_sync

# Step 2: Synchronize upstream folders using relative paths
python -c "
import os, shutil

# Sync ECC skills
ecc_src = os.path.join('temp_ecc_sync', '.agents', 'skills')
ecc_skills = [
    'fastapi-patterns', 'nextjs-turbopack', 'python-testing',
    'react-patterns', 'react-performance', 'react-testing'
]
for skill in ecc_skills:
    src_dir = os.path.join(ecc_src, skill)
    dst_dir = os.path.join('.', skill)
    if os.path.exists(src_dir):
        if os.path.exists(dst_dir):
            shutil.rmtree(dst_dir)
        shutil.copytree(src_dir, dst_dir)
        print(f'Synchronized ECC skill: {skill}')

# Sync Bruno skills
bruno_src = 'temp_bruno_sync'
bruno_dst = os.path.join('.', 'bruno-api-clint')
bruno_skills = ['bruno-collection-generator', 'bruno-test-writer']
for skill in bruno_skills:
    src_dir = os.path.join(bruno_src, skill)
    dst_dir = os.path.join(bruno_dst, skill)
    if os.path.exists(src_dir):
        if os.path.exists(dst_dir):
            shutil.rmtree(dst_dir)
        shutil.copytree(src_dir, dst_dir)
        print(f'Synchronized Bruno skill: {skill}')
"

# Step 3: Remove temporary sync directories
rm -rf temp_ecc_sync temp_bruno_sync

# Step 4: Validate git status
git status
```

### Manual Update Steps

1. Visit the respective upstream repositories:
   - ECC: `https://github.com/affaan-m/ecc/tree/main/.agents/skills`
   - Bruno: `https://github.com/bruno-collections/bruno-agent-skills`
   - ClaudeKit: `https://github.com/claudekit/skills`
2. Inspect recent commits and release tags for bug fixes or updated specifications.
3. Copy new or updated skill folders into their corresponding local directories (`./`, `./ui-ux-pro-max/`, or `./bruno-api-clint/`).
4. Ensure every modified or new directory maintains a valid `SKILL.md` file with standard YAML frontmatter (`name`, `description`).
5. Update the fast routing tables and domain catalogs in this `README.md` if new skill capabilities or files are introduced.

---

## AI Agent Directive: How to Query This Handbook

When prompted: *"Please find the proper skill for my task from this repo, from this folder"*, execute this deterministic 4-step decision sequence:

1. Analyze Intent and Stack:
   - Identify the user objective: visual design, token definition, banner creation, presentation layout, React component development, Next.js bundling, performance audit, backend API creation, test writing, or API client generation.
   - Extract the target tech stack: React 18/19, Next.js 16+, Tailwind CSS, Radix/shadcn, Chart.js, FastAPI, Pydantic v2, pytest, React Testing Library, or Bruno.

2. Scan the Fast Routing Matrix and Domain Catalogs:
   - Lookup the immediate problem in the [Task-to-Skill Fast Routing Matrix](#task-to-skill-fast-routing-matrix).
   - Locate related skills and companion options within the relevant section of the [Functional Domain Catalogs](#functional-domain-catalogs).
   - Use the [Master Alphabetical Directory (A-Z)](#master-alphabetical-directory-a-z) when searching by exact directory name.

3. Inspect the Target `SKILL.md` File:
   - Load and read `./path/to/SKILL.md` using relative paths.
   - Review frontmatter rules, `## When to Use` trigger conditions, architectural principles, and any referenced files in `./path/to/scripts/` or `./path/to/references/`.

4. Apply Patterns and Chain Companion Skills:
   - Apply the specific conventions, guidelines, and constraints defined in the skill.
   - Where a task spans multiple disciplines, chain companion skills (e.g., for a complete full-stack feature: `fastapi-patterns` + `python-testing` + `bruno-collection-generator` + `react-patterns` + `ui-styling` + `react-testing`).

---

## Task-to-Skill Fast Routing Matrix

Use this rapid lookup table to match specific development requirements directly to primary and companion skills:

| Goal / User Problem | Core Objective | Recommended Primary & Companion Skills |
| :--- | :--- | :--- |
| **Full-Stack Application Feature** | End-to-end implementation spanning FastAPI backend, React UI, and verification | Primary: [`fastapi-patterns`](./fastapi-patterns/SKILL.md), [`react-patterns`](./react-patterns/SKILL.md)<br>Companion: [`python-testing`](./python-testing/SKILL.md), [`ui-styling`](./ui-ux-pro-max/ui-styling/SKILL.md), [`react-testing`](./react-testing/SKILL.md) |
| **UI/UX Design Intelligence & Style Selection** | Determine color palettes, font pairings, visual styles, and UX rules for 192 product types | Primary: [`ui-ux-pro-max`](./ui-ux-pro-max/ui-ux-pro-max/SKILL.md)<br>Companion: [`ui-styling`](./ui-ux-pro-max/ui-styling/SKILL.md), [`design`](./ui-ux-pro-max/design/SKILL.md) |
| **Design System & Token Architecture** | Build three-layer tokens (primitive -> semantic -> component) and CSS variable scales | Primary: [`design-system`](./ui-ux-pro-max/design-system/SKILL.md)<br>Companion: [`ui-styling`](./ui-ux-pro-max/ui-styling/SKILL.md), [`ui-ux-pro-max`](./ui-ux-pro-max/ui-ux-pro-max/SKILL.md) |
| **Component Library with shadcn/ui & Tailwind** | Implement accessible components (dialogs, dropdowns, forms, data tables) with Tailwind CSS | Primary: [`ui-styling`](./ui-ux-pro-max/ui-styling/SKILL.md)<br>Companion: [`react-patterns`](./react-patterns/SKILL.md), [`ui-ux-pro-max`](./ui-ux-pro-max/ui-ux-pro-max/SKILL.md) |
| **Brand Identity, Logo & Corporate Identity** | Establish brand voice, messaging guidelines, logo concepts, and CIP mockup deliverables | Primary: [`brand`](./ui-ux-pro-max/brand/SKILL.md), [`design`](./ui-ux-pro-max/design/SKILL.md)<br>Companion: [`banner-design`](./ui-ux-pro-max/banner-design/SKILL.md) |
| **Marketing Banners & Website Heroes** | Generate multi-format banners for social media, display ads, hero headers, and print | Primary: [`banner-design`](./ui-ux-pro-max/banner-design/SKILL.md)<br>Companion: [`design`](./ui-ux-pro-max/design/SKILL.md), [`brand`](./ui-ux-pro-max/brand/SKILL.md) |
| **HTML Pitch Decks & Data Presentations** | Create responsive HTML presentations with Chart.js, layout grids, and copywriting formulas | Primary: [`slides`](./ui-ux-pro-max/slides/SKILL.md)<br>Companion: [`design-system`](./ui-ux-pro-max/design-system/SKILL.md), [`design`](./ui-ux-pro-max/design/SKILL.md) |
| **React 18/19 Architecture & RSC Boundaries** | Structure React components, Server/Client components, Suspense boundaries, and custom hooks | Primary: [`react-patterns`](./react-patterns/SKILL.md)<br>Companion: [`react-performance`](./react-performance/SKILL.md), [`ui-styling`](./ui-ux-pro-max/ui-styling/SKILL.md) |
| **Web Performance & Waterfall Elimination** | Optimize bundle size, eliminate async waterfalls, reduce re-renders, and improve Core Web Vitals | Primary: [`react-performance`](./react-performance/SKILL.md)<br>Companion: [`react-patterns`](./react-patterns/SKILL.md), [`nextjs-turbopack`](./nextjs-turbopack/SKILL.md) |
| **Next.js 16+ Dev Tooling & Turbopack** | Configure Next.js with Turbopack, diagnose slow startup, and implement `proxy.ts` / `middleware.ts` | Primary: [`nextjs-turbopack`](./nextjs-turbopack/SKILL.md)<br>Companion: [`react-patterns`](./react-patterns/SKILL.md), [`react-performance`](./react-performance/SKILL.md) |
| **FastAPI REST API & Async Architecture** | Build production FastAPI services with Pydantic v2, dependency injection, and clean routing | Primary: [`fastapi-patterns`](./fastapi-patterns/SKILL.md)<br>Companion: [`python-testing`](./python-testing/SKILL.md), [`bruno-collection-generator`](./bruno-api-clint/bruno-collection-generator/SKILL.md) |
| **React Component & Hook Testing** | Write unit and integration tests using React Testing Library, Vitest, MSW, and accessibility checks | Primary: [`react-testing`](./react-testing/SKILL.md)<br>Companion: [`react-patterns`](./react-patterns/SKILL.md), [`ui-styling`](./ui-ux-pro-max/ui-styling/SKILL.md) |
| **Python Backend TDD & Pytest Suites** | Construct test suites using pytest, fixtures, parametrization, async tests, and coverage enforcement | Primary: [`python-testing`](./python-testing/SKILL.md)<br>Companion: [`fastapi-patterns`](./fastapi-patterns/SKILL.md) |
| **Scaffolding Bruno API Collections** | Extract endpoints from code or OpenAPI into `.bru` and OpenCollection YAML with environments | Primary: [`bruno-collection-generator`](./bruno-api-clint/bruno-collection-generator/SKILL.md)<br>Companion: [`bruno-test-writer`](./bruno-api-clint/bruno-test-writer/SKILL.md) |
| **Automated API Testing in Bruno** | Write Chai test assertions, request chaining scripts (`bru.setVar`), and pre/post-request logic | Primary: [`bruno-test-writer`](./bruno-api-clint/bruno-test-writer/SKILL.md)<br>Companion: [`bruno-collection-generator`](./bruno-api-clint/bruno-collection-generator/SKILL.md) |
| **Accessibility (WCAG 2.2 AA) Audit** | Audit contrast, keyboard navigation, focus rings, ARIA roles, and screen-reader readiness | Primary: [`ui-ux-pro-max`](./ui-ux-pro-max/ui-ux-pro-max/SKILL.md)<br>Companion: [`ui-styling`](./ui-ux-pro-max/ui-styling/SKILL.md), [`react-testing`](./react-testing/SKILL.md) |

---

## Functional Domain Catalogs

The 15 skills in this collection are organized across 5 technical domains:

### 1. UI/UX Design Intelligence, Systems & Visual Media

This domain contains 7 specialized skills covering visual style determination, component styling, design token systems, brand standards, banner generation, and presentation slides.

| Skill Folder & Link | Skill Title | What It Explains & Core Capabilities | When to Trigger / Tech Keywords |
| :--- | :--- | :--- | :--- |
| [`ui-ux-pro-max`](./ui-ux-pro-max/ui-ux-pro-max/SKILL.md) | **UI/UX Pro Max - Design Intelligence** | BM25 search engine querying 79 visual styles, 192 product palettes, 74 font pairings, 119 UX guidelines, 105 icons, 25 chart types, and 22 technology stacks. Enforces WCAG 2.2 AA accessibility, touch targets (min 44x44px), responsive breakpoints, and priority-ranked design decisions. Includes local CLI query tools (`scripts/search.py`, `scripts/design_system.py`). | Trigger when choosing visual styles, product palettes, font combinations, chart selections, UX guidelines, or auditing interface quality. Keywords: design system, style selection, color palette, typography pairing, chart type, WCAG, contrast, touch target, animation presets, responsive layout. |
| [`ui-styling`](./ui-ux-pro-max/ui-styling/SKILL.md) | **UI Styling Skill** | Production styling combining shadcn/ui components (Radix UI primitives), Tailwind CSS utility-first styling, and canvas-based visual compositions. Covers dark mode, CSS variables, theme customization, dialogs, forms, data tables, and automated component generators (`scripts/shadcn_add.py`, `scripts/tailwind_config_gen.py`). | Trigger when building or styling UI with React/Next.js/Vite using shadcn/ui and Tailwind CSS. Keywords: shadcn/ui, Radix UI, Tailwind CSS, dark mode, theme tokens, accessible forms, responsive layout, modal dialog, toast notification, data table. |
| [`design-system`](./ui-ux-pro-max/design-system/SKILL.md) | **Design System** | Three-layer token architecture (`primitive` -> `semantic` -> `component`), CSS variable systems, spacing and typography scales, component state definitions (hover, active, focus-visible, disabled), and design-to-code handoff specifications. | Trigger when establishing design tokens, standardizing CSS variables, configuring Tailwind theme scales, or writing component specifications. Keywords: design tokens, CSS variables, spacing scale, typography scale, semantic tokens, component state, design-to-code, token contract. |
| [`design`](./ui-ux-pro-max/design/SKILL.md) | **Design (Master Director)** | Master orchestrator coordinating brand identity, logo design (55 styles), Corporate Identity Program (CIP mockups across 50 deliverables), presentations, banners, and vector icons. Directs tasks to specialized sub-skills or executes unified design workflows. | Trigger when initiating end-to-end design projects requiring multi-discipline coordination. Keywords: master design, logo generation, brand identity, CIP program, mockups, vector icons, marketing creative, creative direction, design workflow. |
| [`brand`](./ui-ux-pro-max/brand/SKILL.md) | **Brand** | Brand voice definition, tone of voice guidelines, messaging frameworks, visual identity standards, asset organization, color governance, and brand consistency audits. Includes scripts for brand synchronization and guideline generation. | Trigger when establishing brand guidelines, writing branded copy, auditing asset compliance, or formalizing brand voice. Keywords: brand voice, tone of voice, messaging framework, visual identity, brand guidelines, style guide, asset governance, brand audit. |
| [`banner-design`](./ui-ux-pro-max/banner-design/SKILL.md) | **Banner Design - Multi-Format Creative Banner System** | Multi-platform creative banner generation across 12 art direction styles (minimalist, bold typography, gradient, 3D, retro, glassmorphism, neon, editorial). Formats for Facebook, Twitter/X, LinkedIn, YouTube, Instagram, Google Display, and website hero banners. | Trigger when designing promotional banners, website heroes, social headers, display ads, or campaign visuals. Keywords: banner design, hero section, display ads, social media cover, LinkedIn banner, YouTube header, open graph image, advertising banner. |
| [`slides`](./ui-ux-pro-max/slides/SKILL.md) | **Slides** | Strategic HTML presentation generation utilizing Chart.js, CSS grid layouts, responsive viewports, copywriting formulas, and contextual pitch deck structures. Supports slide decks, investor pitches, and marketing presentations. | Trigger when creating browser-based presentations, pitch decks, data-driven slides, or executive reports. Keywords: HTML slides, pitch deck, Chart.js presentations, executive summary slides, responsive presentations, slide layout, copywriting framework. |

---

### 2. Frontend Web Frameworks & Performance

This domain contains 3 skills focused on React 18/19 architecture, Next.js App Router, Turbopack bundling, and performance optimization.

| Skill Folder & Link | Skill Title | What It Explains & Core Capabilities | When to Trigger / Tech Keywords |
| :--- | :--- | :--- | :--- |
| [`react-patterns`](./react-patterns/SKILL.md) | **React Patterns** | React 18 and 19 patterns covering Server/Client Component (RSC) boundaries, hooks discipline (`useActionState`, `useOptimistic`, `useId`), state location decision trees, Suspense and Error Boundaries, form actions, and accessibility-first composition. | Trigger when creating or refactoring React components, custom hooks, or state architecture in React 18/19 and Next.js App Router. Keywords: React 18, React 19, RSC, Server Components, Client Components, custom hooks, Suspense, ErrorBoundary, form actions, state lifting. |
| [`react-performance`](./react-performance/SKILL.md) | **React Performance** | 70+ performance rules based on Vercel Engineering best practices structured across 8 priority tiers: eliminating waterfalls, bundle size reduction, server-side caching, client-side data fetching, re-render prevention, DOM virtualization, and micro-optimizations. | Trigger when diagnosing slow initial load, high client CPU, bundle size regressions, or Core Web Vitals (LCP, CLS, INP) issues. Keywords: React performance, Next.js performance, waterfalls, bundle size, code splitting, dynamic import, re-render, memoization, useMemo, useCallback, Core Web Vitals, LCP, INP. |
| [`nextjs-turbopack`](./nextjs-turbopack/SKILL.md) | **Next.js and Turbopack** | Next.js 16+ Turbopack compiler guidance, incremental Rust bundling, file-system caching, dev startup optimization, Turbopack vs Webpack tradeoffs, and Next.js 16 conventions including `proxy.ts` / `middleware.ts`. | Trigger when configuring or troubleshooting Next.js 16+ builds, optimizing development startup, or migrating from Webpack. Keywords: Next.js 16, Turbopack, App Router, Rust bundler, HMR, dev startup, fast refresh, webpack fallback, proxy.ts, middleware.ts. |

---

### 3. Python & FastAPI Backend Engineering

This domain contains production patterns for building modern asynchronous Python backend services.

| Skill Folder & Link | Skill Title | What It Explains & Core Capabilities | When to Trigger / Tech Keywords |
| :--- | :--- | :--- | :--- |
| [`fastapi-patterns`](./fastapi-patterns/SKILL.md) | **FastAPI Patterns** | Production FastAPI architecture covering application factories, lifespan context managers, Pydantic v2 schemas (`model_validate`, `ConfigDict`), dependency injection (`Depends`), async request handlers, transactional service layers, and route-level authorization. | Trigger when building, reviewing, or refactoring FastAPI backend applications, APIs, and Pydantic schemas. Keywords: FastAPI, Pydantic v2, dependency injection, async def, lifespan, APIRouter, OAuth2, JWT auth, service layer, SQLAlchemy async, httpx testing. |

---

### 4. Automated Testing, QA & Verification

This domain contains 2 skills covering test-driven development, unit testing, integration testing, network mocking, and accessibility assertions across frontend and backend environments.

| Skill Folder & Link | Skill Title | What It Explains & Core Capabilities | When to Trigger / Tech Keywords |
| :--- | :--- | :--- | :--- |
| [`react-testing`](./react-testing/SKILL.md) | **React Testing** | Component testing methodology using React Testing Library, Vitest or Jest, Mock Service Worker (MSW) for network mocking, user interaction simulation via `userEvent`, custom hook testing (`renderHook`), and accessibility assertions with `jest-axe`. | Trigger when writing, updating, or fixing tests for React components, custom hooks, or frontend user flows. Keywords: React Testing Library, RTL, Vitest, Jest, userEvent, renderHook, MSW, Mock Service Worker, axe, jest-axe, screen.getByRole, accessibility testing. |
| [`python-testing`](./python-testing/SKILL.md) | **Python Testing Patterns** | Python testing methodology centered on pytest and TDD. Covers fixture scopes, parametrization (`@pytest.mark.parametrize`), markers, mocking with `unittest.mock` and `pytest-mock`, async test execution with `pytest-asyncio`, exception validation, and test organization. | Trigger when writing pytest suites, setting up testing infrastructure, or enforcing coverage requirements in Python projects. Keywords: pytest, TDD, red-green-refactor, fixtures, parametrization, pytest-asyncio, unittest.mock, pytest-cov, test coverage, async testing. |

---

### 5. API Client Scaffolding & Contract Verification

This domain contains 2 skills for managing API collections and tests in Bruno, the Git-friendly, open-source API client.

| Skill Folder & Link | Skill Title | What It Explains & Core Capabilities | When to Trigger / Tech Keywords |
| :--- | :--- | :--- | :--- |
| [`bruno-collection-generator`](./bruno-api-clint/bruno-collection-generator/SKILL.md) | **Bruno Collection Generator** | Scaffolds organized Bruno collections, `.bru` request files, OpenCollection YAML files, and multi-environment configurations (`baseUrl`, auth headers) from FastAPI routes, OpenAPI/Swagger specifications, or cURL commands. | Trigger when creating, converting, or structuring Bruno API collections, folders, and request definitions. Keywords: Bruno, usebruno, .bru, OpenCollection YAML, API collection, OpenAPI to Bruno, cURL to Bruno, environments, API scaffolding, collection generator. |
| [`bruno-test-writer`](./bruno-api-clint/bruno-test-writer/SKILL.md) | **Bruno Test Writer** | Generates Chai test assertions, pre-request setup scripts, post-response data extraction (`bru.setVar`), dynamic request chaining, schema checks, and negative path testing for Bruno API requests without hardcoding credentials. | Trigger when writing assertions, response validation, variable chaining, or regression tests in Bruno requests. Keywords: Bruno tests, Chai assertions, bru.setVar, req.setHeader, res.getBody, request chaining, API regression, pre-request scripts, post-response scripts. |

---

## Master Alphabetical Directory (A-Z)

This directory provides a numbered index of all 15 skills sorted alphabetically:

| Number | Skill Name & Entry Link | Technical Domain | One-Sentence Summary |
| :--- | :--- | :--- | :--- |
| 1 | [`banner-design`](./ui-ux-pro-max/banner-design/SKILL.md) | UI/UX & Visual Media | Generates multi-format creative banners across 12 artistic styles for social media, display ads, hero headers, and print. |
| 2 | [`brand`](./ui-ux-pro-max/brand/SKILL.md) | UI/UX & Visual Media | Establishes brand voice, tone guidelines, messaging frameworks, visual identity standards, and compliance audits. |
| 3 | [`bruno-collection-generator`](./bruno-api-clint/bruno-collection-generator/SKILL.md) | API Tooling & Bruno Client | Scaffolds Bruno API collections, `.bru` files, OpenCollection YAML, and environment configs from code or OpenAPI specs. |
| 4 | [`bruno-test-writer`](./bruno-api-clint/bruno-test-writer/SKILL.md) | API Tooling & Bruno Client | Writes Chai test assertions, request chaining scripts, and schema checks for Bruno API requests. |
| 5 | [`design`](./ui-ux-pro-max/design/SKILL.md) | UI/UX & Visual Media | Serves as the master design director orchestrating logos, CIP mockups, presentations, banners, and vector icons. |
| 6 | [`design-system`](./ui-ux-pro-max/design-system/SKILL.md) | UI/UX & Visual Media | Defines three-layer token architectures, CSS variable systems, typography/spacing scales, and component specs. |
| 7 | [`fastapi-patterns`](./fastapi-patterns/SKILL.md) | Python & Backend Engineering | Teaches production FastAPI architecture, Pydantic v2 schemas, lifespan handlers, and dependency injection. |
| 8 | [`nextjs-turbopack`](./nextjs-turbopack/SKILL.md) | Frontend Web Frameworks | Provides Next.js 16+ Turbopack compilation guidance, Rust bundling tradeoffs, and `proxy.ts`/`middleware.ts` naming. |
| 9 | [`python-testing`](./python-testing/SKILL.md) | Automated Testing & QA | Implements pytest testing strategies, TDD red-green-refactor loops, fixtures, parametrization, and async testing. |
| 10 | [`react-patterns`](./react-patterns/SKILL.md) | Frontend Web Frameworks | Enforces React 18/19 patterns, RSC Server/Client boundaries, custom hooks discipline, and Suspense composition. |
| 11 | [`react-performance`](./react-performance/SKILL.md) | Frontend Web Frameworks | Provides 70+ Vercel-derived optimization rules for eliminating waterfalls, trimming bundle sizes, and preventing re-renders. |
| 12 | [`react-testing`](./react-testing/SKILL.md) | Automated Testing & QA | Guides component testing using React Testing Library, Vitest/Jest, MSW network mocking, and accessibility assertions. |
| 13 | [`slides`](./ui-ux-pro-max/slides/SKILL.md) | UI/UX & Visual Media | Builds responsive HTML slide decks featuring Chart.js visualizations, grid layouts, and copywriting formulas. |
| 14 | [`ui-styling`](./ui-ux-pro-max/ui-styling/SKILL.md) | UI/UX & Visual Media | Combines shadcn/ui accessible components, Tailwind CSS utility styling, and canvas-based visual systems. |
| 15 | [`ui-ux-pro-max`](./ui-ux-pro-max/ui-ux-pro-max/SKILL.md) | UI/UX & Visual Media | Delivers searchable UI/UX intelligence across 79 styles, 192 product palettes, 74 font pairings, and 119 UX guidelines. |

---

## Standard Skill Anatomy

Every skill within this handbook conforms to a standardized, machine-readable architecture designed for AI agent execution:

1. YAML Frontmatter:
   - `name`: Unique kebab-case identifier matching the directory name.
   - `description`: Machine-readable description outlining core capabilities and exact activation scenarios.
   - `license` / `metadata`: Provenance, version, author, and licensing details.

2. Heading & Overview:
   - Official title and concise summary explaining what the skill enables.

3. Trigger Conditions (`When to Use` / `When to Activate`):
   - Explicit bullet points detailing file types (`.tsx`, `.py`, `.bru`), frameworks, command executions, and developer scenarios that require activating the skill.

4. Core Concepts & Architectural Rules:
   - Architectural patterns, strict constraints, priority rankings, and anti-patterns to avoid.

5. Step-by-Step Execution Workflow:
   - Deterministic sequential instructions explaining how to complete tasks using the skill.

6. Code Examples & Reference Implementation:
   - Complete, copy-pasteable snippets demonstrating production usage, test setup, or configuration.

7. Verification & Quality Gating:
   - Concrete test commands, lint checks, or criteria to verify that implementation meets quality standards.

---

## Safety, Secrets, and Security Guardrails

All AI agents and developers operating with skills from this directory must strictly comply with the following security guardrails:

1. Zero Hardcoded Credentials:
   - Never embed database passwords, API tokens, JWT secrets, or cloud keys in source code, `.bru` files, or test suites.
   - Always use environment variables, `.env` references, or placeholder syntax (e.g., `{{api_key}}`, `bru.getSecretVar('TOKEN')`, `os.getenv('SECRET_KEY')`).

2. Untrusted Prompt and Payload Handling:
   - External API payloads, swagger specifications, and user-supplied cURL snippets must be treated as untrusted data.
   - Sanitize inputs before generating shell commands, file system paths, or automated requests.

3. Personally Identifiable Information (PII) Masking:
   - Never use real customer names, phone numbers, email addresses, or payment cards in tests or sample data fixtures.
   - Use synthetic fixtures (e.g., `user@example.com`, Faker library, or standard mock UUIDs).

4. Destructive Operation Gating:
   - Gated consent is strictly required before executing destructive actions such as database migrations, dropping tables, purging folders, or overwriting configuration files.
   - Live HTTP requests modifying server data (`POST`, `PUT`, `PATCH`, `DELETE`) against production or non-local environments are strictly prohibited without human authorization.

---

## AI Prompting Tips & Usage Examples

Use these prompt templates to instruct AI agents to leverage individual or chained skills effectively:

### Example 1: Full-Stack Feature Implementation
```text
I need to add an organization membership feature to our application.
Please read and apply the following skills from this directory:
- Primary backend: ./fastapi-patterns/SKILL.md
- Backend tests: ./python-testing/SKILL.md
- Primary frontend: ./react-patterns/SKILL.md
- UI styling: ./ui-ux-pro-max/ui-styling/SKILL.md
- Component tests: ./react-testing/SKILL.md
Follow TDD: write pytest tests first for the backend, then implement the FastAPI router and service, and finally create the React components with accessible shadcn/ui dialogs and RTL tests.
```

### Example 2: UI/UX Design Direction & Style Selection
```text
We are designing a SaaS Analytics Dashboard for B2B logistics.
Please query the UI/UX intelligence skill at ./ui-ux-pro-max/ui-ux-pro-max/SKILL.md:
1. Search the recommended product palette and reasoning profile for B2B Analytics.
2. Select appropriate font pairings and visual style.
3. Review WCAG 2.2 AA accessibility requirements for data charts and contrast ratios.
4. Output the design system tokens according to ./ui-ux-pro-max/design-system/SKILL.md.
```

### Example 3: Performance Optimization Audit
```text
Our Next.js 16 application is suffering from slow initial page load and layout shifts.
Please review ./react-performance/SKILL.md and ./nextjs-turbopack/SKILL.md:
1. Identify any data fetching waterfalls in our Server Components.
2. Check bundle size optimizations and code-splitting opportunities.
3. Verify Turbopack configuration and App Router component boundaries.
4. Propose prioritized fixes based on the Vercel Engineering 8-tier priority matrix.
```

### Example 4: Bruno API Collection & Automated Test Generation
```text
I have a set of FastAPI routes defined in app/routers/billing.py.
Please read:
- ./bruno-api-clint/bruno-collection-generator/SKILL.md
- ./bruno-api-clint/bruno-test-writer/SKILL.md
Scaffold an OpenCollection YAML collection in bruno/billing with appropriate environment variables, and generate Chai assertions for status codes, schema validation, and token chaining.
```
