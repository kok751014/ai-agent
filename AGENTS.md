# Work intake
- Natural-language requests are actionable as-is; no prompt template is required.
- Determine the requested outcome before branch/task/delivery decisions: read-only inquiry or analysis, PR creation, explicitly limited changes, or full implementation.
- Read-only inquiries, explanations, reviews and analysis proceed after preflight on the current working tree even when unrelated changes, unpushed commits or unfinished Feature tasks exist. Preserve files and Git state, do not update todo, do not resume implementation/delivery, and finish by reporting findings.
- For change requests, full end-to-end delivery is the default scope; merely mentioning spec, todo, documentation or planning inside a request never narrows it.
- Only an explicit limit stated by the human narrows a change request (for example: spec only, todo only, planning only, documentation only, do not implement, or wording that clearly means no code implementation). Judge this by meaning, never by keyword matching.
- Explicitly limited change request: deliver only the requested scope; never expand it into production implementation, test implementation or full Feature delivery. `spec.md` still holds current truth, `todo.md` still holds progress only, and no session handoff, chat summary or extra state file is created.
- Preflight first: current branch, git status, uncommitted diff, unpushed commits, this file, then only the relevant project docs/spec/todo/code/tests. Preserve unrelated work.
- Do not make the human provide branch names, task IDs, commit messages, test commands, remote names or spec paths when the repository can determine them safely.
- Rebuild state from the repository every time; chat history is never project state.
- When continuing the current Feature, finish its verified but uncommitted or unpushed delivery first, then continue `[~]` or start the next `[ ]`; never sweep unrelated changes into it.
- Preserve the legacy project's existing architecture, frameworks, naming, dependencies and build flow unless the current requirement explicitly changes them. Never use this workflow as an excuse for unrelated refactoring.
- Decide low-risk, reversible and architecture-consistent details from the request and repository without confirmation. A topic alone is never a reason to ask; ask exactly one blocking question only when the available requirement, docs/spec, code, tests and existing architecture cannot settle an important business, API, security, destructive-data, major-architecture, external-provider/licensing or other mutually incompatible choice safely.

