---
name: summarize
description: Intelligent summarization with format-aware templates for executive summaries, bullet points, topic-organized summaries, TLDR single-paragraph summaries, action-item extraction, and structured meeting notes. Handles documents, articles, email threads, research papers, and transcripts.
version: 2.0.0
author: Hermes Skills Team
license: MIT
platforms: [macos, linux, windows]
metadata:
  hermes:
    tags: [summarization, extraction, meeting-notes, documents, productivity]
    category: productivity
    related_skills: [deep-research, self-improving-agent]
    config:
      - key: default_format
        description: "Default summary format (executive/bullet/tldr/action-items/meeting)"
        default: "auto"
        prompt: "Select default summary format or 'auto' for intelligent selection"
      - key: max_summary_length
        description: "Maximum tokens for summary output"
        default: "1000"
        prompt: "Maximum length for summaries in tokens?"
---

# Summarize Skill

Advanced content condensation that transforms lengthy content into structured, actionable intelligence.

## When to Use

Trigger this skill when user requests:
- Summarization of any text ("summarize", "tldr", "give me the gist")
- Meeting transcript processing ("summarize this meeting")
- Email thread digestion ("what's the status of this thread?")
- Document analysis ("key points from this document")
- Research paper breakdown ("main findings of this paper")
- Action item extraction ("what are the next steps?")
- Quick briefing ("catch me up on this")

**Auto-detection**: Automatically triggers when pasting/uploading long text (>500 words) without specific instructions.

## Quick Reference

| Input Type | Best Output Format | Command |
|------------|-------------------|---------|
| Long article/document | Executive Summary | `summarize exec [text]` |
| Meeting transcript | Action Items + Decisions | `summarize meeting [text]` |
| Email thread | Status + Next Steps | `summarize email [text]` |
| Research paper | Findings + Methodology | `summarize paper [text]` |
| Multiple sources | Comparative Summary | `summarize compare [texts]` |
| Any content | TLDR (1 paragraph) | `summarize tldr [text]` |

## Procedure

### Phase 1: Content Analysis (30 seconds - 2 minutes)

1. **Assess Input Characteristics**
   - **Length**: < 500 words (brief), 500-2000 (standard), > 2000 (long)
   - **Type**: Article, transcript, email thread, document, code, mixed
   - **Structure**: Has headings? Lists? Timestamps? Q&A format?
   - **Domain**: Technical, business, academic, legal, casual
   - **Urgency**: Time-sensitive? Decision-required? Archive?

2. **Identify Key Information Patterns**
   Scan for these high-value elements:
   - ✅ **Decisions made** (concluded, agreed, decided)
   - ✅ **Action items** (next steps, to-do, follow-up, owner + deadline)
   - ✅ **Key findings/data** (results, numbers, statistics, conclusions)
   - ✅ **Deadlines & dates** (due by, scheduled, milestone)
   - ✅ **Open questions/issues** (unresolved, TBD, pending, blocked)
   - ✅ **Stakeholders** (participants, responsible parties, mentioned people)
   - ⚠️ **Risks/concerns** (challenges, obstacles, caveats)

3. **Determine Optimal Format**
   
   Auto-selection logic:
   ```
   IF contains timestamps AND multiple speakers → Meeting Notes format
   IF contains email headers/replies → Email Thread Summary
   IF contains abstract/methodology/results → Academic Paper format
   IF contains decisions/deadlines → Action Item focus
   IF user is busy/executive → Executive Briefing format
   ELSE → Standard Structured Summary
   ```

### Phase 2: Information Extraction (1-5 minutes)

4. **Hierarchical Extraction Strategy**
   
   **Pass 1: Skeleton Extraction** (10% of content)
   - Extract all headings and section titles
   - Identify thesis statement or main argument
   - Capture conclusion if present
   - Note structure and flow

   **Pass 2: Critical Details** (30% of content)
   - Under each heading, extract top 2-3 most important points
   - Preserve key statistics, quotes, and specific data
   - Capture cause-effect relationships
   - Note contradictions or debates

   **Pass 3: Contextual Enrichment** (remaining as needed)
   - Fill gaps that affect understanding
   - Add necessary background for standalone comprehension
   - Include definitions of domain-specific terms if audience may not know
   - Preserve tone and intent of original

