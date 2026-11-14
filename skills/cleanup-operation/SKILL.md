---
name: cleanup-operation
description: "Use only when the user asks for cleanup-operation, or to run a big cleanup, untangle or weeding operation across a codebase."
---

A cleanup operation often deletes tens or hundreds of thousands of lines of code. Treat the codebase as a garden: pull every weed, and set it up so weeds don't grow back. The goal is a codebase that is more pleasant, simpler and easier to work in, so changes land faster with confidence, with fewer bugs (ideally none) and high quality. Hold a high bar and go above and beyond.

## 1. Fill the slots

You need four facts: the **project** (one or more repos), the **deadline** (when usage or the session ends), the **usage left** (how much of the model usage limit remains), and what is **out of scope** (for example "mobile, nobody uses it"). Take them from the user's message. Ask once, in one batch, only for those they didn't give; out of scope defaults to nothing. Turn the deadline into an absolute time with `date` in the user's timezone; when it has passed or is under 75 minutes away, say so and ask for a new one before launching anything. Done when all four are known and the deadline is in the future.

When the user names several projects, run one lead agent per repo with this skill, and have each lead touch only its own repo.

## 2. Set up

- Switch to the default branch, fetch, and work in a worktree.
- Run heavy lanes on the machine or cloud runner that can take the load, not on a light laptop.
- Spend the usage. Whatever is left before the deadline is a blessing of abundance, so parallelize hard. Invoking this skill is the user's go-ahead to use the strongest model at high effort, with multi-agent workflows, for audit, redesign and hard refactors. Use a faster model for implementation, reconciliation and mechanical edits.
- Keep a decision log and a session log in the repo's `scratchpad/`, because context compaction will happen.

Done when the worktree is on a fresh default branch and the first audit lanes are running.

## 3. Scope

This is one operation. When the user adds scope mid-run, it adds to the work and never changes its direction, so keep every other item going.

1. **Untangle.** Run [untangle-codebase.md](references/untangle-codebase.md) over the whole repo: read-only audit lanes per area first, then implementation lanes. Skip its step 1 ("Ask"): do both Comments and Tests.
2. **Tests.** Beyond untangle's test pass, delete tests that mock your own code and only assert calls, keep tests that run against real dependencies, and cover critical journeys with end-to-end tests instead.
3. **Static counts to zero.** Bring dead code, complexity and duplicated blocks to zero, and satisfy every static checker the repo has or should have (for example Fallow, knip or React Doctor). They are static checks, so use judgment, but aim for zero.
4. **Deep cleanup.** Search deeply, with parallel agents, for deep code, overcomplicated code, migrations that should no longer exist, leaky and poorly designed abstractions, CRAP (high complexity with low coverage), and anything duplicated or confusing. Make it DRY and SOLID, and above all KISS.
5. **Oversized files.** Treat any file over 300 lines as a problem. Split every one that isn't cohesive, including the existing ones; don't park them at a baseline. Question anything absurdly big for what it does, such as a scripts folder with tens of thousands of lines.
6. **Docs.** Code is the source of truth. Trim every Markdown file to what is worth having, delete what repeats the code, move plans into `docs/plans` marked as not authoritative, and delete stale trackers. Verify every doc you keep against the code.
7. **State and bugs.** Find the implicit state machines, model the risky ones formally (TLA+ or a similar model checker), and fix every real bug the model finds, each with its own PR. Record how each bug was found: from where and from doing what.
8. **Issues.** Take care of open issues in the tracker that fall in scope.
9. **Slow checks.** A CI gate that costs more time than the signal it gives is a weed: cut it or speed it up.
10. **Keep weeds out.** Add CI ratchets so every count above can only go down, and a lint or check for anything people keep fixing by hand.
11. **Everything else.** Whatever else the audit turns up that you can improve, remove or rethink, do it, and apply any other principle or technique that makes the garden healthier.

Done when the untangle step 5 checks find nothing worth fixing and every static count is at zero or has a written reason.

## 4. PRs and proof

- Open small, focused PRs per area, rebased and green.
- Have an independent reviewer (a different model, or a fresh agent without your context) review every PR in rounds until it approves, including PRs opened earlier in the operation.
- Write every PR description so a reviewer understands what changed, why, and how it was verified, and give the user a clickable link to every PR.
- Prove every user-facing change end to end by running the app (a live preview, browser automation or computer use), and record who ran it, how, and whether it ran smoothly.

## 5. Endgame

Land ready work before the deadline, so nobody sorts through it later and burns usage again.

1. **75 minutes before:** merge or queue every PR that is green and done, rebase the rest, and close PRs another PR replaced. New lanes and PRs are still fine; they don't have to merge today.
2. **Throughout:** every lane branch with real commits gets a draft PR, so no work is lost or hidden.
3. **30 minutes before:** last merge into the default branch, so post-merge CI and deploys finish while agents are still running.
4. **20 minutes before:** everything is pushed, every open PR has a "Status at handoff" note in its body, lanes are stopped and worktrees are cleaned.

Done when no lane is running and every branch is either merged or has a PR with a handoff note.

## 6. Report

- Assume the user knows nothing, and give a full, coherent status.
- Visualize the measurable change across all PRs, drafts included, as an HTML page or chart: lines deleted, cyclomatic complexity, dead code, duplicated blocks, files over 300 lines, bugs fixed, test count and runtime, and refactors. Show before and after for the file tree and a few key functions.
- Say honestly whether the result is a real step up and worth it, beyond smaller numbers.
- End with only the decisions that need the user, numbered.
