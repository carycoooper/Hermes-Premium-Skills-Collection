---
name: email-management
description: Comprehensive email handling including triage, categorization, drafting, automated responses, newsletter monitoring, attachment processing, and unsubscribe audits. Works with IMAP/SMTP, Gmail API, and Outlook/Exchange via CLI tools or API integrations.
version: 2.0.0
author: Hermes Skills Team
license: MIT
platforms: [macos, linux, windows]
metadata:
  hermes:
    tags: [email, communication, productivity, triage, automation]
    category: communication
    related_skills: [summarize, mission-control, self-improving-agent]
    config:
      - key: email_provider
        description: "Email service provider (gmail, outlook, imap-generic)"
        default: "gmail"
        prompt: "Which email service do you use?"
      - key: triage_categories
        description: "Categories for email classification"
        default: "urgent,important,to-read,newsletter,promotional,spam"
        prompt: "What categories should emails be sorted into?"
      - key: auto_reply_enabled
        description: "Enable automatic response drafting for common scenarios"
        default: "false"
        prompt: "Should the agent draft replies automatically?"
required_environment_variables:
  - name: EMAIL_ADDRESS
    prompt: "Your email address"
    help: "Used for authentication and filtering"
    required_for: "all operations"
  - name: EMAIL_APP_PASSWORD
    prompt: "App-specific password or OAuth token"
    help: "Get from your email provider's security settings"
    required_for: "sending and full access"
---

# Email Management Skill

Transform chaotic inbox into organized, actionable intelligence with intelligent triage and automation.

## When to Use

Trigger this skill when user requests anything email-related:
- **Triage/Overview**: "Check my email", "any urgent messages?", "summarize inbox"
- **Search**: "find emails from X", "emails about Y topic", "last week's threads"
- **Draft**: "draft reply to [person/thread]", "write email about [topic]"
- **Analyze**: "email trends", "response time stats", "unsubscribe audit"
- **Process**: attachments, newsletters, receipts, confirmations

**Auto-trigger opportunities** (configurable):
- New urgent email detected (immediate notification)
- Daily inbox digest (morning briefing integration)
- Unsubscribe recommendations (weekly)

## Quick Reference

| Command | Description | Example |
|---------|-------------|---------|
| `/email triage` | Classify recent emails | Show last 50 emails categorized |
| `/email search [query]` | Search inbox/sent | `search from:boss subject:Q1` |
| `/email draft [thread_id]` | Draft reply to thread | `draft latest from client` |
| `/email send [draft_id]` | Send drafted email (with approval) | `send draft 3` |
| `/email unread` | Count/show unread emails | "You have 23 unread messages" |
| `/email newsletters` | Analyze newsletter subscriptions | List, summarize, suggest unsubscribes |
| `/email attachments` | Process recent attachments | Extract/download recent PDFs |
| `/email stats` | Email analytics | Response rates, volume trends |

## Procedure

### Phase 1: Inbox Triage & Classification (2-5 minutes)

1. **Fetch Recent Emails**
   - Default: Last 100 unread + last 50 read (for context)
   - Configurable: Adjust volume based on typical inbox size
   - Metadata captured: From, Subject, Date, Preview, Labels, Thread ID

2. **Multi-Dimensional Classification**
   
   Classify each email along these axes:
   
   **Axis 1: Urgency**
   ```
   🔴 URGENT: Requires response < 2 hours
      - Markers: "ASAP", "urgent", "immediate", "deadline today", sender is boss/client
      - Action: Flag for immediate attention, notify user
   
   🟠 HIGH: Respond within 24 hours
      - Markers: Direct questions, action items, awaiting your input
      - Action: Include in daily briefing, suggest response time
   
   🟡 NORMAL: Respond within 3 days
      - Markers: FYI, informational, no explicit ask
      - Action: Queue for review, lower priority
   
   🟢 LOW: Can wait or skip
      - Markers: Newsletters, promotional, bulk, automated
      - Action: Batch process, consider unsubscribe
   ```

   **Axis 2: Category**
   ```
   Categories (customizable):
   - work_internal: Colleagues, team, company-wide
   - work_external: Clients, partners, vendors, recruiters
   - personal: Friends, family, personal accounts
   - finance: Bills, receipts, bank notifications, invoices
   - notifications: Social media, shipping, account alerts
   - newsletters: Subscribed content, digests, marketing
   - spam/junk: Obvious spam (usually filtered already)
   ```

   **Axis 3: Action Required**
   ```
   - reply_needed: Explicit question or request for response
   - review_only: No response needed but should read
   - action_item: Contains task/to-do for user (separate from replying)
   - fyi: Purely informational
   - archive: No value, safe to delete/archive
   ```

