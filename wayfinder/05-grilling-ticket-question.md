# Grilling Ticket — Complete Explanation

This lesson is about **making an important product or architectural decision through a structured conversation with an AI agent.**

In the previous lesson, you created a Wayfinder map. That map identified different areas of the project that need investigation or decisions.

Now, you are going to take the first decision ticket and resolve it through a process called **Grilling**.

Let's understand everything step by step.

# 1. First, understand what changed in this lesson

Look at the CommitMap:

```text
Grilling Ticket
│
├── Commit 1: remove-research-docs
│   └── Start the lesson
│       ├── Research documents removed
│       └── Notifications terms added to GLOSSARY.md
│
└── Commit 2: add-discussion-terms
    └── See my solution
        └── Discussions section added to GLOSSARY.md
            ├── Discussion Question
            └── Discussion Reply
```

There are two important changes.

### Commit 1: remove-research-docs

The research documents have been removed.

Why?

Because the project is moving from the research phase towards making concrete decisions.

Instead of keeping research documents as the source of truth, the project is organizing its terminology in `GLOSSARY.md`.

It also introduces terminology related to Notifications.

### Commit 2: add-discussion-terms

The solution introduces a new Discussions section in `GLOSSARY.md`.

It contains two terms:

- **Discussion Question:** A question raised during a grilling session.
- **Discussion Reply:** An answer or response to that question.

These terms help establish a consistent vocabulary for recording decisions.

---

# 2. What exactly is a Grilling Ticket?

This is the most important concept in this lesson.

A ticket is a unit of work.

For example, imagine you are building a notification system for an application called Cadence.

You have several unanswered questions:

- What notifications should the application send?
- Who should receive them?
- When should notifications be triggered?
- Should notifications be sent through email, Slack, or both?
- Should users be able to disable notifications?

These are not necessarily coding tasks.

They are **product and architectural decisions**.

You create a ticket:

```text
Ticket: Which Notifications does Cadence send,
        and to whom?

Label: wayfinder:grilling
```

This is called a Grilling Ticket.

Why grilling?

Because instead of asking the AI agent to immediately implement something, you engage in a structured discussion.

The agent asks questions.

You provide answers.

The agent might challenge your answers or recommend alternatives.

You can accept or overrule its recommendations.

This continues until both sides agree on a decision.

### Important distinction

| Ticket type | Purpose |
|---|---|
| Research ticket | Investigate something and gather information. |
| Grilling ticket | Discuss and resolve an open decision. |
| Implementation ticket | Build something based on an established decision. |

A grilling ticket is primarily about resolving ambiguity, not writing code.

---

# 3. Why do we need a fresh session?

The lesson specifically asks you to execute:

```text
/clear
```

This is not an arbitrary instruction.

Remember the prerequisite about the Smart Zone and Dumb Zone.

An AI agent has a context window. It contains information from previous interactions, instructions, tool outputs, and other conversation history.

When you previously charted the map, the agent accumulated context from that activity.

If you continue using the same session, the agent might:

- Carry forward assumptions from previous discussions.
- Rely on outdated decisions.
- Mix research activities with decision-making.
- Make recommendations influenced by irrelevant conversation history.

The solution is to start fresh.

```text
Previous session
       |
       v
Chart the Map
       |
       v
     /clear
       |
       v
Fresh context
       |
       v
Grilling session
```

**The important principle: Give the agent only the context it needs for the current phase of work.**

Clearing the context does not delete your project files, GitHub issues, or Wayfinder map.

It clears the agent's conversation context.

The agent can still access project information through its configured tools and skills.

---

# 4. What happens when you execute /wayfinder?

After clearing the context, you execute:

```text
/wayfinder <URL-of-your-ticket>
```

For example:

```text
/wayfinder https://github.com/company/cadence/issues/123
```

Here is what happens conceptually:

```text
                YOU
                 |
                 |
              /clear
                 |
                 v
          Fresh AI Session
                 |
                 |
        /wayfinder <ticket URL>
                 |
                 v
          WAYFINDER SKILL
                 |
                 v
         Reads the Ticket
                 |
                 v
       Understands the Decision
                 |
                 v
          Starts Grilling
                 |
                 v
       +---------------------+
       | AI asks a question  |
       +---------------------+
                 |
                 v
          You provide answer
                 |
                 v
       AI evaluates your answer
                 |
                 v
       More questions/recommendations
                 |
                 v
          Decision reached
                 |
                 v
           Close the Ticket
```

Notice something important.

You are explicitly passing the ticket URL.

This gives the agent a specific objective instead of asking it to explore the entire project.

---

# 5. How does the actual Grilling session work?

Let's take the example from this lesson.

**Ticket: Which Notifications does Cadence send, and to whom?**

Imagine the agent starts asking questions.

### Question 1: What notifications should Cadence send?

Agent:

Should Cadence send notifications when:

1. A new task is created?
2. A task is assigned to someone?
3. A task is completed?
4. A task fails?

You:

Yes, but I don't think we need notifications for every task creation.

