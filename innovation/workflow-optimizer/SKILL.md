---
name: workflow-optimizer
description: Advanced workflow analysis and optimization engine that identifies bottlenecks, automates repetitive processes, suggests efficiency improvements, and implements systematic productivity enhancements for individuals and teams.
version: 1.0.0
author: Hermes Skills Team
license: MIT
platforms: [macos, linux, windows]
metadata:
  hermes:
    tags: [productivity, automation, workflow, efficiency, process-improvement]
    category: innovation
    related_skills: [mission-control, capability-evolver, self-improving-agent]
    config:
      - key: analysis_depth
        description: "Depth of workflow analysis (quick/thorough/comprehensive)"
        default: "thorough"
        prompt: "How detailed should the analysis be?"
---

# Workflow Optimizer Skill

Transform chaotic workflows into streamlined, automated systems through intelligent analysis and systematic optimization.

## When to Use

Trigger this skill when you want to:
- **Analyze Workflow**: "Map out my current process for [task]"
- **Find Bottlenecks**: "What's slowing down my [process]?"
- **Automate**: "Automate this repetitive task"
- **Optimize**: "Make this workflow more efficient"
- **Document**: "Create SOP for [process]"
- **Measure**: "Track time spent on activities"

## Quick Reference

| Command | Description |
|---------|-------------|
| `/workflow analyze [process]` | Map and analyze current workflow |
| `/workflow optimize [process]` | Generate optimized version |
| `/workflow automate [task]` | Create automation recipe |
| `/workflow document [process]` | Generate SOP documentation |
| `/workflow audit` | Comprehensive productivity audit |
| `/workflow metrics` | Show KPIs and trends |

## Core Methodology

### Phase 1: Workflow Discovery & Mapping (30 min - 2 hours)

1. **Process Identification**
   ```
   Interview/Gather Information:
   - What triggers this workflow? (event, schedule, request)
   - Who is involved? (people, roles, systems)
   - What are the inputs? (data, documents, approvals)
   - What are the outputs? (deliverables, decisions, artifacts)
   - What tools/systems are used?
   - How long does each step take?
   - Where do delays/bottlenecks occur?
   - What exceptions or edge cases exist?
   ```

2. **Create Visual Workflow Map**
   
   ```mermaid
   graph TD
       A[Trigger Event] --> B{Decision Point}
       B -->|Yes| C[Action 1]
       B -->|No| D[Action 2]
       C --> E[Parallel Process]
       D --> E
       E --> F[Review Step]
       F -->|Approved| G[Completion]
       F -->|Changes Needed| B
   ```

   **Text-based representation (for non-visual tools):**
   ```markdown
   ## Workflow: [Name]
   
   **Trigger:** [what starts it]
   **Owner:** [who is responsible]
   **Frequency:** [how often]
   
   ### Step-by-Step Flow:
   
   1. **[Step Name]** 
      - **Who:** [role/person]
      - **Tool:** [system used]
      - **Input:** [what's needed]
      - **Output:** [what's produced]
      - **Time:** [duration typical]
      - **Dependencies:** [what must come first]
      - **Bottleneck Risk:** [yes/no, why]
      
         ↓
         
   2. **[Next Step]**
      ...
      
   ### Decision Points:
   - **[Decision 1]** at Step X
     * If [condition]: go to Step Y
     * If [else condition]: go to Step Z
   
   ### Exception Paths:
   - **[Exception]**: [when it happens] → [alternative flow]
   ```

3. **Data Collection Methods**
   
   - **Time Tracking**: Use built-in timer or manual logging for 1 week
   - **Screen Recording**: Capture actual process (with permission)
   - **Interviews**: Talk to all stakeholders about pain points
   - **Artifact Review**: Examine outputs, emails, tickets generated
   - **System Logs**: Analyze tool usage data if available

### Phase 2: Bottleneck Analysis (1-2 hours)

