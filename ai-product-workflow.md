# AI-Powered Product Development Workflow

A detailed, step-by-step workflow for building a product feature from research to prototype using AI (Claude Code) as a co-pilot. Based on the Instance History feature for YuMove by Yulu — a real project that went from raw research inputs to a stakeholder-ready PRD, interactive prototype, design system audit, and user flow documentation.

---

## Overview

### What this workflow produces
- Problem statement grounded in user evidence
- Competitive analysis
- PRD with acceptance criteria, edge cases, and engineering notes
- Interactive HTML prototype (no build tools needed)
- Design system compliance audit
- Stakeholder feedback integration loop
- User flow documentation with screenshots
- Audit checklist with pass/fail against every requirement

### Prerequisites
- Claude Code (CLI, desktop app, or IDE extension)
- A project folder with company context and research inputs
- Figma file (optional — for design reference)

### Time investment
This workflow was executed across multiple sessions. Each phase can be done independently.

---

## Phase 0: Project Setup

### 0.1 Create the starter kit

Set up a project folder with foundational files that give AI the context it needs to make good decisions.

```
project/
├── company-context.md      ← Company overview, personas, competitors, business model, metrics
├── prd-template.md         ← PRD structure to follow (ensures consistency)
├── socratic-questioning.md ← Framework to sharpen thinking before drafting
├── CLAUDE.md               ← Instructions for the AI (read order, conventions, output paths)
├── research/               ← Drop raw inputs here
│   ├── user-interviews.csv
│   ├── meeting-transcripts.md
│   └── screenshots/
└── outputs/                ← All AI-generated documents go here
```

### 0.2 Write CLAUDE.md (AI instructions)

This is the most important file. It tells the AI:
- What files to read first (and in what order)
- What workflow to follow
- Where to save outputs
- Naming conventions

**Key principle:** The AI can only be as good as the context you give it. Front-load the context.

### 0.3 Build company-context.md

Include:
- Company description and product overview
- User personas (with behavioral details, not just demographics)
- Business model and key metrics
- Competitor landscape (who, what they do, how they differ)
- Current priorities and constraints

**Why this matters:** Every AI suggestion will be filtered through this context. A delivery rider persona produces very different PRD decisions than a SaaS admin persona.

---

## Phase 1: Research Synthesis

### 1.1 Drop raw research into `research/`

Gather everything you have:
- User interview transcripts or call notes
- Survey data (CSV exports)
- Support ticket patterns
- Existing app screenshots
- Meeting notes from stakeholders
- Any data exports (funnel data, experiment results)

### 1.2 Ask AI to synthesize research

**Prompt pattern:**
> "Read all files in `research/` and `company-context.md`. Synthesize the key findings into a user research synthesis document. Group by theme, include direct quotes, and flag contradictions."

**What AI produces:**
- Thematic grouping of user pain points
- Direct quotes tied to specific users (with order counts, churn status)
- Behavioral signals (what users do, not just what they say)
- Contradictions or gaps in the research

**Output:** `outputs/user-calling-synthesis.md`

### 1.3 Ask AI to analyze existing state

**Prompt pattern:**
> "Analyze the existing app (use screenshots in `research/Screenshots/`) and document what exists today vs. what's missing. Compare against competitors."

