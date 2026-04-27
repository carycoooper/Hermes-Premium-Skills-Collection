---
name: ai-model-comparator
description: Comprehensive AI/LLM model comparison and benchmarking tool that evaluates models across multiple dimensions including reasoning capability, code generation, instruction following, safety, cost-effectiveness, and use-case suitability.
version: 1.0.0
author: Hermes Skills Team
license: MIT
platforms: [macos, linux, windows]
metadata:
  hermes:
    tags: [ai, llm, benchmarking, model-comparison, evaluation, machine-learning]
    category: innovation
    related_skills: [deep-research, capability-evolver]
    config:
      - key: test_categories
        description: "Categories to evaluate (reasoning,coding,safety,cost)"
        default: "all"
        prompt: "Which aspects to compare?"
---

# AI Model Comparator Skill

Systematic framework for evaluating and comparing AI/LLM models across standardized benchmarks and real-world scenarios.

## When to Use

Trigger this skill when you need:
- **Model Selection**: "Which LLM is best for my use case?"
- **Benchmark Comparison**: "Compare GPT-4 vs Claude 3.5 vs Gemini Pro"
- **Cost Analysis**: "Most cost-effective model for code generation"
- **Performance Evaluation**: "Test this model's reasoning capabilities"
- **Procurement Decision**: "Recommendation for enterprise LLM adoption"

## Quick Reference

| Command | Description |
|---------|-------------|
| `/ai-compare [modelA] vs [modelB]` | Head-to-head comparison |
| `/ai-benchmark [model]` | Run standard test suite |
| `/ai-recommend [use-case]` | Get model recommendation |
| `/ai-cost-analysis [models]` | Compare pricing and value |
| `/ai-trend-report` | Latest model landscape overview |

## Evaluation Framework

### Dimension 1: Reasoning & Intelligence
```markdown
## 🧠 Reasoning Capability Assessment

### Test Categories:

**1. Logical Reasoning**
- Syllogism validity testing
- Multi-step deduction problems
- Pattern recognition sequences
- Abstract reasoning puzzles

**2. Mathematical Problem Solving**
- Arithmetic accuracy (basic → calculus)
- Word problems with multiple steps
- Statistical reasoning
- Optimization problems

**3. Commonsense Reasoning**
- Physical world understanding
- Social norms and conventions
- Cause-effect relationships
- Temporal reasoning

**4. Knowledge Integration**
- Cross-domain knowledge application
- Fact synthesis from multiple sources
- Handling contradictory information
- Domain expertise depth

### Scoring Rubric (1-10):
- 9-10: Near-human or superhuman performance
- 7-8: Strong performance, minor errors
- 5-6: Adequate, notable limitations
- 3-4: Significant weaknesses
- 1-2: Fails consistently
```

### Dimension 2: Code Generation & Technical Tasks
```markdown
## 💻 Code Generation Assessment

### Test Scenarios:

**Algorithm Implementation**
- Sorting algorithms (quick, merge, heap)
- Data structures (trees, graphs, hash maps)
- Dynamic programming problems
- Recursion optimization

**Real-World Coding Tasks**
- API integration (REST, GraphQL)
- Database queries (SQL complexity)
- File I/O operations
- Error handling patterns

**Code Quality Metrics**
- Correctness: Does it work?
- Efficiency: Time/space complexity
- Readability: Naming, structure, comments
- Best Practices: Language idioms, security
- Test Coverage: Can it write tests?

### Language Support Matrix:
| Language | Syntax | Algorithms | Debugging | Refactoring |
|----------|--------|------------|-----------|-------------|
| Python | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐ |
| JavaScript | ⭐⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐ |
| Rust | ⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐ | ⭐⭐ |
| Go | ⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐⭐ |
```

### Dimension 3: Instruction Following & Alignment
```markdown
## 🎯 Instruction Following Quality

### Test Categories:

**Format Compliance**
- JSON output strictness
- Markdown formatting adherence
- Table structure preservation
- Code block syntax highlighting

**Constraint Satisfaction**
- Length limits (max tokens, words)
- Style constraints (formal/casual)
- Content restrictions (no jargon, explain like I'm 5)
- Negative constraints (don't do X)

**Multi-step Instructions**
- Sequential task completion
- Conditional branching logic
- Parallel independent tasks
- Error recovery from mistakes

**Ambiguity Handling**
- Asking clarifying questions
- Making reasonable assumptions
- Stating assumptions explicitly
- Graceful degradation

### Scoring:
✅ Perfect compliance (follows all constraints)
⚠️ Minor deviations (cosmetic issues, acceptable interpretation)
❌ Major failures (misses critical constraints, hallucinates requirements)
```