5. **Quality Filtering**
   Apply these filters during extraction:
   - **Relevance Filter**: Does this directly support understanding?
   - **Novelty Filter**: Is this new info or repetition of earlier point?
   - **Specificity Filter**: Prefer concrete details over vague statements
   - **Actionability Filter**: Can reader do something with this information?

### Phase 3: Synthesis & Formatting (1-3 minutes)

6. **Apply Selected Template**

   ### Template A: Executive Summary (for leaders/busy users)
   ```markdown
   ## 📋 Executive Summary: [Title]

   **Bottom Line:** [One sentence core message]

   ---
   
   **🎯 Key Points** (3-5 bullets)
   - [Most critical finding/decision]
   - [Second most important]
   - [Third most important]
   
   **📊 Key Data**
   - Metric 1: [value] ([context])
   - Metric 2: [value] ([context])
   
   **⚠️ Risks/Concerns**
   - [Top 1-2 risks]
   
   **✅ Recommended Actions**
   1. [Immediate action if needed]
   2. [Follow-up action]
   
   **📎 Full Detail Available**: [if user wants more]
   
   ---
   *Summary of [N] words from original [N] word document*
   ```

   ### Template B: Meeting Notes (for transcripts/calls)
   ```markdown
   ## 📝 Meeting Summary: [Meeting Name]
   **Date:** [date] | **Duration:** [length] | **Attendees:** [names]

   ---
   
   **✅ Decisions Made**
   1. **[Decision Title]** 
      - Decision: [what was decided]
      - Rationale: [why]
      - Owner: [who owns implementation]
   
   **📋 Action Items**
   | Task | Owner | Deadline | Priority | Status |
   |------|-------|----------|----------|--------|
   | [Action] | [Name] | [Date] | High/Med/Low | Todo/Done |
   
   **💬 Key Discussion Points**
   - **[Topic]**: [2-3 sentence summary of discussion]
   - **[Topic]**: [summary]
   
   **❓ Open Issues / Blocked**
   - [Issue]: [Why it's blocking, what's needed]
   
   **📌 Next Meeting**
   - Date: [when]
   - Agenda items: [what to prepare/discuss]
   
   ---
   *Transcript: [N] words → Summary: [N] words ([X]% reduction)*
   ```

   ### Template C: Email Thread Digest
   ```markdown
   ## 📧 Thread Summary: [Subject]
   **Thread Length:** [N] messages | **Participants:** [names] | **Last Activity:** [date]

   ---
   
   **🎯 Current Status:** [Active/Pending/Resolved/Blocked]
   
   **📜 Timeline**
   - **[Date] - [Person]**: [One-line summary of their contribution]
   - **[Date] - [Person]**: [summary]
   - ... (chronological, 2-4 lines max total)
   
   **✅ What's Been Decided/Agreed**
   - [Decision 1]
   - [Decision 2]
   
   **❓ Still Open / Needs Response**
   - [Question/issue]: [Who needs to respond, by when]
   
   **👤 Who's Waiting On Whom**
   - [Person A] → [Person B]: [for what]
   
   **📎 Suggested Reply/Next Step**
   [If appropriate: draft response or clear next action]
   ```

   ### Template D: Academic Paper Breakdown
   ```markdown
   ## 🔬 Paper Summary: "[Title]"
   **Authors:** [names] | **Published:** [venue/date] | **Field:** [domain]

   ---
   
   **🎯 Research Question**
   [What problem are they trying to solve?]
   
   **🔬 Methodology**
   - Approach: [experimental/survey/theoretical/simulation]
   - Data: [dataset size, source, characteristics]
   - Methods: [key techniques used]
   
   **📊 Key Findings**
   1. **Finding 1**: [result with statistic if available]
   2. **Finding 2**: [result]
   3. **Finding 3**: [result]
   
   **💡 Main Contribution**
   [What's novel/important about this work?]
   
   **⚠️ Limitations**
   - [Acknowledged by authors]
   - [Potential concerns not addressed]
   
   **🔗 Practical Applications**
   - How can this be used in real world?
   - Relevance to your work/context
   
   **📚 Related Work Context**
   [How does this fit into broader field?]
   ```

   ### Template E: TLDR (Ultra-Brief)
   ```markdown
   ## TLDR: [Topic]
   
   **The Gist:** [2-3 sentences capturing essence]
   
   **In One Bullet:** [Single most important takeaway]
   
   **If You Only Remember One Thing:** [memorable key point]
   
   **Good For:** [quick scan, decision making, sharing with others]
   ```

