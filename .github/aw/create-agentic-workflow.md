---
description: Schema and validation blueprint for creating and updating the agentic workflow.
runtimes:
  node: "22"
network:
  allowed:
    - github.com
    - github.blog
    - raw.githubusercontent.com
tools:
  github:
    toolsets: [default]
safe-outputs:
  report-failure-as-issue: false
---

# Create Agentic Workflow Blueprint

This file serves as the strict validation context for the latest-github-updates workflow execution.

## Agent Instructions

1. Verify secure repository access and parameters permissions.
2. Synchronize and pull the repository update logs without generating loops.
3. Use the network tools exclusively to look up authorized domains.
4. Ensure the output is safely delivered via a structured pull request context.