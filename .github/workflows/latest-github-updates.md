---
on:
  workflow_dispatch:

permissions:
  contents: read
  issues: read
  pull-requests: read

network:
  allowed:
    - github.com
    - github.blog
    - raw.githubusercontent.com

safe-outputs:
  create-pull-request:
    title-prefix: "[latest-github-updates] "
    draft: true
    max: 1
    fallback-as-issue: false
  missing-data:
    create-issue: false
    max: 5
  report-failure-as-issue: false
---

# latest-github-updates

## Instructions

1. Read the latest public GitHub updates from:
   - https://github.blog
   - https://github.com/changelog

2. Synthesize the information into the following categories:
   - Features
   - Security Updates
   - Improvements
   - Breaking Changes

3. Update the local Markdown content file named `latest-github-updates.md`.

4. Propose the changes through a structured Pull Request using the `create-pull-request` safe-output.

## Notes

- Do not create issues for missing data or failure reports.
- If required context is missing, report it in the workflow output only.