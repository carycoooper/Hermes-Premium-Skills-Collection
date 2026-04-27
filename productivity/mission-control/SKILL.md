---
name: mission-control
description: Daily productivity command center that aggregates calendar events, task lists, overdue items, and contextual priorities into structured briefings. Supports scheduled delivery, on-demand briefings, end-of-day summaries, and multi-project routing.
version: 2.0.0
author: Hermes Skills Team
license: MIT
platforms: [macos, linux, windows]
metadata:
  hermes:
    tags: [productivity, calendar, tasks, scheduling, daily-planning, briefings]
    category: productivity
    related_skills: [self-improving-agent, capability-evolver, summarize]
    config:
      - key: briefing_time
        description: "Default time for daily briefing delivery"
        default: "07:00"
        prompt: "What time should your daily briefing arrive?"
      - key: calendar_sources
        description: "Calendar integrations (google-calendar, apple-calendar, outlook)"
        default: "[]"
        prompt: "Which calendars should Mission Control monitor?"
      - key: task_sources
        description: "Task management integrations (things3, todoist, notion, linear)"
        default: "[]"
        prompt: "Which task managers should Mission Control connect to?"
required_environment_variables: []
---

# Mission Control Skill

Your personal productivity command center that delivers actionable daily intelligence so you start every day prepared.

## When to Use

**Automatic Triggers:**
- Scheduled morning briefing (default: 7:00 AM on weekdays)
- Configurable additional schedules (end-of-day wrap-up, Friday weekly digest)

**Manual Commands:**
- `/mission-control` or `/briefing` - Get immediate on-demand briefing
- `/mission-control today` - Focus on today only
- `/mission-control week` - Weekly overview
- `/mission-control configure` - Set up or modify configuration
- `/mission-control history` - View past briefings

**Natural Language Triggers:**
- "What's on my agenda today?"
- "Give me a morning briefing"
- "What should I be working on right now?"
- "What happened yesterday?"
- "What's my schedule look like this week?"

## Quick Reference

| Briefing Type | When | Contents | Duration |
|---------------|------|----------|----------|
| Morning Briefing | Daily AM | Calendar, tasks, priorities, weather | 60 sec read |
| Pre-Meeting Prep | On demand | Next 2h context, materials needed | 30 sec read |
| End-of-Day Wrap | PM (optional) | Completed, carrying forward, tomorrow preview | 45 sec read |
| Weekly Digest | Friday PM | Week progress, goals status, next week preview | 2 min read |

## Procedure

### Setup Phase (One-Time Configuration)

1. **Connect Data Sources**
   
   Supported integrations:
   ```yaml
   Calendars:
     - Google Calendar (via gcal CLI or API)
     - Apple Calendar (local macOS)
     - Outlook/Exchange (via OWA or CLI)
     - CalDAV (generic, supports Nextcloud, Fastmail)
   
   Task Managers:
     - Things 3 (macOS)
     - Todoist (API)
     - Notion (API/database queries)
     - Linear (API)
     - Microsoft To Do (Graph API)
     - Todo.txt (local file)
     - Any .ics/.ical feed
   ```

2. **Configure Preferences**
   ```yaml
   mission_control:
     scheduling:
       morning_briefing:
         enabled: true
         time: "07:00"
         days: [monday, tuesday, wednesday, thursday, friday]
         channels: [terminal, telegram, slack] # optional
     
       end_of_day:
         enabled: false  # opt-in
         time: "18:00"
       
       weekly_digest:
         enabled: true
         day: friday
         time: "16:00"
     
     content_preferences:
       include_weather: true
       location: "user's city"  # for weather
       priority_threshold: "high"  # what to highlight
       task_limit: 10  # max tasks to show
       calendar_lookahead_days: 3
       show_overdue: true
       show_upcoming_deadlines: true
       proactive_suggestions: true  # AI-generated tips
   ```

3. **Test Integration**
   - Run test fetch from each connected source
   - Verify permissions and data access
   - Confirm briefing renders correctly
   - Adjust formatting preferences

### Morning Briefing Generation (Daily Automated Process)

