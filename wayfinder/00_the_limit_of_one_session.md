# The Limits of One Session — Explained from the Beginning

This lesson introduces an important problem in AI-assisted software engineering: **Why can't we use one AI agent session to plan an entire large software project?**

The author explains this problem and introduces a planning technique called **Wayfinder**.

Let's understand the reasoning step by step.

## 1. The bottleneck in software engineering has changed

Traditionally, software development had a straightforward bottleneck:

1. You have an idea.
2. You need to plan the implementation.
3. Developers write the code.
4. You test and deploy it.

The biggest limitation was often the amount of development work that needed to be done.

With AI coding agents, things have changed.

You can give an agent a task, and it can independently:
- Understand the requirement.
- Explore the codebase.
- Write code.
- Run tests.
- Make changes.

Now imagine having multiple agents working simultaneously.

You might have five agents available, but you only have two well-defined tasks to give them.

**Your bottleneck is no longer necessarily implementation. It is planning.**

The author realized that he needed a planning process capable of producing sufficiently large, well-defined chunks of work to keep multiple agents occupied.

## 2. Problems with single-session grilling

Before understanding the problem, we need to understand grilling.

### What is grilling?

Grilling is a technique where you ask an AI agent to interview you about a software feature until both of you understand the design.

For example, imagine you want to build an order management system.

You tell the agent:

"I want to build an order management system."

The agent starts asking questions:

- Who can create orders?
- Can customers modify existing orders?
- What happens when payment fails?
- How do we handle duplicate orders?
- Should we support partial cancellations?
- How should inventory be updated?

You answer these questions, and the agent asks more questions based on your answers.

Eventually, you and the agent develop a shared understanding of the feature.

This process is called **grilling**.

### The problem: One session has limited context.

Every interaction consumes context.

As the conversation grows, the agent has to deal with increasingly large amounts of information.

Eventually, the session enters what the author calls the **Dumb Zone**.

The agent's performance deteriorates as its context fills up.

This creates a problem when planning large software projects.

## 3. What happens at 200,000–300,000 tokens?

Suppose you are using `/grill-with-docs`.

You have spent several hours discussing:
- Requirements.
- Architecture.
- Business rules.
- Edge cases.
- Database design.
- API contracts.
- Integration requirements.

Your session has reached 200,000 tokens, but you still have many decisions to make.

What are your options?

The author discusses two approaches.

### Approach A: Compaction

Compaction reduces the conversation history by creating a smaller summary.

However, there is a problem.

Your original conversation contains:
- Detailed answers.
- Reasoning.
- Decisions.
- Context.
- Important discussions.

When you compact the conversation, the summary might preserve the main decisions but lose some of the reasoning and details.

**The problem is that the original session history is valuable, and compaction can discard rich information.**

Therefore, compaction isn't an ideal solution for very large planning sessions.

### Approach B: Split the work across sessions

Another approach is to create multiple sessions.

For example:

| Session | Responsibility |
|---|---|
| Session 1 | Authentication |
| Session 2 | Order management |
| Session 3 | Payment processing |
| Session 4 | Notification system |

Initially, this looks like a good solution.

But there is another problem.

What happens when Session 1 makes a decision that affects Session 3?

For example:
- Authentication defines the user identity model.
- Order management depends on user identity.
- Payment processing also depends on user identity.

Now you have dependencies between different sessions.

You must transfer context between sessions and maintain consistency.

This creates additional context management and session-switching overhead.

The author also highlights another problem:

**You need to know the shape of the work upfront to decide how many sessions to create.**

But when requirements are still unclear, you don't necessarily know how the work should be divided.

## 4. Prototyping makes the problem worse

Consider the same situation.

You are grilling an agent about a payment processing feature.

During the discussion, you realize that you need to validate whether a particular technical approach will work.

You decide to build a prototype.

Now you have:

1. Your original grilling session.
2. A separate prototyping session.
3. The need to transfer prototype findings back into the original session.

You end up managing multiple contexts.

This becomes difficult because the original planning discussion and the prototype are connected.

The author wants to eliminate this unnecessary session management.

---

# 5. Lessons from Shape Up

This is where the author introduces an important concept.

He refers to a book called *Shape Up* by Ryan Singer.

The book discusses:
- Finding the right shape of work.
- Having discussions at the right level of detail.
- Planning work appropriately.
- Shipping the right thing.

The important concept introduced in this lesson is **Planning Fidelity**.

## 6. Understanding Planning Fidelity

Fidelity means the level of detail or commitment involved in planning something.

The author introduces five levels of planning fidelity.

| Level | Meaning | Example |
|---|---|---|
| 1. Discussion | Talking about the feature | Discussing how authentication should work |
| 2. Breadboarding | Understanding actions using boxes and arrows | User → Login → Validate → Dashboard |
| 3. Wireframes | Understanding the organization of the interface | Designing login and dashboard screens |
| 4. Prototyping | Creating partially working code | Building a basic login prototype |
| 5. Working Code | Actual implementation | Production-ready authentication |

Let's understand each level.

### Level 1: Discussion

This is the lowest level of commitment.

You are discussing an idea without committing to implementation.

Example:

"I want users to authenticate using Google and email/password."

You are still exploring the requirements.

### Level 2: Breadboarding

Here, you start identifying the actions required.

You might represent the workflow using boxes and arrows.

For example:

```text
User
  |
  v
Login
  |
  v
Validate Credentials
  |
  v
Generate Token
  |
  v
Access Dashboard
```

