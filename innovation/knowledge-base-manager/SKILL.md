---
name: knowledge-base-manager
description: Personal and team knowledge base construction, organization, and maintenance system. Creates structured knowledge repositories from documents, conversations, research, and experiences using Zettelkasten-inspired methodologies with AI-enhanced tagging and linking.
version: 1.0.0
author: Hermes Skills Team
license: MIT
platforms: [macos, linux, windows]
metadata:
  hermes:
    tags: [knowledge-management, zettelkasten, notes, organization, second-brain, pkm]
    category: innovation
    related_skills: [deep-research, self-improving-agent, summarize]
    config:
      - key: kb_location
        description: "Path to knowledge base directory"
        default: "~/knowledge-base"
        prompt: "Where should your knowledge base be stored?"
      - key: methodology
        description: "Note-taking method (zettelkasten/para/atomic)"
        default: "zettelkasten"
        prompt: "Which knowledge management method to use?"
---

# Knowledge Base Manager Skill

Build a powerful "second brain" – an organized, searchable, interconnected knowledge repository that compounds in value over time.

## When to Use

Trigger this skill when you want to:
- **Create Note**: "Save this to my knowledge base" / "Create a zettel about [topic]"
- **Organize**: "Organize my notes on [subject]" / "Structure my research"
- **Find Knowledge**: "What do I know about [topic]?" / "Search my second brain"
- **Connect Ideas**: "Link these concepts together" / "Find connections between X and Y"
- **Review**: "Review my recent notes" / "Show me orphaned ideas"
- **Synthesize**: "Turn my notes into [article/talk/decision framework]"
- **Maintain**: "Clean up duplicates" / "Update outdated information"

## Quick Reference

| Command | Description |
|---------|-------------|
| `/kb create [topic]` | Create new note/zettel |
| `/kb find [query]` | Search knowledge base |
| `/kb link [note-id]` | Find/create connections |
| `/kb review` | Show recent additions, suggest reviews |
| `/kb synthesize [topic]` | Generate synthesis from related notes |
| `/kb stats` | Knowledge base health metrics |
| `/kb export [format]` | Export as PDF/HTML/Markdown |

## Core Philosophy: Zettelkasten Method

### What is Zettelkasten?

A **slip box** (German for "card box") is a personal knowledge management system where:
- Each idea lives in its own **atomic note** (one concept per note)
- Notes are **connected by links** (not organized in folders)
- New insights emerge from **unexpected connections**
- The system **compounds** over time (network effect)

### Key Principles

1. **Atomicity**: One idea per note (small, focused)
2. **Connectivity**: Link related ideas explicitly
3. **Own Words**: Write in your understanding, not copy-paste
4. **Unique IDs**: Every note has a permanent identifier
5. **No Hierarchy**: Flat structure, emergent organization via links

## System Architecture

### Directory Structure
```
~/knowledge-base/
│
├── inbox/                      # Unprocessed inputs (temporary)
│   ├── 2026-04-27-article.pdf
│   └── 2026-04-27-meeting-notes.txt
│
├── zettel/                     # Permanent atomic notes
│   ├── 202604271430-idea.md     # Format: YYYYMMDDHHMM-title.md
│   ├── 202604271445-concept.md
│   └── 202604271500-insight.md
│
├── structure-notes/            # Higher-level overview notes
│   ├── projects/
│   │   └── hermes-skills-project.md
│   ├── areas/
│   │   ├── ai-agents.md
│   │   └── productivity.md
│   └── mocs/
│       └── knowledge-management-moc.md  # Map of Content
│
├── resources/                  # Reference materials (read-only)
│   ├── papers/
│   ├── books/
│   └── web-clippings/
│
├── templates/                  # Note templates
│   ├── zettel-template.md
│   ├── literature-note.md
│   └── project-note.md
│
├── journal/                    # Daily/weekly journals
│   ├── 2026/
│   │   ├── 04-April.md
│   │   └── 27-Friday.md
│   └── weekly/
│       └── 2026-W17.md
│
├── output/                    # Synthesized outputs
│   ├── articles/
│   ├── presentations/
│   └── decisions/
│
└── kb-config.yaml             # Configuration file
```

## Procedure

### Phase 1: Capture (Continuous)