4. **Data Aggregation** (runs at configured time)
   
   Fetch from all sources in parallel:
   
   **Calendar Events (Today + Tomorrow)**
   - Meetings with times, durations, locations
   - All-day events (deadlines, reminders)
   - Recurring events instance for today
   - Conflict detection (overlapping meetings)
   
   **Task Status**
   - Overdue tasks (past due date, not completed)
   - Due today (high urgency)
   - Due this week (medium urgency)
   - In-progress items (active work streams)
   - Recently completed (yesterday/today for context)
   
   **Contextual Intelligence**
   - Yesterday's unfinished work (carry-forward items)
   - Upcoming deadlines (next 7 days, especially < 48h away)
   - Project milestones approaching
   - Waiting-on items (blocking dependencies)
   
   **Environmental Factors** (optional)
   - Weather forecast (if location configured)
   - Important dates (tax day, holidays, personal events from calendar)
   - Commute/traffic alerts (if relevant)

5. **Priority Calculation**
   
   Score each item using weighted algorithm:
   ```
   Priority_Score = (
     Urgency_Weight × Time_Urgency +     # due soon = higher
     Importance_Weight × Staked_Value +   # high impact = higher  
     Dependency_Weight × Blocking_Factor + # blocks others = higher
     Recency_Weight × Freshness +        # newer tasks slightly favored
     Personal_Weight × User_Preference   # learned preferences
   ) / Normalization_Factor
   ```

   Output ranking: P0 (Critical), P1 (High), P2 (Medium), P3 (Low)

6. **Generate Briefing Document**
   
   ```markdown
   # ☀️ Morning Briefing - [Day], [Month DD, YYYY]
   **Generated:** [time] | **Next Briefing:** [tomorrow's time]

   ---
   
   ## ⏰ Today's Schedule
   
   ### 📅 Calendar - [N] Events
   
   | Time | Event | Location | Duration | Type |
   |------|-------|----------|----------|------|
   | 09:00 | Team Standup | Zoom | 30m | Recurring |
   | 11:00 | Client Call | Conference Room B | 1h | One-time |
   | 14:00 | [Event] | [location] | [duration] | [type] |
   
   ⚠️ **Conflict Alert:** [if overlapping meetings detected]
   
   ### 🎯 Top Priorities - Today (Ranked)
   
   **🔴 P0 - Must Complete Today**
   1. **[Task Name]** 
      - Due: [time] | Project: [name]
      - Est. effort: [time] | Blocker: [if any]
   
   **🟡 P1 - High Priority**
   2. **[Task Name]**
      - Due: [time/date] | ...
   
   **🟢 P2 - Should Address**
   3. **[Task Name]**
      - ...
   
   ---
   
   ## ⚠️ Attention Required
   
   ### 🚨 Overdue Items - [N] Tasks
   - **[Task]** - Overdue by [X] days (Originally due: [date])
     *Suggestion:* [AI recommendation]
   
   ### ⏳ Upcoming Deadlines (Next 48 Hours)
   - **[Task/Milestone]** - Due: [date/time] ([X] hours from now)
   
   ### 🔄 Carried Forward from Yesterday
   - [Item 1] - [status update]
   - [Item 2] - [status update]
   
   ---
   
   ## 💡 Proactive Suggestions
   
   Based on your current workload and patterns:
   
   1. **Time Management Tip**
      - You have [N] hours of meetings today. Consider:
        * [Suggestion based on gap analysis]
        * [Alternative scheduling idea]
   
   2. **Priority Recommendation**
      - [Task A] is due soon but [Task B] has higher stakeholder visibility.
        Recommend completing [B] first if possible, or communicate timeline to [stakeholder].
   
   3. **Prep Reminder**
      - Before your [Time] [Meeting]:
        * Review: [document/link]
        * Prepare: [talking points/question]
        * Bring: [physical items if needed]
   
   ---
   
   ## 📊 Yesterday's Wrap-Up (Quick Recap)
   
   ✅ **Completed:** [N] tasks
   - [Major accomplishment 1]
   - [Major accomplishment 2]
   
   📝 **In Progress:** [N] items carried forward
   
   ---
   
   ## 🌤️ Environment (Optional)
   - **Weather:** [condition], [high/low]°F, [precipitation chance]%
   - **Commute Note:** [traffic alert, transit issue, etc.]
   
   ---
   
   *Briefing generated by Mission Control v2.0*
   *Data sources: [list connected services]*
   *Last sync: [timestamp]*
   ```

