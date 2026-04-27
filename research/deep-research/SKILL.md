---
name: deep-research
description: Advanced multi-source research skill that systematically explores topics across multiple sources, cross-references findings, identifies contradictions, and produces structured analytical reports with citations. Combines web browsing, content extraction, and synthesis capabilities.
version: 2.0.0
author: Hermes Skills Team
license: MIT
platforms: [macos, linux, windows]
metadata:
  hermes:
    tags: [research, web-browsing, analysis, reporting, fact-checking]
    category: research
    requires_toolsets: [web]
    related_skills: [summarize, capability-evolver]
    config:
      - key: max_sources
        description: "Maximum number of sources to analyze per query"
        default: "10"
        prompt: "How many sources should be analyzed per research query?"
      - key: depth_level
        description: "Research depth: quick (1-2 sources), standard (5-8 sources), deep (10+ sources)"
        default: "standard"
        prompt: "Select research depth level (quick/standard/deep)"
required_environment_variables: []
---

# Deep Research Skill

Advanced systematic research capability for comprehensive topic analysis and intelligence gathering.

## When to Use

Trigger this skill when the user requests:
- In-depth research on any topic ("research", "deep research", "analyze", "investigate", "audit")
- Competitive landscape analysis or market research
- Technology evaluation and comparison studies
- Due diligence on companies, products, or people
- Academic-style research with citation requirements
- Fact-checking and verification of claims
- Comprehensive background research before meetings or decisions

## Quick Reference

| Command | Description |
|---------|-------------|
| `deep-research [topic]` | Start deep research on a topic |
| `deep-research compare [A] vs [B]` | Comparative analysis |
| `deep-research verify [claims]` | Fact-check specific claims |
| `deep-research monitor [topic]` | Ongoing monitoring setup |

## Procedure

### Phase 1: Query Analysis & Planning (2-3 minutes)

1. **Understand Research Goals**
   - Identify core questions to answer
   - Determine required depth (quick/standard/deep)
   - Define success criteria for the research output
   - Clarify any constraints (timeframe, geography, industry focus)

2. **Formulate Search Strategy**
   - Break complex queries into 3-5 sub-questions
   - Identify key terms, synonyms, and related concepts
   - Plan source diversity (news, academic, official, community)
   - Estimate time and source requirements based on depth setting

### Phase 2: Multi-Source Information Gathering (5-15 minutes)

3. **Initial Broad Search**
   - Use web_search for overview and recent developments
   - Capture 8-12 potential sources from initial results
   - Prioritize authoritative sources (.gov, .edu, established news)
   - Note publication dates to ensure recency

4. **Deep Content Extraction**
   - Use WebFetch to extract full content from top 5-10 sources
   - For each source, capture:
     * Title, author, publication date, URL
     * Key claims and data points
     * Supporting evidence and citations
     * Methodology (if applicable)
   - Use browser automation if JavaScript-rendered content is needed

5. **Targeted Follow-up Searches**
   - Based on initial findings, search for:
     * Contradicting viewpoints or counter-evidence
     * Expert opinions and analysis pieces
     * Statistical data and official reports
     * Recent updates or breaking news

### Phase 3: Synthesis & Analysis (5-10 minutes)

6. **Cross-Reference Findings**
   - Create a finding matrix comparing all sources
   - Identify points of consensus vs. disagreement
   - Flag contradictions requiring further investigation
   - Note information gaps that couldn't be filled

7. **Critical Evaluation**
   - Assess source credibility and potential bias
   - Evaluate evidence strength for each major claim
   - Identify logical fallacies or weak reasoning
   - Distinguish between facts, opinions, and speculation

8. **Synthesize Key Insights**
   - Extract 5-7 most important findings
   - Group related insights into themes
   - Identify patterns, trends, and anomalies
   - Formulate balanced conclusions acknowledging uncertainty

### Phase 4: Report Generation (3-5 minutes)

9. **Structure the Final Report**

   ```markdown
   # Research Report: [Topic]

   ## Executive Summary (TL;DR)
   - 3-5 bullet points of most critical findings

   ## Key Findings
   ### Finding 1: [Title]
   - Detailed explanation with supporting evidence
   - Source citations: [1], [3], [5]

   ### Finding 2: [Title]
   - ...

   ## Contradictions & Uncertainties
   - Areas where sources disagree
   - Limitations of available information

   ## Sources Analyzed
   | # | Source | Date | Credibility | Key Contribution |
   |---|--------|------|-------------|------------------|
   | 1 | ... | ... | High/Med/Low | ... |

   ## Recommendations (if applicable)
   - Actionable next steps based on findings

   ## Methodology Notes
   - Search strategy used
   - Sources excluded and why
   - Known limitations
   ```

