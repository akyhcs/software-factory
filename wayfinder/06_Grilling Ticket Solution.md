# Grilling Ticket Solution – Complete Explanation

This lesson demonstrates what happens **after the agent has completed its initial research and prepared a Grilling Ticket**.

The objective is to establish a shared understanding of the notification requirements before implementing anything.

The important idea is:

**We are not asking the agent to write code. We are asking it to challenge our requirements, identify ambiguities, recommend decisions, and document the final agreement.**

Let's break it down.

---

# 1. Where are we in the overall workflow?

In the previous lessons, we created a Wayfinder map and prepared a Grilling Ticket.

The ticket is about:

**Which notifications should Cadence send, and to whom?**

The agent has already performed its initial research. Now we tell it:

```text
Keep going
```

This instructs the agent to resume its workflow.

### What does the agent do?

The agent:

1. Claims the Grilling Ticket.
2. Reads `GLOSSARY.md`.
3. Retrieves all related issues.
4. Understands dependencies between those issues.
5. Examines the existing codebase.
6. Identifies relevant domain concepts.
7. Prepares questions for the grilling session.

Let's visualize this.

### ASCII diagram – Agent's preparation

```text
              Developer
                  |
                  |
             Keep going
                  |
                  v
          Wayfinder Agent
                  |
                  v
          Claim Grilling Ticket
                  |
                  v
           Read GLOSSARY.md
                  |
                  v
          Retrieve Related Issues
                  |
                  v
         Analyze Dependencies
                  |
                  v
          Inspect Existing Code
                  |
                  v
          Identify Domain Concepts
                  |
                  v
          Prepare Grilling Questions
                  |
                  v
          START GRILLING
```

**Important:** The agent is gathering context before asking questions. It isn't blindly generating questions without understanding the application.

---

# 2. What exactly is Grilling?

Grilling is a requirements-discovery process.

Imagine you tell an AI agent:

"We need a notification system."

That sounds straightforward, but there are many unanswered questions:

- What events trigger notifications?
- Who receives notifications?
- Should notifications be enabled by default?
- Can users customize notification preferences?
- Should administrators receive notifications?
- What happens when an entity is edited or deleted?

If the agent starts implementing immediately, it will make assumptions.

Grilling prevents this.

The process looks like this:

```text
       Initial Requirement
               |
               v
       Agent asks Question
               |
               v
       Agent provides Recommendation
               |
               v
       Developer evaluates it
               |
          +----+----+
          |         |
         Agree    Override
          |         |
          +----+----+
               |
               v
        Record Decision
               |
               v
       Next Grilling Question
```

The agent is not simply interviewing you.

It is bringing its understanding of the codebase and domain model into the conversation.

---

# 3. Round 1 – Ten Questions

The agent asks ten questions.

Let's understand the important decisions.

## Question 1: What counts as a notification?

The first question establishes the boundaries of the notification domain.

For example, consider these events:

| Event | Notification? |
|---|---|
| Student enrolls in a course | Yes |
| Student requests a refund | Yes |
| Instructor receives a discussion reply | Yes |
| New lesson gets added | Depends on requirements |
| Student completes a quiz | Depends on requirements |

Why is this important?

Because without defining what counts as a notification, the implementation scope is unclear.

The developer agrees with the agent's recommendation.

**Concept: Domain boundary**

We are defining which events belong to the notification domain.

## Question 2: Refund request outcomes

Consider this scenario:

A student requests a refund.

There are multiple possible outcomes:

```text
             Refund Request
                   |
          +--------+--------+
          |        |        |
          v        v        v
        Pending Approved Rejected
                   |
                   v
             Refund Processed
```

The question is:

Should notifications be sent for these different outcomes?

The developer agrees with the recommendation.

The important distinction is that submitting a refund request and approving a refund are two different business events.

They shouldn't automatically be treated as the same notification.

## Question 3: Refund requests and platform admins

This question demonstrates the importance of domain modeling.

Suppose a student requests a refund.

Who should know about it?

- The student?
- The instructor?
- The platform administrator?

The agent makes a recommendation based on the existing domain relationships.

The developer agrees.

### Why does domain modeling matter here?

Instead of treating notifications as isolated messages, we understand the relationships between business entities.

For example:

```text
             Platform
                |
                |
          Platform Admin
                |
                | manages
                |
              Course
                |
                | has
                |
             Instructor
                |
                | teaches
                |
              Student
                |
                | submits
                |
           Refund Request
```

These relationships help determine who should receive notifications.

This is much better than randomly assigning recipients.

## Question 4: Refund payouts

This is one of the most important observations in this lesson.

The glossary mentions refund payouts.

However, refund payouts do not exist in the running application.

This creates a difference between:

1. What the documentation describes.
2. What the actual application implements.