### On-Demand Briefing Variations

7. **Pre-Meeting Briefing** (`/mission-control prep [timeframe]`)
   
   Focused on immediate context:
   ```markdown
   ## 🎯 Pre-[Meeting Name] Briefing
   
   **Meeting In:** [X] minutes | **Duration:** [Y] min | **With:** [attendees]
   
   **📋 Agenda Preview:**
   1. [Item 1] - [your role/action needed]
   2. [Item 2] - [prep required]
   
   **📚 Materials to Review:**
   - [Document 1] - [link] - [why it matters]
   - [Document 2] - [link] - [key points to know]
   
   **💬 Talking Points / Questions:**
   - [Point you want to make]
   - [Question to ask]
   
   **⏱️ Post-Meeting:**
   - [Next action/item due]
   ```

8. **End-of-Day Wrap-Up** (if enabled)
   
   ```markdown
   # 🌙 End of Day - [Date]
   
   ## ✅ Accomplishments Today
   - [Completed task 1]
   - [Completed task 2]
   - [Meeting completed: [name] - outcome summary]
   
   ## 📊 Progress Metrics
   - Tasks completed: [N]/[M] planned ([Z]%)
   - Meetings attended: [N] ([total hours])
   - Deep work blocks: [N] hours
   
   ## 📋 Carrying Forward
   - [Incomplete task 1] - [reason/difficulty] - Rescheduled to: [new date]
   - [Incomplete task 2] - ...
   
   ## 🎯 Tomorrow's Preview
   - [Highest priority item]
   - [Upcoming deadline]
   - [Early meeting]
   
   ## 🧠 Reflection Prompt
   - What went well today?
   - What would you do differently?
   - [Personalized based on self-improvement patterns]
   ```

9. **Weekly Digest** (Friday afternoon)
   
   ```markdown
   # 📈 Weekly Digest - Week of [Date Range]
   
   ## 🎯 Goals Progress
   
   | Goal | Target | Actual | Status | Notes |
   |------|--------|--------|--------|-------|
   | [Goal 1] | [metric] | [achieved] | ✅ On Track | ... |
   | [Goal 2] | [metric] | [achieved] | ⚠️ At Risk | ... |
   
   ## 📊 This Week By The Numbers
   - Total tasks completed: [N]
   - Meetings: [N] ([hours] total)
   - Productivity score: [calculated metric]
   - Top productive day: [day name]
   
   ## 🏆 Highlights
   1. [Biggest win this week]
   2. [Second win]
   3. [Third win]
   
   ## 📚 Learnings & Improvements
   - [Process improvement discovered]
   - [Efficiency gain implemented]
   - [Insight about work patterns]
   
   ## 🔮 Next Week Preview
   - [Major deadline/event]
   - [Key objective]
   - [Area needing attention]
   
   ## 🎯 Weekly Goals (Suggested)
   Based on current trajectory and upcoming commitments:
   1. [Goal 1]
   2. [Goal 2]
   3. [Goal 3]
   ```

## Configuration Commands

### Basic Setup Wizard
```
/mission-control setup
→ Interactive guided configuration
→ Connects calendars/tasks step-by-step
→ Tests connections
→ Sets schedule
→ Sends test briefing
```

### Advanced Customization
```yaml
# Advanced config options (edit ~/.hermes/config/mission-control.yaml)

briefing_sections:
  - name: calendar
    enabled: true
    order: 1
    show: [today, tomorrow]
    
  - name: tasks
    enabled: true
    order: 2
    group_by: [priority, project, due_date]
    limit: 15
    
  - name: overdue
    enabled: true
    order: 3
    highlight_color: red
    
  - name: suggestions
    enabled: true
    order: 4
    ai_insights: true
    max_suggestions: 3
    
  - name: weather
    enabled: false  # opt-in
    order: 5
    
  - name: reflection
    enabled: true
    trigger: end_of_day_only

notification_channels:
  terminal:
    enabled: true
    format: rich  # plain/rich/markdown
    
  # Optional: Configure messaging platforms
  # telegram:
  #   enabled: false
  #   bot_token: "${TELEGRAM_BOT_TOKEN}"
  #   chat_id: "YOUR_CHAT_ID"
    
  # slack:
  #   enabled: false
  #   webhook_url: "${SLACK_WEBHOOK_URL}"
  #   channel: "#personal-briefings"

output_formatting:
  use_emojis: true
  compact_mode: false  # true for minimal version
  include_metadata: true
  language: auto  # or en, zh, etc.
```

