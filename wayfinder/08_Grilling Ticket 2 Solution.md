# Grilling Ticket 2 Solution — Complete Explanation

This lesson is an important continuation of **Grilling Ticket 1**. The focus is not on writing code. Instead, we are learning how an AI agent can help us make architectural and product decisions, record those decisions, and update our project plan.

Let's understand the entire workflow.

## 1. What is happening in this lesson?

First, understand the context.

We are building an application called **Cadence**, which needs a notification system.

We need to select a notification vendor that can handle things such as:

- Email notifications.
- Push notifications.
- In-app notifications.
- Notification feeds.
- Notification delivery and tracking.

Some potential vendors are:

| Vendor | Purpose |
|---|---|
| Knock | Notification infrastructure |
| Courier | Notification infrastructure |
| MagicBell | Notification infrastructure |

The team has already done some research, and now we need to decide which vendor to use.

However, instead of immediately selecting a vendor, the agent starts a grilling session.

**What is grilling?**

Grilling is a structured conversation in which the agent asks questions to clarify decisions, assumptions, constraints, and dependencies before proceeding.

---

# 2. Understanding the starting point

The lesson starts with:

```bash
npm run seed-wayfinder-issues 02-grilling-ticket
```

What does this command do?

It creates the GitHub issues corresponding to a particular checkpoint in the project.

Think of it as loading a predefined project state.

The starting state contains:

- Research tickets completed.
- First grilling ticket completed.
- Pick the notification vendor ticket open.
- No implementation work happening.

The target checkpoint is:

`03-grilling-ticket-2`

**Important:** We are modifying GitHub issues, not application source code.

This is an important concept in AI-assisted software engineering.

The agent is managing the project's planning and decision-making layer.

---

# 3. Starting the Wayfinder session

The developer executes:

```text
/clear
```

Then:

```text
/wayfinder #<ticket-number>
```

Let's understand these commands.

### `/clear`

Clears the previous agent conversation.

Why?

Because we want a fresh session without unnecessary conversational context from the previous task.

This helps establish a clean working context.

### `/wayfinder #<ticket-number>`

This instructs the agent to start working on a specific GitHub issue using the Wayfinder workflow.

For example:

```text
/wayfinder #42
```

The agent reads the issue and starts asking questions.

Notice something important:

The agent isn't immediately implementing anything.

It first tries to understand the decision.

---

# 4. Question 1 — Should we commit to Knock?

The agent asks whether we should commit to Knock as our notification vendor.

The developer responds:

> We can really only commit to Knock once we've seen a prototype.

This is an extremely important engineering principle.

### Why shouldn't we commit immediately?

Because research and actual implementation experience are different.

Suppose we researched Knock and discovered:

- It supports email.
- It supports push notifications.
- It provides APIs.
- It supports notification workflows.

Everything looks good on paper.

But when we actually integrate it with Cadence, we might discover:

- Its API doesn't support something we need.
- Its notification feed doesn't fit our requirements.
- Its integration is complicated.
- Its pricing becomes expensive.
- Its developer experience isn't satisfactory.

Therefore, committing before prototyping would be premature.

### What decision are we making?

We are making a distinction between two things:

| Decision | Meaning |
|---|---|
| Initial selection | Let's try Knock |
| Final commitment | We will use Knock permanently |

The developer agrees to the first but not the second.

This is a form of **reversible decision-making**.

We keep our options open until we have sufficient evidence.

**My takeaway:** Don't confuse selecting a candidate with making a permanent architectural commitment.

---

# 5. Question 2 — Extra sign-ups

This is where the lesson introduces an interesting concept.

The agent explains that selecting a notification vendor might require additional external services.

For example:

### A. Firebase Cloud Messaging (FCM)

Knock uses Firebase Cloud Messaging to deliver web push notifications.

What is FCM?

Firebase Cloud Messaging is a Google service that allows applications to send notifications to devices and browsers.

Consider this architecture:

```text
             Cadence Application
                     |
                     |
                   Knock
                     |
                     |
          Firebase Cloud Messaging
                     |
              ----------------
              |              |
           Browser         Mobile
           Notification    Notification
```

Knock manages notification workflows, while FCM provides the underlying push delivery infrastructure.

To use this functionality, we may need:

1. A Firebase project.
2. Firebase configuration.
3. Appropriate credentials.
4. Integration with Knock.

This means choosing Knock introduces another external dependency.

### B. Email provider

The agent also explains that Knock, Courier and MagicBell require an email provider to be connected for production email delivery.

For example:

