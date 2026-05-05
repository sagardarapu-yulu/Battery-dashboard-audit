# Yumove Feature Project

This is a feature development project for **Yumove by Yulu** — a last-mile delivery platform powered by electric vehicles.

## Before Starting Any Feature Work

Read these files first:
1. `company-context.md` — Company overview, product, personas, priorities, competitor landscape
2. `prd-template.md` — PRD structure to follow
3. `socratic-questioning.md` — Framework to sharpen feature thinking before drafting

## Workflow

1. **Understand** — Read company context + any research files in `research/`
2. **Question** — Use the Socratic framework to ask 3-5 sharpening questions before writing
3. **Draft** — Generate PRD using the template, incorporating all context and research
4. **Review** — Offer to get feedback from engineering, executive, and user research perspectives
5. **Output** — Save all generated documents to `outputs/`

## Research Files

Drop any supporting data into `research/` before starting:
- CSV exports (funnel data, experiment results, usage data)
- User interview notes or call synthesis docs
- Survey responses
- Competitor analysis

## Review Skills

After drafting a PRD, use these slash commands to review it from three perspectives:
- `/review-engineer` — Engineering feasibility, data sources, computation rules, edge cases
- `/review-designer` — Information hierarchy, DS compliance, user literacy, visual consistency
- `/review-business` — Business outcomes, support deflection, metric alignment, user trust

Skills are in `skills/` (editable). Slash commands in `.claude/commands/` point to them.

## AI Product Workflow

See `ai-product-workflow.md` for the full 10-phase workflow used to build a feature from research to engineering handoff using Claude Code.

## Conventions

- All output files go in `outputs/`
- Name PRDs as `[feature-name]-prd.md`
- Name analysis docs as `[feature-name]-analysis.md`
- When generating multiple PRD versions, suffix with `-v1`, `-v2`, `-v3`
- Final locked PRDs use `-final` suffix
