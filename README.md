# simple-claude-container-example

A simple Node.js Hello World app with Docker Compose, PostgreSQL, and Claude Code.

## Prerequisites

- Docker and Docker Compose
- Anthropic API key

## Quick Start

```bash
# Set your API key
export ANTHROPIC_API_KEY=your-key-here

# Start all services
docker compose up -d

# Open interactive Claude Code session
docker compose exec claude-code claude
```

## Running Multiple Instances with Git Worktrees

Git worktrees let you work on multiple branches simultaneously, each with its own Claude Code instance.

### Setup Worktrees

```bash
# Create worktrees for different features/branches
git worktree add ../feature-auth feature/auth
git worktree add ../feature-api feature/api
git worktree add ../bugfix-123 bugfix/123
```

### Run Each Worktree with a Different Port

Each worktree gets its own isolated Docker environment. Use `APP_PORT` to avoid conflicts:

```bash
# Main worktree (default port 3000)
cd /path/to/main
docker compose up -d
docker compose exec claude-code claude

# Feature auth worktree (port 3001)
cd /path/to/feature-auth
APP_PORT=3001 docker compose up -d
docker compose exec claude-code claude

# Feature api worktree (port 3002)
cd /path/to/feature-api
APP_PORT=3002 docker compose up -d
docker compose exec claude-code claude
```

### Worktree Tips

- Each worktree has isolated containers (named by directory)
- Database volumes are separate per worktree
- Claude Code in each container only sees that worktree's files
- Use different terminal windows/tabs for each worktree

### Cleanup

```bash
# Stop containers for a specific worktree
cd /path/to/worktree
docker compose down

# Remove a worktree when done
git worktree remove ../feature-auth
```

## Services

| Service | Description | Port |
|---------|-------------|------|
| app | Node.js Hello World server | `${APP_PORT:-3000}` |
| db | PostgreSQL 16 | Internal only |
| claude-code | Claude Code CLI | None |

## Commands

```bash
# Start services in background
docker compose up -d

# View logs
docker compose logs -f

# Open Claude Code interactive session
docker compose exec claude-code claude

# Stop all services
docker compose down

# Stop and remove volumes
docker compose down -v
```

## Testing the App

```bash
curl http://localhost:3000
# Returns: Hello World
```