7. **Refine and Polish**
   - Check for clarity (would someone unfamiliar with original understand?)
   - Verify accuracy (no misrepresentation of original intent)
   - Ensure appropriate detail level (not too dense, not too sparse)
   - Add formatting for readability (bold, lists, tables where helpful)
   - Include metadata (source length, compression ratio, date summarized)

## Pitfalls & Quality Standards

### ❌ Common Mistakes:

**Losing Nuance**
- Problem: Oversimplification removes important context
- Prevention: Include "context" or "caveats" section for complex topics
- Test: Would summary lead to wrong decision if acted upon alone?

**Introducing Bias**
- Problem: Selective emphasis changes meaning of original
- Prevention: Represent all major viewpoints proportionally
- Check: Does summary fairly represent original author's stance?

**Missing Actionability**
- Problem: Summary informs but doesn't enable action
- Prevention: Always extract explicit or implicit next steps
- Verify: Reader knows what to do after reading summary

**False Precision**
- Problem: Rounding numbers or approximating loses critical detail
- Prevention: Keep exact figures when they matter; note when approximate
- Rule: If a number affects decision-making, preserve precision

**Ignoring Audience**
- Problem: Same summary style for executives and technical teams
- Prevention: Adapt terminology, detail level, and format to user
- Signal: Ask clarifying question if unsure about audience

### ✅ Quality Checklist:

Before delivering any summary, verify:

- [ ] Captures main argument/purpose accurately
- [ ] Includes all decisions made (if applicable)
- [ ] Lists action items with owners and deadlines (if applicable)
- [ ] Preserves critical data points, statistics, and quotes
- [ ] Notes uncertainties, disagreements, or open issues
- [ ] Appropriate length for content volume (typically 10-20% of original)
- [ ] Standalone comprehensible without seeing original
- [ ] Formatted for quick scanning (headings, bullets, bold text)
- [ ] Includes source metadata (original length, date, type)
- [ ] Tone matches original (formal→formal, casual→casual)

## Advanced Features

### Comparative Summarization
When given multiple documents on same topic:
```markdown
## Comparative Analysis: [Topic]

| Aspect | Source A | Source B | Source C | Consensus? |
|--------|----------|----------|----------|------------|
| [Point 1] | [View] | [View] | [View] | Yes/No/Partial |
| [Point 2] | [View] | [View] | [View] | ... |

**Key Differences:**
- [Major divergence areas]

**Areas of Agreement:**
- [Where sources align]
```

### Progressive Disclosure
For long summaries, use layered approach:
1. **Layer 1** (TLDR): 3 bullets, 30 seconds to read
2. **Layer 2** (Standard): All sections above, 2-3 minutes
3. **Layer 3** (Full): Complete details, 5-10 minutes
4. **Layer 4** (Source): Link to original for verification

User can request deeper layers on demand.

### Custom Templates
Users can define custom formats:
```
/summarize template create "my-format"
# Define sections, ordering, what to emphasize
# Save for reuse across sessions
```

## Integration Notes

Works excellently with:
- **Deep Research**: Summarizes individual sources before synthesis
- **Self-Improving Agent**: Learns user's preferred summary formats
- **Email Management**: Auto-summarizes long threads
- **Mission Control**: Condenses daily briefings

## Performance Metrics

Target performance standards:
- **Compression Ratio**: Original → 10-20% of length while preserving 95%+ of key information
- **Processing Speed**: < 30 seconds for standard documents (< 2000 words)
- **Accuracy Rate**: > 98% factual accuracy (verified against original)
- **Action Item Extraction**: > 90% recall of explicit tasks/deadlines/owners

---

*Based on OpenClaw's most universally-used skill with enhanced multi-template support, quality checklists, and Hermes-native formatting options.*