### Dimension 4: Safety & Reliability
```markdown
## 🔒 Safety & Reliability Assessment

### Safety Dimensions:

**Refusal Appropriateness**
- Refuses harmful requests appropriately ✅
- Doesn't over-refuse benign requests ✅
- Provides safe alternatives when refusing ✅
- Explains reasoning for refusal ✅

**Hallucination Rate**
- Factual claims accuracy
- Citation verification
- Confidence calibration (knows when uncertain)
- Admits ignorance gracefully

**Bias & Fairness**
- Stereotype avoidance
- Balanced perspectives on controversial topics
- Cultural sensitivity
- Inclusive language

**Robustness**
- Adversarial prompt resistance
- Jailbreak attempt handling
- Consistent behavior across sessions
- Error recovery without crashing

### Reliability Metrics:
- Uptime / Availability: ___%
- Latency (p50/p95/p99): ___ms
- Error Rate: ___%
- Reproducibility Score: ___/10
```

### Dimension 5: Cost-Effectiveness Analysis
```markdown
## 💰 Total Cost of Ownership (TCO) Model

### Direct Costs:
| Factor | Model A | Model B | Model C |
|--------|---------|---------|---------|
| Input Price ($/M tokens) | $X.XX | $X.XX | $X.XX |
| Output Price ($/M tokens) | $X.XX | $X.XX | $X.XX |
| Context Window (tokens) | 128K | 200K | 32K |
| Effective Cost per Task* | $X.XX | $X.XX | $X.XX |

*Based on standardized task benchmark

### Indirect Costs:
- **Integration Effort**: API compatibility, SDK quality
- **Latency Cost**: User wait time = productivity loss
- **Retraining Cost**: Switching from existing model
- **Vendor Lock-in Risk**: Portability, data export
- **Compliance Cost**: GDPR, SOC2, data residency

### Value Calculation:
```
Value Score = (Performance × Reliability) / (Cost × Risk_Penalty)

Where:
- Performance: Weighted average of dimension scores 1-4
- Reliability: Uptime × (1 - Error_Rate)
- Cost: Normalized TCO (lower = better score)
- Risk_Penalty: Vendor lock-in + compliance gaps
```

## Output Templates

### Template A: Executive Comparison Summary
```markdown
# 🤖 AI Model Comparison Report
**Date:** [date] | **Evaluator:** Hermes AI Comparator v1.0

---

## 🏆 Overall Rankings

| Rank | Model | Overall Score | Best For | Avoid For |
|------|-------|---------------|----------|-----------|
| 1 | [Model] | 9.2/10 | Complex reasoning | Real-time apps |
| 2 | [Model] | 8.7/10 | Code generation | Creative writing |
| 3 | [Model] | 8.3/10 | Multimodal tasks | Long context |

---

## 📊 Dimension Breakdown

| Dimension | Model A | Model B | Model C | Winner |
|-----------|---------|---------|---------|--------|
| Reasoning | 9.5 | 8.8 | 7.9 | Model A |
| Coding | 8.2 | 9.1 | 6.5 | Model B |
| Instruction Following | 9.0 | 8.5 | 8.8 | Model A |
| Safety | 9.8 | 9.5 | 8.0 | Model A |
| Cost-Effectiveness | 7.5 | 8.0 | 9.2 | Model C |

---

## 💡 Recommendations

### For Enterprise Adoption:
**Primary Recommendation:** [Model name]
- Rationale: [2-3 sentence justification]
- Estimated Annual Cost: $[X] based on [usage pattern]
- Risk Level: Low/Medium/High

### For Individual Developers:
**Primary Recommendation:** [Model name]
- Rationale: [justification]
- Free Tier Available: Yes/No (limits)
- Getting Started: [quick start guide]