4. **Identify Inefficiency Patterns**
   
   Common bottleneck categories:
   
   **🐌 Time Bottlenecks**
   - Manual data entry/re-entry
   - Waiting for approvals (sequential dependencies)
   - Context switching between tools
   - Searching for information/files
   - Unnecessary meetings or status updates
   
   **🔄 Process Bottlenecks**
   - Redundant steps (same info processed twice)
   - Unclear ownership (who does what?)
   - Missing handoff protocols
   - No escalation paths for exceptions
   - Over-complicated approval chains
   
   **🔧 Tool Bottlenecks**
   - Systems don't integrate (manual export/import)
   - Outdated or inadequate tools for the job
   - Lack of templates/standardization
   - Manual notifications/reminders
   - Version control chaos
   
   **👥 People Bottlenecks**
   - Single point of failure (only one person knows how)
   - Skill gaps or training needs
   - Communication overhead
   - Unclear priorities or expectations
   - Resistance to change/new processes
   
5. **Quantify Impact**
   
   For each identified bottleneck:
   ```markdown
   ## Bottleneck #[ID]: [Description]
   
   **Location:** Step(s) [X], [Y]
   **Type:** [Time/Process/Tool/People]
   **Frequency:** Occurs [X]% of the time
   **Impact Severity:** 
   - Time wasted: [X] hours/week
   - Cost impact: $[X]/month (time × hourly rate)
   - Quality impact: [X]% error rate increase
   - Frustration level: 😠😠😠😠☹️ (1-5 scale)
   
   **Root Cause Analysis (5 Whys):**
   1. Why does this happen? → [answer]
   2. Why does [that] happen? → [deeper answer]
   3. ... (continue until root cause found)
   
   **Quick Win Potential:** 🟢 High / 🟡 Medium / 🔴 Low
   **Effort to Fix:** 🟢 Easy ( < 1 day) / 🟡 Moderate (1-3 days) / 🔴 Hard (> 3 days)
   ```

### Phase 3: Optimization Design (2-4 hours)

