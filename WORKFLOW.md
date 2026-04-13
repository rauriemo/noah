---
tracker:
  kind: github
  repo: "rauriemo/noah"
  labels:
    active: ["todo", "in-progress"]
    terminal: ["done", "canceled"]

polling:
  interval_ms: 10000

workspace:
  root: "./workspaces"

hooks: {}

channels:
  - kind: dispatch
    target: "localhost:8086"
    events: [task.completed, task.failed]
  - kind: prism
    target: "localhost:3107"
    events: [task.completed, task.failed]
agent:
  command: "claude"
  max_turns: 10
  max_concurrent: 1
  stall_timeout_ms: 300000
  max_retry_backoff_ms: 300000
  permission_mode: "dontAsk"

system:
  workflow_changes_require_approval: true

server:
  port: 0
---

You are an expert software engineer working on {{.issue.title}}.

## Task
{{.issue.body}}

## Rules
- Make small, focused commits
- Run tests before marking a task as done
- Guest agent definitions live in the agents/ directory (if present)
