---
name: Linear
description: Fetch and work with Linear issues when user mentions issue IDs (e.g., FAT-123, ABC-456)
---

# Linear Skill

## When to Use

Automatically activate when user mentions a Linear issue ID (format: `TEAM-###` like `FAT-123`) or asks to search/list Linear issues.

## Usage

1. **Read API key** from `~/.secrets/claude.env` (format: `LINEAR_API_KEY: lin_api_xxx`)
2. **Run commands** using: `LINEAR_API_KEY=<key> linear <command>`
3. **Display results** to user before proceeding with task

## Key Commands

```bash
LINEAR_API_KEY=<key> linear issue FAT-123          # Fetch issue details
LINEAR_API_KEY=<key> linear search "query"          # Search issues
LINEAR_API_KEY=<key> linear mine                    # Show my issues
LINEAR_API_KEY=<key> linear update FAT-123 "Done"   # Update status
LINEAR_API_KEY=<key> linear comment FAT-123 "text"  # Add comment
```

Run `linear` for full usage. Repository: https://github.com/davekinkead/linear-rb

## Error Handling

If `LINEAR_API_KEY` not found in `~/.secrets/claude.env`, direct user to: https://linear.app/settings/api
