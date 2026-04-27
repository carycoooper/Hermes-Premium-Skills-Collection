---
name: github-integration
description: Complete GitHub workflow integration for issue triage, PR review assistance, release notes generation, codebase onboarding, dependency monitoring, and repository activity tracking via GitHub CLI or API.
version: 2.0.0
author: Hermes Skills Team
license: MIT
platforms: [macos, linux, windows]
metadata:
  hermes:
    tags: [github, development, version-control, issues, pull-requests, devops]
    category: development
    related_skills: [frontend-design, capability-evolver, summarize]
    config:
      - key: default_repo_scope
        description: "Default repositories to monitor (owner/repo format)"
        default: "[]"
        prompt: "Which repos should be monitored by default?"
      - key: pr_review_depth
        description: "Depth of PR analysis (quick/standard/thorough)"
        default: "standard"
        prompt: "How detailed should PR reviews be?"
required_environment_variables:
  - name: GITHUB_TOKEN
    prompt: "GitHub Personal Access Token"
    help: "Generate at GitHub Settings > Developer settings > Personal access tokens. Required scopes: repo, read:org, workflow"
    required_for: "all operations"
---

# GitHub Integration Skill

Seamless GitHub workflow automation for developers using Hermes as a coding companion.

## When to Use

Trigger this skill for any GitHub-related task:

**Issue Management:**
- "Show me open bugs in [repo]"
- "Triage new issues in [org/repo]"
- "Create issue for [bug/feature]"

**Pull Request Workflow:**
- "Review PR #123 in [repo]"
- "What's the status of my open PRs?"
- "Generate release notes from merged PRs"

**Repository Intelligence:**
- "Explain the architecture of [repo]"
- "What's been happening in [repo] this week?"
- "Check dependencies for vulnerabilities"

**Daily Development:**
- "/github standup" - Quick overview of activity across watched repos
- "/github notifications" - Review mentions, reviews requested, assignments

## Quick Reference

| Command | Description | Example |
|---------|-------------|---------|
| `/github issues [repo]` | List and classify issues | `issues owner/repo --label bug` |
| `/github pr [number]` | Detailed PR review | `pr 142 --review` |
| `/github releases [repo]` | Generate release notes | `releases --since v1.2.0` |
| `/github activity [repo]` | Recent repository activity | `activity --days 7` |
| `/github deps [repo]` | Dependency vulnerability scan | `deps --severity high` |
| `/github onboard [repo]` | Repository onboarding guide | `onboard owner/repo` |

## Procedure

### Phase 1: Issue Triage & Management (3-8 minutes)

1. **Fetch Issues with Context**
   ```bash
   gh issue list --repo [owner/repo] --state open --limit 50 \
     --json number,title,labels,createdAt,author,assignee,milestone,comments,url
   ```

2. **Multi-Dimensional Classification**
   
   Classify each issue across these dimensions:
   
   **By Type:**
   - 🐛 Bug Report: Something broken, error, unexpected behavior
   - ✨ Feature Request: New functionality desired
   - 📚 Documentation: Docs improvement needed
   - 🔧 Technical Debt: Code quality, refactoring, optimization
   - ❓ Question: Support inquiry, how-to
   - ⚠️ Security: Vulnerability, security concern
   
   **By Severity:**
   ```
   P0 - Critical: Production down, data loss, security breach
   P1 - High: Major feature broken, significant user impact
   P2 - Medium: Workaround exists, limited impact
   P3 - Low: Minor annoyance, cosmetic, nice-to-have
   ```
   
   **By Urgency:**
   - 🔥 Hot: Being discussed actively, blocking someone
   - 📅 Scheduled: Linked to milestone/release deadline
   - 📌 Backlog: No immediate pressure but should address

