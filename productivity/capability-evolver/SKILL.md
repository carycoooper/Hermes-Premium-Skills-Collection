---
name: capability-evolver
description: Self-evolution engine that continuously improves how the agent handles specific workflows over time. Monitors interaction patterns, identifies suboptimal responses, and applies learnings to future sessions without manual tuning.
version: 2.0.0
author: Hermes Skills Team
license: MIT
platforms: [macos, linux, windows]
metadata:
  hermes:
    tags: [optimization, evolution, self-improvement, workflow, performance]
    category: productivity
    related_skills: [self-improving-agent, deep-research]
    config:
      - key: evolution_schedule
        description: "When to run evolution cycle (daily/weekly/on-demand)"
        default: "daily"
        prompt: "How often should the agent evolve its capabilities?"
      - key: min_interactions_for_learning
        description: "Minimum interactions before extracting patterns"
        default: "10"
        prompt: "Minimum number of interactions needed to detect improvement opportunities?"
      - key: conservative_mode
        description: "Require user approval before applying changes"
        default: "true"
        prompt: "Should changes require your approval before applying?"
required_environment_variables: []
---

# Capability Evolver Skill

Autonomous self-improvement system that makes your Hermes agent measurably better at your specific workflows over time.

## When to Use

This skill runs **automatically in the background** but can also be controlled manually:

**Automatic Triggers:**
- Scheduled evolution cycles (configurable: daily/weekly)
- After significant interaction milestones (50+ tool calls in a session)
- When quality metrics drop below threshold

**Manual Commands:**
- `/capability-evolver status` - View current evolution state
- `/capability-evolver evolve` - Trigger immediate evolution cycle
- `/capability-evolver history` - Show past evolutions and their impact
- `/capability-evolver rollback [version]` - Revert to previous state
- `/capability-evolver suggest` - Get improvement suggestions without applying

## Quick Reference

| Component | Function | Frequency |
|-----------|----------|-----------|
| Pattern Analyzer | Identifies recurring inefficiencies | Continuous |
 Evolution Engine | Generates improvements | Daily/Scheduled |
 Change Applier | Implements validated changes | With approval |
 Impact Tracker | Measures improvement effectiveness | Ongoing |

## Procedure

### Phase 1: Pattern Monitoring (Continuous Background Process)

1. **Interaction Logging**
   - Log every agent interaction with metadata:
     * Timestamp, duration, tool calls used
     * User request type and complexity
     - Initial response and final accepted version
     * Corrections applied (if any)
     * User satisfaction signals (explicit or implicit)

2. **Pattern Detection Algorithms**
   
   ```python
   # Pseudo-code for pattern detection
   
   def detect_patterns(interaction_log):
       patterns = []
       
       # Detect correction hotspots
       corrections = filter(lambda x: x.was_corrected, log)
       hotspot_topics = cluster_by_topic(corrections)
       if count(hotspot_topics) > 2:
           patterns.append({
               'type': 'recurring_correction',
               'topics': hotspot_topics,
               'severity': high if count > 5 else medium
           })
       
       # Detect response quality issues
       low_quality = filter(lambda x: x.quality_score < 0.7, log)
       if len(low_quality) / len(log) > 0.2:
           patterns.append({
               'type': 'quality_degradation',
               'context': get_common_context(low_quality)
           })
       
       # Detect efficiency bottlenecks
       slow_responses = filter(lambda x: x.duration > avg_duration * 2, log)
       bottlenecks = identify_root_cause(slow_responses)
       if bottlenecks:
           patterns.append({
               'type': 'efficiency_bottleneck',
               'cause': bottlenecks
           })
       
       return patterns
   ```

3. **Metric Collection**
   Track these KPIs continuously:
   - **Response Accuracy Rate**: % of responses accepted without correction
   - **First-Contact Resolution**: % of issues resolved in single exchange
   - **Average Response Time**: Time from request to satisfactory completion
   - **Correction Frequency**: How often user corrects output
   - **Tool Usage Efficiency**: Success rate per tool invocation
   - **User Satisfaction Score**: Implicit signals (follow-up questions, rephrasing)

### Phase 2: Analysis & Hypothesis Generation (Scheduled - Typically Overnight)

4. **Aggregate Data Analysis**
   - Combine logs from configurable time window (default: 7 days)
   - Segment by task type, complexity, time of day
   - Compare against baseline metrics
   - Identify statistically significant deviations