### For Specific Use Case: [User's scenario]
**Best Fit:** [Model name]
- Why: [specific alignment with needs]
- Alternative: [backup option]
- Configuration Tips: [optimization advice]

---

## ⚠️ Important Caveats

- Benchmarks may not reflect real-world performance exactly
- Models update frequently; re-evaluate quarterly
- Your specific workload may differ from test scenarios
- Consider running your own custom evaluations

---

*Report generated using standardized methodology*
*Last model data refresh: [date]*
```

### Template B: Detailed Technical Deep-Dive
```markdown
# 🔬 Technical Analysis: [Model Name] vs [Model Name]

## Methodology
- **Test Suite:** [name/version]
- **Sample Size:** N prompts per category
- **Evaluation:** Automated scoring + Human review (20% sample)
- **Environment:** [specs]

## Detailed Results

### Reasoning Benchmarks
[Detailed breakdown with examples]

### Code Generation Results
[Code samples, pass/fail rates, quality metrics]

### Failure Mode Analysis
[Common error types, severity, frequency]

## Verdict
[Comprehensive technical recommendation]
```

## Current Model Landscape (2026 Q1 Snapshot)

> ⚠️ **Note:** This data represents a snapshot. Always verify current pricing and capabilities before making decisions.

| Model | Provider | Context | Input Price | Output Price | Strengths |
|-------|----------|---------|-------------|--------------|-----------|
| GPT-4o | OpenAI | 128K | $2.50/M | $10.00/M | Balanced, popular |
| Claude 3.5 Sonnet | Anthropic | 200K | $3.00/M | $15.00/M | Long context, analysis |
| Gemini 1.5 Pro | Google | 1M+ | $1.25/M | $5.00/M | Massive context, multimodal |
| Llama 3.1 405B | Meta (Open) | 128K | Free (self-host) | Free | Open source leader |
| Mistral Large | Mistral AI | 128K | $2.00/M | $6.00/M | Efficient, fast |
| Command R+ | Cohere | 128K | $2.50/M | $10.00/M | RAG, enterprise |
| Qwen 2.5 72B | Alibaba (Open) | 128K | Free | Free | Multilingual, strong coding |

## How to Use This Skill Effectively

### Step 1: Define Your Requirements
Be specific about:
- Primary use case (coding, writing, analysis, chatbot...)
- Volume expectations (requests/day, month)
- Budget constraints
- Latency requirements (real-time? batch?)
- Data sensitivity (can use cloud API? need on-prem?)
- Context length needs
- Multimodal requirements?

### Step 2: Request Targeted Comparison
```
Compare top 3 models for:
- Use case: Building a customer support chatbot
- Volume: 50K conversations/month
- Budget: <$500/month
- Requirements: Low latency (<2s), high safety, multilingual
```

### Step 3: Evaluate Recommendations
Don't just trust the ranking:
- Check if evaluation criteria match YOUR needs
- Look at the detailed breakdown, not just overall score
- Consider running your own small-scale pilot test
- Factor in non-technical aspects (vendor relationship, support)

### Step 4: Make Decision & Monitor
- Choose primary model + fallback option
- Implement monitoring (latency, cost, quality)
- Re-evaluate every quarter as models evolve
- Be ready to switch if better options emerge

## Pitfalls to Avoid

❌ **Common Mistakes:**

1. **Benchmark Chasing** - Optimizing for test scores rather than real utility
2. **Ignoring Total Cost** - Looking only at per-token price, not integration/maintenance costs
3. **Over-Provisioning** - Using expensive model when cheaper one suffices
4. **Vendor Lock-in** - Building deep integrations without portability plan
5. **Static Thinking** - Not re-evaluating as competitive landscape shifts rapidly

✅ **Best Practices:**

- **AB Test in Production**: Route 10% traffic to new model, compare metrics
- **Hybrid Approaches**: Use cheap model for simple tasks, expensive for complex ones
- **Fallback Chains**: Primary → Secondary → Rule-based → Human escalation
- **Cost Alerts**: Set budgets and alerts to prevent bill shocks
- **Regular Audits**: Monthly review of usage, cost, quality metrics

---

*Innovation Skill - Designed for the rapidly evolving AI landscape of 2026*
