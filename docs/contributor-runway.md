# Contributor Runway

This guide defines how first-time contributors can land meaningful changes quickly.

## Fast Start

1. Clone with submodules and run bootstrap:

```bash
git clone --recurse-submodules <repo-url>
cd organvm-v-logos
./tools/bootstrap_dev.sh
```

2. Pick a scoped issue labeled `good first issue`.
3. Run `./validate.sh` before opening a PR.

## First Contribution Scope

A first contribution should:

- Touch a small, isolated surface area.
- Have clear acceptance criteria.
- Be testable with existing workflows.
- Avoid cross-repo architectural refactors.

## Review SLA Targets

- First maintainer response: within 3 business days.
- Review turnaround after requested changes: within 3 business days.
- Merge decision after final approval: within 2 business days.

## Required PR Evidence

- What changed and why.
- Validation output summary.
- Linked issue with acceptance criteria.

## Escalation

If blocked for >7 days without review, open a `documentation` issue in `organvm-v-logos/.github` with link to the PR and blocker details.
