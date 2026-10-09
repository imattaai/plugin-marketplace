---
name: taskn
description: Manage the user's TASKn tasks and projects. Use when the user asks about their tasks, what's on their plate today, or wants to create, update, complete, search, or organize tasks and projects.
---

# TASKn

Talk to the user's TASKn account: their tasks, projects, and daily stats, right from the conversation. The TASKn MCP server exposes 10 tools; authentication is OAuth via the user's TASKn sign-in (Google, Apple, or email) and is handled by the MCP client — never ask for passwords or API keys.

## When to use

- The user asks what's on their plate, what to focus on today, or for a status overview.
- The user wants to create, update, complete, delete, or search tasks.
- The user wants to browse projects or see what's shared with them.
- The user asks for daily productivity stats.

## Inputs

- Natural language is enough: task titles, project names, due dates ("tomorrow", "next Tuesday"), priorities.
- Prefer IDs returned by previous tool calls over re-searching. `get_task` accepts a task or project id and returns full details plus subtasks.

## Sequence

1. **Orient first.** For "what's on my plate" questions, call `get_task_feed` (AI-ranked, same order as the app home screen) or `list_projects` (priority order — the FIRST project is highest priority).
2. **Drill down** with `get_task` when you need subtasks, statuses, or share info before acting.
3. **Search** with `search_tasks` when the user names a specific task.
4. **Act** with `create_task` / `update_task` / `complete_task`. Nest under a parent via `parent_id` when the user names a project.
5. **Summarize** what changed: task title, project, due date. Keep it to one or two lines.

## Validation

- If a tool call fails with an auth error, stop and tell the user to connect / re-authenticate the TASKn connector in their client settings, then retry.
- Dates are ISO 8601. When the user gives a relative date ("tomorrow"), resolve it against today's date before calling.
- `update_task` only changes provided fields; use `clear_due_date` to remove a due date.

## Output

- Lead with the result, not the tool calls. "Done — 'Call dentist' added to Personal, due tomorrow."
- For feed/status questions, give the top items with one line each; offer to drill into a project rather than dumping everything.

## Boundaries

- **Confirm before `delete_task`.** Deletion is irreversible — state exactly what will be deleted and wait for explicit confirmation.
- Complete tasks only on explicit user request ("done", "complete", "finished") — never infer completion.
- The connector only ever sees the signed-in user's own TASKn account. Never claim access to anyone else's tasks.
- Do not train on or repeat task contents beyond the current conversation.