5. **Generate Improvement Hypotheses**
   
   For each detected pattern, generate hypotheses:

   **Example Pattern**: User frequently corrects code output style
   - **Hypothesis 1**: Default code style doesn't match user's project conventions
     * *Proposed Fix*: Detect project linter/config, adapt output accordingly
     * *Expected Impact*: Reduce code corrections by 60%
     * *Risk*: Low (style adaptation, not logic change)

   - **Hypothesis 2**: User prefers more verbose comments in code
     * *Proposed Fix*: Increase comment density in generated code
     * *Expected Impact*: Reduce "add comments" corrections by 80%
     * *Risk*: Medium (may over-comment for some contexts)

   - **Hypothesis 3**: Code examples don't match user's tech stack
     * *Proposed Fix*: Infer tech stack from workspace, prioritize relevant examples
     * *Expected Impact*: Reduce "wrong language/framework" corrections by 90%
     * *Risk*: Low (improves relevance)

6. **Prioritize Improvements**
   Rank hypotheses by:
   - **Impact Score** (expected improvement × frequency)
   - **Confidence Level** (strength of evidence)
   - **Implementation Risk** (potential for negative side effects)
   - **Effort** (complexity of change)

   Output prioritized list:
   ```
   Priority | Improvement | Impact | Confidence | Risk | Effort
   ---------|-------------|--------|------------|------|-------
   P0       | [Hypothesis] | High   | High       | Low  | Low
   P1       | [Hypothesis] | Med    | Med        | Med  | Med
   P2       | [Hypothesis] | Low    | Low        | Low  | High
   ```

### Phase 3: Validation & Application (With User Approval)

7. **Simulation & Testing**
   - For P0/P1 improvements, run A/B test simulation
   - Apply change to sandbox environment
   - Test against historical interaction patterns
   - Measure simulated impact on quality metrics
   - Validate no regression in other areas

8. **Generate Change Proposal**
   
   ```markdown
   ## Evolution Proposal #[ID]
   **Generated**: [timestamp]
   **Pattern Detected**: [description]
   **Evidence Base**: [N] interactions analyzed
   
   ### Proposed Changes
   1. **Change Title**
      - What: [specific modification]
      - Why: [rationale backed by data]
      - Where: [which behaviors/prompts affected]
   
   ### Expected Impact
   - Metric: [Response Accuracy] → [+X%]
   - Based on: [historical pattern analysis]
   
   ### Risk Assessment
   - Potential Side Effects: [list]
   - Mitigation: [plan if things go wrong]
   - Rollback Plan: [how to revert]
   
   ### Approval Required
   [ ] Auto-approve (low-risk changes)
   [ ] Review required (medium-risk)
   [ ] Explicit approval needed (high-risk)
   ```

9. **Apply Validated Changes**
   - If conservative_mode=true: Wait for user approval
   - If conservative_mode=false: Apply low-risk changes automatically
   - Update relevant configuration/prompt components
   - Log change with full audit trail
   - Set monitoring period to detect issues early

### Phase 4: Impact Measurement (Ongoing)

10. **Track Post-Evolution Metrics**
    - Compare pre/post evolution KPIs for 7-14 days
    - Calculate actual vs. predicted improvement
    - Document variance and investigate surprises

11. **Learn from Results**
    - If improvement confirmed: Lock in change, increase confidence in approach
    - If no improvement: Revert, analyze why hypothesis was wrong
    - If regression detected: Immediate rollback, root cause analysis
    - Feed results back into pattern detector for continuous improvement

## Evolution Types Catalog

### Type 1: Prompt Optimization
**What**: Refine system prompts and instructions based on usage patterns
**Example**: Detect that users often ask for shorter responses → adjust default verbosity
**Detection**: Response length corrections, "too long/too short" feedback
**Risk**: Low-Medium

### Type 2: Tool Selection Improvement
**What**: Better match tools to task types
**Example**: Notice web_fetch fails for JS sites → prefer browser automation for those domains
**Detection**: Tool failure rates, retries, manual tool switches
**Risk**: Low

### Type 3: Workflow Adaptation
**What**: Optimize multi-step procedure sequences
**Example**: Users always want proofreading after drafting → make it automatic step
**Detection**: Frequently repeated manual instruction sequences
**Risk**: Medium

### Type 4: Knowledge Enhancement
**What**: Proactively acquire domain knowledge for frequent topics
**Example**: User often asks about Kubernetes → build k8s expertise module
**Detection**: Topic clustering in request history
**Risk**: Low (information-only, no behavior change)