## Pitfalls & Troubleshooting

### ❌ Common Issues:

**Data Source Connection Failures**
- Symptom: Briefing shows "No calendar/task data"
- Causes: API token expired, permission denied, service offline
- Fixes:
  * Check token validity: `/mission-control test-connections`
  * Verify OAuth scopes granted
  * Review service status pages
  * Enable fallback: Use local files (.ics, todo.txt) when APIs fail

**Information Overload**
- Symptom: Briefing is too long, takes > 2 minutes to read
- Causes: Too many tasks/events, low priority threshold
- Fixes:
  * Increase priority threshold (show only P0-P1)
  * Enable compact mode
  * Reduce task_limit in config
  * Use progressive disclosure: brief first, expandable sections

**Irrelevant Suggestions**
- Symptom: AI suggestions don't match your context
- Causes: Insufficient learning history, generic recommendations
- Fixes:
  * Give feedback: "That suggestion wasn't helpful because..."
  * Allow 1-2 weeks of usage for pattern learning
  * Manually tune suggestion types in config

**Schedule Missed**
- Symptom: Morning briefing didn't arrive
- Causes: Hermes not running, system asleep, timezone mismatch
- Fixes:
  * Ensure Hermes autostart or always-on device
  * Configure catch-up: "Send missed briefing on next launch"
  * Verify timezone setting matches your location

**Privacy Concerns**
- Symptom: Uncomfortable with amount of data aggregated
- Mitigations:
  * Disable specific data sources individually
  * Opt out of AI suggestions (use rule-based only)
  * Review what's included before enabling new sources
  * Data stays local unless explicitly synced to cloud

### ✅ Best Practices:

**Start Simple**
- Begin with just 1 calendar + 1 task manager
- Get comfortable with basic briefing format
- Gradually add sources and features

**Customize Iteratively**
- After 1 week of usage, adjust based on what you actually use
- Disable sections you consistently skip
- Tune priority thresholds to match your workflow

**Review Weekly**
- Spend 2 minutes Friday reviewing weekly digest accuracy
- Adjust configurations based on what was helpful vs noise
- Provide feedback on suggestions to improve AI quality

**Use Proactively**
- Don't wait for scheduled briefing—use on-demand anytime
- Especially useful: pre-meeting prep, context switching between projects
- Combine with other skills: "Summarize my unread emails, then give me a briefing"

## Verification Checklist

Validate Mission Control is working correctly:

**Setup Verification:**
- [ ] All intended data sources connected successfully
- [ ] Test briefing received in expected format
- [ ] Schedule configured correctly (check timezone!)
- [ ] Notification channel working (if configured)

**Daily Health Check:**
- [ ] Morning briefing arrived on time
- [ ] Data appears current (synced within last hour)
- [ ] No connection errors in logs
- [ ] Priorities seem reasonable and accurate

**Weekly Review:**
- [ ] Weekly digest received (if enabled)
- [ ] Suggestion quality improving over time
- [ ] No significant data gaps or missing items
- [ ] Configuration still matches current workflow

## Integration Notes

**Synergistic Skills:**
- **Self-Improving Agent**: Learns which briefing sections you use, optimizes defaults
- **Capability Evolver**: Optimizes timing, content density, suggestion relevance over time
- **Summarize**: Can pre-process long meeting notes before including in briefing
- **Deep Research**: Can provide context for upcoming meetings/projects

**Data Flow:**
```
External Sources (Calendars, Tasks)
        ↓
   Mission Control (Aggregation + Analysis)
        ↓
   Briefing Generation (Template Rendering)
        ↓
Output Channels (Terminal, Messaging Apps)
```

---

*Based on OpenClaw's flagship productivity skill with enhanced multi-platform support, AI-powered suggestions, and comprehensive customization for Hermes.*
