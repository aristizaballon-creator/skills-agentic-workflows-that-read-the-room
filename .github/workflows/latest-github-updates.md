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
    max: 5
  report-failure-as-issue: false
---

# latest-github-updates

## Task Overview

Your job is to fetch the latest GitHub updates and create a pull request with synthesized information.

## Instructions

### Step 1: Fetch Latest GitHub Updates
Read the latest public GitHub updates from these sources:
- https://github.blog (primary source for announcements)
- https://github.com/changelog (detailed changelog)

Gather information about recent changes, features, and improvements.

### Step 2: Categorize the Information
Organize the collected updates into these categories:
- **Features**: New capabilities and major feature releases
- **Security Updates**: Security fixes, vulnerability patches, and security improvements
- **Improvements**: Performance enhancements, UI/UX improvements, and optimizations
- **Breaking Changes**: Deprecations, API changes, or behavioral changes that may require action

### Step 3: Update the Target File
Read the existing file `latest-github-updates.md` in the repository root.

Update each section (Features, Security Updates, Improvements, Breaking Changes) with the latest information you've gathered.

Maintain the markdown structure and keep the content concise but informative.

### Step 4: Create Pull Request (REQUIRED)
**You MUST create a pull request with your changes.**

Use the `create-pull-request` safe-output tool to:
1. Create a branch with your updates
2. Commit the changes to `latest-github-updates.md`
3. Open a draft pull request with a clear title and description

The PR title will be automatically prefixed with `[latest-github-updates]`.

Provide a detailed body describing:
- What updates were added
- Which categories were updated
- Date of the update fetch

## Success Criteria

✓ Successfully fetched latest GitHub updates
✓ Organized information into the four categories
✓ Updated `latest-github-updates.md` with new content
✓ **Created and opened a pull request** (REQUIRED)

## Notes

- Do not create issues for missing data—report in workflow output only
- Always attempt to create the pull request, even if some data is incomplete
- If you encounter access restrictions, report them as missing_tool or missing_data