3. **Generate Triage Dashboard**
   
   ```markdown
   # 🐙 GitHub Issues Dashboard: [owner/repo]
   **Last Updated:** [timestamp] | **Total Open:** [N] issues

   ---
   
   ## 🚨 Priority Matrix
   
   ### P0 - Critical ([N] issues)
   | # | Title | Age | Labels | Assignee | Blockers |
   |---|-------|-----|--------|----------|----------|
   | #[num] | [title] | [X]d | bug,critical | @user | Blocks PR #XX |
   
   **Immediate Action Needed:**
   - **#[num]** [title] - [why it's critical, suggested action]
   
   ### P1 - High ([N] issues)
   | # | Title | Age | Labels | Assignee |
   |---|-------|-----|--------|----------|
   | ... | ... | ... | ... | ... |
   
   ---
   
   ## 📊 Issue Distribution
   
   **By Type:**
   - 🐛 Bugs: [N] ([X]%)
   - ✨ Features: [N] ([X]%)
   - 📚 Documentation: [N] ([X]%)
   - 🔧 Tech Debt: [N] ([X]%)
   - ❓ Questions: [N] ([X]%)
   
   **By Age:**
   - Stale (> 90 days): [N] ⚠️ Consider closing or updating
   - Aging (30-90 days): [N]
   - Recent (< 30 days): [N]
   
   **Unassigned:** [N] issues need owners
   
   ---
   
   ## 💡 Recommendations
   
   1. **Quick Wins:** [Issues that could be closed/resolved quickly]
   2. **Triage Suggestions:**
      - Issue #[num]: Suggest relabeling from [old] to [new]
      - Issue #[num]: Should be assigned to @user (domain expert)
   3. **Process Improvements:**
      - [Pattern noticed: e.g., many duplicate issues → improve templates]
   
   ---
   
   ## 🔄 Recent Activity (Last 7 Days)
   - **New Issues:** [N] created
   - **Closed:** [N] resolved
   - **Comments:** [N] added (most active: #[num])
   ```

4. **Automated Actions** (with approval)
   - Add labels based on classification
   - Assign to appropriate maintainers (based on code ownership rules)
   - Comment with initial assessment or questions
   - Link related/duplicate issues
   - Suggest milestones or projects

### Phase 2: Pull Request Review (5-15 minutes)

5. **Comprehensive PR Analysis**
   
   When reviewing a PR:
   
   **Step A: Metadata & Context Gathering**
   ```bash
   gh pr view [PR_number] --repo [owner/repo] \
     --json title,body,author,state,additions,deletions,changedFiles,baseRefName,headRefName,labels,milestone,reviews,commits,statusCheckRollup
   ```
   
   **Step B: Diff Analysis**
   ```bash
   gh pr diff [PR_number] --repo [owner/repo]
   ```
   
   Extract key metrics:
   - Lines changed (+additions/-deletions)
   - Files modified (and types: .py, .js, .md, etc.)
   - Complexity indicators (large functions, nested logic)
   - Test coverage changes (if visible)

6. **Structured Review Framework**
   
   ```markdown
   # 🔍 Pull Request Review: #[PR_number]
   **Repository:** [owner/repo] | **Author:** [@user] | **Branch:** [branch] → [base]
   
   ---
   
   ## 📋 PR Summary
   
   **Title:** [PR title]
   **Description:** [2-3 sentence summary of what this PR does]
   **Type:** 🐛 Bug Fix / ✨ Feature / 🔧 Refactor / 📚 Docs / ⚡ Performance / 🔒 Security
   **Size:** [+X additions / -Y deletions] ([Z] files changed)
   
   ---
   
   ## ✅ Strengths
   
   - [Positive aspect 1: clean code, good tests, clear docs, etc.]
   - [Positive aspect 2]
   - [Positive aspect 3]
   
   ---
   
   ## ⚠️ Areas for Consideration
   
   ### Code Quality
   | File | Line(s) | Concern | Suggestion |
   |------|---------|---------|------------|
   | path/to/file.py | L42-L58 | [issue] | [suggestion] |
   | path/to/file.js | L120 | [issue] | [suggestion] |
   
   **Issues Found:**
   - 🔴 Critical: [Must fix before merge] (if any)
   - 🟡 Important: [Should address] (list)
   - 🟢 Suggestion: [Nice to have] (list)
   
   ### Testing
   - [ ] Tests added for new functionality? Yes/No/Partial
   - [ ] Existing tests still passing? (check CI status)
   - [ ] Edge cases covered? 
   - [ ] Test coverage adequate? (estimate if visible)
   
   ### Documentation
   - [ ] README/docs updated if needed?
   - [ ] Code comments for complex logic?
   - [ ] API changes documented?
   - [ ] Changelog entry included?
   
   ### Performance & Security
   - **Performance:** Any concerns? (N+1 queries, memory leaks, etc.)
   - **Security:** Any vulnerabilities? (injection, exposure, auth issues)
   - **Dependencies:** New dependencies introduced? Are they safe?
   
   ---
   
   ## 🎯 Alignment Check
   
   - [ ] Links to related issue? Yes → #[issue_number] / No (should add)
   - [ ] Scope appropriate? (not too large, focused change) Yes/No/Suggest split
   - [ ] Follows project conventions? (style, patterns, architecture) Yes/Mostly/No
   - [ ] Breaking changes? If yes, are they documented/necessary?
   
   ---
   
   ## 💬 Verdict
   
   **Recommendation:** ✅ Approve / 🟡 Request Changes / ❌ Request Changes (significant)
   
   **Summary:** [One paragraph overall assessment]
   
   **Must Address Before Merge:**
   1. [Critical item 1]
   2. [Critical item 2]
   
   **Nice to Have (can follow-up PR):**
   1. [Improvement suggestion]
   2. [Improvement suggestion]
   
   ---
   
   *Review generated by Hermes GitHub Integration*
   *CI Status:* [passing/failing/pending]
   ```

