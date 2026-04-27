---
name: self-improving-agent
description: Captures corrections, preferences, and learned facts into structured memory files. Automatically promotes recurring learnings to permanent memory after 3+ occurrences within 30 days. Creates a personalized agent that adapts to communication style and eliminates repetitive preference explanations.
version: 2.0.0
author: Hermes Skills Team
license: MIT
platforms: [macos, linux, windows]
metadata:
  hermes:
    tags: [memory, learning, personalization, preferences, self-improvement]
    category: productivity
    related_skills: [capability-evolver, mission-control]
    config:
      - key: promotion_threshold
        description: "Number of occurrences before promoting to permanent memory"
        default: "3"
        prompt: "How many times should a correction repeat before becoming permanent?"
      - key: memory_retention_days
        description: "Days to track occurrence frequency"
        default: "30"
        prompt: "How many days back to look for repeating patterns?"
---

# Self-Improving Agent Skill

Transforms Hermes into a personalized agent that learns from every interaction and remembers preferences across sessions.

## When to Use

This skill is **always active** and automatically triggers when:
- User corrects agent output ("I prefer X instead of Y")
- User provides personal preferences ("always use my nickname")
- User shares contextual facts ("my timezone is Eastern")
- User expresses communication style preferences ("use bullet points")
- User repeats an instruction given in previous sessions

The skill operates passively in the background but can also be invoked manually:
- `/self-improving-agent review` - Show recent learnings
- `/self-improving-agent forget [pattern]` - Remove a learned pattern
- `/self-improving-agent export` - Export all learnings as backup

## Quick Reference

| Trigger | Action | Memory Type |
|---------|--------|-------------|
| Correction received | Log correction pattern | Working Memory |
| Preference stated | Store preference | Session Memory |
| Fact shared | Record fact | Long-term Memory |
| Pattern repeats 3× in 30 days | Promote to permanent | Permanent Memory |

## Procedure

### Phase 1: Real-Time Learning Capture (Continuous)

1. **Detect Learning Opportunities**
   - Monitor for explicit corrections: "No, I meant...", "Actually...", "Don't do X, do Y"
   - Identify implicit preferences from user reactions
   - Note contextual facts mentioned casually
   - Recognize repeated instructions across sessions

2. **Classify Learning Type**

   ```markdown
   ## Learning Categories

   ### Communication Style Preferences
   - Format preferences (bullet points vs numbered lists, short vs detailed)
   - Tone preferences (formal vs casual, concise vs explanatory)
   - Language preferences (terminology, abbreviations, jargon usage)

   ### Factual Knowledge
   - Personal info (name, role, timezone, contact details)
   - Project context (team members, tools, workflows, conventions)
   - Domain knowledge (industry-specific terms, company specifics)

   ### Behavioral Corrections
   - Mistakes to avoid (errors made and corrected)
   - Approaches that don't work for this user
   - Workflow preferences (how tasks should be handled)

   ### Task-Specific Patterns
   - Recurring task formats and structures
   - Standard outputs expected for specific request types
   - Approval workflows and decision patterns
   ```

3. **Store in Appropriate Memory Layer**

   **Working Memory** (`~/.hermes/memory/working.md`)
   - Temporary storage for new learnings
   - Holds last 50 entries
   - Reviewed and cleaned periodically

   **Session Memory** (`~/.hermes/memory/session-[date].md`)
   - Learnings from current session
   - Archived when session ends
   - Used for session-level pattern detection

   **Permanent Memory** (`~/.hermes/memory/permanent.md`)
   - Promoted learnings (after threshold met)
   - Survives across all sessions
   - Manually editable by user

### Phase 2: Pattern Recognition & Promotion (Daily)

4. **Analyze Learning Patterns**
   - Scan working memory for repetitions
   - Count occurrences of similar corrections/facts
   - Check time window (default: 30 days)
   - Identify clusters of related learnings

5. **Apply Promotion Rules**
   
   ```
   IF correction_pattern occurs ≥ 3 times AND
      within retention_period (30 days) AND
      NOT already in permanent memory
   THEN:
     - Promote to permanent memory
     - Add metadata (first_seen, count, last_seen, source_sessions)
     - Notify user: "I've learned that you prefer [X]. Added to permanent memory."
   ```

6. **Handle Conflicts**
   - If new learning contradicts existing permanent memory:
     * Flag as conflict for user review
     * Keep both versions with timestamps
     * Ask user to resolve: "You previously said [X], now mentioned [Y]. Which is correct?"
   - If pattern evolves over time (gradual shift):
     * Update existing entry with new value
     * Archive old version with date range

### Phase 3: Active Application (Every Interaction)

7. **Load Relevant Memory Before Responding**
   - Query permanent memory for applicable preferences
   - Check working memory for recent corrections
   - Load project/context-specific facts
   - Apply communication style settings

8. **Apply Learned Behaviors**
   - Use preferred format automatically
   - Avoid known mistakes and anti-patterns
   - Incorporate domain knowledge naturally
   - Reference prior context when relevant