1. **Quick Capture Anything Interesting**
   
   Don't judge yet, just capture:
   
   ```markdown
   ## 📥 Inbox Processing
   
   **Sources of Input:**
   - 📚 Reading (books, papers, articles)
   - 💬 Conversations (meetings, podcasts, talks)
   - 💡 Ideas (shower thoughts, insights)
   - 🔬 Research (web searches, experiments)
   - 📝 Experiences (lessons learned, failures)
   - ❓ Questions (things to explore later)
   
   **Capture Methods:**
   
   Quick capture (mobile/fast):
   /kb quick-capture "[brief idea or quote]"
   → Creates inbox item for later processing
   
   Structured capture (desktop):
   /kb create --source="book" --title="[topic]"
   → Opens template for detailed note
   
   Import existing:
   /kb import path/to/document.pdf
   → Extracts key points, creates draft zettels
   ```

2. **Inbox Zero Daily Practice**
   
   Process inbox every day (takes 10-20 min):
   
   ```
   For each inbox item:
   
   1. READ/SKIM it quickly
   2. DECIDE: 
      - 💎 Valuable? → Create zettel(s) from it
      - 📌 Reference only? → Move to resources/
      - 🗑️ Not useful? → Delete
      - ⏳ Need more info? → Keep in inbox, add research task
   3. PROCESS: If creating zettel, extract atomic ideas
   4. LINK: Connect to existing notes immediately
   5. EMPTY: Item removed from inbox
   ```

### Phase 2: Process & Create Zettels (Daily)

3. **Extract Atomic Ideas**
   
   From one source (article, book chapter, conversation), create multiple focused notes:
   
   ```markdown
   ---
   id: 202604271530
   title: "Hermes Skill Architecture Uses Progressive Disclosure"
   tags: [hermes, architecture, performance, design-patterns]
   source: Hermes official documentation
   created: 2026-04-27T15:30:00Z
   linked-from: [202604271520]
   ---
   
   # Hermes Skill Architecture Uses Progressive Disclosure
   
   ## Core Concept
   Hermes loads skills in three levels to minimize token usage:
   - Level 0: Metadata only (~3000 tokens) - always loaded
   - Level 1: Full SKILL.md content - loaded on demand
   - Level 2: Reference files within skill - deep dive
   
   ## Why This Matters
   This design choice means agents can have access to hundreds of skills without context window bloat. Only the specific skill needed gets fully loaded.
   
   ## Connection to Other Ideas
   - Related to [[lazy-loading]] in software engineering
   - Similar to [[index-based search]] vs full-scan approaches
   - Contrasts with [[monolithic-prompt]] designs
   
   ## My Thoughts / Questions
   Could this approach be applied to other agent architectures beyond Hermes?
   What's the actual latency cost of loading Level 1 vs having it pre-loaded?
   
   ---
   *Created from reading Hermes docs, Section 2.3*
   ```

4. **Zettel Quality Standards**
   
   Each zettel should pass this checklist:
   
   ✅ **One core idea** (if you need to say "and also", split into two)
   ✅ **Complete thought** (understandable in isolation, without original source)
   ✅ **Own words** (not copied; demonstrates comprehension)
   ✅ **Links included** (at least 1-2 outbound links to related zettels)
   ✅ **Tags applied** (for discoverability beyond links)
   ✅ **Source cited** (where the idea came from)
   ✅ **Personal reflection** (what does this mean to you? questions?)

### Phase 3: Connect & Organize (Weekly)

5. **Linking Strategies**
   
   **Explicit Links (in content):**
   ```markdown
   See also: [[related-zettel-id]]
   Builds upon: [[foundational-concept]]
   Contradicts: [[alternative-viewpoint]]
   Example of: [[broader-principle]]
   ```
   
   **Implicit Connections (via tags):**
   ```yaml
   tags: [ai-agents, productivity, tool-evaluation]
   # Notes with shared tags are implicitly related
   ```
   
   **Backlink Discovery:**
   ```
   /kb backlinks [zettel-id]
   → Shows all notes that link TO this one
   → Helps identify connection opportunities
   ```

