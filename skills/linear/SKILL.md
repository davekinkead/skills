---
name: Linear
description: Fetch and work with Linear issues when user mentions issue IDs (e.g., FAT-123, ABC-456)
---

# Linear Skill

Automatically fetch Linear issue information when the user mentions issue IDs in their requests.

## When to Use This Skill

- User mentions a Linear issue ID (format: `TEAM-###` like `FAT-123`)
- User asks to implement, fix, update, or work on a Linear issue
- User wants to create a new issue
- User wants to update an issue's status or details

## Configuration Discovery

**Required**: `LINEAR_API_KEY` from `~/.secrets/claude.env`

**Optional**: `TEAM_ID` and `PROJECT_ID` in `./linear.env` (only needed for creating new issues)

Config format:
- `~/.secrets/claude.env`: `LINEAR_API_KEY: lin_api_xxx...`
- `./linear.env`: `TEAM_ID: <uuid>` and `PROJECT_ID: <uuid>`

## GraphQL Endpoint

- URL: `https://api.linear.app/graphql`
- Auth: `Authorization: <API_KEY>` header
- Method: POST with JSON body
- Use curl: `curl -X POST -H "Content-Type: application/json" -H "Authorization: <API_KEY>" --data '{"query": "<graphql>"}' https://api.linear.app/graphql`

## Common Operations

### Fetch Issue Details

When user mentions an issue ID (e.g., "Implement FAT-123"):

```graphql
query Issue {
  issue(id: "FAT-123") {
    id
    title
    description
    state {
      name
    }
    assignee {
      name
    }
    priority
    labels {
      nodes {
        name
      }
    }
  }
}
```

**Display to user**: Log the issue title, description, status, and assignee before proceeding with the implementation.

### Update Issue Status

```graphql
mutation UpdateIssue {
  issueUpdate(
    id: "<issue-id>",
    input: {
      stateId: "<state-id>"
    }
  ) {
    success
    issue {
      id
      title
      state {
        name
      }
    }
  }
}
```

To get available states:
```graphql
query WorkflowStates {
  workflowStates {
    nodes {
      id
      name
      type
    }
  }
}
```

### Add Comment to Issue

```graphql
mutation CreateComment {
  commentCreate(
    input: {
      issueId: "<issue-id>",
      body: "Comment text here"
    }
  ) {
    success
    comment {
      id
    }
  }
}
```

### Create New Issue

**Requires**: TEAM_ID from config (run `/linear-setup` if not configured)

```graphql
mutation CreateIssue {
  issueCreate(
    input: {
      teamId: "<team-id>",
      title: "Issue title",
      description: "Issue description",
      priority: 2
    }
  ) {
    success
    issue {
      id
      title
      identifier
    }
  }
}
```

Priority values: 0=No priority, 1=Urgent, 2=High, 3=Medium, 4=Low

If TEAM_ID not found, suggest running `/linear-setup` to configure.

### Query Team's Issues

```graphql
query TeamIssues {
  team(id: "<team-id>") {
    issues {
      nodes {
        id
        identifier
        title
        state {
          name
        }
      }
    }
  }
}
```

## Workflow

### For existing issues (read/edit):
1. **Detect issue ID** in user's request
2. **Load `LINEAR_API_KEY`** from `~/.secrets/claude.env`
3. **Fetch issue details** using GraphQL
4. **Display to user**: Show issue title, description, and current status
5. **Proceed with task**: Implement, fix, or update as requested
6. **Optional**: Ask user if they want to update issue status or add comment when done

### For creating new issues:
1. **Load `LINEAR_API_KEY`** from `~/.secrets/claude.env`
2. **Load `TEAM_ID`** from `./linear.env`
3. If `TEAM_ID` not found, suggest running `/linear-setup` first
4. **Create issue** with provided details
5. **Return issue identifier** (e.g., FAT-123)

## Error Handling

- If `LINEAR_API_KEY` not found in `~/.secrets/claude.env`, inform user to add it
- If creating issue and `TEAM_ID` not found in `./linear.env`, suggest running `/linear-setup`
- If issue ID not found, verify the team prefix and ID
- If GraphQL errors, show the error message from response