7. **Automated Review Comments** (optional, with approval)
   - Inline comments on specific lines with suggestions
   - Overall PR comment with structured feedback
   - Approval or request-for-changes status

### Phase 3: Release & Project Intelligence (5-10 minutes)

8. **Release Notes Generation**
   
   ```markdown
   # 📦 Release Notes: [version]
   **Date:** [release date] | **Tag:** [tag_name]
   
   ---
   
   ## ✨ Features
   - **[PR #123] - [Feature title]** by [@author] ([PR link])
     [One-line description of user-facing change]
   
   - **[PR #124] - [Feature title]** by [@author]
     [description]
   
   ## 🐛 Bug Fixes
   - **[PR #125]** - Fixed [what was broken] ([issue #456])
   - **[PR #126]** - Fixed [what was broken]
   
   ## 🔧 Internal Changes
   - **[PR #127]** - Refactored [component] for [benefit]
   - Updated [dependency] from X.Y to Z.W
   
   ## ⚠️ Breaking Changes
   - [If any breaking changes, detail migration path]
   
   ## 🔒 Security
   - Fixed [vulnerability] (CVE-XXXX-XXXXX) via [PR #128]
   
   ## 👥 Contributors
   - [@user1] ([N] commits)
   - [@user2] ([N] commits)
   
   ## 📊 Stats
   - Total PRs merged: [N]
   - Lines changed: +[X]/-[Y]
   - Contributors: [N]
   - Time since last release: [X] days
   
   ---
   
   **Full Changelog:** [link to compare view]
   **Upgrade Instructions:** [if applicable]
   ```

9. **Repository Onboarding Guide**
   
   For new team members or when exploring a new repo:
   
   ```markdown
   # 🏗️ Repository Guide: [owner/repo]
   **Stars:** [N] | **Language:** [primary language] | **License:** [license]
   
   ---
   
   ## 📖 What Is This Project?
   
   **One-Line Description:** [from README]
   
   **Purpose:** [2-3 sentences explaining what it does and who it's for]
   
   **Key Features:**
   1. [Feature 1]
   2. [Feature 2]
   3. [Feature 3]
   
   ---
   
   ## 🏗️ Architecture Overview
   
   **High-Level Structure:**
   ```
   [directory]/
   ├── src/                    # Main source code
   │   ├── core/              # Core business logic
   │   ├── api/               # API endpoints/handlers
   │   └── utils/             # Shared utilities
   ├── tests/                 # Test suite
   ├── docs/                  # Documentation
   ├── config/                # Configuration files
   └── scripts/               # Build/deployment scripts
   ```
   
   **Key Components:**
   | Component | Location | Responsibility |
   |-----------|----------|----------------|
   | [Name] | path/to/ | [What it does] |
   | [Name] | path/to/ | [What it does] |
   
   **Data Flow:**
   [How data moves through the system - simple diagram in text]
   
   **External Dependencies:**
   - [Database]: [type, what for]
   - [Cache]: [type, what for]
   - [APIs]: [which external services, why]
   
   ---
   
   ## 🛠️ Development Setup
   
   **Prerequisites:**
   - [Language runtime] >= [version]
   - [Package manager]
   - [Other tools]
   
   **Quick Start:**
   ```bash
   git clone https://github.com/[owner/repo].git
   cd [repo]
   [install command]
   [setup command]
   [run command]
   ```
   
   **Configuration:**
   - Environment variables needed: `[VAR1]`, `[VAR2]`
   - Config file location: `path/to/config`
   
   ---
   
   ## 📋 Conventions & Standards
   
   **Code Style:**
   - Linter: [tool name]
   - Formatter: [tool name]
   - Key rules: [2-3 most important style conventions]
   
   **Git Workflow:**
   - Branch naming: [convention, e.g., feature/xxx, fix/xxx]
   - PR process: [how PRs are reviewed and merged]
   - Commit message style: [conventional commits? other?]
   
   **Testing:**
   - Test framework: [name]
   - How to run tests: `[command]`
   - Coverage requirement: [X% minimum?]
   - CI/CD: [what CI runs, where to see results]
   
   ---
   
   ## 👥 Team & Governance
   
   **Maintainers:** [@user1], [@user2]
   **Contributors:** [N] total (top: [@user], [@user])
   
   **Communication:**
   - Discussions: [link to GitHub Discussions or other channel]
   - Issues: [how issues are used/triaged]
   - Roadmap: [where future plans live]
   
   **Contributing:**
   - CONTRIBUTING.md exists: Yes/No
   - First-timer friendly: Yes/No (are there good-first-issues?)
   - Response time: typically [X] hours/days on PRs/issues
   
   ---
   
   ## 🎯 Where to Start Contributing
   
   **Good First Issues:**
   - #[issue] - [title] (good for beginners)
   - #[issue] - [title]
   
   **Areas Needing Help:**
   - [Area 1]: [what's needed]
   - [Area 2]: [what's needed]
   
   **Quick Wins:**
   - [Something easy but valuable to do first]
   
   ---
   
   *Guide generated on [date]. Repo last updated: [date]*
   ```