6. **Create Structure Notes (Periodically)**
   
   When you have 10+ zettels on a topic, create a structure note:
   
   ```markdown
   ---
   type: structure-note
   area: ai-agents
   topic: Hermes Agent Ecosystem
   last-reviewed: 2026-04-27
   ---
   
   # Hermes Agent Ecosystem
   
   ## Overview
   Hermes is an open-source AI agent framework emphasizing memory, learning, and extensibility.
   
   ## Key Concepts (Zettels)
   
   ### Architecture
   - [[hermes-progressive-disclosure]] - Skill loading strategy
   - [[hermes-memory-system]] - MEMORY.md + USER.md design
   - [[hermes-tool-integration]] - MCP and native tool support
   
   ### Skills System
   - [[skill-format-specification]] - SKILL.md structure
   - [[conditional-activation]] - requires_tools mechanism
   - [[skill-discovery-patterns]] - How agents find right skill
   
   ### Comparison with Alternatives
   - [[hermes-vs-openclaw]] - Feature comparison
   - [[hermes-vs-langchain-agent]] - Architectural differences
   
   ## Open Questions
   - [ ] How does Hermes handle multi-agent collaboration?
   - [ ] What's the roadmap for enterprise features?
   
   ## Connections to Other Areas
   - Related to [[personal-ai-assistants]] area
   - Informs [[my-agent-setup-decisions]]
   
   ---
   *Last updated: reviewed 15 related zettels*
   ```

7. **Map of Contents (MOC) for Major Topics**
   
   Create index notes that serve as entry points:
   
   ```markdown
   # MOC: Knowledge Management Systems
   
   ## Sub-topics (each links to its own structure note)
   - [[zettelkasten-method]] - Atomic note linking
   - [[para-method]] - Projects/Areas/Resources/Resources
   - [[building-second-brain]] - Tiago Forte's approach
   - [[digital-gardening]] - Cultivating knowledge over time
   - [[obsidian-workflow]] - Tool-specific implementation
   
   ## Key Insights Across All Methods
   1. Capture everything initially (reduce friction)
   2. Process regularly (don't let backlog grow)
   3. Connect ideas explicitly (creates network value)
   4. Review periodically (keeps system healthy)
   5. Output/synthesize (ultimate purpose is creation, not hoarding)
   ```

### Phase 4: Review & Evolve (Monthly)

8. **Knowledge Base Health Check**
   
   ```markdown
   # 📊 KB Health Report - [Month Year]
   
   ## Metrics
   
   | Metric | Value | Trend | Target |
   |--------|-------|-------|--------|
   | Total Zettels | [N] | ↗️/↘️/→️ | Growing steadily |
   | Created This Month | [N] | - | ≥ 20/month |
   | Avg Links Per Zettel | [X.X] | - | ≥ 2.0 |
   | Orphaned Notes (no links) | [N]% | ↓ decreasing | < 10% |
   | Inbox Size | [N] items | - | < 5 (daily processing) |
   | Last Review Date | [date] | - | Within 30 days |
   
   ## Action Items from Review
   
   ### Orphaned Notes to Connect
   - [[id1]]: "About X" → Should link to [[related-topic]]
   - [[id2]]: "About Y" → Maybe part of [[broader-area]]?
   
   ### Outdated Notes to Update
   - [[id3]]: Information from 2024, may be superseded
   - [[id4]]: References deprecated library version
   
   ### Notes to Merge or Split
   - [[id5]] and [[id6]] seem to cover same ground → Consider merging?
   - [[id7]] is too long (covers 3 ideas) → Split into atomic parts?
   
   ### Emerging Themes (New Structure Notes Needed?)
   - I have 8+ zettels about [[emerging-topic]] but no structure note yet
   - Time to create [[emerging-topic-moc]]?
   ```

9. **Spaced Repetition Review**
   
   Implement review schedule based on note age:
   
   ```
   Schedule:
   - New notes (0-7 days old): Review tomorrow (consolidate memory)
   - Recent notes (1-4 weeks): Weekly scan
   - Established notes (1-3 months): Bi-weekly glance
   - Mature notes (3+ months): Monthly check during health review
   
   During each review:
   - Does this still resonate? (Update or archive if not)
   - Any new connections to add? (Link to recently created notes)
   - Still accurate? (Facts may have changed)
   - Can I synthesize something from this cluster?
   ```