3. **Generate Triage Report**
   
   ```markdown
   # 📧 Email Triage Report
   **Scanned:** [N] emails (unread: [N]) | **Time:** [timestamp]

   ---
   
   ## 🔴 URGENT - Immediate Attention ([N] emails)
   
   | From | Subject | Received | Action Needed | Deadline |
   |------|---------|----------|---------------|----------|
   | [Name] | [Subject] | [time ago] | Reply/Decide | [deadline] |
   | [Name] | [Subject] | [time ago] | Approve/Reject | [deadline] |
   
   **⚡ Quick Summary of Urgent Items:**
   1. **[From] - [Subject]**
      - [2-3 sentence gist]
      - Suggested response approach: [brief guidance]
      - Time sensitive: [yes/no, why]
   
   ---
   
   ## 🟠 HIGH PRIORITY - Respond Today ([N] emails)
   
   **Work - External:**
   - [Client Name]: "[Subject]" - [one-liner]
   - [Partner Name]: "[Subject]" - [one-liner]
   
   **Work - Internal:**
   - [Colleague]: "[Subject]" - [one-liner]
   
   **Personal:**
   - [Person]: "[Subject]" - [one-liner]
   
   ---
   
   ## 📊 Inbox Overview
   
   **Unread Breakdown:**
   - 🔴 Urgent: [N]
   - 🟠 High: [N]
   - 🟡 Normal: [N]
   - 🟢 Low/Newsletters: [N]
   
   **Category Distribution:**
   - Work External: [N]% | Work Internal: [N]%
   - Personal: [N]% | Finance: [N]%
   - Notifications: [N]% | Newsletters: [N]%
   
   **Estimated Processing Time:**
   - Urgent items: ~[X] minutes
   - High priority: ~[X] minutes
   - Everything else: ~[X] minutes (can batch)
   
   ---
   
   ## 💡 Recommendations
   
   1. **First Action**: [Most critical email and suggested first step]
   2. **Batch Opportunity**: [Group of similar emails that can be handled together]
   3. **Unsubscribe Candidate**: [Newsletter you haven't opened in 90+ days]
   4. **Time Block Suggestion**: "[X] minutes recommended for email processing now"
   ```

### Phase 2: Deep Dive & Processing (On Demand)

4. **Thread Reading & Summarization**
   
   When user selects an email or thread:
   ```markdown
   # 📧 Thread: "[Subject]"
   **From:** [sender] | **Started:** [date] | **Messages:** [N]
   
   ---
   
   ## 📜 Thread Summary
   
   **Current Status:** Active/Pending/Resolved/Blocked
   
   **Timeline:**
   - **[Date] - [Sender]**: [Opening message summary - 2-3 lines]
   - **[Date] - [Reply Author]**: [Response summary]
   - **[Date] - [Author]**: [Latest message summary]
   
   **✅ Key Decisions/Agreements:**
   - [Decision 1]
   - [Decision 2]
   
   **❓ Open Questions:**
   - [Question]: [Who needs to answer]
   
   **📋 Action Items:**
   | Task | Owner | Due Date | Status |
   |------|-------|----------|--------|
   | [Action] | [Person] | [date] | Pending/Done |
   
   **🎯 Your Next Step (if applicable):**
   - [Clear recommendation what to do]
   - [Suggested response angle/approach]
   - [Any attachments or references to review first]
   ```

5. **Attachment Processing**
   
   For emails with attachments:
   - **PDFs**: Extract text, summarize contents, identify type (invoice, report, contract)
   - **Spreadsheets**: Parse key data, identify structure, flag anomalies
   - **Images**: Describe content, OCR if text-containing
   - **Documents**: Summarize, extract key clauses if contract/legal
   - **Archives**: List contents, extract if small/safe
   
   Output:
   ```markdown
   ## 📎 Attachments: [N] files
   
   | File | Type | Size | Content Summary | Action Needed |
   |------|------|------|-----------------|---------------|
   | Q4_Report.pdf | PDF | 2.3MB | Financial results, revenue up 15% | Review before board meeting |
   | invoice_123.pdf | PDF | 156KB | Invoice #$123 for $4,500 due 2026-05-15 | Pay or forward to finance |
   | data_export.xlsx | XLSX | 89KB | Customer list, 250 rows | Import to CRM? |
   ```

### Phase 3: Drafting & Response Assistance (3-10 minutes)