```text
           Cadence
              |
              |
       Notification Vendor
              |
              |
         Email Provider
              |
              |
         User's Inbox
```

The notification vendor manages notification orchestration, while the email provider handles actual email delivery.

### What decision does the developer make?

The developer says:

> Let's put off any service sign-up until we actually know it's required.

This is another important principle.

**Don't introduce external dependencies before you need them.**

Why?

- Additional configuration.
- Credential management.
- More infrastructure.
- More operational complexity.
- Potential costs.

We should first understand what the prototype requires.

---

# 6. Question 3 — The pricing cliff

The agent asks whether the $250/month pricing cliff is acceptable.

What does pricing cliff mean?

Imagine a pricing structure like this:

| Monthly usage | Cost |
|---|---:|
| Lower usage | Free |
| Moderate usage | $50 |
| Higher usage | $250 |

Suppose the vendor has a usage threshold.

Once you cross that threshold, your monthly cost increases significantly.

That sudden increase is called a pricing cliff.

The developer accepts the pricing.

Why is this important?

Because pricing is an architectural constraint too.

Imagine selecting a service that works perfectly technically but becomes unaffordable when the application grows.

That creates a problem later.

Therefore, the agent explicitly asks about the acceptable pricing before moving forward.

---

# 7. Feeling the fog — Why is this important?

This section contains an interesting observation.

The developer says he feels the fog whenever he runs Wayfinder.

Why?

Because every decision exposes additional unknowns.

Initially, the developer thought:

"We just need to select a notification vendor."

But now he has discovered:

- Knock requires additional configuration.
- Web push requires Firebase.
- Email requires an external provider.
- Pricing has limitations.
- Vendor suitability needs to be tested.

This is what happens during software architecture and planning.

You start with a relatively simple requirement.

As you investigate it, you discover more dependencies and constraints.

The important thing is that **the agent is helping expose unknowns before implementation**.

That reduces the likelihood of expensive surprises later.

---

# 8. Question 4 — What does the prototype have to prove?

This is probably the most important question in the entire lesson.

The agent suggests creating two separate prototype tickets.

Why two?

Because there are actually two different questions we need to answer.

### Prototype 1: Notification feed UI

We already have a ticket that investigates how the notification feed should look inside Cadence.

This focuses on the user interface.

For example:

```text
+-----------------------------------+
|          Cadence                  |
+-----------------------------------+
| Notifications                     |
|                                   |
|  New message received             |
|  Your report is ready             |
|  Task assigned to you              |
|                                   |
+-----------------------------------+
```

Questions this prototype answers:

- How should notifications appear?
- How should unread notifications look?
- How should users mark notifications as read?
- How should notification history work?

This is primarily a UI/UX investigation.

### Prototype 2: Does Knock hold up against Cadence?

This is a different investigation.

We want to establish whether Knock actually satisfies our application's requirements.

Questions include:

- Can Knock integrate with Cadence?
- Can we implement the required notification workflows?
- Can we retrieve notification history?
- Can we manage read/unread states?
- Can we support our expected user experience?
- Are there any API or integration limitations?

This is a vendor feasibility investigation.

### Why separate these tickets?

Because they have different objectives.

| Prototype | Objective |
|---|---|
| Notification feed prototype | Validate UI/UX |
| Knock prototype | Validate vendor suitability |

This is a good example of breaking a large, ambiguous task into smaller, independently verifiable tasks.

---

# 9. What are worktrees, and why does he mention them?

The developer says:

> We can even prototype both at the same time in different worktrees.

Let's understand this.

A Git worktree allows you to check out multiple branches of the same Git repository into separate directories.

For example:

```text
                  Git Repository
                        |
             ------------------------
             |                      |
             |                      |
       Worktree 1              Worktree 2
             |                      |
       UI Prototype           Knock Prototype
             |                      |
       Notification UI        Vendor Integration
```

Both prototypes can be developed independently.

Example:

```bash
git worktree add ../cadence-ui prototype/notification-ui

git worktree add ../cadence-knock prototype/knock-integration
```

Now we have two separate working directories.

Benefits:

1. Independent development.
2. Less interference between tasks.
3. Easier parallelization.
4. Independent testing.
5. Easier comparison of results.

This becomes especially useful when working with multiple AI coding agents.

One agent can work on the notification UI while another investigates Knock integration.

---

# 10. Question 5 — What is the fallback?

The agent asks:

What happens if Knock fails the prototype?

The developer responds:

> Courier sounds fine as a fallback.

This introduces the concept of a fallback vendor.

Our decision now looks like this:

