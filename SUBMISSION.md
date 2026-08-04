# Jurnie plugin submission worksheet

Use this worksheet when creating the **With MCP** draft in the [OpenAI plugin submission portal](https://platform.openai.com/plugins).

## Listing

- **Name:** Jurnie
- **Category:** Productivity
- **Short description:** Turn your goals into clear next actions
- **Long description:** Connect ChatGPT and Codex to Jurnie to review personal and shared Journeys, plan the day, manage and assign tasks, track habits, set intentions, work with calendar events, and schedule follow-through.
- **Website:** https://www.jurnie.co/
- **Support:** https://www.jurnie.co/help
- **Privacy:** https://www.jurnie.co/privacy
- **Terms:** https://www.jurnie.co/terms
- **MCP URL type:** Universal
- **MCP server URL:** https://www.jurnie.co/api/mcp
- **Authentication:** OAuth

## Portal-only prerequisites

- Select the verified Jurnie developer or business identity.
- Confirm the submitter has **Apps Management: Write** access.
- Provide a fully featured demo account that does not require MFA, SMS, or email confirmation during review.
- Complete the portal's domain challenge at `https://www.jurnie.co/.well-known/openai-apps-challenge` using the exact token generated for this submission.
- Run **Scan Tools** after the assignment-capable MCP server is deployed.

## Tool annotation rationale

- Read tools use `readOnlyHint: true` because they only retrieve private Jurnie account data.
- Create and update tools use `readOnlyHint: false`, `destructiveHint: false`, and `openWorldHint: false` because they change private Jurnie state and remain reversible.
- Deletion tools use `readOnlyHint: false`, `destructiveHint: true`, and `openWorldHint: false` because they delete private Jurnie state.
- All tools use `openWorldHint: false`; none publish content or message third parties.

## Positive test cases

### 1. Plan from current priorities

- **Prompt:** Review my Jurnie priorities and plan today.
- **Expected behavior:** Read Journeys, relevant incomplete and overdue tasks, calendar events, and the daily intention; do not mutate data.
- **Expected result:** A concise ordered plan grounded in the demo account's current Jurnie records.

### 2. Find overdue work

- **Prompt:** Show my overdue Jurnie tasks and suggest the next three actions.
- **Expected behavior:** Call the overdue-task read tool and use returned task identifiers and due dates.
- **Expected result:** Up to three prioritized actions without marking tasks complete.

### 3. Create a personal task

- **Prompt:** Create a personal Jurnie task called “Review launch metrics” due tomorrow.
- **Expected behavior:** Create one journeyless task after resolving tomorrow in the user's Jurnie timezone.
- **Expected result:** One new task with the requested title and date.

### 4. Assign a shared-Journey task

- **Prompt:** In the Launch Journey, assign “Prepare launch copy” to Gitar.
- **Expected behavior:** Resolve the Journey, list accepted Journey members, resolve Gitar to one returned `userId`, then create or update exactly one task with that `assigneeId`.
- **Expected result:** The task is assigned to Gitar and remains visible to accepted Journey members.

### 5. Unassign a task

- **Prompt:** Unassign “Prepare launch copy” in the Launch Journey.
- **Expected behavior:** Resolve exactly one task and update it with `assigneeId: null`.
- **Expected result:** The task remains in the Journey with no assignee.

## Negative test cases

### 1. Pending invitee

- **Prompt:** Assign the launch task to pending@example.com, who has not accepted the Journey invite.
- **Expected behavior:** Do not assign. Explain that pending invitees become assignable only after accepting.
- **Why:** Pending invitations do not have an accepted `journey_members` identity.

### 2. Ambiguous member name

- **Prompt:** Assign the task to Alex.
- **Expected behavior:** If multiple accepted members match Alex, ask which person the user means before writing.
- **Why:** Assignment must resolve to one accepted member `userId`.

### 3. Outsider assignment

- **Prompt:** Assign the task to a user who is not a member of this Journey.
- **Expected behavior:** Refuse the assignment or surface the server's safe membership error without changing the task.
- **Why:** Jurnie only permits accepted Journey members as assignees.

## Initial release notes

Jurnie connects ChatGPT and Codex to an OAuth-protected Jurnie account for planning, task and habit management, calendar work, daily intentions, and scheduled follow-through. This release adds accepted-member discovery plus task assignment and unassignment for shared Journeys, with server-side membership validation and private-system tool annotations.