The agent might ask:

Why should task creation be excluded?

You:

Because creating tasks is a frequent operation. Too many notifications will create notification fatigue.

The agent now understands the reasoning behind your decision.

### Question 2: Who should receive notifications?

Agent:

Should notifications go to:

- The task creator?
- The assigned user?
- The project administrator?
- All project members?

You:

Only the assigned user should receive task assignment notifications. The creator should receive completion notifications.

The agent may recommend additional recipients.

You can accept or reject those recommendations.

### Question 3: What notification channels should we support?

Agent:

Should Cadence support:

- Email?
- Slack?
- In-app notifications?

You:

Let's start with in-app notifications and email. Slack can be considered later.

The discussion continues.

Eventually, you reach an agreed decision.

**Important:** These are illustrative questions, not the actual questions or decisions from the course's solution.

---

# 6. What are Discussion Questions and Discussion Replies?

This is where the second commit becomes relevant.

During grilling, the conversation contains two fundamental elements.

**Discussion Question**

A question that needs to be resolved.

Example:

```text
Should users receive an email when a task
is assigned to them?
```

**Discussion Reply**

The response to that question.

Example:

```text
Yes. Users should receive an email when a task
is assigned to them, provided email notifications
are enabled in their preferences.
```

Conceptually:

```text
GRILLING TICKET
      |
      |
      v
Discussion Question 1
      |
      v
Discussion Reply 1
      |
      v
Discussion Question 2
      |
      v
Discussion Reply 2
      |
      v
Discussion Question 3
      |
      v
Discussion Reply 3
      |
      v
Final Decision
```

The distinction is important because a decision should not simply appear out of nowhere.

There should be a discussion explaining how the decision was reached.

This creates a record of the reasoning behind the decision.

---

# 7. What does it mean to close the ticket?

After the discussion is complete, the ticket should be closed.

But closing a ticket does not simply mean that the conversation has ended.

It means the underlying decision has been resolved.

For example:

```text
Ticket:
Which Notifications does Cadence send,
and to whom?

Status: CLOSED

Decision:
1. Assignment notifications go to assigned users.
2. Completion notifications go to task creators.
3. Email and in-app notifications are supported.
4. Slack notifications are deferred.
```

The actual decisions will depend on your grilling session.

The important point is that the ticket should no longer contain an unresolved question.

---

# 8. Why does the lesson emphasize answering AND overruling the agent?

This is an important aspect of AI-assisted software engineering.

The agent is not necessarily the decision-maker.

It can:

- Identify missing requirements.
- Challenge assumptions.
- Identify edge cases.
- Recommend alternatives.
- Highlight potential problems.

But you remain responsible for the final decision.

For example:

Agent:

I recommend sending notifications to all project members.

You:

I disagree. That could create unnecessary notifications and notification fatigue. Only relevant users should receive them.

The agent should incorporate your decision.

This is why the lesson describes grilling as a conversation where you can answer or overrule until you agree.

**The objective is not to let AI make every decision. It is to use AI to improve the quality of your decisions.**

---

# 9. Complete workflow of this lesson

Here is the entire workflow in one diagram.

```text
          PREVIOUS LESSON
          Chart the Map
                 |
                 v
        Wayfinder Map Created
                 |
                 v
       First Grilling Ticket
                 |
                 v
          GitHub Issue
                 |
                 v
          Copy Ticket URL
                 |
                 v
             /clear
                 |
                 v
          Fresh AI Context
                 |
                 v
       /wayfinder <ticket URL>
                 |
                 v
        Agent Reads the Ticket
                 |
                 v
         Grilling Begins
                 |
                 v
       +---------------------+
       | Discussion Question |
       +---------------------+
                 |
                 v
       +---------------------+
       | Discussion Reply    |
       +---------------------+
                 |
                 v
          More Discussion
                 |
                 v
       Decision Agreed Upon
                 |
                 v
         Ticket Closed
                 |
                 v
           Verification
          in GitHub
```

# 10. What are you actually learning from this lesson?

There are four important engineering principles here.

| Principle | What you learn |
|---|---|
| Context management | Start a fresh session when moving between phases. |
| Structured decision-making | Resolve ambiguity through grilling instead of immediately coding. |
| Human-in-the-loop | AI recommends, challenges and investigates, but humans retain decision authority. |
| Decision traceability | Questions, replies and final decisions provide a record of why something was decided. |

### My biggest takeaway

Think about a traditional software development workflow:

```text
Requirement
    |
    v
Developer interprets requirement
    |
    v
Developer makes assumptions
    |
    v
Developer writes code
    |
    v
Potential rework
```

The Wayfinder approach introduces an additional step:

```text
Requirement
    |
    v
Wayfinder identifies open decisions
    |
    v
Grilling session
    |
    v
Decisions explicitly resolved
    |
    v
Implementation
```

This reduces the chances of building something based on incorrect assumptions.

**The central idea of this lesson: Before asking an AI agent to build something, make sure the important decisions about what to build have actually been resolved.**
