---
description: Initialize Linear configuration for creating new issues
---

# Linear Setup

Discover available teams and projects in your Linear workspace and create a `linear.env` file for this project.

**Note**: This is only required if you want to create new Linear issues. Reading and editing existing issues only requires the API key in `~/.secrets/claude.env`.

## Steps

1. **Read the global Linear API key** from `~/.secrets/claude.env` (key: `LINEAR_API_KEY`)

2. **Query available teams** using:
   ```graphql
   query Teams {
     teams {
       nodes {
         id
         name
         key
       }
     }
   }
   ```

3. **Query available projects** using:
   ```graphql
   query Projects {
     projects {
       nodes {
         id
         name
         teams {
           nodes {
             id
             name
           }
         }
       }
     }
   }
   ```

4. **Present the teams and projects to the user** and ask which ones they want to use for this project

5. **Create a `linear.env` file** in the current directory with only the team and project IDs:

  ```
  TEAM_ID: <selected team id>
  PROJECT_ID: <selected project id>
  ```

## GraphQL Endpoint

- URL: `https://api.linear.app/graphql`
- Auth header: `Authorization: <API_KEY>`
- Use curl with `-X POST -H "Content-Type: application/json" -H "Authorization: <API_KEY>"`
- Query format: `--data '{ "query": "<graphql query>" }'`

## Output

Confirm the `linear.env` file has been created and show the selected team and project names.
