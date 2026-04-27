# GitHub Actions Workflow Standards

Load this file when the task needs detailed guidance for writing, reviewing, or
hardening GitHub Actions workflows.

## Scope

Apply these conventions to workflow files in `.github/workflows/`, reusable
workflows, and pull requests that change CI/CD automation.

## Workflow Design

- Keep workflows task-oriented.
  Split CI, release, deploy, and maintenance workflows unless there is a strong reason to combine them.
- Use clear workflow and job names.
  Favor names that match repository intent, such as `ci`, `release`, `deploy-preview`, or `dependency-update-check`.
- Prefer small jobs with explicit `needs` relationships over a single oversized job.
- Keep shell steps focused and short.
  If shell logic becomes complex, move it into versioned scripts in the repository.

## Trigger Design

- Use the narrowest trigger that matches the job.
- Add branch, tag, and path filters when they materially reduce noise or cost.
- Use `workflow_dispatch` for manual operations such as ad-hoc releases, backfills, or administrative tasks.
- Use `workflow_call` for stable reusable workflows shared across repositories or across multiple workflows in the same repository.
- Be cautious with `pull_request_target`.
  Treat it as a high-risk event and avoid it unless the workflow specifically requires base-repository privileges.

## Runners

- Prefer GitHub-hosted runners unless the task needs private network access, special hardware, or a preloaded environment.
- Choose the smallest runner class that is sufficient for the job.
- If a workflow must support multiple operating systems or multiple language versions, use a matrix only when those dimensions are meaningful to the repo.

## Permissions and Credentials

- Set explicit top-level or job-level `permissions` instead of relying on broad defaults.
- Prefer least privilege. Grant write scopes only to jobs that actually mutate repository state, publish artifacts, or create releases.
- Prefer `GITHUB_TOKEN` for GitHub API and repository operations when it is sufficient.
- Prefer OIDC over long-lived cloud credentials when the target platform supports it.
- Use environments and protection rules for production deployments and other privileged release paths.

## Secrets and Untrusted Input

- Treat workflow contexts and user-controlled event payloads as untrusted input.
- Do not interpolate untrusted values directly into inline shell where they can change command structure.
- Prefer passing values through environment variables or action inputs with careful quoting.
- Avoid exposing secrets to jobs that run on untrusted pull request code paths.

## Third-Party Actions and Reusable Workflows

- Prefer reputable actions with active maintenance and a clear security posture.
- Pin third-party actions to a full-length commit SHA when the repo's security bar requires strong supply-chain integrity.
- Reusing third-party workflows carries similar trust risks to third-party actions.
- Prefer reusable workflows for repeated multi-job pipelines.
- Prefer composite actions for repeated step bundles that do not need multi-job orchestration.

## Caching and Artifacts

- Add caching only when the saved runtime justifies the extra complexity.
- Treat caches as untrusted data.
- Avoid writing caches from untrusted pull request contexts.
- Use artifacts for outputs that must move across jobs or be inspected after a run.
- Keep artifact retention and scope aligned with the repository's operational needs.

## Matrix Strategy

- Use a matrix for dimensions that matter to correctness, compatibility, or release confidence.
- Avoid oversized matrices that mostly add cost and noise.
- Add fail-fast behavior or selective exclusions when some combinations are known to be unnecessary.
- Keep deployment jobs outside the test matrix unless each matrix dimension genuinely deploys separately.

## Concurrency and Cancellation

- Use workflow-level or job-level `concurrency` when duplicate runs create waste or risk.
- For branch-oriented CI workflows, consider canceling in-progress runs for the same branch when a newer commit arrives.
- Ensure concurrency group names are scoped so unrelated workflows do not cancel each other accidentally.

## Reuse and Duplication

- When the same pipeline shape appears across multiple repositories or multiple workflow files, prefer reusable workflows.
- When only a small step sequence is repeated, prefer a composite action or a checked-in script.
- Do not over-abstract early. Duplicate first if the pattern is still unstable.

## Deployments and Releases

- Separate test/build validation from privileged deploy or publish jobs when possible.
- Use protected environments for production or sensitive targets.
- Use OIDC for cloud deploys and package publishing where supported.
- Keep release workflows explicit about tags, branches, build artifacts, and approval points.

## Review Heuristics

When reviewing a workflow change, check:

- whether the trigger is too broad or too narrow
- whether permissions are broader than necessary
- whether secrets can reach untrusted code paths
- whether third-party dependencies are pinned appropriately
- whether matrices, caches, and concurrency improve or degrade maintainability
- whether the workflow duplicates an existing reusable pattern
