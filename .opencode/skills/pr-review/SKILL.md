---
name: pr-review
description: Use when reviewing a Presswerk pull request or asked to post review findings as a PR comment. Reviews the PR diff against the surrounding code, then posts each confirmed finding as a short GitHub comment with an introductory problem sentence, numbered repro steps, and impact. Do not use for whole-repo audits or local uncommitted changes.
---

# Review a Presswerk PR

Reviews a pull request and posts findings as GitHub PR comments via `gh`.

## Review

1. Read `gh pr view <N> --json title,body,files` and `gh pr diff <N>`.
2. Read the code the diff touches: callers of changed functions, the functions it calls, the tests. Fetch PR-head versions with `gh api repos/pklink/presswerk/contents/<path>?ref=<headOid> -H 'Accept: application/vnd.github.raw+json'`; never check out the branch.
3. Judge against the app's context: single user, but multiple open tabs are realistic. Look for bugs, data loss, and races between tabs - not style.
4. Every finding needs a concrete case: this input or situation leads to this wrong result. No case, no comment.

## Comment format

One comment per finding. Post with `gh pr comment <N> --body ...`; update an existing comment with `gh api repos/pklink/presswerk/issues/comments/<id> -X PATCH -f body=...`.

Structure, in English:

- **Introductory sentence** describing the problem. No heading, no "Must fix" label, no verdict.
- File and function reference in backticks, e.g. `` `quick_update_status/2` in `lib/presswerk/preorders.ex` ``.
- **Repro:** numbered steps, each with its observable result. End with the wrong outcome, marked ❌.
- Why any safeguard that looks like it should catch this does not (one line).
- **Impact:** one sentence, what the user loses or sees.

Do not propose a fix. Do not add summary sections, checklists, or sign-offs.

Example shape (from PR #50):

> When the same preorder is open in two tabs, an outdated tab can silently undo a newer status change: `quick_update_status/2` in `lib/presswerk/preorders.ex` checks allowed transitions against the tab's stale copy, not the DB.
>
> **Repro**
>
> 1. Open the same preordered record in two tabs
> 2. Tab A: *Mark as received* → DB is `received`
> 3. Tab B: *Mark as shipped* → DB is back to `shipped` ❌
>
> `StaleEntryError` doesn't fire - the record still exists.
>
> **Impact:** an outdated tab can reopen a received record or overwrite a cancellation.
