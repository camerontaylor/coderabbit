# Shared CodeRabbit configuration

Central CodeRabbit defaults for repositories owned by `camerontaylor`.
The repository must be named `coderabbit`, with `.coderabbit.yaml` at its root
on the default branch. Local checkout: `/Volumes/offload/neptune/repos/coderabbit`
(also reachable through `~/repos/coderabbit` on Neptune).

## Review policy

- One initial automatic review for eligible non-draft PRs; no automatic review
  on subsequent pushes (`reviews.auto_review.auto_incremental_review: false`).
- Review the default branch and `merge-queue`; skip titles containing `WIP` or
  `do-not-review`.
- Concise, high-confidence feedback focused on correctness, security, API
  compatibility, and data integrity. Keep summaries, status, and agent prompts;
  disable poems, fortunes, and chat art.
- Keep reviews advisory. Deterministic CI and repository rules enforce merging.
- Keep tests, manifests, lockfiles, and workflows in scope. Exclude dependency,
  coverage, and local agent scratch directories. Define build-output exclusions
  per repository rather than assuming every `dist/` directory is disposable.
- Use each repository's own detected coding guidelines. The cq-toolkit kernel,
  doctrine, test runner, and workflow-template rules are intentionally not global.

This limits automatic push-triggered reviews, not the lifetime review count.
Manual commands can request more reviews:

```text
@coderabbitai review
@coderabbitai full review
```

The first requests an incremental review; the second re-reviews the full PR.
The commit-count pause setting is not a review-count cap and is unnecessary
when automatic incremental reviews are disabled. Draft PRs are skipped; verify
review behavior when moving your first draft to ready for review.

## Enable and verify

1. Ensure the CodeRabbit GitHub App can access this repository and the
   repositories to review. An installation covering all repositories should
   include it; a selected-repositories installation must add `coderabbit`.
   Manage access at <https://github.com/settings/installations>.
2. Repositories without their own `.coderabbit.yaml` use central configuration.
   For a repository with local rules, add `inheritance: true` at its YAML root.
   Keep the local rules; they override central values where specified.
   See [the minimal example](examples/repository.coderabbit.yaml).
3. On a suitable PR, comment `@coderabbitai configuration` and check that the
   resolved configuration includes the central source and
   `auto_incremental_review: false`. Also verify that another push does not
   start an automatic incremental review.

CodeRabbit documents central configuration in organization terms. This repo
uses the same owner namespace as `camerontaylor/cq-toolkit` (a personal GitHub
account). Confirm discovery using the resolved-config command before assuming
account-wide adoption; schema validation alone cannot prove discovery or access.

`cq-toolkit` already has local YAML without inheritance, so creating this repo
alone does not change its effective configuration. Adding `inheritance: true`
there is a separate adoption change; it retains its specialized instructions.
The CLI's author-side review cycles are separate from GitHub App auto-reviews.

Configuration sources do not merge by default. Inheriting repositories merge
objects, override scalars, and combine arrays (path instructions deduplicate by
path). This central file deliberately sets `inheritance: false` to stop the chain
at these version-controlled defaults. Global overrides in the CodeRabbit UI
still take precedence. Check those if the effective settings differ.

## Maintenance

Edit `.coderabbit.yaml`, explain non-obvious settings in comments, and validate
against CodeRabbit's current schema before committing:

```sh
coderabbit config validate .coderabbit.yaml
coderabbit config validate examples/repository.coderabbit.yaml
git diff --check
```

These local validation commands do not require a CodeRabbit login. Keep the
config small; use repository-specific YAML for language tools, generated paths,
architecture constraints, or stricter review profiles.

Recommended GitHub settings for this configuration repository: public visibility,
squash merges, delete merged branches, and disable unused wiki/projects. Keep
issues enabled for policy discussions. Add branch protection and a required
schema-validation check when a CI validator is established; do not require a
check that does not exist. There is no mandatory CI gate in this bootstrap.

## References

- [Central configuration](https://docs.coderabbit.ai/configuration/central-configuration)
- [Automatic review controls](https://docs.coderabbit.ai/configuration/auto-review)
- [Configuration inheritance](https://docs.coderabbit.ai/configuration/configuration-inheritance)
- [Configuration reference](https://docs.coderabbit.ai/reference/configuration)