The developer recommends putting refund payouts under **Not Yet Specified**.

### Why?

Because documentation alone doesn't prove that functionality exists.

Consider:

```text
             Domain Glossary
                   |
          +--------+--------+
          |                 |
          v                 v
   Existing Concepts   Future Concepts
          |                 |
          v                 v
    Implemented       Not Yet Specified
```

The agent must distinguish between existing functionality and proposed functionality.

**Key takeaway:** Never assume that something documented in a glossary is already implemented in the codebase.

## Question 5: Claimed seats and team admins

This question concerns enrollment capacity and team administration.

The agent recommends how notifications should work when seats are claimed and who should receive them.

The developer agrees.

The important concept is recipient identification.

A notification system needs to understand not only what happened but also who is responsible for that particular resource.

## Question 6: Notification on every new enrollment

This is where the developer overrides the agent.

The agent asks whether instructors should receive notifications for every new enrollment.

The developer wants this to be customizable per instructor.

Why?

Different instructors have different preferences.

For example:

- Instructor A wants to know about every enrollment.
- Instructor B receives hundreds of enrollments and doesn't want constant notifications.

Therefore, the requirement becomes:

**Instructors should be able to configure whether they receive notifications for new enrollments.**

This introduces an important domain concept:

`Notification Preferences`

An example:

```text
         Instructor
              |
              v
      Notification Preferences
              |
         +----+----+
         |         |
         v         v
      Enabled   Disabled
         |         |
         v         v
     Send alert  Skip alert
```

Notice something important.

The agent proposed a recommendation, but the developer understood the business context better.

This is exactly how human-agent collaboration should work.

## Question 7: Questions and replies

The agent recommends adding two concepts to the glossary:

- Question
- Reply

However, the developer identifies a naming conflict.

The application already has quiz questions.

If we introduce a generic `Question` concept, it could become ambiguous.

For example:

```text
Question
   |
   +---- Quiz Question
   |
   +---- Discussion Question
```

Similarly:

```text
Reply
   |
   +---- Discussion Reply
```

Therefore, the developer instructs the agent to use:

- `Discussion Question`
- `Discussion Reply`

This is a domain terminology decision.

### Why is this important?

Imagine a developer reading:

```java
Question question;
```

Is this a quiz question or a discussion question?

The name doesn't communicate enough information.

A more explicit name reduces ambiguity.

**Key takeaway:** Domain terminology should be precise, consistent, and unambiguous.

## Question 8: Ratings and instructors

The agent recommends how ratings should relate to instructors and notifications.

The developer agrees.

The important point is that notifications should respect the actual business relationships.

## Question 9: New lesson notification for enrolled students

Should students receive notifications whenever an instructor adds a new lesson?

The developer agrees that enrolled students should not automatically receive these notifications.

This is an example of avoiding unnecessary notifications.

A notification system should not notify users about every possible event.

It should notify them about meaningful events according to the agreed requirements.

## Question 10: Notifications for platform administrators

The final question concerns platform administrators.

The agent recommends which administrative events should generate notifications.

The developer agrees.

This completes Round 1.

---

# 4. Glossary Updates

After the first round, the agent updates `GLOSSARY.md`.

Two new terms are introduced:

```text
Discussions

- Discussion Question
- Discussion Reply
```

Why update the glossary?

Because the glossary is the shared vocabulary of the project.

It ensures that the developer, agent, and other contributors use the same terminology.

Think of it this way:

```text
           GLOSSARY.md
                |
       +--------+--------+
       |                 |
       v                 v
Discussion Question  Discussion Reply
       |                 |
       v                 v
  Ask something      Respond to it
```

The glossary is not just documentation.

It is part of the project's shared domain understanding.

---

# 5. Round 2 – Five Follow-up Questions

After the first ten questions, the agent asks five additional questions.

These questions refine the decisions already made.

Let's understand them.

| Question | Topic | What it establishes |
|---|---|---|
| 11 | Enrollment notification default | Whether notifications are enabled by default |
| 12 | Which enrollments count? | Exact conditions for triggering notifications |
| 13 | Who hears about a discussion reply? | Notification recipients |
| 14 | Editing/deleting discussions | Whether modifications trigger notifications |
| 15 | Discussion questions from admins/instructors | Whether these users can participate and trigger notifications |

### Why is a second round necessary?

Because the first round establishes high-level requirements.

The second round removes remaining ambiguities.

For example:

**Round 1:**

Should instructors receive notifications for new enrollments?

Answer: Make it customizable.

**Round 2:**

Should the notification be enabled or disabled by default?

This is a more specific question.

The distinction is:

```text
Round 1
   |
   v
Establish Business Rules
   |
   v
Round 2
   |
   v
Clarify Edge Cases and Defaults
   |
   v
Complete Requirements
```

This prevents the agent from making implementation decisions based on incomplete information.