### Type 5: Error Prevention
**What**: Install guardrails for known mistake patterns
**Example**: Keep forgetting timezone in meeting scheduling → add mandatory timezone check
**Detection**: Recurring error patterns, user frustration signals
**Risk**: Low

## Configuration Options

### Conservative Mode (Recommended for Most Users)
```yaml
conservative_mode: true
auto_apply_threshold: "low_risk_only"
review_required_for:
  - prompt_changes
  - workflow_modifications
  - new_tool_integrations
evolution_frequency: daily
max_changes_per_cycle: 3
rollback_enabled: true
monitoring_period_days: 7
```

### Aggressive Mode (For Power Users Who Want Fast Adaptation)
```yaml
conservative_mode: false
auto_apply_threshold: "medium_risk"
review_required_for:
  - high_risk_changes_only
evolution_frequency: twice_daily
max_changes_per_cycle: 5
rollback_enabled: true
monitoring_period_days: 3
```

### Custom Mode
Users can fine-tune:
- Which evolution types to enable/disable
- Minimum evidence threshold before suggesting changes
- Specific topics/domains to exclude from evolution
- Maximum memory/disk usage for evolution logs

## Pitfalls & Safeguards

### 🛡️ Safety Mechanisms:

**Change Caps**
- Maximum 3-5 changes per evolution cycle (prevents drastic shifts)
- Maximum 20% prompt modification per cycle (maintains stability)
- Cooldown period between changes to same component (24h minimum)

**Rollback System**
- Every change is versioned and reversible
- One-click rollback to any previous state
- Automatic rollback if metrics drop > 15% post-change
- Full audit trail of all evolutions with timestamps

**Human Oversight**
- High-risk changes always require explicit approval
- Evolution proposals are human-readable and explain rationale
- User can disable evolution entirely at any time
- Emergency stop: `/capability-evolver pause`

**Data Privacy**
- All analysis done locally (no data sent externally)
- Interaction logs can be auto-deleted after analysis
- Sensitive content detection excludes private data from pattern analysis
- GDPR/CCPA compliant by design

### ⚠️ Risks to Manage:

**Over-Optimization**
- Problem: Agent becomes too specialized, loses generalization
- Prevention: Maintain baseline capabilities, don't optimize away flexibility
- Monitor: Diversity of successful interaction types

**Concept Drift**
- Problem: User's needs change but agent is optimized for old patterns
- Prevention: Weight recent interactions higher than historical ones
- Monitor: Recency-weighted metric trends

**Feedback Loops**
- Problem: Evolution optimizes for wrong metric (gaming the system)
- Prevention: Use multiple independent metrics, human judgment checks
- Monitor: Correlation between automated metrics and user satisfaction

**Evolution Fatigue**
- Problem: Too many small changes confuse user or break workflows
- Prevention: Batch related changes, communicate clearly
- Monitor: User-initiated rollback frequency

## Verification & Testing

Validate evolution system health:

**Daily Health Check:**
- [ ] Evolution cycle completed successfully
- [ ] No unexpected rollbacks in last 24 hours
- [ ] Metric trends stable or improving
- [ ] Disk usage within limits (< 500MB for logs)

**Weekly Review:**
- [ ] Top 3 evolutions this week and their measured impact
- [ ] User approval/rejection rate on proposals
- [ ] Patterns that keep recurring (unresolved issues)
- [ ] Suggestions from evolution engine for user consideration

**Monthly Audit:**
- [ ] Overall trajectory: is agent measurably better than last month?
- [ ] Evolution history makes sense (no concerning changes)
- [ ] Resource usage reasonable
- [ ] User satisfaction trending positively

## Example Evolution Timeline

```
Day 1-7: Baseline collection (no changes, just monitoring)
Day 8: First evolution cycle proposes 2 improvements
  - P0: Adjust response length based on request type (+15% accuracy expected)
  - P1: Add timezone awareness to scheduling tasks (-40% corrections expected)
Day 9: Both approved, applied, monitoring begins
Day 16: Second evolution cycle
  - P0 validated: +18% accuracy (exceeded expectation!)
  - P1 validated: -35% corrections (close to prediction)
  - New P0 proposed: Better code example selection
Day 23: Cumulative improvement measurable
  - Response accuracy: 72% → 84% (+17% absolute)
  - Correction rate: 22% → 11% (-50% relative)
  - User satisfaction signals: noticeably improved
```

---

*Based on OpenClaw's Capability Evolver (35K+ installs, most downloaded non-browsing skill) with enhanced safety mechanisms, rollback systems, and Hermes-native integration.*