**What AI produces:**
- Current feature inventory
- Gap analysis (what competitors have that we don't)
- Existing UX patterns we should reuse

**Output:** `outputs/existing-state-analysis.md`

### 1.4 Ask AI to run competitive study

**Prompt pattern:**
> "Analyze competitors [X, Y, Z] for the [feature area]. Use the screenshots and any research. Document what each does, common patterns, and unique differentiators."

**Output:** `outputs/competitive-study.md`

---

## Phase 2: Problem Definition

### 2.1 Use Socratic questioning before writing

Before jumping to solutions, use a questioning framework to sharpen thinking:

**Prompt pattern:**
> "Using the socratic-questioning framework, ask me 3-5 sharpening questions about this feature before we draft the PRD."

AI will ask questions like:
- "What's the one metric this feature should move?"
- "Who is the primary persona — the high-performing rider or the average rider?"
- "Should this be a separate page or integrated into existing flows?"

Answer these. The answers become PRD constraints.

### 2.2 Draft problem statement

**Prompt pattern:**
> "Based on the research synthesis, write a problem statement. Include: what's broken today, who it affects, what the cost of inaction is, and supporting evidence (user quotes + behavioral data)."

**Output:** `outputs/problem-statement.md`

---

## Phase 3: PRD Drafting (v1)

### 3.1 Generate first PRD draft

**Prompt pattern:**
> "Generate a PRD using `prd-template.md` as the structure. Incorporate all context from company-context, research synthesis, competitive study, and problem statement. Include scope, solution, acceptance criteria, edge cases, and success metrics."

**What AI produces:**
- Full PRD with all sections populated
- Acceptance criteria derived from stakeholder inputs
- Edge cases derived from user research (e.g., "what happens when an order is cancelled mid-delivery?")
- Success metrics tied to the problem (e.g., "30% reduction in support tickets")

**Output:** `outputs/[feature]-prd-v1.md`

### 3.2 Build v1 prototype

**Prompt pattern:**
> "Build an interactive HTML prototype for this PRD. Single file, no build tools. Use the design system tokens from [design-system reference]. Include all screens: home, daily view, weekly view."

**Key decisions:**
- Single HTML file with inline CSS and JS — no dependencies, opens in any browser
- Use the actual design system tokens (colors, typography, spacing) from your DS docs
- Mock data that covers all edge cases (delivered, cancelled, rejected, in-progress, penalties, etc.)
- Navigation between screens via JS

**Output:** `app/index.html`

### 3.3 Review from multiple perspectives

**Prompt pattern:**
> "Review this PRD from three perspectives: (1) Engineering — is it buildable? What's missing? (2) Design — does it follow our DS? Is the information hierarchy right? (3) Business — does it address the stated problem? Are metrics realistic?"

---

## Phase 4: Stakeholder Review

### 4.1 Conduct stakeholder meeting

Share the prototype + PRD with stakeholders. Record feedback.

**What to capture per stakeholder:**
- Name and role
- Specific feedback items (not just "looks good")
- Decisions made (with who drove them)
- Open questions

### 4.2 Structure stakeholder feedback

**Prompt pattern:**
> "Restructure the raw meeting notes into a structured stakeholder feedback document. Group by feature section, then by stakeholder. Keep it as bullet points."

**Output:** `Stakeholder Prototype Feedback.md`

**Key insight from this project:** Keep feedback structured by feature section (Earnings, Shifts, Login) with stakeholder names inline. Don't over-engineer with tables or status tags — clean bullets are most scannable.

---

## Phase 5: PRD Refinement (v2)

### 5.1 Process stakeholder feedback into PRD

**Prompt pattern:**
> "Update the PRD to incorporate all stakeholder feedback from [feedback file]. For each piece of feedback, either accept it, modify it, or defer it. Update acceptance criteria, edge cases, and taxonomy accordingly."

**What changes in v2:**
- Taxonomy refinements (e.g., reducing 11 line item types to 8)
- Filter simplification (e.g., 7 chips → 4)
- Visual treatment changes (e.g., "no blue in design system")
- New statuses added (e.g., "Partial" shift status)
- Sort order standardized across all sections

### 5.2 Track decisions with traceability

Add a Stakeholder Refinement Log to the PRD documenting:
- What was decided
- Who drove the decision
- What it supersedes (if anything)
- Date

This is crucial — when engineering asks "why is it this way?", you have the answer with attribution.

### 5.3 Iterate prototype to match v2

**Prompt pattern:**
> "Update the prototype to match all v2 PRD changes: [list specific changes]."

Changes are applied incrementally. AI reads the existing code, makes targeted edits, and preserves everything else.

---

## Phase 6: Design System Audit

### 6.1 Document design system learnings

As you build, capture patterns that aren't in the official DS but emerge from the work:
- Spacing conventions between sections
- Component patterns not in the DS (date pills, cumulative summaries, bar charts)
- Conventions that override DS defaults (e.g., "colored text, not chips for status")
- Known discrepancies between Figma and DS, with resolutions

**Output:** `Design System Learnings.md`

### 6.2 Run compliance audit

**Prompt pattern:**
> "Audit the prototype against the PRD, stakeholder feedback, and design system. Create an exhaustive checklist of every acceptance criterion and check whether the prototype passes."

**What AI produces:**
- Numbered checklist (145+ items in this project)
- Pass/Fail/Partial/N/A status per item
- Source traceability (which PRD section, which stakeholder, which DS rule)
- Summary table with counts
- List of issues to fix

**Output:** `outputs/prototype-audit-checklist.md`

### 6.3 Fix audit failures

AI identifies the exact issues and fixes them in the prototype. Re-audit until clean.

---

## Phase 7: Scope Management

### 7.1 Remove out-of-scope items cleanly

When scope changes (e.g., removing a feature dimension):

**Prompt pattern:**
> "Remove [feature X] from current scope. Save it to a separate file for future pickup. Clean up all references in the PRD, prototype, and README."

**What AI does:**
- Extracts the section to a standalone file
- Removes HTML/CSS/JS from prototype
- Updates PRD metadata, scope table, entry points
- Updates README
- Leaves a pointer to the extracted file

### 7.2 Add new scope items

When new dimensions emerge from stakeholder work:

**Prompt pattern:**
> "Add [new feature] as a new dimension. It should reuse data from [existing source]. Summary should show [metrics], breakdown should show [buckets], filters should have [chips]."

AI builds the full page (HTML + JS + data), wires navigation, and updates the PRD.

---

## Phase 8: Drill-Down Pages

### 8.1 Use Figma as reference

When building detail pages, provide Figma URLs:

**Prompt pattern:**
> "Use [Figma URL 1] for the order details page and [Figma URL 2] for the payment receipt. Adapt to our design system and prototype conventions."

AI fetches the Figma design via MCP tools, extracts the structure and patterns, and builds HTML that matches the design but uses your DS tokens.

### 8.2 Wire tap behaviors

Connect list items to drill-down pages:
- Order rows → Order Details → Payment Receipt
- Incentive rows → Incentive Detail → Incentive Progress

Each tap shows a chevron (›) icon and opens an overlay page.

---

## Phase 9: User Flow Documentation

### 9.1 Take screenshots programmatically

**Prompt pattern:**
> "Take full-screen screenshots of every page in the prototype and create a user flow document."

AI installs Puppeteer temporarily, scripts navigation to each screen, takes screenshots at 2x retina resolution, and cleans up the dependency after.

### 9.2 Build the flow diagram

**Prompt pattern:**
> "Create a user flow HTML page showing all screens in a horizontal layout with connecting arrows."

**Layout pattern:**
```
Row 1: Home → Earnings Daily → Earnings Weekly
Row 2: Home → Orders Daily → Orders Weekly
Row 3: Home → Shifts Daily → Shifts Weekly
Row 4: Home → Login Daily → Login Weekly
```

Each row shows the actual screenshots in phone frames with labeled arrows between them. The HTML is print-ready (Cmd+P → PDF).

---

## Phase 10: Engineering Handoff Readiness

### 10.1 Audit PRD for engineering completeness

**Prompt pattern:**
> "Audit the PRD and evaluate which sections are relevant for engineering to build the backend, and which are frontend/design only. Question me on any missing business logic."

**What AI identifies:**
- Sections relevant for backend (data model, enums, computation rules, pagination)
- Sections that are frontend-only (colors, typography, spacing)
- Missing business logic (e.g., "how is shift completion % calculated?", "what creates a login session?")

### 10.2 Resolve gaps

Answer AI's questions. Most answers fall into:
- "Engineering will define in their tech doc" (API contracts, data sources)
- "Existing mechanism" (login CTA, real-time updates)
- "Specific business rule" (net earnings for bar chart, peak = login time during peak shifts)

Update the PRD with resolved answers. Close open items.

### 10.3 Final PRD cleanup

**Prompt pattern:**
> "Update the PRD to align everything with the final prototype. Fix any stale sections, contradictions, or outdated formats."

**What gets cleaned up:**
- Scenarios tables updated to match current taxonomy
- Sections marked "pending" → "done"
- Open items resolved and closed
- Superseded decisions marked clearly
- Reference ASCII diagrams added for every section

---

## Deliverables Summary

By the end of this workflow, you have:

| Deliverable | File | Purpose |
| --- | --- | --- |
| Company context | `company-context.md` | Foundation for all AI decisions |
| Research synthesis | `outputs/user-calling-synthesis.md` | Thematic user evidence |
| Competitive study | `outputs/competitive-study.md` | Market positioning |
| Existing state analysis | `outputs/existing-state-analysis.md` | Gap identification |
| Problem statement | `outputs/problem-statement.md` | Why this matters |
| PRD v1 | `outputs/[feature]-prd-v1.md` | Initial spec |
| PRD v2 (final) | `outputs/[feature]-prd-v2.md` | Refined spec with stakeholder decisions |
| Stakeholder feedback | `Stakeholder Prototype Feedback.md` | Structured meeting outputs |
| Design system learnings | `Design System Learnings.md` | DS patterns discovered during build |
| Prototype | `app/index-v2.html` | Interactive, self-contained HTML |
| Audit checklist | `outputs/prototype-audit-checklist.md` | 145+ item compliance check |
| User flow | `app/user-flow.html` + screenshots | Stakeholder-ready flow diagram |
| Deferred scope | `outputs/[deferred]-prd.md` | Preserved for future pickup |

---

## Phase 11: Jira Workflow — PRD to Platform Handoff

### Lifecycle

```
TRIGGER                    PRD BOARD                         PLATFORM BOARD
─────────                  ─────────                         ──────────────

Problem created     ──→  PRD ticket exists?
in GitHub repo           ├─ YES → Update to Prioritized
                         └─ NO  → Create Epic (Prioritized)
                                      │
                         ┌─────────────┘
                         ▼
                    [Prioritized]
                         │
                    Transition to In Analysis
                    when work begins
                         │
                         ▼
                    [In Analysis]
                         │
                    Research, PRD drafting,
                    stakeholder review,
                    prototype, audit
                         │
                         ▼
                    [Handover Ready]
                         │
                    Create linked Epic on
                    platform board ──────────→  [Platform Epic - TO DO]
                         │                           │
                    Auto-routed by POD           Engineering breaks
                                                 into stories/tasks
                         │
                         ▼
                    [Launched]  ←──── (manual, after platform ships)
```

### 11.1 Trigger — Problem creates PRD ticket

When a problem statement is created in the GitHub repo (`outputs/problem-statement.md`):

1. Check if a PRD Jira Epic already exists for this feature
2. If YES → transition to **Prioritized**, add repo link
3. If NO → create a new Epic on the PRD board with status **Prioritized**
4. Set: POD (auto-detect from problem context), priority, platform, reporter, assignee
5. Label with `AI-Workflow`

Sources that can trigger this:
- **Product initiative** — problem identified from research/data in repo
- **Business request** — business creates ticket directly on PRD board
- **User research signals** — patterns from support tickets, user calling, churn data

### 11.2 PRD Board Statuses

| Status | Meaning | What happens |
|--------|---------|-------------|
| **Backlog** | Idea captured, not yet prioritized | Ticket exists but no active work |
| **Prioritized** | Problem validated, in queue | Repo work may have started |
| **In Analysis** | Actively working | Research, synthesis, stakeholder alignment, PRD drafting |
| **Handover Ready** | PRD finalized, ready for engineering | Prototype built, audited, reviews complete |
| **Launched** | Feature shipped | Manual transition after platform confirms |

### 11.3 Platform Routing Rules

When PRD reaches **Handover Ready**, create a linked Epic on the correct platform board based on the POD field:

| POD Category | Routes to |
|-------------|-----------|
| Mobility (all sub-PODs) | **CA** (Customer App) |
| Logistics (YuMove, Order Mgmt, Rider Mgmt, Vendor Onboarding) | **CA** |
| Field Operations (all sub-PODs) | **YZN** |
| Fleet Operations (all sub-PODs) | **YZN** |
| IoT (all sub-PODs) | **YZN** |
| Supply Chain (Inventory, Asset Lifecycle) | **YDS** |

### 11.4 Platform Handoff

**Prompt pattern:**
> "Create a linked Epic on [platform board] for [feature]. Pull summary from the final PRD. Link to PRD ticket. Label with AI-Workflow."

What the platform Epic contains:
- Problem summary + objective (concise)
- Scope table (dimensions/instances/breakdowns)
- Key decisions (what was decided and locked)
- Links to: PRD Jira, final PRD on GitHub, prototype, repo
- Same priority as PRD ticket
- Assignee = engineering owner for that platform

What gets linked:
- PRD ticket ↔ Platform ticket (Relates link)
- Both labeled `AI-Workflow`

### 11.5 Post-Launch

- Engineering breaks the platform Epic into stories/tasks
- When feature ships, manually transition PRD ticket to **Launched**
- Close the loop: update success metrics in PRD based on actual post-launch data

---

## Key Principles

### 1. Context is everything
The AI is only as good as the context you provide. Front-load company context, personas, research, and conventions before asking for any output.

### 2. Iterate, don't regenerate
Never throw away and start over. Edit incrementally. The AI reads existing code/docs and makes targeted changes, preserving everything else.

### 3. Stakeholder feedback is a data input, not a rewrite trigger
Structure feedback, process it through the PRD, and track decisions with attribution. Don't let feedback become a free-for-all rewrite.

### 4. Prototype is the source of truth
When PRD and prototype diverge, align the PRD to the prototype (not the other way around). The prototype is what stakeholders saw and approved.

### 5. Audit everything
Run compliance checks against every requirement source (PRD, stakeholder feedback, design system). Automated audits catch drift that manual review misses.

### 6. Scope changes are surgical
When adding or removing scope, touch every affected file (PRD, prototype, README, user flow). AI is good at this — it finds all references and updates them consistently.

### 7. Design system is law
All visual decisions map to DS tokens. No raw hex values, no ad-hoc colors. When the DS doesn't cover something, document the pattern in Design System Learnings for reuse.

### 8. Every decision needs a "why" and a "who"
The stakeholder refinement log in the PRD is not busywork — it's the answer to every future "why is it this way?" question.