---

# 6. Shared Understanding

After completing all 15 questions, the agent produces a complete catalogue of decisions.

This is particularly important.

Why?

Because a conversation alone is not a reliable specification.

The agent needs to convert the discussion into something persistent.

For example:

```text
             Grilling Session
                    |
                    v
              15 Decisions
                    |
                    v
          Consolidated Decision List
                    |
                    v
             Ticket Resolution
                    |
                    v
              Close Ticket
```

The agent posts the complete decision catalogue as a resolution comment on the GitHub issue.

The developer confirms:

```text
Yes, that's it, go ahead and close it out
```

The ticket is then closed.

---

# 7. What does staying inside the Smart Zone mean?

This is an important concept from the course.

The developer deliberately avoids converting the grilling session into an implementation session.

Let's understand the difference.

### Grilling session

```text
Requirement
    |
    v
Explore ambiguities
    |
    v
Discuss alternatives
    |
    v
Make decisions
    |
    v
Document decisions
    |
    v
Close Grilling Ticket
```

### Implementation session

```text
Approved Requirements
        |
        v
Design Solution
        |
        v
Modify Code
        |
        v
Write Tests
        |
        v
Review Changes
        |
        v
Create PR
```

These are two different activities.

The purpose of grilling is to establish what needs to be built.

The purpose of implementation is to build it.

**Smart Zone means keeping the agent focused on the current task instead of allowing the workflow to expand unnecessarily.**

The developer can finish the requirements discussion, save the decisions, and move to the next ticket.

---

# 8. What happens on the Wayfinder Map?

After closing the ticket, the developer checks GitHub and returns to the Wayfinder map.

The map now reflects the updated status.

```text
                Notification System
                       |
                       |
                Parent Issue
                       |
          +------------+-------------+
          |                          |
          v                          v
    Grilling Ticket            Grilling Ticket
    Notifications              Pick Notification
    (CLOSED)                    Vendor (NEXT)
          |
          v
     Decisions Recorded
          |
          v
    Which notifications
    does Cadence send
    and to whom?
```

The parent issue reflects the completed work.

The next ticket is:

**Pick the notification vendor.**

Notice that the developer hasn't implemented the notification system yet.

They have only established the notification requirements.

The next grilling ticket will explore which notification vendor should be selected.

---

# 9. The Complete Workflow – From Grilling to Implementation

Let's combine everything into one diagram.

```text
                  WAYFINDER MAP
                        |
                        v
                Grilling Ticket
                        |
                        v
                 Agent Claims Ticket
                        |
                        v
                Research Codebase
                        |
                        v
                Read GLOSSARY.md
                        |
                        v
               Prepare Questions
                        |
                        v
                ROUND 1: 10 Questions
                        |
                        v
               Developer Decisions
                        |
                 +------+------+
                 |             |
                 v             v
               Agree        Override
                 |             |
                 +------+------+
                        |
                        v
                 Update Glossary
                        |
                        v
                ROUND 2: 5 Questions
                        |
                        v
               Refine Requirements
                        |
                        v
              Consolidate Decisions
                        |
                        v
                Update GitHub Issue
                        |
                        v
                 Close Ticket
                        |
                        v
                Update Wayfinder Map
                        |
                        v
               Next Grilling Ticket
                        |
                        v
              Pick Notification Vendor
```

---

# 10. Important Concepts to Remember

Here are the main engineering concepts demonstrated in this lesson.

| Concept | Meaning |
|---|---|
| Grilling | Structured requirements discovery through questions |
| Domain modeling | Understanding business entities and their relationships |
| Domain glossary | Maintaining consistent terminology across the application |
| Existing vs future functionality | Separating implemented concepts from proposed concepts |
| Developer override | Human authority to reject or modify agent recommendations |
| Notification preferences | Making notification behavior configurable |
| Follow-up questions | Resolving remaining ambiguities |
| Shared understanding | Ensuring both developer and agent agree on requirements |
| Ticket resolution | Persisting the final decisions in GitHub |
| Smart Zone | Keeping the agent focused on the current workflow |

## 11. My biggest takeaway from this lesson

I would highlight three things.

**1. The agent should understand before implementing.**

The agent first examines the codebase, glossary, and related issues. It uses that context to ask meaningful questions.

**2. The human remains responsible for business decisions.**

Question 6 and Question 7 demonstrate this particularly well. The developer overrides the agent when business context or domain terminology requires it.

**3. Decisions must become persistent artifacts.**

The final decisions are recorded in:
- `GLOSSARY.md`
- GitHub ticket resolution
- Wayfinder map

This means the next agent or developer doesn't have to reconstruct the entire conversation.

**The fundamental philosophy: Don't rush from an idea to code. First establish what the system should actually do, document that understanding, and only then move towards implementation.**
