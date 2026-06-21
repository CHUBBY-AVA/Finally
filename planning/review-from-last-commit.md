# Review From Last Commit

## Findings

1. **Medium - Reviewer agent is fragile because it delegates the whole review to an external Codex process**
   - File: `.claude/agents/reviewer.md:7`
   - The reviewer subagent is a Claude agent, but its instructions say not to review anything itself and instead run `codex exec ...`. That makes the agent depend on a separate CLI being installed, authenticated, on PATH, and allowed to run from the subagent environment. If any of those assumptions fail, the advertised "reviewer" agent produces no review. It also hides the actual review work in a nested tool invocation, making failures and context harder to inspect from the Claude session.
   - Recommendation: either have the reviewer agent perform the review directly with shell/read tools, or document this as a local convenience wrapper around Codex rather than the implementation of the review agent.

2. **Medium - `/doc-review` appends transient review comments into the source document**
   - File: `.claude/commands/doc-review.md:1`
   - The command tells the assistant to add questions, clarifications, feedback, and simplification opportunities to a new section at the end of the planning file named by `$ARGUMENTS`. That mixes review notes into canonical planning documents, where later agents can accidentally treat unresolved questions as part of the accepted spec.
   - Recommendation: write review output to a separate file such as `planning/REVIEW.md` or `planning/<doc>-review.md`, unless the user explicitly asks for inline comments in the source document.

## Notes

- `planning/PLAN.md:178` and `planning/PLAN.md:179` now describe a change-gated, batched SSE payload. That matches the current backend implementation in `backend/app/market/stream.py` and the serialized `PriceUpdate` fields in `backend/app/market/models.py`.
- `planning/REVIEW.md` appears to be generated review output from an earlier run. I did not flag it as an application issue, but it may be worth deciding whether generated review reports should be committed.
- `.claude/settings.local.json` is ignored by global git config, so I did not treat its local hook/permission settings as part of the reviewable changes.
- I did not run tests; the reviewed changes are documentation and Claude automation files.
