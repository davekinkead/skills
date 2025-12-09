---
description: Initialize Linear configuration for creating new issues
---

# Linear Setup

Verify your Linear API key is configured and display available teams in your workspace.

## Prerequisites

The `LINEAR_API_KEY` environment variable must be set. Get your API key from: https://linear.app/settings/api

Set it in your environment or add to `~/.secrets/claude.env`:
```
LINEAR_API_KEY=lin_api_xxx...
```

## Steps

1. **Read LINEAR_API_KEY** from `~/.secrets/claude.env`

2. **Verify the API key** is set by running:
   ```bash
   LINEAR_API_KEY=<key> linear teams
   ```

3. **Display available teams** to the user with their team keys

4. Inform the user they can now:
   - Fetch issues: `LINEAR_API_KEY=<key> linear issue TEAM-123`
   - Search issues: `LINEAR_API_KEY=<key> linear search "query" --team=TEAM`
   - View their issues: `LINEAR_API_KEY=<key> linear mine`
   - Update issues: `LINEAR_API_KEY=<key> linear update TEAM-123 "Done"`
   - Comment on issues: `LINEAR_API_KEY=<key> linear comment TEAM-123 "comment"`

## Output

Show the list of teams and confirm the Linear integration is ready to use.
