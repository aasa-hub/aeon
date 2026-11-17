---
name: makefun-tiny-chess
description: Build, validate, transcript-bundle, push, and submit a fee-free Make.fun tiny-chess-01 entry using Hermes.
metadata:
  title: Make.fun Tiny Chess
  category: dev
  var: ""
  mode: write
  commits: false
  permissions:
    - contents:write
  requires:
    - GH_GLOBAL?
    - MAKEFUN_API_KEY
    - MAKEFUN_KEYPAIR_JSON
    - OPENROUTER_API_KEY
  tags:
    - makefun
    - bounty
    - hermes
    - chess
---

Build the first authorized fee-free Make.fun bounty entry for `tiny-chess-01`.

The target public repository is `aasa-hub/tiny-chess-hermes-01`. The Make.fun kit is available at `https://make.fun/c/tiny-chess-01/kit`; use the kit's own `cli/makefun.mjs` and `RULES.txt`. The repository was created after the competition opened and is intentionally empty. Do not use the Aeon repository as the entry repository.

Before writing entry code, read the public requirements at `https://make.fun/api/competitions/tiny-chess-01/requirements`, fetch the kit, and inspect its RULES.txt and starter. Use the protected `MAKEFUN_KEYPAIR_JSON` as a mode-600 file at `$HOME/.makefun/makefun-keypair.json`, and use `MAKEFUN_API_KEY` only through the Make.fun CLI environment. Never print either value, put them in source, or include them in a transcript. Do not push provider keys or any secrets to the competition.

Clone or initialize the empty target repository. Run Make.fun `init --license MIT --keypair "$HOME/.makefun/makefun-keypair.json"` from the target repository so the required freshness/anchor and OSI-approved LICENSE are created. Preserve the anchor history.

Build an original deterministic Python 3.13 chess engine at repository-root `main.py`, at most 4,096 bytes, using only the Python standard library. It must speak the kit protocol through `MAKEFUN_ENV_URL`, never call any model or external service during grading, never use wall-clock time or unseeded randomness, and stay within the published resource limits. Use the kit starter only as a protocol reference; do not copy another entrant's implementation. Write and revise `main.py` in at least two separate assistant edits, run the kit's local checks, and keep the entire repository within its published size cap.

Create `README.md` using the exact Make.fun requirements template with the owner `aasa-hub`, repository `tiny-chess-hermes-01`, a concise description, the correct local run command, referral code from `node cli/makefun.mjs referrals` if required, and all three live badges for `tiny-chess-01`. Any README text should be written by this Hermes session. Commit the finished files on top of the Make.fun anchor and push `main` to `https://github.com/aasa-hub/tiny-chess-hermes-01`.

From the kit, run the pre-submit checks against the committed repository and the local grade if the kit supports it. For this competition, use no provider key for the entry runtime. Then create a transcript bundle from the actual Hermes session store, not an export or reconstructed log:

```bash
node cli/makefun.mjs bundle --repo "$BOT" --hermes-dir "$HOME/.hermes" --harness hermes --provider openrouter --model default --effort standard --prompt-file /tmp/makefun-user-prompt.txt --prompt-origin typed --mcp none --instructions withheld:private-system-and-repository-instructions --out /tmp/tiny-chess-transcripts.tar.gz
```

The prompt file must contain the user's exact authorization message for this run, not a summary. Do not delete, rewrite, retry, undo, or compress Hermes messages while working. If bundle reports a missing requirement, fix the build or bundle invocation before submitting; never fabricate a transcript.

If and only if the local checks pass, the transcript bundle is non-empty and the target commit is pushed, submit the fee-free entry with:

```bash
node cli/makefun.mjs submit --competition tiny-chess-01 --bundle /tmp/tiny-chess-transcripts.tar.gz --repo-url https://github.com/aasa-hub/tiny-chess-hermes-01 --commit "$(git -C "$BOT" rev-parse HEAD)" --title 'Hermes Tiny Chess Engine' --keypair "$HOME/.makefun/makefun-keypair.json" --repo "$BOT"
```

Poll the returned submission status until the initial checks complete. If it passes, report the submission ID, repository URL, commit SHA, practice status/score if available, and the exact next status-poll route. If it fails, do not submit another attempt automatically: report every failed check and its published fix so the next Hermes run can address it. Do not claim or spend any reward during this build run; reward claiming is a separate post-resolution action.