### Phase 5: Output & Create (As Needed)

10. **Synthesize Knowledge into Outputs**
    
    The ultimate goal isn't just collecting – it's creating new value:
    
    ```markdown
    # 🎨 Output Generation
    
    ## Types of Outputs
    
    ### Writing
    - Blog posts / Articles (from interconnected zettels on topic)
    - Essays / Thought pieces (synthesizing multiple perspectives)
    - Documentation / Guides (practical how-to from experience notes)
    - Newsletters / Digests (curated insights for audience)
    
    ### Presentations
    - Talk outlines (structure notes become slide frameworks)
    - Workshop materials (process notes → teaching content)
    - Meeting prep (relevant zettels → briefing document)
    
    ### Decisions
    - Decision matrices (pros/cons from research zettels)
    - Strategy docs (connected insights inform direction)
    - Project plans (experience notes guide execution)
    
    ### Creative Works
    - Fiction/worldbuilding (ideas file → narrative elements)
    - Code/architecture (pattern notes → implementation)
    - Product features (user pain points → solution design)
    
    ## Synthesis Workflow
    
    1. Define output goal ("Write article about X")
    2. Query KB: /kb find [topic] --deep
    3. Gather relevant zettels (likely 10-30 notes)
    4. Identify themes and narrative arc
    5. Outline using structure notes as skeleton
    6. Flesh out with zettel content (rewrite, don't copy)
    7. Add new insights generated during writing (create new zettels!)
    8. Output polished result
    9. Link output back to source zettels (bidirectional)
    ```

## Templates

### Template A: Standard Zettel
```markdown
---
id: YYYYMMDDHHMM
title: "[Descriptive Title Using Complete Sentence]"
tags: [tag1, tag2, tag3]
source: [Where did this come from?]
created: [ISO timestamp]
---
# [Title]

## Core Idea
[The main point in 1-3 sentences]

## Explanation
[Elaboration, examples, context]

## Why It Matters
[Significance, implications, applications]

## Connections
- See also: [[related-zettel]]
- Builds on: [[foundational-idea]]
- Contrasts with: [[alternative-viewpoint]]

## Personal Reflection
[Your take, questions, ideas sparked]
```

### Template B: Literature Note
```markdown
---
type: literature-note
source:
  type: [book/article/paper/video/podcast]
  title: "[Work Title]"
  author: "[Author Name]"
  url: "[link if available]"
  date-accessed: [date]
  notes-location: [page numbers or timestamp]
tags: [topic-tags]
---
# Literature Note: "[Title]" by [Author]

## Summary (In Own Words)
[2-3 paragraph summary of key points]

## Key Quotes
> "Exact quote that resonated"

*My interpretation of what this means:*

## Atomic Ideas Extracted
(These will become separate zettels - list them here first)
1. [[idea-1-title]] - One sentence summary
2. [[idea-2-title]] - One sentence summary
3. ...

## My Critique / Response
- Strengths: ...
- Weaknesses/Gaps: ...
- How it connects to what I already knew: ...
- Questions it raises: ...

## Action Items
- [ ] Explore [concept mentioned]
- [ ] Read more by this author on [topic]
- [ ] Apply [specific idea] to [my project]
```

### Template C: Project Note
```markdown
---
type: project-note
project: "[Project Name]"
status: [active/paused/completed]
start-date: [date]
target-end-date: [date or open]
tags: [project-tags]
---
# Project: [Name]

## Objective
[What am I trying to achieve? Why does it matter?]

## Background & Context
[Why this project exists, what led to it]

## Approach / Methodology
[How I'm going to do it]

## Resources Needed
- [Resource 1]: [details]
- [Resource 2]: [details]

## Timeline / Milestones
| Milestone | Target Date | Status | Notes |
|-----------|------------|--------|-------|
| [Phase 1] | [date] | ☐/🔵/✅ | ... |

## Linked Zettels (Project-Specific Knowledge)
- [[project-idea-1]]
- [[project-learning-2]]
- [[project-decision-3]]

## Lessons Learned (update throughout)
- What worked: ...
- What didn't: ...
- Would do differently: ...

## Output / Deliverables
- [What will this project produce?]
```

## Advanced Features