# Branch
- Determine the repository's normal integration branch and the remote/upstream used for that branch from existing docs, branch tracking, branch usage or PR conventions. If either cannot be determined safely when a remote update is required, ask one blocking question.
- Before any new change request that does not continue an existing Feature, including documentation-only or explicitly limited changes: after preflight and the existing-work checks below, switch to the integration branch, run `git pull --ff-only <remote> <integration-branch>` (or the repository-equivalent command using that branch's configured upstream), confirm the local integration branch equals the remote tip obtained by that pull, then create `feature/{short-readable-english-kebab-case}` from the updated integration branch before changing files. Do not ask the human to switch or pull when the repository makes these values clear.
- Even when already on the integration branch, a new change request still performs the required `--ff-only` pull before creating the feature branch.
- Follow-up requirement, acceptance fix, bug or next Task for an existing Feature: continue the current feature branch without switching to the integration branch, pulling, or creating another branch, even in a new chat/session.
- Unrelated change request while the current branch has uncommitted changes, an uncommitted or unpushed delivery, or any `[ ]` / `[~]` task belonging to the Feature in progress: stop and report; never switch away, stash or hide the work.
- Only unfinished tasks of the Feature in progress block switching; `[ ]` backlog belonging to other future Features does not.
- If switching or pulling fails, or the local integration branch does not equal the remote tip obtained by that pull (including local-only commits), stop and report without creating the new branch or starting changes. Never fall back to a stale baseline, merge, rebase or reset.
- The new-work `--ff-only` pull above, including the fetch it performs, is the only automatic remote-update exception. Otherwise never fetch, pull, merge or rebase automatically. Explicit human authorization in the current conversation may permit a specific synchronization/integration action.
- Read-only inquiries stay on the current working tree and do not enter branch/update flow. PR creation follows the separate Pull requests rules below.

# Spec and todo
- Establish or update the relevant spec/todo before production implementation when work changes long-lived functional/technical current truth: a non-trivial Feature or major Feature extension, business rule, API/request/response contract (including fields, types or semantics), important state/acceptance behaviour, persistence/schema/data rule, permission/security behaviour, or other multi-step work likely to span sessions.
- Reuse an existing relevant spec/todo when present; do not create a second Feature folder for the same area or because a new session starts.
- For a legacy area that has no spec/todo yet, create them lazily only when the current work meets the trigger above. Do not scan the whole repository to backfill old modules, and never create a historical todo full of `[x]` items for already merged work.
- Reuse the repository's existing documentation convention when one exists; otherwise use `specs/{area-or-feature}/spec.md` and `specs/{area-or-feature}/todo.md`.
- `spec.md` holds current functional/technical truth only. Rewrite it when intended behaviour changes.
- `spec.md` never contains task progress, pending/completed/next-task text, implementation history, AI reasoning, session handoff or design diary.
- `todo.md` holds progress only: `[ ]` not started, `[~]` in progress, `[x]` done; never copy the full requirement into it.
- When implementation/work on a task begins, the task is `[~]`; never create a task directly as `[x]`.
- Under an explicitly limited planning/spec/todo/docs request, a task that is written but not actually started stays `[ ]`.
- Tiny wording changes, formatting, documentation-only edits, non-functional comments and narrow fixes may skip creating new spec/todo when they do not change a real requirement, contract or long-lived current truth. A small change that does change such truth must at least update the relevant spec; use todo as well when implementation progress needs tracking.
- Git history holds past implementation/change history.

# Tasks
- Split work only when there are several independently meaningful and verifiable steps.
- A task is the smallest meaningful implementable, testable and committable unit.
- Do not manufacture tasks for process ceremony or split one small fix into multiple tasks.
- The human's bullet points are not task boundaries; fold a tiny change tightly related to another task into that task instead of mechanically creating one task per bullet.

# Delivery
- For tasks that change files, normal scope per meaningful task is: todo `[~]` → implement → targeted verification → required docs/contracts → the verification this change actually needs → diff self-review → todo `[x]` → explicit staging → one logical commit → push current feature branch.
- Explicitly limited changes run only the steps needed to produce and validate that requested output; still self-review, explicitly stage, commit and push the resulting output, but do not enter omitted implementation steps. A task written but not started stays `[ ]`.
- Verification strength follows the actual change. Production/runtime code, project/build files, tests, scripts, configuration, dependencies/lockfiles, API contracts, persistence, background jobs, security or anything else that can affect runtime behaviour use the repository's full verification path; a mixed task takes the higher strength.
- A change confined to Markdown or other prose documents with no runtime/contract/tooling impact uses only relevant content/format/diff checks and the whole-task self-review instead of forcing full repository verification.
- Read-only inquiries, explanations, reviews and analysis run no build/test/verification and never enter this delivery loop. PR creation follows Pull requests, including any required delivery fixes or verification.
- Use the repository's existing verification entry point when available. Otherwise use its documented build/test/lint/format commands. Do not invent a parallel toolchain merely for this workflow.
- If full verification is required but no full verification command exists, run the strongest existing build/test/lint checks that fit the change and report the limitation.
- A finished changed-file task is tested/checked, committed and pushed without asking whether to do those routine steps.
- Mark `[x]` only after implementation, required tests/checks, the required verification and diff self-review have all completed successfully.
- If implementation, configuration, spec, documentation, tests or dependencies change during or after verification, keep/revert the task to `[~]` and rerun the required verification on the final stable tree.
- Flipping `[~]` to `[x]` after successful verification/self-review is a progress marker only and does not itself require another verification run.
- Follow the repository's existing commit convention. If none exists, use `<type>: <concise Traditional Chinese result>` where type is one of `feat`, `fix`, `refactor`, `perf`, `docs`, `test`, `chore`.
- Keep commits logical and small; do not squash or rewrite earlier commits unless the human asks.
- Do not add AI attribution such as `Generated by AI` or AI `Co-Authored-By` trailers unless the human explicitly requests it.
- Never use `git add .`, `reset --hard`, `clean -fd`, `restore .`, `checkout -- .`, or force push.

# Pull requests
- Finishing implementation never creates a PR by itself. Create one only when the human explicitly says `建立 PR`.
- PR creation is a separate packaging workflow, not an ordinary read-only inquiry and not a second delivery/verification stage: confirm the Feature is complete, the working tree is clean, there is no uncommitted or unpushed delivery, relevant spec/todo match reality, and self-review the whole Feature diff once.
- Reuse verification already completed by Task delivery. Do not rerun build/test/lint/format/contract/migration/deployment or another full verification merely because a PR is being created. Rerun only the verification required by changes made after the last successful required verification, or when the current branch tip cannot be established as that verified final tree. Todo marking, commit, push and PR-description work do not by themselves trigger re-verification.
- If PR preflight finds a spec/todo/documentation/implementation gap, leave PR packaging and return to the normal delivery loop to fix, verify, self-review, commit and push it before trying the PR again.
- Use read-only remote-ref checks (for example `git ls-remote`) to confirm the feature branch is pushed at current HEAD and the remote target integration branch is still at the commit used as the current integration/verification baseline. If the target SHA differs, stop and report that the Feature has not been verified against the latest integration baseline; if the remote cannot be confirmed, report it as unconfirmed rather than equal. Never fetch, pull, merge or rebase automatically to resolve this.
- If the human explicitly authorizes syncing or integrating the target branch after such a stop, follow that authorization and the repository's safe Git conventions.
- Search for the repository/company PR template in this order: current branch/HEAD, PR target branch, repository default branch. Resolve the default branch with a read-only remote query such as `git ls-remote --symref origin HEAD` or the equivalent for the repository's primary remote; before reading a local remote-tracking copy, verify its SHA matches the remote SHA. If the default-branch object/template cannot be verified because the ref is stale, shallow/single-branch history is missing, or another read-only check fails, stop/report instead of assuming no template; do not fetch/pull automatically. Only after all three locations are confirmed absent may you write a concise engineering PR description.
- Target the repository's normal integration branch. If the target cannot be determined safely, ask one blocking question.
- Never approve or merge the PR.
- PR descriptions contain engineering information only; no AI attribution, marketing text or session diary.

# Safety
- Never overwrite or discard unrelated local work.
- Never commit credentials, tokens, private keys, connection strings or local secrets.
- Never execute destructive database/data operations without explicit authorization.
- Do not create AI session logs or handoff logs.
- Do not add new architecture rules, packages, CI, hooks, skills, verification frameworks or other tooling unless the current task actually requires them.