10. **Quality Review**
    - Verify all citations are accurate and accessible
    - Check for balance in presenting different viewpoints
    - Ensure executive summary captures essential insights
    - Confirm report answers original research questions

## Pitfalls & Common Mistakes

### ❌ Avoid These:

**Confirmation Bias**
- Don't only seek sources that confirm pre-existing beliefs
- Actively search for opposing viewpoints
- Present disagreements fairly even when you favor one side

**Source Quality Issues**
- Avoid relying solely on user-generated content (forums, social media) without verification
- Be cautious of sources with clear commercial or political agendas
- Check publication dates; information can become outdated quickly

**Overconfidence**
- Don't present uncertain findings as definitive facts
- Use appropriate hedging language ("suggests," "indicates," "appears")
- Acknowledge when evidence is limited or contradictory

**Scope Creep**
- Stick to the defined research scope unless user explicitly expands it
- If research is too broad, suggest narrowing or splitting into phases
- Manage time expectations for deep research (15-30 minutes typical)

**Citation Errors**
- Always preserve original URLs for verification
- Don't paraphrase quotes without indicating it's a paraphrase
- Include access dates for time-sensitive content

### ✅ Best Practices:

**Source Diversity**
- Mix news outlets, official sources, academic papers, expert analysis
- Include both primary sources and reputable secondary analysis
- Seek international perspectives for global topics

**Structured Note-Taking**
- Maintain organized notes during gathering phase
- Tag findings by theme for easy synthesis later
- Preserve exact quotes for key claims

**Iterative Refinement**
- After first draft, identify weakest sections
- Do targeted follow-up searches to strengthen weak areas
- Revise conclusions if new evidence contradicts initial findings

## Verification Checklist

Before delivering the final report, verify:

- [ ] All factual claims have source citations
- [ ] At least 5 unique sources consulted (for standard depth)
- [ ] Contradictory evidence is acknowledged and addressed
- [ ] Publication dates are noted for time-sensitive info
- [ ] Executive summary accurately represents detailed findings
- [ ] Report directly addresses user's original questions
- [ ] URLs are valid and content is still accessible
- [ ] Language is objective and professional
- [ ] Methodology limitations are disclosed
- [ ] Recommendations are grounded in evidence presented

## Output Templates

### Template 1: Executive Briefing (for busy users)
```markdown
## 📊 [Topic] - Executive Briefing

**Bottom Line:** [One sentence conclusion]

**Key Points:**
1. [Most important finding]
2. [Second most important]
3. [Third most important]

**Risk Level:** 🟢 Low / 🟡 Medium / 🔴 High

**Confidence:** High/Medium/Low (based on source quality)

**Recommended Action:** [One specific next step]

**Full Report Available:** [If user wants details]
```

### Template 2: Comparative Analysis
```markdown
## Comparison: [Option A] vs [Option B]

| Criterion | Option A | Option B | Winner |
|-----------|----------|----------|--------|
| [Feature 1] | ✅/❌ | ✅/❌ | A/B |
| [Feature 2] | ✅/❌ | ✅/❌ | A/B |
| Price | $X | $Y | A/B |

**Verdict:** [Clear recommendation with reasoning]
```

### Template 3: Fact-Check Report
```markdown
## Fact Check: [Claim(s) to Verify]

| Claim | Verdict | Evidence | Confidence |
|-------|---------|----------|------------|
| [Claim 1] | ✅ True / ⚠️ Partially True / ❌ False | [Summary of evidence] | High/Med/Low |

**Context:** [Important nuances or caveats]
```

## Integration Notes

This skill works best when paired with:
- **Summarize skill**: For condensing long source articles before analysis
- **Capability Evolver**: Learns your research preferences over time
- **Web Search toolset**: Required for information gathering
- **Browser toolset**: Optional, for JavaScript-heavy sites

## Performance Tips

**Speed Optimization:**
- Use "quick" mode for simple factual questions (2-3 sources, ~5 min)
- Reserve "deep" mode for important decisions (10+ sources, ~25 min)
- Set realistic time expectations with user upfront

**Quality Optimization:**
- Always use multiple search queries with varied phrasing
- Don't rely on first-page search results exclusively
- Consult both recent articles and foundational/evergreen content
- Use specialized searches (news, scholarly) when available

**Token Efficiency:**
- Summarize long source documents before including in context
- Focus extraction on relevant sections rather than full page content
- Use progressive disclosure: brief summary first, detail on demand

---

*Based on OpenClaw's Deep Research skill (180K+ installs) with Hermes-specific optimizations including structured templates, quality verification checklists, and integration with Hermes toolsets.*
