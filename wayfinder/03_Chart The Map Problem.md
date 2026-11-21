# Chart The Map — Complete Explanation

This lesson demonstrates how to use **Wayfinder to plan a large feature before writing any code.**

In the previous lesson, you learned what Wayfinder is, how it manages tasks, and how it identifies the frontier.

Now, this lesson puts those concepts into practice by building a real feature: **In-app notifications for Cadence.**

Let's understand the entire lesson step by step.

# 1. What are we trying to build?

The example application is called Cadence.

It has two types of users:
- **Learners:** Need notifications that encourage them to return to the application.
- **Instructors:** Need notifications when someone comments on their content.

The requirement is to introduce an in-app notification system.

For example:

Imagine a learner is using Cadence.

Someone replies to their comment.

The learner should see something like:

```text
       Cadence
  -----------------------
       🔔 Notifications (1)

  Sarah replied to your comment.

  John commented on your lesson.

  Your instructor published
  a new lesson.

  -----------------------
```

However, there are several unanswered questions:

1. Where should the notification bell appear?
2. What happens when someone clicks it?
3. Which events should generate notifications?
4. Should notifications be stored in our database?
5. Should we use a third-party notification provider?
6. How do we handle read and unread notifications?
7. Should notifications be sent in real time?

The developer doesn't know all these answers yet.

**This uncertainty is precisely why we need Wayfinder.**

Instead of asking an AI agent to immediately implement notifications, we ask it to investigate the requirements and establish a clear destination.

---

# 2. Why are there two paths?

The lesson provides two ways to follow the exercise.

### Path 1: Follow the author's setup

You use exactly the same map and tickets created by the author.

The author has already completed the planning exercise.

You can import his work into your GitHub Issues.

This is useful because you can compare your understanding with an experienced implementation.

### Path 2: Follow your own way

You independently plan the notification feature.

You start a Wayfinder session and answer its questions.

Wayfinder generates:
- Your specification.
- Your map.
- Your implementation tickets.
- Your research tickets.

You can compare your results with the author's solution afterward.

**My recommendation:** Initially follow Path 2 to understand how Wayfinder thinks. Then use Path 1 to compare your map and tickets against the author's.

---

# 3. Start the Wayfinder session

The first instruction is:

```text
/wayfinder
```

This activates the Wayfinder skill inside your coding agent.

After activating it, describe the feature.

For example:

```text
I want to build an in-app notification system for Cadence.

The system should support notifications for both learners
and instructors.

I want a real third-party service to handle notifications.

However, I am not sure:

1. Which events should generate notifications.
2. How notifications should appear in the application.
3. Where the notification interface should be placed.
4. Which third-party notification provider we should use.

I want you to help me discover the requirements,
investigate the unknowns, and create a detailed
specification.

The goal of this session is to define the destination,
not to implement the feature.
```

Notice the final sentence.

**We are asking the agent to create a specification, not implement the feature.**

This distinction is extremely important.

---

# 4. What happens during grilling?

Once the session starts, Wayfinder begins asking questions.

This is called **grilling**.

Think of it as a requirements-discovery interview between you and the AI agent.

For example:

| Agent's question | Possible answer |
|---|---|
| Who receives notifications? | Learners and instructors |
| What events trigger notifications? | Comments, replies, new lessons |
| Should notifications be real-time? | Preferably yes |
| Should users see read/unread status? | Yes |
| Should notifications be persistent? | Yes |
| Should we use a third-party service? | Yes |

But you don't need to know everything.

The lesson explicitly says:

**Saying "I don't know" is absolutely fine.**

Why?

Because the purpose of this exercise is to identify uncertainty rather than hide it.

For example:

You might not know which third-party notification provider is suitable.

Instead of randomly selecting one, Wayfinder can identify this as something that needs research.

This brings us to the next important concept.

---

# 5. What are Research Tickets?

This is one of the most important parts of the lesson.

During grilling, Wayfinder might discover questions that cannot be answered confidently using the existing project information.

For example:

**Question:** Which third-party notification service should we use?

Possible providers:
- OneSignal
- Novu
- Knock
- Courier

You don't necessarily know which one is suitable.

Wayfinder can create a research ticket.

For example:

```text
Research Ticket #1

Title:
Evaluate third-party notification providers.

Objective:
Identify a suitable notification provider
for Cadence.

Investigate:
- API capabilities
- Pricing
- Webhook support
- Real-time delivery
- Integration complexity
- Scalability

Expected outcome:
A recommendation with supporting evidence.
```

Notice something important.

The agent is not immediately implementing OneSignal or Novu.

It first creates a research task to investigate the available options.

### Another example: Web Push

Suppose you want notifications even when the user is not actively viewing Cadence.

This introduces another question:

Can Web Push notifications reach all our intended users?

Wayfinder might create another research ticket:

