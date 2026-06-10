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

safe-outputs:
  create-issue:
    max: 5

---

# latest-github-updates

## Instructions

1. Use the network fetch tool to read the latest context, features, and technical updates from the following external URLs:
   - https://github.blog (GitHub Official Blog)
   - https://github.com/changelog (GitHub Changelog)
2. Synthesize and organize the gathered information logically by categories: Features, Security Updates, Improvements, and Breaking Changes.
3. Update the local Markdown content file named `latest-github-updates.md` with this processed information.
4. Use the secure `safe-outputs` tool configuration to propose these changes by automatically creating a structured Pull Request for review.

## Notes

- Run `gh aw compile` to generate the GitHub Actions workflow
- See https://github.github.com/gh-aw/ for complete configuration options and tools documentation
