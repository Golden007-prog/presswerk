---
name: user-story-creator
description: Create Presswerk feature issues as user stories when asked to create an issue, ticket, or story, including "Erstelle ein Issue" or "Erstelle ein Ticket". Draft first, publish after approval. Not for bugs or implementation.
---

# User Story Creator

- Repository: `pklink/presswerk`.
- Conversation: user's language. Title and body: English.
- Scope: single-user app; no invented features or implementation details.

## Workflow

1. Extract scope and user benefit. Clarify material gaps; distinguish ambiguous bugs from features.
2. For bugs, request reproduction steps, expected and actual behavior. Stop this workflow.
3. Draft title without commit prefixes, body using the template, and suitable labels.
4. Keep ACs observable, testable, request-specific; include relevant regression coverage.
5. Show the draft and labels for approval. After corrections, show the revised draft. Publish only after explicit approval of the current draft; never in plan mode.
6. Verify labels: `gh label list --repo pklink/presswerk --limit 100`. Never create labels. Obtain approval for replacements if labels are missing.
7. Publish: `gh issue create --repo pklink/presswerk --title ... --body ... --label ...`. Safely quote literal approved text; include all approved labels.
8. Return the issue URL only on confirmed success. On failure, report the error and check for an existing issue before retrying.

No code changes, assignees, milestones, or additional skills.

## Body template

```markdown
**As a** <user>,
**I want** <capability>,
**so that** <benefit>.

### Acceptance criteria
- [ ] <observable outcome>
- [ ] <observable outcome>
```

## Relevant labels

Repository snapshot; verify proposed labels before publishing.

| Label | Use |
| --- | --- |
| `accessibility` | Accessibility barriers. |
| `documentation` | Documentation requests. |
| `enhancement` | Feature requests; normal story label. |
| `good first issue` | Demonstrably small, newcomer-friendly scope; not merely a short request. |
| `help wanted` | Only when the user requests contributor help. |