You are now thinking about how the system works.

However, you haven't started implementing it.

### Level 3: Wireframes

Now you start understanding how the feature is organized on the screen.

For example:

- Login page.
- Registration page.
- Forgot password page.
- Dashboard.

You also understand how users navigate between these pages.

### Level 4: Prototyping

Now you build partially working code.

For example:
- Implement Google login.
- Generate a JWT.
- Create a basic login screen.
- Test the authentication flow.

The prototype helps validate whether your proposed approach works.

### Level 5: Working Code

This is the highest fidelity.

You have actual working code that is intended to run in production.

---

## 7. Low Fidelity vs High Fidelity

This distinction is particularly important.

**Low fidelity = Low commitment.**

You are discussing ideas and exploring possibilities without spending significant effort implementing them.

**High fidelity = Higher commitment.**

You are investing more effort in producing something concrete.

Consider this example:

You want to introduce a notification system.

At low fidelity:

"We need notifications when an order is completed."

At higher fidelity:

- Decide which events trigger notifications.
- Define the notification architecture.
- Build a prototype.
- Implement the actual notification service.

The higher you move on the fidelity ladder:
- The more concrete your understanding becomes.
- The more implementation effort is required.
- The more commitment you make to a particular approach.

However, higher fidelity also takes longer.

**The objective is not to always work at the highest fidelity. It is to choose the appropriate fidelity for the problem.**

---

# 8. Agents enable Higher Fidelity

This is one of the most important observations in the lesson.

Before AI coding agents, creating prototypes was relatively expensive.

You needed a developer to:
- Understand the requirement.
- Write code.
- Configure the environment.
- Implement the prototype.
- Test it.

Now agents can produce prototypes much more cheaply.

This changes how we approach planning.

For example:

Suppose you are unsure whether to use synchronous REST communication or asynchronous messaging.

Traditionally, you might spend considerable time discussing the architecture.

With agents, you can potentially ask them to create small prototypes of both approaches.

You can then evaluate the results.

This allows you to move from discussion to prototyping much faster.

However, the author makes an important distinction:

**You should not always jump directly into prototyping.**

Some problems are better solved through discussion.

Others benefit from breadboarding or wireframes.

The objective is to use higher fidelity when it provides value.

---

# 9. Introducing Wayfinder

Now we reach the main concept of this lesson.

The author introduces **Wayfinder**.

Wayfinder is a planning skill that helps you:

1. Plan work of different sizes.
2. Work across multiple sessions.
3. Avoid manually managing all the session context.
4. Choose the appropriate planning fidelity for different parts of the work.

The author specifically designed it to solve two problems:

**Problem 1: Session management**

Instead of struggling with one enormous grilling session or manually splitting everything into separate sessions, Wayfinder supports planning across multiple sessions.

**Problem 2: Planning fidelity**

Instead of discussing everything at the same level of detail, Wayfinder chooses the appropriate fidelity for each part of the plan.

### When is Wayfinder particularly useful?

The author highlights projects involving unknown unknowns.

These are situations where you don't know:
- Exactly how to implement something.
- Whether a particular technical approach will work.
- How different components will integrate.
- What challenges you will encounter during implementation.

Wayfinder helps you navigate these uncertainties.

---

# 10. Complete lesson summary

Here is the entire conceptual flow:

```text
                IDEA
                  |
                  v
          AI Coding Agents
                  |
                  v
       Implementation becomes
          relatively cheap
                  |
                  v
       PLANNING becomes the
          bottleneck
                  |
                  v
       Single-session grilling
                  |
                  v
          Context fills up
                  |
                  v
             DUMB ZONE
                  |
          +-------+-------+
          |               |
          v               v
       Compaction     Multiple Sessions
          |               |
          v               v
     Loss of detail   Context management
                      and dependencies
          |               |
          +-------+-------+
                  |
                  v
         Need a better approach
                  |
                  v
           SHAPE UP IDEAS
                  |
                  v
         Planning Fidelity
                  |
                  v
             WAYFINDER
                  |
                  v
       Multi-session planning
       + Appropriate fidelity
```

## 11. Important concepts to remember

| Concept | What you should understand |
|---|---|
| Session | A conversation with an agent containing its context and history |
| Context | Information available to the agent during a session |
| Grilling | Interviewing the user until both share the same design |
| Compaction | Reducing conversation history into a smaller representation |
| Dumb Zone | The state where an agent's performance deteriorates as context fills |
| Fidelity | The level of detail and commitment in planning |
| Breadboarding | Representing actions and workflows using boxes and arrows |
| Wireframes | Understanding the organization of an interface |
| Prototyping | Creating partially working code |
| Wayfinder | A multi-session planning skill that chooses appropriate fidelity |

## 12. My understanding of the central takeaway

There is a deeper engineering principle behind this lesson:

**Don't force every software engineering problem into a single planning session or a single level of detail.**

Different problems require different levels of exploration.

For example:

- A straightforward CRUD API might only need discussion and breadboarding.
- A complex payment integration might require prototyping.
- A complicated distributed system might require several rounds of discussion, architecture exploration and prototyping.

Wayfinder is designed to manage this planning process across sessions.

The next lessons will explore how to use Wayfinder to plan difficult and large pieces of software work.

---

**One important distinction:** This lesson introduces Wayfinder and explains why it exists. It does not yet teach its internal workflow, commands, configuration, or implementation. Those details belong to the subsequent lessons. Pasted text(4)