```text
             Notification Vendor
                      |
                      |
                    Knock
                      |
                Prototype Test
                      |
              -----------------
              |               |
            PASS             FAIL
              |               |
         Use Knock          Courier
```

Why is this important?

Because we aren't blindly committing to one vendor.

We have a primary candidate and an alternative.

This reduces the risk of getting stuck if our preferred vendor doesn't work.

Notice that Courier is not being implemented immediately.

It is simply being identified as the fallback.

---

# 11. Question 6 — What waits for the prototype?

This question introduces one of the most important concepts in Wayfinder:

**Dependencies and blocking relationships.**

The agent explains that the prototype is currently blocked by:

1. Signing up for the notification vendor.
2. Provisioning development keys.

Let's understand this.

Suppose we want to integrate Knock.

Before we can build and test the integration, we need:

- A Knock account.
- Development API credentials.
- Appropriate configuration.

Without these, the prototype cannot proceed.

Therefore:

```text
       Sign up for Knock
                |
                |
                v
       Provision Dev Keys
                |
                |
                v
       Knock Integration Prototype
                |
                |
                v
       Validate Knock Against Cadence
```

These are dependency relationships.

A task cannot proceed until its prerequisites are satisfied.

The developer accepts the agent's recommendation.

---

# 12. Understanding the extended map

Now comes the most important outcome of this lesson.

The agent updates the GitHub project map.

Previously, we had:

```text
Research
   |
   v
Pick Notification Vendor
```

Now we have additional decisions and tickets.

Conceptually:

```text
                 Research
                    |
                    v
          Pick Notification Vendor
                    |
                    v
              Grilling Session
                    |
          -----------------------
          |                     |
          v                     v
    Feed Prototype        Knock Prototype
                                |
                         Blocked By
                                |
                    ----------------------
                    |                    |
                    v                    v
               Sign Up              Provision
               for Knock             Dev Keys
```

The map now represents more than just a list of tasks.

It represents:

- Decisions.
- Tasks.
- Dependencies.
- Blockers.
- Future work.
- Unresolved questions.

This is what makes Wayfinder interesting.

**The map evolves as we learn more about the system.**

We don't need to know everything at the beginning.

We progressively discover the work and update the map.

---

# 13. What is the actual significance of this lesson?

Let's consolidate the engineering concepts.

| Concept | What we learned |
|---|---|
| Reversible decisions | Don't permanently commit before validating |
| Prototyping | Test assumptions before full implementation |
| External dependencies | Identify additional services required by vendors |
| Cost constraints | Understand pricing before committing |
| Task decomposition | Break large problems into smaller tickets |
| Parallel development | Use separate worktrees for independent prototypes |
| Fallback strategy | Keep an alternative vendor ready |
| Dependency management | Explicitly record what blocks a task |
| Evolving project map | Update the plan as new information emerges |

## 14. How does this relate to AI-assisted software engineering?

Consider traditional software development.

A developer might:

1. Research notification vendors.
2. Select Knock.
3. Start implementing.
4. Discover Firebase requirements.
5. Discover email provider requirements.
6. Discover API limitations.
7. Abandon the implementation.

A lot of time could be wasted.

With Wayfinder, the workflow becomes:

```text
       Define Requirement
               |
               v
          Research
               |
               v
         Grilling Session
               |
               v
       Clarify Decisions
               |
               v
       Identify Unknowns
               |
               v
       Create Prototype
               |
               v
       Validate Assumptions
               |
               v
       Make Final Decision
               |
               v
       Begin Implementation
```

The AI agent is not simply generating code.

It is helping manage the engineering process.

---

## 15. My three biggest takeaways

**1. Don't make irreversible decisions with incomplete information.**

Research gives us an initial understanding. Prototypes give us practical evidence.

**2. A project plan should evolve.**

You don't need to predict every task at the beginning. As you discover dependencies, add them to the map.

**3. Tickets should represent independently understandable pieces of work.**

The notification UI and vendor integration are different problems. Keeping them separate makes development, testing, and parallel execution easier.

### One concept I particularly want you to understand

The most important distinction in this lesson is:

**A task is not the same as a decision.**

For example:

- Decision: Should we use Knock?
- Task: Build a Knock integration prototype.
- Dependency: Create a Knock account.
- Dependency: Provision development keys.
- Decision after prototype: Commit to Knock or switch to Courier.

These are different entities in the engineering workflow.

Once you understand this distinction, the Wayfinder map becomes much easier to understand.

**In short:** Grilling Ticket 2 teaches us how an AI agent converts an uncertain architectural decision into concrete, testable work with explicit dependencies and fallback options.
