# Milestone Narrative: The Great Merge

- **Identifier:** NARRATIVE-2026-02-11-001
- **Version:** v0.1.0
- **Date:** 2026-02-11
- **Status:** Active
- **Author:** Historian
- **Scale:** Project-wide

---

## Starting State

On 2026-02-10, the Issues-FS ecosystem comprised 18 repositories (1 parent,
6 modules, 10 roles, 1 human) connected through git submodules. None of the
17 submodules were on their default branch. Feature branches had proliferated
across 6 repositories, accumulating 136 unmerged commits with zero successful
merges since the repositories were created.

The DevOps role produced its first comprehensive ecosystem audit on that date,
quantifying the divergence: 114 tracked issues across the ecosystem, 73 test
files concentrated primarily in the core library, and 4 submodule pointer
drifts in the parent repository. The health assessment classified 8 repos as
Green, 7 as Yellow, and 2 as Red (QA and Human -- both lacking CI
infrastructure entirely).

This was not a crisis of code quality. It was a structural debt: the ecosystem
had been built outward (scaffolding roles, establishing CI patterns, creating
issue tracking) without ever consolidating inward.

---

## Catalyst

Two events on 2026-02-10 created the conditions for the merge:

**1. The DevOps Audit.** The report "136 Commits, Zero Merges" made the
divergence visible and quantifiable for the first time. Before this report,
the state of the ecosystem was known only implicitly through individual
working sessions. The audit converted tacit knowledge into an artefact --
a primary source that could be shared, discussed, and acted upon.

**2. The Stakeholder Interview.** Conducted through ChatGPT voice mode on
behalf of the Journalist role, the interview with Dinis Cruz captured a
clear strategic signal: "We need to ship." The stakeholder reframed the
project's priorities away from code and toward environment, history, and
structure. This gave the merge both urgency (ship working software) and
justification (consolidate the environment before adding more features).

---

## The Merge (2026-02-11)

The stakeholder personally reviewed all 17 submodules, merged all outstanding
branches to their default branches, and deleted every feature branch. This
was a manual, deliberate act -- not an automated process.

### Post-Merge Snapshot

| Repository | Version | Default Branch |
|------------|---------|----------------|
| Issues-FS__Dev (parent) | v0.2.14 | dev |
| Issues-FS (core) | v0.5.1 | dev |
| Issues-FS__CLI | v0.3.1 | main |
| Issues-FS__Docs | v0.1.3 | main |
| Issues-FS__Service | v0.2.2 | main |
| Issues-FS__Service__Client__Python | v0.2.2 | main |
| Issues-FS__Service__UI | v0.2.1 | main |
| 10 Role repos | v0.1.0 -- v0.2.1 | main (8), dev (2) |
| Human repo | (untagged) | dev |

All HEAD commits followed the standard release pattern ("Update release
badge and version file"), indicating that CI pipelines had run cleanly
after the merge.

---

## What the Merge Revealed

The consolidation exposed structural issues that had been invisible while
work was partitioned across feature branches:

1. **`.gitmodules` branch mismatch.** 12 of 17 submodules were configured
   as `branch = dev` in `.gitmodules`, but only 2 repos actually use `dev`
   as their default. This creates a silent failure mode for
   `git submodule update --remote`.

2. **Orphaned remote branches.** 12-14 `claude/*` branches remained on
   GitHub remotes across 6 repos. The git proxy blocks `git push --delete`,
   requiring GitHub API calls for cleanup.

3. **Inconsistent branching strategy.** 2 repos use `dev` as default,
   12 use `main`, 3 have both. No documented convention explains the split.

4. **CLI still non-functional.** The P0 double-path bug (`Path__Handler__Graph_Node`
   prepending `.issues` to paths already rooted in `.issues/`) was fixed in
   the core library but the CLI still returned 0 results on real repos with
   114 issues on disk. The merge consolidated the fix but did not verify
   end-to-end functionality.

These were not new problems. They were pre-existing conditions that the merge
made visible by removing the complexity of divergent branches.

---

## Pivot Point Analysis

The Great Merge is a pivot point at three scales:

### Micro: Development Workflow
Before the merge, each working session created feature branches that
accumulated indefinitely. After it, the ecosystem has a clean baseline.
The Conductor's Workstream and Iteration plan (produced immediately after
the merge) is the first document that could reference a consistent, shared
state across all repos. Planning was impossible when every repo was on a
different branch at a different commit.

### Project: From Scaffolding to Integration
The merge marks the boundary between two epochs. The first epoch
(roughly 2026-01-27 to 2026-02-10) was characterised by outward
expansion: creating repositories, defining roles, establishing CI patterns,
writing ROLE.md files, provisioning infrastructure. The second epoch
(beginning 2026-02-11) is characterised by inward integration: making the
pieces work together, resolving the issues surfaced by the audit, shipping
working software.

The evidence for this transition is structural, not aspirational. The
Conductor's Iteration 1 plan is titled "Ship and Stabilise" and prioritises
bug fixes and cataloguing over new features. The stakeholder's interview
priorities ("we need to ship", "Librarian is one of the most important
roles") align with this shift.

### Meta: Role-Based Coordination
The merge is the first event that required coordination between a human
stakeholder, a DevOps audit, a Journalist article, and a Conductor plan.
Four distinct roles contributed to different phases: DevOps surfaced the
data, Journalist synthesised the narrative, the stakeholder acted, and
the Conductor planned what comes next. This is the role-based workflow
operating as designed, not as documentation.

---

## Causation Chain

```
DevOps Audit (2026-02-10)
  └── Made divergence visible and quantifiable
       └── Journalist synthesis: "136 Commits, Zero Merges"
            └── Stakeholder reads audit + completes interview
                 └── "We need to ship" + manual merge of all 17 submodules
                      └── Clean baseline enables:
                           ├── Conductor: First Workstream/Iteration plan
                           ├── DevOps: Post-merge tag/commit mapping
                           ├── Journalist: "The Great Merge" article
                           └── Historian: This milestone narrative
```

---

## Primary Sources

| Source | Location | Type |
|--------|----------|------|
| DevOps pre-merge audit | `roles/Issues-FS__Dev__Role__DevOps/docs/report__2026-02-10__submodule-status.md` | Report |
| DevOps post-merge mapping | `roles/Issues-FS__Dev__Role__DevOps/docs/report__2026-02-11__tag-commit-mapping.md` | Report |
| Journalist pre-merge article | `roles/Issues-FS__Dev__Role__Journalist/publications/_articles/2026-02-10__submodule-ecosystem-status.md` | Article |
| Journalist merge article | `roles/Issues-FS__Dev__Role__Journalist/publications/_articles/2026-02-11__the-great-merge.md` | Article |
| Stakeholder interview | `roles/Issues-FS__Dev__Role__Journalist/publications/interviews/2026-02-10/v0.1.3__chatgtp-stakeholder__raw-interview.md` | Transcript |
| Conductor Iteration 1 plan | `roles/Issues-FS__Dev__Role__Conductor/docs/plan__2026-02-11__workstream-and-iteration.md` | Plan |

---

*Narrative constructed from primary sources listed above. Interpretive claims
are labelled as such. All factual claims are traceable to the source documents.*