### Phase 4: Daily Developer Workflow (2-5 minutes)

10. **Standup Briefing**
    
    ```markdown
    # 🐙 GitHub Standup Briefing
    **Date:** [date] | **Repos Monitored:** [list]
    
    ---
    
    ## 📬 Notifications Requiring Action
    
    ### Reviews Requested
    - **[owner/repo] PR #[num]**: "[title]" - by [@author]
      *Urgency:* [high/medium] *Size:* [small/medium/large]
    
    ### Issues Assigned to You
    - **#[num]** "[title]" in [repo] - Due: [date] (if applicable)
    
    ### Mentions
    - [Comment in issue/PR where you were mentioned]
    
    ---
    
    ## 📊 Your Recent Activity
    
    **Open PRs:**
    | PR | Repo | Status | Age | Reviews |
    |----|------|--------|-----|---------|
    | #XX | [repo] | In Review | 3d | 2 approved, 1 changes req |
    
    **Recently Closed:**
    - Merged PR #XX: "[title]" (+X/-Y lines)
    - Closed issue #XX: "[title]" as [resolution]
    
    ---
    
    ## 🔄 Watched Repos Activity (24h)
    
    **[repo1]:**
    - New issue: #[num] "[title]"
    - PR merged: #[num] "[title]"
    - Release: v[X.Y.Z] published
    
    **[repo2]:**
    - [summary of notable activity]
    
    ---
    
    ## ⚠️ Attention Items
    
    - **Dependency Alert:** [vulnerability in dependency used by your repos]
    - **Stale PR:** Your PR #XX has been open [X] days without activity
    - **Approaching Deadline:** Milestone "[name]" due in [X] days
    
    ---
    
    ## 💡 Suggestions
    
    1. **Priority Action**: [Most important thing to do today on GitHub]
    2. **Follow-up Needed**: [Something that's been waiting too long]
    3. **Opportunity**: [Good time to contribute to X, or claim Y issue]
    ```

11. **Dependency Monitoring**
    
    Scan for security vulnerabilities:
    ```bash
    gh api repos/[owner]/contents/package.json --jq '.content' | base64 -d
    # Parse and check against vulnerability databases
    # Or use: gh dependabot alerts list -R [owner/repo]
    ```
    
    Output:
    ```markdown
    # 🔒 Dependency Security Report
    **Repository:** [owner/repo] | **Scan Date:** [date]
    
    ## Vulnerabilities Found: [N]
    
    | Package | Current | Fixed In | Severity | Type | Affected Files |
    |---------|---------|----------|----------|------|---------------|
    | [pkg] | X.Y.Z | A.B.C | 🔴 Critical | CVE-XXXX | package.json |
    | [pkg] | X.Y | A.B | 🟡 High | CVE-YYYY | requirements.txt |
    
    ## Recommended Actions:
    1. **[Package]**: Upgrade to version [A.B.C] to fix [CVE]
       - Breaking changes: [yes/no]
       - PR ready: [auto-generate upgrade PR?]
    
    ## Outdated Dependencies (Non-Security):
    - [Package]: X.Y (latest: Z.W) - [major/minor behind]
    - Consider updating during next maintenance window
    ```

## Configuration Options