6. **Generate Optimization Strategies**
   
   Strategy categories:
   
   **🤖 Automation Opportunities**
   ```markdown
   ## Automation Candidate: [Task Name]
   
   **Current State:**
   - Manual effort: [X] minutes per occurrence
   - Frequency: [X] times per [day/week/month]
   - Total manual time: [X] hours/month
   
   **Proposed Automation:**
   - Tool/Method: [Hermes skill / Zapier / Custom script / API]
   - Setup time: [X] hours (one-time)
   - Maintenance: [X] minutes/month
   - Cost: $[X]/month (if any tool subscription)
   
   **ROI Calculation:**
   - Time saved monthly: [X] hours
   - Value of time saved: $[X] (at $[Y]/hour)
   - Payback period: [X] months
   - First year savings: $[X]
   
   **Risk Assessment:**
   - What could go wrong: [list]
   - Mitigation: [plan]
   - Fallback: If automation fails, manual process still documented
   
   **Implementation Priority: P0 (Do first) / P1 (This month) / P2 (Next quarter)
   ```
   
   **📋 Process Improvements**
   - Eliminate unnecessary steps (question each one's value)
   - Parallelize sequential steps where possible
   - Batch similar tasks together
   - Standardize inputs/outputs/templates
   - Implement checklists for consistency
   - Reduce approval layers (empower decision-making)
   
   **🛠️ Tool Optimizations**
   - Integrate disconnected tools (APIs, middleware)
   - Upgrade outdated tools
   - Create templates/macros for repetitive actions
   - Implement keyboard shortcuts for frequent operations
   - Set up dashboards for visibility
   
   **👥 People & Organization**
   - Cross-train team members (eliminate single points of failure)
   - Clarify RACI matrix (Responsible, Accountable, Consulted, Informed)
   - Implement async communication (reduce meeting load)
   - Document tribal knowledge (create knowledge base)
   - Establish SLAs for response times

7. **Design Future State Workflow**
   
   Present optimized version:
   ```markdown
   ## 🚀 Optimized Workflow: [Name]
   
   **Before:** [X] steps, [Y] hours total, [Z] pain points
   **After:** [X'] steps (↓[X-X']), [Y'] hours (↓[Y-Y'%]), [Z'] pain points (↓[Z-Z'])
   
   **Improvement Summary:**
   - Steps eliminated: [N] ([X]% reduction)
   - Time saved: [X] hours/run ([Y]% faster)
   - Automations added: [N]
   - Bottlenecks resolved: [N] of [total]
   
   ### New Process Flow:
   [Simplified visual/text map showing improvements highlighted]
   
   ### Key Changes:
   1. ~~Old Step~~ → **New Approach** (why it's better)
   2. Added: **Automation** (handles [what])
   3. Modified: **Step X** now parallel with **Step Y** (was sequential)
   
   ### Remaining Risks:
   - [Risk]: Mitigation strategy
   ```

### Phase 4: Implementation Roadmap (1-4 weeks)

8. **Prioritize & Plan**
   
   Create phased implementation:
   
   ```markdown
   ## 🗺️ Implementation Roadmap
   
   ### Phase 1: Quick Wins (Week 1)
   **Goal:** Immediate visible improvement, build momentum
   
   | # | Improvement | Impact | Effort | Owner | Status |
   |---|-------------|--------|--------|-------|--------|
   | 1 | [Quick win 1] | High | Low | [Name] | Todo |
   | 2 | [Quick win 2] | Med | Low | [Name] | Todo |
   
   **Expected Outcome:** Save [X] hours/week immediately
   
   ---
   
   ### Phase 2: Automations (Week 2-3)
   **Goal:** Eliminate most repetitive manual work
   
   | # | Automation | Time Saved/Month | Setup Time | Owner |
   |---|------------|------------------|------------|-------|
   | 1 | [Auto task 1] | [X] hrs | [Y] hrs | [Name] |
   | 2 | [Auto task 2] | [X] hrs | [Y] hrs | [Name] |
   
   **Expected Outcome:** Free up [X] hours/week for high-value work
   
   ---
   
   ### Phase 3: Structural Changes (Week 3-4)
   **Goal:** Address root causes, systemic improvements
   
   - [Change 1]: [description]
   - [Change 2]: [description]
   - Training needed: [yes/no, what]
   
   **Expected Outcome:** Sustainable long-term efficiency gains
   
   ---
   
   ### Success Metrics (KPIs)
   
   Track these to validate improvements:
   
   | Metric | Baseline | Target (Month 1) | Target (Month 3) | Measurement Method |
   |--------|----------|------------------|------------------|-------------------|
   | Cycle Time | [X] hours | [-X%] | [-Y%] | Timer/tracking |
   | Error Rate | [X]% | [<X%] | [<Y%] | Quality checks |
   | Manual Touches | [N] per run | [-N%] | [-M%] | Process count |
   | User Satisfaction | [X]/10 | [>X] | [>Y] | Survey |
   | Cost per Transaction | $[X] | $[-Y%] | $[-Z%] | Finance data |
   ```

## Specialized Workflows

### Workflow Type A: Content Creation Pipeline
```
Typical inefficiencies found:
- Writer's block / blank page syndrome
- Inconsistent style/branding
- Multiple revision rounds
- Approval bottlenecks
- Asset management chaos

Optimization strategies:
- Template libraries (outlines, formats, styles)
- Brand voice guidelines (AI-enforceable)
- Structured feedback loops (not open-ended reviews)
- Parallel review tracks (copy vs design vs legal)
- Digital asset management system
```

### Workflow Type B: Customer Support Ticket Resolution
```
Typical inefficiencies:
- Information scattered across systems
- Repeated questions (lack of KB)
- Escalation confusion
- Inconsistent responses
- No proactive follow-up

Optimization strategies:
- Unified customer view (CRM integration)
- Auto-suggested responses from past tickets
- Clear escalation matrix (decision tree)
- Response templates + personalization layer
- Post-resolution satisfaction auto-check
```

### Workflow Type C: Software Development Cycle
```
Typical inefficiencies:
- Context switching (meetings interrupting flow)
- Environment/setup issues
- Code review bottlenecks
- Deployment anxiety (manual process)
- Bug recurrence (no root cause fixes)

Optimization strategies:
- Protected maker time (no-meeting blocks)
- Development environment as code (reproducible)
- Async code review (not synchronous pairing)
- CI/CD pipeline (automated testing + deployment)
- Blameless postmortems (learning culture)
```

## Output Deliverables

### Deliverable 1: Current State Audit Report
```markdown
# 📊 Workflow Audit: [Process Name]
**Date:** [date] | **Auditor:** Hermes Workflow Optimizer

## Executive Summary
[2-3 paragraph overview of findings]

## Key Findings
1. **Finding 1**: [description] - Impact: [High/Med/Low]
2. **Finding 2**: ...
...

## Detailed Analysis
[Full breakdown per phase above]

## Top 5 Bottlenecks (Ranked by Impact)
| Rank | Bottleneck | Time Wasted | Fix Difficulty | Priority |
|------|-----------|-------------|----------------|----------|
| 1 | ... | ... | ... | P0 |
...
```

### Deliverable 2: Optimization Proposal
```markdown
# 🎯 Optimization Plan: [Process Name]

## Recommended Changes
[List of all proposed optimizations with ROI]

## Implementation Timeline
[Gantt-style text timeline]

## Resource Requirements
- Tools needed: [list with costs]
- Training required: [topics, duration]
- People involved: [roles, time commitment]
- Budget estimate: $[X] one-time + $[Y]/month

## Risk Register
[Risks with probability, impact, mitigation]
```

### Deliverable 3: SOP Documentation (Post-Implementation)
```markdown
# 📖 Standard Operating Procedure: [Process Name]
**Version:** 2.0 (Optimized) | **Effective:** [date]
**Owner:** [name/team] | **Review Frequency:** [quarterly]

## Purpose
[Why this process exists, what it achieves]

## Scope
[When to use this SOP, what it covers]

## Prerequisites
[What must be in place before starting]

## Procedure
[Numbered step-by-step instructions with:
 - Screenshots/diagrams where helpful
 - Decision trees for exceptions
 - Time estimates for each step
 - Quality checkpoints]

## Troubleshooting
[Common problems and solutions]

## Related Documents
[Links to templates, forms, referenced docs]

## Revision History
| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 2.0 | [date] | [name] | Optimized version |
| 1.0 | [date] | [name] | Original |
```

## Continuous Improvement Loop

After implementation, establish rhythm:

```markdown
## 🔄 Optimization Cadence

**Weekly:**
- [ ] Quick check: Are new bottlenecks emerging?
- [ ] Metric review: Are KPIs moving in right direction?
- [ ] Team feedback: Any friction points?

**Monthly:**
- [ ] Full metric report vs baseline
- [ ] Identify next optimization opportunities
- [ ] Update documentation if process changed
- [ ] Celebrate wins! Share improvements made

**Quarterly:**
- [ ] Comprehensive workflow re-audit
- [ ] Technology review (new tools available?)
- [ ] Training needs assessment
- [ ] Strategic alignment (does this process still serve goals?)
```

## Success Stories (Example Outcomes)

> **Case Study: Marketing Content Pipeline**
> - **Before:** 5-day turnaround, 3 revision cycles, inconsistent quality
> - **After:** 2-day turnaround, 1 revision cycle (minor tweaks), brand-consistent
> - **Time Saved:** 60% reduction in production time
> - **Quality Improvement:** 40% fewer client revision requests
> - **Key Change:** Implemented template system + AI-assisted draft generation + structured feedback protocol

> **Case Study: Customer Onboarding**
> - **Before:** Manual 15-step process, 45 min avg, frequent errors
> - **After:** 8-step automated workflow, 12 min avg, near-zero errors
> - **Time Saved:** 73% faster onboarding
> - **Customer Satisfaction:** NPS increased from +25 to +62
> - **Key Change:** Self-service portal + automated verifications + progress tracking dashboard

---

*Innovation Skill - Designed for the productivity-obsessed professionals of 2026*
