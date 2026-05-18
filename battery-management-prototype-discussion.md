# Battery Management System — Prototype Discussion Notes

## Overview

This document captures a prototype/design discussion around a **battery ageing and inventory management system** for a vehicle operations facility (warehouse/VC). The conversation covers bucketing logic, physical rack assignment, app-side visibility, and mechanic/QA workflows.

---

## Core Problem Statement

> **How do we ensure mechanics pick the oldest batteries first, and that batteries don't sit idle for too long?**

- Batteries that are not used within **48 hours** are considered aged/problematic.
- The goal is to reduce the number of batteries sitting idle beyond 48 hours.
- Mechanics should pick the **oldest** batteries for RTD (Ready to Deploy) bikes.
- R&M (Repair & Maintenance) mechanics should pick the **youngest/newest** batteries.

---

## Key Concepts Discussed

### Battery States
- **Charged** — ready to use
- **Discharged** — needs charging
- **Faulty** — defective, to be sent to quality/Yuma
- **Missing** — not found during audit
- **Extra** — found during audit but not logged in the system
- **Inside bike** — currently attached to a vehicle

### Battery Age Buckets (Proposed)

| Bucket | Criteria | Action |
|--------|----------|--------|
| **Red (P1)** | Oldest batteries / >48 hours idle | Priority pick for RTD bikes by QA |
| **Yellow (P2)** | 24–48 hours idle | Mixed use — RTD or R&M |
| **Green (P3)** | <24 hours idle | R&M mechanics should pick from here |

> **Note:** The exact threshold logic (fixed count vs. time-based) was debated and not fully resolved. Two main approaches were discussed:
> 1. **Top N oldest batteries** (e.g., top 10–15) form the red bucket — dynamic membership, fixed count.
> 2. **Time-based threshold** — all batteries >48 hours go into red automatically.

---

## Roles & Responsibilities

| Role | Responsibility |
|------|----------------|
| **IA (Inventory/Asset Manager)** | Manages battery movement, does audits, maintains rack buffers, acts on app visibility |
| **Mechanic (R&M)** | Picks youngest/charged batteries for repair tasks; removes battery after R&M task |
| **QA** | Picks oldest/priority batteries for RTD bikes |

---

## App Features Discussed

### 1. Visibility Dashboard (for IA)
- Count and list of batteries in each bucket (Red / Yellow / Green)
- Idle battery count
- Faulty battery count
- Batteries inside bikes

### 2. Audit Feature (existing, to be integrated)
- IA audits all batteries at the start of shift (or at least once per shift)
- System shows: **Pending** (missing) vs. **Audited**
- Missing = battery not found; Extra = unlogged battery found
- Audit covers **loose batteries only** — batteries inside bikes are tracked separately

### 3. Mark as Faulty (Quick Access)
- Existing feature — to be surfaced as a **quick-access button** on the dashboard
- IA can mark faulty batteries during audit or inspection
- Once marked, battery moves to the faulty rack/state

### 4. Notification / Trigger for IA
- When Red (P1) bucket drops below a threshold (e.g., **5 batteries**), IA gets a notification
- IA refills P1 up to the buffer target (e.g., **15 batteries**)
- This avoids constant manual monitoring

### 5. Battery List View (on clicking a bucket)
- Shows battery IDs in that bucket
- **No in-app CTA** for physical movement — action is done offline
- App gives visibility; physical action is manual

---

## Physical Rack / Bucketing

### Proposed Setup
- Dedicated **physical shelves/racks** for each bucket (P1 / P2 / P3 or Red / Yellow / Green)
- IA physically moves batteries between shelves based on app visibility
- Mechanics and QA pick from designated racks

### Key Challenge
- Time-based bucket membership is **dynamic** — a battery at 45 hours becomes 48+ hours after 3 hours
- Keeping physical racks and app state in sync is **operationally difficult**
- Consensus: physical bucketing accuracy will degrade over time; app visibility is the source of truth

### Resolution
- App shows the correct groupings at all times
- IA periodically (not in real time) reconciles physical racks with app state
- Notification-driven refill of P1 is the primary control mechanism

---

## Open Questions / Unresolved Items

1. **Red bucket definition** — Fixed top-N (e.g., oldest 10–15) vs. time-threshold (>48 hrs)?
2. **Mechanic compliance** — How do we enforce that mechanics pick from the right bucket? (Currently relies on manual instruction, not app enforcement)
3. **Battery age baseline** — When does the age clock start? From trip end? From bike detachment? From arrival at warehouse?
4. **Rack assignment feature** — Should the app include a formal rack-assignment flow, or is visibility alone sufficient?
5. **Safety audit integration** — Should the existing audit feature be merged into the ageing dashboard, or kept separate?
6. **Inside-bike batteries** — Charge status is partially known; faulty status inside bikes is not tracked. Should this be in scope?

---

## Agreed Points

- **Colour coding** (Red / Yellow / Green) is the agreed bucketing approach — no misalignment on this.
- **Faulty marking** is an existing feature; needs to be promoted as a quick-access option.
- **Audit feature** should be visible within the same battery dashboard screen.
- **IA owns** the responsibility of shifting batteries and maintaining rack buffers.
- **Mechanics (R&M)** must always use a **charged battery** — they must put one in to do R&M, and remove it after.
- The system will not track real-time physical movement between racks — visibility drives manual action.
- Scale is manageable: ~20–30 batteries per VC at any given time.

---

## Proposed Feature Scope Summary

| Feature | Priority | Notes |
|---------|----------|-------|
| Battery ageing visibility (Red/Yellow/Green buckets) | High | Core feature |
| Idle battery count & IDs | High | Part of dashboard |
| Faulty battery quick-access marking | High | Existing, needs surfacing |
| Audit integration in dashboard | High | Existing feature, merge view |
| P1 buffer notification for IA | Medium | Trigger-based refill |
| Rack assignment flow (in-app) | Low / Deferred | Operationally complex; revisit |
| Inside-bike battery tracking | Out of scope (for now) | Separate concern |
| Safety audit (battery scan) | Out of scope (for now) | Separate project |

---

*Notes compiled from prototype discussion session. Next step: review open questions, align on red bucket definition, and confirm feature scope before development.*