### Repository Scopes
```yaml
monitored_repos:
  - owner/repo:          # Full monitoring (issues, PRs, releases)
    priority: primary     # Shows in daily standup
    watch_issues: true
    watch_prs: true
    notifications: all
  
  - org/another-repo:
    priority: secondary   # Only show in weekly digest
    watch_issues: true
    watch_prs: false
    notifications: mentions_only
```

### Review Preferences
```yaml
pr_review:
  depth: standard         # quick (2 min), standard (5 min), thorough (15 min)
  auto_comment: false     # Auto-post review comments (true/false)
  check_list:
    - code_quality        # Always check
    - testing             # Always check
    - documentation       # Always check
    - performance         # Optional, enable for perf-sensitive repos
    - security            # Optional, enable for public-facing code
  approve_threshold:      # Auto-approve if no issues found (risky, off by default)
```

### Notification Settings
```yaml
notifications:
  standup_time: "09:00"   # Daily briefing time
  include_in_mission_control: true  # Integrate with daily briefings
  alert_on:
    - pr_review_requested
    - issue_assigned
    - mention
    - dependency_vulnerability
    - stale_pr_or_issue   # After X days of inactivity
```

## Pitfalls & Best Practices

### ❌ Common Mistakes:

**Over-Automating Reviews**
- Problem: Auto-generated reviews miss context, annoy maintainers
- Prevention: Always flag as "AI-assisted", require human verification before posting
- Best practice: Generate draft review, let human edit/approve before posting

**Missing Repository Context**
- Problem: Generic advice that doesn't match project conventions
- Prevention: Read CONTRIBUTING.md, recent PRs, codebase structure before advising
- Verify suggestions align with existing patterns in the codebase

**Permission Issues**
- Problem: Token lacks scopes for certain operations
- Prevention: Validate token early, provide clear error messages about missing permissions
- Recommend minimal required scopes (principle of least privilege)

**Noise Generation**
- Problem: Too many notifications/comments, ignored like spam
- Prevention: Be selective, only alert on truly important items
- Allow fine-grained control over notification preferences

### ✅ Best Practices:

**Respect Human Workflows**
- Don't auto-merge, auto-comment, or auto-close without explicit permission
- Frame suggestions as options, not directives ("Consider..." vs "You must...")
- Learn from feedback: if maintainers dislike certain types of comments, stop making them

**Provide Actionable Value**
- Every review comment should have a clear "what" and "why"
- Include links to documentation or examples when suggesting changes
- Offer to implement fixes, not just identify problems

**Stay Current**
- Understand the repo's current state, not just the PR diff
- Be aware of ongoing discussions, recent decisions, roadmap direction
- Check if similar issues/PRs were recently handled and how

**Security Consciousness**
- Never expose tokens, secrets, or sensitive config in comments
- Flag potential security issues privately (not in public PR comments)
- Be cautious about suggesting dependency upgrades (may introduce new vulnerabilities)

## Verification Checklist

**Initial Setup:**
- [ ] GitHub CLI installed and authenticated (`gh auth status`)
- [ ] Token has required scopes (repo, read:org, workflow)
- [ ] Can successfully fetch issues from target repos
- [ ] Can read PR diffs and post comments (test mode)

**Ongoing Validation:**
- [ ] Daily standup generates useful, actionable briefings
- [ ] PR reviews are accurate and helpful (track acceptance rate)
- [ ] Release notes capture key changes accurately
- [ ] Dependency alerts catch real vulnerabilities (not false positives)
- [ ] Notifications are relevant, not noisy (track ignore/dismiss rate)

## Integration Notes

**Powerful Combinations:**
- **Frontend Design**: Review UI components specifically for design system compliance
- **Capability Evolver**: Learn which types of PR feedback are most valued by team
- **Summarize**: Condense long issue threads or PR discussions
- **Mission Control**: Include GitHub action items in daily briefings

**Example Workflow:**
```
Morning:
  1. Mission Control briefing includes "2 PRs need your review"
  2. GitHub skill provides detailed analysis of each PR
  3. You spend 10 minutes reviewing instead of 30

During Coding:
  4. You push code, create PR
  5. GitHub skill monitors for review comments/CI status
  6. Notifies you when review is complete or CI fails

Weekly:
  7. Analyze contribution patterns, identify areas needing attention
  8. Generate release notes for upcoming release
  9. Check dependencies for newly disclosed vulnerabilities
```

---

*Based on OpenClaw's highly-rated GitHub integration skill with enhanced review frameworks, security scanning, and developer workflow optimization for Hermes.*