### Tag Taxonomy Development
Over time, evolve your tagging system:

```yaml
# Suggested tag hierarchy (customize for your domains)

# Domain Tags (broad topics)
domains:
  - ai-machine-learning
  - software-engineering
  - productivity
  - personal-finance
  - knowledge-management
  - writing-communication

# Method Tags (how you know this)
methods:
  - from-reading          # Books, articles, papers
  - from-experience      # Direct personal experience
  - from-conversation     # Discussions, meetings
  - from-experiment       # Things I tested myself
  - from-reflection       - Thinking deeply about topic

# Type Tags (nature of the insight)
types:
  - concept               # Abstract idea or definition
  - principle             # Fundamental truth or law
  - pattern               # Recurring observable structure
  - technique             # Practical how-to method
  - tool                  # Software, framework, physical tool
  - metaphor              # Analogical thinking aid
  - counterargument       # Challenge to prevailing view
  - question              - Open inquiry, not yet answered
  - hypothesis           - Speculative claim to test

# Status Tags (state of understanding)
status:
  - developing            # Early understanding, still forming
  - solidifying           # Gaining confidence, more evidence
  - established           - Well-understood, frequently used
  - challenged            - Found contradictions or limits
  - superseded            - Replaced by better understanding
```

### Graph Visualization (Future Enhancement)
```
When integrated with graph visualization tools like Obsidian:

/kb graph [--filter tag:productivity]
→ Shows visual network of connected ideas
→ Identifies clusters (highly interconnected topics)
→ Finds bridges (notes connecting different clusters)
→ Highlights orphans (isolated notes needing links)
```

## Integration with Other Skills

**Powerful Combinations:**

- **Deep Research** → Auto-create literature notes from findings
- **Summarize** → Convert long documents into extractable zettels
- **Self-Improving Agent** → Learn your knowledge organization preferences
- **Email Management** → Save important email insights to KB
- **GitHub Integration** → Document technical decisions and learnings
- **Mission Control** → Track knowledge projects alongside work tasks

**Example Workflow:**
```
Morning:
  1. Deep Research on topic → 15 raw findings
  2. Process into 8 atomic zettels (lunch break activity)
  
During Day:
  3. Interesting conversation → Quick capture to inbox
  4. Reading article during commute → Highlight for evening processing
  
Evening:
  5. Inbox processing (20 min) → 3 new zettels from captures
  6. Link new zettels to existing knowledge (10 min)
  
Weekly:
  7. Review session → Update stale notes, find connections
  8. Identify emerging theme → Create new structure note
  
Monthly:
  9. Synthesis → Write blog post from interconnected zettels
  10. Health check → Metrics, cleanup, reorganization
```

## Pitfalls & Solutions

### ❌ Common KM Mistakes:

**Collector's Fallacy**
- Problem: Saving everything but never reviewing/using it
- Solution: Focus on outputs; process only what serves creation goals
- Rule: If you haven't revisited a note in 6 months, archive or delete

**Organization Overkill**
- Problem: Spending more time organizing than creating
- Solution: Embrace messiness; let structure emerge organically
- Rule: Max 20% time on organization, 80% on consumption/creation

**Perfectionism Paralysis**
- Problem: Each note must be perfect before moving on
- Solution: Good enough notes > no notes; iterate later
- Rule: Capture fast (inbox), refine slow (weekly review)

**Template Rigidity**
- Problem: Forcing content into rigid structures kills creativity
- Solution: Templates are starting points, not straitjackets
- Rule: Break format when the content demands it

**Isolation Silo**
- Problem: Knowledge base becomes personal echo chamber
- Solution: Share outputs, get feedback, incorporate external perspectives
- Rule: Monthly: publish/sync at least one synthesis externally

### ✅ Success Indicators:

You're doing it right when:
- ✅ You can find any piece of knowledge within 2 minutes
- ✅ Unexpected connections emerge during review sessions
- ✅ Creating outputs (writing, decisions) feels easier over time
- ✅ Your notes feel like a conversation with your past self
- ✅ You're excited to add to your KB, not burdened by it
- ✅ Others benefit from knowledge you've synthesized

---

*Innovation Skill - Inspired by Niklas Luhmann's Zettelkasten, optimized for the AI-augmented era of 2026*
