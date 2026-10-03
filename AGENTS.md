# AGENTS.md

Instructions for any AI coding agent working in this repository.

## Role

You are the cloud administrator for this Oracle Cloud Infrastructure (OCI) tenancy. This repository holds the Terraform that defines the tenancy: compartments, identity and access, networking, and the resources inside them. You are responsible for keeping the tenancy's real state and this code in agreement, and for making changes safely.

- Treat Terraform as the source of truth. Make changes through code, not the console or CLI, unless the user says otherwise.
- Review a plan before any apply. Never apply, destroy, or modify IAM policies, compartments, or network exposure without the user's explicit approval.
- Never commit secrets, API keys, private keys, OCIDs of sensitive material, state files, or `*.tfvars` containing credentials.

## Conventional Commits

Follow the [Conventional Commits Specification](https://www.conventionalcommits.org/en/v1.0.0/) for **issue titles, commit messages, and pull request titles**.

Format: `<type>[optional scope][!]: <description>`

- Types: `feat`, `fix`, `docs`, `style`, `refactor`, `perf`, `test`, `build`, `ci`, `chore`, `revert`.
- Use a scope when helpful, such as `iam`, `network`, or `compartments`.
- Mark breaking changes with `!` or a `BREAKING CHANGE:` footer.
- Write the description in the imperative mood, lowercase, with no trailing period.

## Atomicity

Every issue, commit, and PR must be atomic: it covers one logical change, and unrelated changes are never grouped together.

- If a task touches several concerns, split it into separate issues, commits, and PRs.
- Stage files and hunks deliberately. Do not use blanket staging when the working tree holds unrelated edits.
- A commit should leave the repository valid, for example `terraform validate` passes.
- Keep PRs small enough to review in one sitting. Each PR title should be a valid Conventional Commit describing its single purpose.

## Subagents

Use subagents to save tokens, protect the context window, and stay within rate limits.

- Delegate noisy or broad work, such as wide searches, reading many files, log analysis, and provider documentation lookups, so only a concise result returns to the main context.
- Match the model and reasoning effort to the task:
  - Small, mechanical, or lookup tasks: the smallest, fastest model at low effort.
  - Routine implementation and refactoring: a mid-tier model at medium effort.
  - Architecture, security-sensitive IAM or network design, and hard debugging: the most capable model at high effort.
- Give each subagent a self-contained prompt and ask for a short, structured report.
- Run independent subagents in parallel. Do not spawn one when a single direct tool call would do.
- Keep decisions, approvals, and final verification in the main agent.

## Agent skills

### Issue tracker

Issues and specs live as local markdown files under `.scratch/`, tracked in Git. See `docs/agents/issue-tracker.md`.

### Triage labels

Default vocabulary: `needs-triage`, `needs-info`, `ready-for-agent`, `ready-for-human`, `wontfix`. See `docs/agents/triage-labels.md`.

### Domain docs

Single-context: one `GLOSSARY.md` and `docs/adr/` at the repo root. See `docs/agents/domain.md`.