9. **Continue Learning Loop**
   - Monitor response for user feedback
   - Capture any new corrections immediately
   - Update confidence scores for applied learnings
   - Track which memories are actually useful

## Memory File Formats

### Working Memory Structure
```markdown
# Working Memory
Last Updated: [timestamp]

## Recent Corrections (Last 50)
### [Date] - [Pattern ID]
- **Type**: [correction/preference/fact]
- **Content**: [what was learned]
- **Context**: [what triggered it]
- **Count**: [occurrence count]
- **Status**: [active/superseded/promoted]

## Session Summary: [Date]
- Total learnings captured: N
- Promotions this session: N
- Conflicts detected: N
```

### Permanent Memory Structure
```markdown
# Permanent Memory
Last Updated: [timestamp]

## Communication Style
- **Format**: [bullet-points/numbered/mixed]
- **Tone**: [formal/casual/professional-friendly]
- **Detail Level**: [concise/detailed/adaptable]
- **Language**: [EN/ZH/bilingual]

## Personal Facts
- **Name**: [preferred name/nickname]
- **Role**: [job title/position]
- **Timezone**: [timezone]
- **Team**: [team/group name]

## Project Context
- **Active Projects**: [list]
- **Tools**: [tech stack, platforms]
- **Conventions**: [coding standards, naming, etc.]

## Behavioral Rules
- **Avoid**: [list of mistakes/patterns to avoid]
- **Always**: [preferred approaches]
- **Workflow**: [standard procedures]

## Learned Corrections History
| Date | Pattern | Times Corrected | Resolution |
|------|---------|-----------------|------------|
| ... | ... | ... | ... |
```

## Pitfalls & Solutions

### ❌ Common Problems:

**Over-Automatic Learning**
- Problem: Agent learns from casual mentions that weren't meant to be permanent
- Solution: Require explicit correction language or 3+ confirmations before promotion
- Signal phrases: "remember that", "always", "from now on", "don't forget"

**Stale Memory**
- Problem: Old learnings no longer apply but remain in permanent memory
- Solution: Implement decay mechanism - ask user to confirm memories older than 90 days
- Flag low-confidence or rarely-used memories for review

**Memory Bloat**
- Problem: Too many stored items slow down context loading
- Solution: Limit permanent memory to 100 high-value entries
- Consolidate related items, archive rarely-used ones

**Privacy Concerns**
- Problem: Storing personal facts in plain text files
- Solution: Mark sensitive data, offer encryption option, respect user deletion requests
- Never store passwords, API keys, or auth tokens in memory

**Context Confusion**
- Problem: Applying wrong preferences in different contexts (work vs personal)
- Solution: Tag memories with context scopes (project, domain, general)
- Only load memories matching current context

### ✅ Best Practices:

**Selective Persistence**
- Not everything needs to be remembered forever
- Distinguish between "remember for this session" vs "remember always"
- Use working memory for ephemeral preferences

**Explicit Confirmation**
- Before promoting to permanent memory, show user what will be saved
- Allow opt-out: "Should I remember this for future sessions?"
- Provide easy way to view and edit stored memories

**Regular Maintenance**
- Weekly cleanup of working memory
- Monthly review of permanent memory relevance
- Archive old learnings rather than deleting completely

**Respect User Agency**
- User can always override learned behaviors
- Provide `/forget` command to remove unwanted learnings
- Never argue with explicit user corrections

## Verification & Testing

Test scenarios to validate learning behavior:

1. **Basic Learning Test**
   - User: "I prefer tables over lists"
   - Next request: Should use table format automatically
   - Check: Appears in working memory

2. **Promotion Test**
   - Same correction given 3 times across sessions
   - Check: Promoted to permanent memory after 3rd occurrence
   - Notification sent to user

3. **Conflict Detection Test**
   - Permanent memory: "User prefers formal tone"
   - New correction: "Use casual tone"
   - Check: Conflict flagged, user asked to resolve

4. **Forgetting Test**
   - Command: `/self-improving-agent forget format-preference`
   - Check: Entry removed from all memory layers

5. **Export Test**
   - Command: `/self-improving-agent export`
   - Check: JSON/markdown file generated with all learnings

## Integration Notes

Works synergistically with:
- **Capability Evolver**: Handles behavioral optimization, this handles knowledge accumulation
- **Mission Control**: Uses stored preferences for briefing formatting
- **All other skills**: Benefits from learned preferences automatically

## Statistics Tracking

Monitor these metrics to assess learning effectiveness:
- Learnings per session (target: 2-5)
- Promotion rate (target: 20% of working memory promoted monthly)
- Conflict rate (should be < 5%)
- User satisfaction with auto-applied preferences (track corrections to learned behaviors)
- Memory hit rate (how often stored preferences are relevant)

---

*Based on OpenClaw's highest-starred skill (338 stars, 32K+ installs) with enhanced memory management, conflict resolution, and privacy controls for Hermes.*
