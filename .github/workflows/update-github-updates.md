---
name: Update GitHub Updates
description: Automatically fetch and synthesize the latest GitHub updates to keep Mona's website current
trigger:
  - schedule: "0 0 * * *"  # Daily at midnight UTC
  - workflow_dispatch       # Manual trigger via GitHub UI

agent:
  model: copilot-default
  safe_outputs: true        # Enable sandbox environment for secure file changes
  
tools:
  - type: fetch
    permissions:
      - allowed_domains:
          - github.com
          - github.blog
      - methods: [GET]
      - timeout: 30000      # 30 seconds
      
  - type: edit
    permissions:
      - paths:
          - latest-github-updates.md
      - prevent_direct_commit: true  # Force PR creation instead of direct commits
      - auto_create_pr: true         # Automatically create PR with changes

steps:
  - id: fetch-github-blog
    name: Fetch GitHub Blog Updates
    description: Retrieve latest posts from the GitHub Blog RSS feed
    fetch:
      url: https://github.blog/feed/
      method: GET
      headers:
        User-Agent: "GitHub-Agentic-Workflow/1.0"
    output: blog_feed
    
  - id: fetch-github-changelog
    name: Fetch GitHub Changelog
    description: Retrieve latest updates from the GitHub Changelog
    fetch:
      url: https://github.com/changelog
      method: GET
      headers:
        User-Agent: "GitHub-Agentic-Workflow/1.0"
    output: changelog_content
    
  - id: synthesize-updates
    name: Synthesize GitHub Updates
    description: Process and combine information from GitHub Blog and Changelog
    process:
      inputs:
        - blog_feed
        - changelog_content
      instructions: |
        1. Extract the most recent and relevant updates from both sources
        2. Identify key features, security improvements, and important announcements
        3. Summarize each update in 2-3 sentences with clear, accessible language
        4. Organize updates by category (Features, Security, Improvements, Breaking Changes)
        5. Include publication dates and links to original sources
        6. Format as structured markdown with clear hierarchy
      output: synthesized_updates
    
  - id: update-content-file
    name: Update latest-github-updates.md
    description: Write synthesized updates to the markdown file
    edit:
      file: latest-github-updates.md
      operation: update
      content_source: synthesized_updates
      format: markdown
      frontmatter:
        last_updated: "{current_datetime}"
        source: "GitHub Blog & Changelog"
        auto_generated: true
    output: file_update_summary
    
  - id: create-pull-request
    name: Create Pull Request with Changes
    description: Automatically open a PR to propose the updated content
    pr:
      title: "docs: Update latest GitHub updates and features"
      body: |
        # Automated Update: Latest GitHub Updates
        
        This pull request automatically updates the website with the latest GitHub Blog posts and Changelog entries.
        
        ## Changes
        - Updated `latest-github-updates.md` with the latest GitHub features, improvements, and security updates
        - Fetched from official GitHub Blog and Changelog sources
        - Generated at: {current_datetime}
        
        ## Review Notes
        - All content is sourced from official GitHub channels
        - Network access was limited to github.com and github.blog domains only
        - Please review for accuracy and relevance before merging
      labels:
        - "automated"
        - "documentation"
        - "github-updates"
      branch: "chore/update-github-updates-{timestamp}"
      auto_request_review: false
    
  - id: notify-completion
    name: Log Workflow Completion
    description: Record workflow execution summary
    log:
      level: info
      message: |
        Workflow completed successfully
        - Blog feed entries processed: {blog_entry_count}
        - Changelog updates processed: {changelog_update_count}
        - File: latest-github-updates.md updated
        - Pull Request: {pr_number} created
        - Timestamp: {current_datetime}

on_failure:
  notify: |
    Workflow failed while updating GitHub updates.
    - Check network connectivity to github.com and github.blog
    - Verify the latest-github-updates.md file exists and is accessible
    - Review fetch timeouts or content parsing errors
  
  create_issue: true
  issue_title: "⚠️ Workflow Failed: Update GitHub Updates"
  issue_labels:
    - "bug"
    - "automated"

constraints:
  max_retries: 3
  retry_delay: 300  # 5 minutes between retries
  timeout: 600     # 10 minutes total
  concurrent_limit: 1
  require_approval_for_merge: true  # PR requires review before merge
