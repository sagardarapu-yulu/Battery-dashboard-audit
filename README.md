# Feature Starter Kit

Base project for building new features with Claude Code — from research to engineering-ready PRD.

## Setup
1. Fill in `company-context.md` with your company info
2. Drop any research files (CSVs, interview notes, data) into `research/`

## Usage
Open this folder in Claude Code and prompt:

```
Help me build a PRD for [feature idea]. Use @company-context.md, @prd-template.md, and @socratic-questioning.md
```

Generated docs will be saved to `outputs/`.

## Review Skills

After your PRD is drafted, review it from three stakeholder perspectives:

```
/review-engineer    — Engineering feasibility & buildability
/review-designer    — Design system compliance & user literacy
/review-business    — Business outcomes & support deflection
```

Skills are in `skills/` — edit them to match your team's actual stakeholders.

## AI Product Workflow

See `ai-product-workflow.md` for the detailed 10-phase workflow covering:
Research → Problem → PRD → Stakeholder Review → Refinement → Design Audit → Prototype → User Flow → Engineering Handoff

## Project Structure

```
.
├── company-context.md         # Company overview, personas, competitors
├── prd-template.md            # PRD structure to follow
├── problem-template.md        # Problem statement structure
├── socratic-questioning.md    # Framework to sharpen thinking
├── ai-product-workflow.md     # Full AI-powered product workflow
├── skills/                    # Review skills (editable)
│   ├── review-engineer.md
│   ├── review-designer.md
│   └── review-business.md
├── .claude/commands/          # Slash command pointers
├── research/                  # Drop raw inputs here
└── outputs/                   # All generated docs
```
