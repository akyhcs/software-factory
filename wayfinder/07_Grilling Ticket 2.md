# Grilling Ticket 2 — Complete Explanation

This lesson continues the **Wayfinder / AI Coding Crash Course**. In the previous lessons, we created a map of work using GitHub Issues. Now we are going to make an important product and technical decision: **Which notification vendor should we use?**

Let's understand the entire lesson, why we are doing it, and what the agent is expected to do.

---

## 1. First, understand the current situation

We are building a product called **Cadence**.

One of its requirements is to send notifications to users.

For example:
- A user receives a notification when something important happens.
- The user might receive notifications even when they are not actively using the application.
- We need to support web push notifications without necessarily building a native mobile application.

To implement this functionality, we need a notification service.

Instead of building the entire notification infrastructure ourselves, we want to use a hosted notification vendor.

Examples of notification providers include:
- OneSignal
- Knock
- Novu
- Courier

The question is: **Which one should we choose?**

We shouldn't simply ask the AI agent which vendor it prefers. We need research to support the decision.

---

## 2. Understand the CommitMap

```xml
<CommitMap packageManager="npm">
  <Commit id="add-discussion-terms">
    Start the lesson: the Discussions terms in GLOSSARY.md.
    This lesson only changes GitHub issues, not code
  </Commit>
</CommitMap>
```

This is important.

The CommitMap tells us what changed between checkpoints.

Here:

| Attribute | Meaning |
|---|---|
| Commit ID | add-discussion-terms |
| Starting state | Discussion terminology exists in GLOSSARY.md |
| Changes | GitHub issues |
| Code changes | None |

Notice the sentence:

**This lesson only changes GitHub issues, not code.**

Why?

Because we are still in the decision-making stage.

We haven't selected a vendor yet, so there is no reason to implement anything.

Our job is to discuss, evaluate and finalize the vendor.

---

## 3. Understand the current GitHub Issues map

Before starting this lesson, we have already completed two research tickets.

Let's visualize the workflow.

```text
                 CADENCE PROJECT
                       |
                       |
              Research Phase
                       |
             +---------+---------+
             |                   |
             v                   v
     Research Ticket 1    Research Ticket 2
     Notification vendor   Web Push limitations
             |                   |
             v                   v
          CLOSED              CLOSED
             |                   |
             +---------+---------+
                       |
                       v
              Grilling Ticket
                       |
                       v
          Pick the notification vendor
                       |
                       v
               GRILLING SESSION
                       |
                       v
              Evaluate research
                       |
                       v
              Select ONE vendor
                       |
                       v
                 CLOSE TICKET
                       |
                       v
             Next implementation work
```

There are three important concepts here.

**Research tickets:** Gather information.

**Grilling tickets:** Discuss the available information, challenge assumptions and make decisions.

**Implementation tickets:** Build the functionality based on finalized decisions.

This separation is extremely important in AI-assisted development.

---

## 4. Why do we have two research tickets?

Let's understand the two blockers.

### Research Ticket 1: Which hosted notification vendor fits Cadence?

The purpose is to compare notification vendors.

For example:

| Criteria | OneSignal | Knock | Novu |
|---|---|---|---|
| Web push | Evaluate | Evaluate | Evaluate |
| Email notifications | Evaluate | Evaluate | Evaluate |
| Hosted infrastructure | Evaluate | Evaluate | Evaluate |
| API support | Evaluate | Evaluate | Evaluate |
| Pricing | Evaluate | Evaluate | Evaluate |
| Integration complexity | Evaluate | Evaluate | Evaluate |

We want the agent to investigate:

1. What notification channels does each vendor support?
2. Does it support web push?
3. Does it provide APIs and SDKs?
4. Does it support event-driven notifications?
5. What are the pricing models?
6. How difficult is integration?
7. Does it fit Cadence's requirements?

The objective is to produce evidence, not immediately choose a vendor.

### Research Ticket 2: How far can web push reach without a native app?

This is a different research question.

Suppose we select a notification provider that supports web push.

Does that mean every user can receive notifications?

No.

There are browser and operating-system limitations.

For example:

- Does Chrome support web push?
- Does Firefox support web push?
- Does Safari support web push?
- Can users receive notifications when the browser is closed?
- Do mobile browsers support web push?
- Are there restrictions on iOS?
- Does the user need to grant notification permission?

We need to understand these limitations before selecting a vendor.

**Why is this research important?**

Imagine selecting a notification vendor because it supports web push, but later discovering that our target users cannot receive notifications on their devices under our intended usage model.

That would be a poor architectural decision.

---

## 5. What does blocked mean here?

This is one of the most important concepts from the previous lessons.

The grilling ticket depends on the two research tickets.

```text
                  Research 1
                      |
                      |
                  Research 2
                      |
                      |
                      v
             Pick Notification Vendor
                      |
                      |
                      v
              Vendor Decision
```

The two research tickets are called **blockers**.

A ticket cannot be properly resolved until its dependencies are completed.

In our current situation:

| Ticket | Status |
|---|---|
| Research notification vendors | Closed |
| Research web push limitations | Closed |
| Pick notification vendor | Open and unblocked |

Because both research tickets are closed, we can now make the decision.

---

# 6. What exactly is Grilling?

This is the central concept of this lesson.

Grilling means conducting a structured discussion with the AI agent to challenge its assumptions and arrive at a decision.

It is not simply asking:

"Which notification vendor is best?"

Instead, we should have a conversation that explores the decision from different perspectives.

For example:

**Question 1:**

Which vendors did you evaluate, and what evidence supports each option?

**Question 2:**

Which vendor provides the best web push support without requiring a native mobile application?

**Question 3:**

Are there any browser or operating-system limitations that could affect our users?

**Question 4:**

What are the trade-offs between OneSignal, Knock and Novu?

**Question 5:**

What happens if we choose a vendor that doesn't support one of our future requirements?

**Question 6:**

Considering our research, which vendor should we select?

Notice the progression:

```text
       ASK QUESTIONS
             |
             v
      REVIEW RESEARCH
             |
             v
      CHALLENGE CLAIMS
             |
             v
      DISCUSS TRADE-OFFS
             |
             v
      EVALUATE OPTIONS
             |
             v
      MAKE DECISION
```

The purpose is to avoid blindly accepting the agent's first recommendation.

---

## 7. Why does the lesson say "single hosted vendor"?

Look at the actual ticket:

```md
## Question

Given the research, which single hosted vendor do we commit to?
We'll sign up for this one only and prototype against it.
```

There are three important requirements here.

**1. Single hosted vendor**

We must select one provider.

We shouldn't integrate three different vendors and decide later.

**2. Commit to the decision**

Once we finish the grilling session, we record the selected vendor as the project's decision.

**3. Prototype against it**

We will create an initial implementation to validate whether the selected vendor actually works for our requirements.

This is important because research alone cannot prove that an integration will work in practice.

For example:

```text
Research
   |
   v
Select OneSignal
   |
   v
Create Account
   |
   v
Integrate API / SDK
   |
   v
Prototype Web Push
   |
   v
Validate Functionality
```

The prototype helps validate our assumptions before investing in the complete implementation.

---

# 8. What are we actually changing in this lesson?

This is worth understanding carefully.

We are not writing JavaScript, TypeScript or backend code.

We are changing GitHub Issues.

The workflow is:

1. Find the open ticket: Pick the notification vendor.
2. Read its question.
3. Read both closed research tickets.
4. Start a grilling session with the AI agent.
5. Discuss the available vendors.
6. Finalize one vendor.
7. Record the decision in the ticket.
8. Close the ticket.

The result is a closed decision ticket.

This allows future implementation tickets to depend on a finalized decision.

---

## 9. Understanding the two npm commands

The lesson provides two commands.

### Command 1: Start of the lesson

```bash
npm run seed-wayfinder-issues 02-grilling-ticket
```

This command prepares your GitHub Issues map for the beginning of this lesson.

It creates the following state:

```text
Research Ticket 1       CLOSED
Research Ticket 2       CLOSED
First Grilling Ticket   CLOSED
Pick Notification       OPEN
```

You can use this if you want to follow the instructor's exact workflow.

### Command 2: End of the lesson

```bash
npm run seed-wayfinder-issues 03-grilling-ticket-2
```

This takes you directly to the checkpoint representing the end of the lesson.

You can use it if you want to skip the actual grilling exercise.

**Important:** These are checkpoint-seeding commands. They are not commands that implement notification functionality.

---

# 10. The bigger picture: Where does this fit in AI-assisted software engineering?

This lesson demonstrates an important engineering principle.

An AI agent should not directly jump from a requirement to implementation.

We should establish a decision-making process.

Consider this complete workflow:

```text
         PRODUCT REQUIREMENT
                 |
                 v
          RESEARCH PHASE
                 |
                 v
        COLLECT TECHNICAL DATA
                 |
                 v
          RESEARCH TICKETS
                 |
                 v
         GRILLING SESSION
                 |
                 v
        CHALLENGE ASSUMPTIONS
                 |
                 v
         FINALIZE DECISION
                 |
                 v
          CLOSE THE TICKET
                 |
                 v
        CREATE IMPLEMENTATION
                 |
                 v
           WRITE CODE
                 |
                 v
          TEST PROTOTYPE
                 |
                 v
          REVIEW RESULTS
```

The key distinction:

| Stage | Main responsibility |
|---|---|
| Research | Gather evidence |
| Grilling | Challenge assumptions |
| Decision | Commit to one option |
| Implementation | Build the solution |
| Testing | Validate the solution |

### My biggest takeaway from this lesson

**We are separating research, decision-making and implementation into different stages.**

This is particularly important when working with AI coding agents.

Without this separation, an agent might:
- Select a technology without sufficient research.
- Make assumptions about browser compatibility.
- Start implementing before architectural decisions are finalized.
- Introduce unnecessary complexity.
- Change its technical direction halfway through development.

By using GitHub Issues as a map, we create a structured process where every major implementation decision has supporting research and an explicit resolution.

---

## 11. What should you actually learn from this lesson?

Remember these five concepts:

1. **Research tickets:** Gather technical information before making decisions.
2. **Blockers:** Ensure dependent work is completed before proceeding.
3. **Grilling:** Challenge the AI agent's recommendations instead of blindly accepting them.
4. **Decision tickets:** Record and finalize architectural or product decisions.
5. **Prototyping:** Validate the selected decision before full implementation.

**One important distinction:** The lesson doesn't tell us which notification vendor to select. It asks us to conduct the grilling session using the research results and arrive at that decision. We shouldn't assume OneSignal, Knock or Novu is the final choice without examining the actual research ticket resolutions.