6. **Smart Reply Composition**
   
   Analyze context and draft response:
   
   **Step A: Intent Recognition**
   - What is the sender asking/requesting?
   - What information do they need?
   - What decision or action is required?
   - What tone is appropriate (match sender's formality)?
   
   **Step B: Response Strategy**
   - Direct answer first (don't bury the lede)
   - Supporting context/evidence
   - Clear next steps if applicable
   - Professional closing
   
   **Step C: Draft Generation**
   
   ```markdown
   # 📝 Draft Reply: RE: [Subject]
   **To:** [recipient] | **Original:** [date]
   
   ---
   
   **Draft Option 1: Professional/Formal**
   Hi [Name],
   
   [Direct answer to their question/request].
   
   [Supporting detail or explanation if needed].
   
   [Next steps or call to action].
   
   Best regards,
   [Your name]
   
   ---
   
   **Draft Option 2: Slightly More Casual** (if relationship allows)
   Hey [Name],
   
   [More conversational version].
   
   Thanks,
   [Your name]
   
   ---
   
   **⚠️ Before Sending - Check:**
   - [ ] Answered all questions in original email?
   - [ ] Appropriate tone for recipient/relationship?
   - [ ] No accidental "reply all" when should be direct?
   - [ ] Attachments included if referenced?
   - [ ] Spelling/grammar verified?
   - [ ] Sensitive info appropriately handled (no passwords, etc.)?
   ```

7. **Common Response Templates**
   
   Auto-draft for frequent scenarios:
   
   **Scenario: Meeting Acceptance**
   ```markdown
   Subject: Re: Invitation: [Meeting Name]
   
   Hi [Organizer],
   
   I'll attend the [meeting name] on [date] at [time].
   
   [Optional: I've added it to my calendar / Looking forward to discussing [topic]]
   
   See you then,
   [Name]
   ```
   
   **Scenario: Request for Information**
   ```markdown
   Hi [Name],
   
   Here's the information you requested regarding [topic]:
   
   [Information in clear format - bullets/numbered list/table]
   
   Let me know if you need any clarification or additional details.
   
   Best,
   [Name]
   ```
   
   **Scenario: Delayed Response**
   ```markdown
   Hi [Name],
   
   Apologies for the delay in getting back to you.
   
   [Response to their original request]
   
   [If applicable: I was [brief reason - optional]]
   
   Thanks for your patience,
   [Name]
   ```

### Phase 4: Analytics & Optimization (Weekly/Monthly)

8. **Email Analytics Dashboard**
   
   ```markdown
   # 📊 Email Analytics - [Period]
   
   ## Volume Stats
   - **Received:** [N] emails/day (avg) | **Sent:** [N] emails/day (avg)
   - **Peak hours:** [time range] (most incoming)
   - **Busiest day:** [day of week]
   
   ## Response Performance
   - **Avg response time:** [X] hours (all emails)
   - **Urgent response time:** [X] hours (target: < 2h)
   - **First-contact resolution:** [X]% (resolved in one exchange)
   - **Response rate:** [X]% (of emails requiring response)
   
   ## Category Trends
   | Category | This Month | Last Month | Change |
   |----------|-----------|------------|--------|
   | Work External | [N] | [N] | [+/-X%] |
   | Newsletters | [N] | [N] | [+/-X%] |
   | ... | ... | ... | ... |
   
   ## Sender Insights
   **Top 5 Senders (by volume):**
   1. [sender] - [N] emails - mostly [category]
   2. [sender] - [N] emails - mostly [category]
   
   **Senders Awaiting Response:**
   - [sender] - waiting [X] days (thread: [subject])
   
   ## Newsletter Audit
   **Subscriptions:** [N] active
   **Unopened (90+ days):** [N] candidates for unsubscribe
   **Recommendations:**
   - [Newsletter 1] - Last opened: [date] - Suggest: Unsubscribe
   - [Newsletter 2] - Last opened: [date] - Suggest: Keep (valuable)
   
   ## Efficiency Opportunities
   - **Time spent on email:** est. [X] hours/week
   - **Potential automation:** [N] emails could have template responses
   - **Filters/rules to create:** [suggestions based on patterns]
   ```

9. **Unsubscribe Workflow**
   
   ```markdown
   # 🗑️ Unsubscribe Recommendations
   
   Based on 90-day open-rate analysis:
   
   ## Candidates for Unsubscribe ([N] newsletters)
   
   | Newsletter | Subscriber Since | Last Opened | Open Rate | Recommendation |
   |------------|------------------|-------------|-----------|----------------|
   | [Name] | [date] | [date] | 0% | 🗑️ Unsubscribe |
   | [Name] | [date] | [date] | 2% | 🗑️ Unsubscribe |
   | [Name] | [date] | [date] | 15% | ✅ Keep (still valuable) |
   
   **Estimated time savings:** [X] minutes/week if all recommended unsubs completed
   
   **Bulk Actions Available:**
   - `/email unsubscribe [newsletter_name]` - Unsubscribe from one
   - `/email unsubscribe-all-candidates` - Unsubscribe all 0% open rate (with confirmation)
   
   **Safe Unsubscribe Practices:**
   - Always use official unsubscribe links (never mark as spam for legitimate newsletters)
   - Verify it's a real unsubscribe (some "unsubscribe" buttons track engagement instead)
   - Keep transactional emails (receipts, shipping, account alerts) even if "unread"
   ```

## Security & Privacy Controls

### 🔒 Safety Features:

**Read-Only Mode (Recommended Initially)**
```yaml
permissions:
  read: true      # Read emails, search, classify
  draft: false    # Cannot compose/send until enabled
  send: false     # Cannot send without explicit approval
  delete: false   # Cannot delete emails
  labels: true    # Can add/remove labels (organizational)
```

**Approval Workflow for Sensitive Actions**
- Draft emails require user review before sending
- Bulk actions (delete many, mass unsubscribe) require confirmation
- External email addresses flagged for verification
- Attachments scanned before opening/processing

**Data Handling**
- Email content processed locally (not sent to external servers)
- Credentials stored securely (env vars, never in logs)
- Automatic redaction of sensitive patterns (SSN, credit card, passwords)
- Option to exclude certain folders/labels from scanning

## Pitfalls & Solutions

### ❌ Common Problems:

**Inbox Overwhelm**
- Problem: Too many emails to process effectively
- Solution: 
  * Strict triage: Only process P0-P1 daily, batch P2-P3 weekly
  * Aggressive unsubscribe: Remove 80% of newsletters
  * Inbox zero technique: Archive everything, search when needed

**Response Procrastination**
- Problem: Important emails sit unanswered
- Solution:
  * 2-minute rule: If reply takes < 2 mins, do it immediately
  * Scheduled email blocks: 2-3 dedicated times per day
  * AI-assisted drafts: Have agent prepare reply, you just edit/send

**Context Switching Cost**
- Problem: Checking email interrupts deep work
- Solution:
  * Batch processing: Check only at scheduled times
  * Separate personal/work accounts if possible
  * Use Mission Control briefing instead of constant checking

**Tone Mismatch**
- Problem: Drafted email doesn't match relationship or situation
- Solution:
  * Analyze previous threads with same person for tone matching
  * Offer multiple draft options (formal/casual/direct)
  * Flag potentially sensitive topics for human review

**Missing Important Emails**
- Problem: Buried in noise or miscategorized
- Solution:
  * Whitelist important senders (always show as high priority)
  * Secondary scan for VIP contacts separate from general triage
  * Periodic "important only" filter review

### ✅ Best Practices:

**The  D's of Email Handling**
1. **Do It**: If < 2 minutes, respond immediately
2. **Delegate**: Forward to appropriate person if not yours
3. **Defer**: Schedule specific time for longer responses
4. **Delete/Archive**: If no value, remove immediately
5. **Draft**: Use AI to compose complex replies efficiently

**Maintain Inbox Health**
- Weekly unsubscribe reviews (keep subscription count < 20)
- Empty spam/promotions folder monthly
- Create filters for recurring senders (auto-label/file)
- Process to zero at least once per week (even if archiving unread)

**Professional Communication Standards**
- Proofread before sending (especially to external parties)
- Use clear subject lines that indicate content/action needed
- One topic per email when possible (easier to reference later)
- Respond within 24 hours even if just to acknowledge receipt

## Verification & Testing

**Initial Setup Validation:**
- [ ] Successfully connected to email provider
- [ ] Can fetch inbox (test with recent emails)
- [ ] Classification categories working correctly
- [ ] Draft composition generates usable outputs
- [ ] Permissions set appropriately (start read-only)

**Ongoing Quality Checks:**
- [ ] Triage accuracy: Are urgent emails properly flagged? (spot check weekly)
- [ ] Draft quality: Do replies require minimal editing? (track acceptance rate)
- [ ] Processing speed: Is triage completing in reasonable time?
- [ ] No false positives in spam detection (important emails marked low priority)

## Integration Notes

**Powerful Combinations:**
- **Mission Control**: Include urgent emails in daily briefing
- **Summarize**: Auto-summarize long threads or newsletters
- **Self-Improving Agent**: Learn your email style, common responses, preferences
- **Calendar**: Extract meeting invites, add to calendar automatically
- **Finance Tracker**: Process invoices, receipts from email attachments

**Workflow Example:**
```
Morning:
  1. Mission Control briefing includes "3 urgent emails"
  2. Email skill provides detailed triage of those 3
  3. User picks one, reads thread summary
  4. Agent drafts reply, user edits and sends
  
During Day:
  5. New urgent email arrives → notification
  6. User asks agent to quickly summarize while in meeting
  
Evening:
  7. Agent processes remaining unread, prepares triage for tomorrow
  8. Weekly analytics updated
```

---

*Based on OpenClaw's top-5 ranked email management skill with enhanced security controls, smart drafting, and comprehensive analytics for Hermes.*
