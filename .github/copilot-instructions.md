# Copilot Onboarding Instructions

## 1. Repository Summary
- Name: **p6m7g8-actions/next-upgrade**
- Purpose: A composite GitHub Action to bump and upgrade dependencies in a Next.js static‐site project, regenerate diffs, and open PRs.
- Tech stack:
  • YAML for workflows  
  • Bash scripts inside composite steps  
  • Node.js via `pnpm` (managed by `p6m7g8-actions/node-setup`)  
  • Tailwind CSS update  
- Size: small (< 50 files). No compiled code beyond workflow shell steps.

## 2. Build & Validation Instructions
Always start from a clean clone:
1. `git clone … && cd next-upgrade`
2. Install tools:
   • Ensure Node.js (v16+) and `pnpm` are installed  
   • Ensure GitHub CLI (`gh`) is authenticated  
3. Dependency sanity (if you ever add JavaScript code):
   ```bash
   pnpm install          # install any added JS dependencies
   ```
4. YAML/JSON validation:
   ```bash
   yamllint .            # catch indentation or schema errors
   jq . .vscode/settings.json  # verify JSONC syntax
   ```
5. Workflow simulation (for changes to `.github/workflows/*`):
   ```bash
   act --secret GITHUB_TOKEN=$GH_TOKEN
   ```
6. Final check: push branch and confirm all CI workflows pass on GitHub.

## 3. Project Layout & Key Files
- `/action.yml`  
  Composite action definition (checkout, node setup, `pnpm up`, PR creation).
- `/README.md`  
  Usage snippet for consuming this action.
- `/LICENSE`  
  Apache 2.0 license.
- `/.vscode/settings.json`  
  Editor formatting preferences.
- `/.github/workflows/`  
  • `release.yml` ‑ version bump & GitHub Release via `gh` CLI  
  • `pull-request-lint.yml` ‑ enforces semantic PR titles  
  • `pr-labeler.yml` ‑ auto-labels core PRs and auto-merge  
  • `build.yml` ‑ no-op build (always passes)  
  • `auto-queue.yml` ‑ merges PRs via merge queue after CI  
  • `auto-approve.yml` ‑ auto-approves eligible PRs  

## 4. Development & Change Tips
- Always run validation (`yamllint`, `act`) before opening a PR.
- If adding scripts, update `action.yml` inputs/steps and rerun CI simulation.
- Trust these instructions first; only grep/search if something seems outdated.
- Confirm any new bash snippet works locally before committing.

> These guidelines are your single source of truth to avoid CI failures, lint errors, and iteration overhead.  