```text
Research Ticket #2

Title:
Investigate Web Push notification reach.

Investigate:
- Browser support
- Mobile browser support
- Permission requirements
- Delivery limitations
- Platform restrictions

Expected outcome:
Determine whether Web Push is suitable
for Cadence.
```

This is how uncertainty becomes actionable work.

### The important distinction

| Implementation ticket | Research ticket |
|---|---|
| Build something | Investigate something |
| Produces working functionality | Produces knowledge and recommendations |
| Has implementation tasks | Has research questions |
| Example: Build notification bell | Example: Evaluate notification vendors |

**Research tickets reduce uncertainty before implementation begins.**

---

# 6. Read What You Made

After grilling is complete, Wayfinder generates the planning artifacts.

You need to inspect two things.

### A. The Map

The map represents the overall journey toward completing the feature.

It contains the specification and the work required to reach the destination.

### B. Individual Tickets

Tickets break the larger feature into smaller, manageable tasks.

For example:

```text
IN-100: Notification System

       |
       |
       +---- Research notification providers
       |
       +---- Research Web Push support
       |
       +---- Design notification architecture
       |
       +---- Implement notification backend
       |
       +---- Implement notification UI
       |
       +---- Add read/unread functionality
       |
       +---- Add notification tests
```

This is an illustrative structure, not necessarily the exact structure generated by Wayfinder.

The important thing is that you now have a defined destination and a sequence of work.

---

# 7. Understanding the GitHub Commands

The last part introduces the author's seed script.

Let's understand what happens.

### Step 1: Close your existing map

```bash
gh issue close <issue-number>
```

Why?

Because the seed script refuses to run if you already have an open map.

This prevents multiple competing maps from remaining active.

### Step 2: Reset the repository

```bash
npm run reset add-seed-wayfinder-issues
```

This moves your repository to the commit named:

`add-seed-wayfinder-issues`

Remember that the course uses Git commits as checkpoints.

You can move between these checkpoints using:

```bash
npm run reset <commit-slug>
```

### Step 3: Import the author's map and tickets

```bash
npm run seed-wayfinder-issues 01-chart-the-map
```

This loads the author's predefined map and tickets into your GitHub Issues.

You now have his planning artifacts instead of your own.

---

# 8. Understanding the CommitMap

At the beginning of the lesson, you saw six checkpoints.

Let's understand their purpose.

| Commit | What changes |
|---|---|
| `add-wayfinder-skill` | Adds Wayfinder, prototype and research skills |
| `add-notification-terms` | Adds notification-related terminology to GLOSSARY.md |
| `add-seed-wayfinder-issues` | Adds the script for importing the author's map and tickets |
| `research-notification-vendors` | Adds research into notification providers |
| `research-web-push-reach` | Adds research into Web Push reach |
| `remove-research-docs` | Shows the final state after removing temporary research documents |

One interesting detail:

The author deliberately removes the research documents in the final checkpoint.

Why?

Because research documents can be temporary artifacts.

Their findings should ultimately inform the specification and implementation decisions.

You don't necessarily need to retain every intermediate research document permanently.

---

# 9. What is the actual learning objective?

Let's step back.

This lesson isn't primarily about implementing a notification system.

It teaches you a workflow for handling uncertain, large engineering tasks.

```text
       FEATURE REQUEST
              |
              |
              v
       START WAYFINDER
              |
              |
              v
           GRILLING
              |
              |
       Identify unknowns
              |
       +------+------+
       |             |
       v             v
   Known items   Unknown items
       |             |
       |             v
       |       Research Tickets
       |             |
       |             v
       |        Investigation
       |             |
       +------+------+
              |
              v
        FINAL SPEC
              |
              |
              v
         CREATE MAP
              |
              |
              v
       BREAK INTO TICKETS
              |
              |
              v
       IMPLEMENT FEATURE
```

The central idea is:

**Don't start implementing a large feature when you haven't established what you're supposed to build.**

First discover the requirements, investigate unknowns, establish the destination, and then organize the journey into tickets.

---

## 10. Five things you should take away

| Concept | What you should understand |
|---|---|
| Wayfinder session | Starts the planning process |
| Grilling | Discovers requirements and exposes uncertainty |
| Research tickets | Convert unanswered questions into investigation tasks |
| Specification | Defines the destination and expected outcome |
| Map and tickets | Organize the journey toward completing the feature |

### One important distinction to remember

There are three different activities happening here:

1. **Discovery:** What exactly should we build?
2. **Research:** What do we need to investigate before deciding?
3. **Planning:** How do we divide the work into executable tickets?

Wayfinder brings these activities together.

**My suggestion:** Pay particular attention to how the agent decides that something deserves a research ticket instead of making an assumption. That is one of the most valuable engineering behaviours this lesson is trying to teach.
