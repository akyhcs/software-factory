The document explains **Wayfinder**, an AI-assisted planning system designed to help developers plan and execute large software projects using AI agents.

The central idea is:

> Don't ask an AI agent to build a massive feature immediately. First, help it discover all the decisions that need to be made, resolve those decisions systematically, create a specification, and then hand the specification to implementation agents.

Let's understand it from the beginning.

# 1. What is Wayfinder?

Imagine you want to build a large application.

For example, you want to build an **AI-powered trading platform** with:

- Real-time market data.
- Technical indicators.
- Strategy backtesting.
- Trading signals.
- Broker integration.
- Automated order execution.
- Risk management.

You cannot simply tell an AI agent:

"Build this entire application."

There are too many unknowns:
- Which broker API?
- Which database?
- Which programming language?
- How will market data be processed?
- How will orders be executed?
- How will failures be handled?
- How will risk be calculated?

You need to resolve these decisions before implementation.

**Wayfinder helps you discover and resolve these decisions.**

It uses two important data structures:

1. Map
2. Decision Tickets

Pasted text(5)

# 2. Understanding the Map and Tickets

Wayfinder uses GitHub Issues to organize the planning process.

There are two levels.

### A. Map

The map is the parent GitHub issue.

It contains:
- Project destination.
- General notes.
- Decisions made so far.
- Decisions that are still unknown (fog of war).
- Links to decision tickets.

The map is labelled:

`wayfinder:map`

### B. Decision Tickets

Each decision is represented by an individual GitHub issue underneath the map.

For example:

**Parent Map: Build AI Trading Platform**

| Ticket | Decision |
|---|---|
| #1 | Select programming language |
| #2 | Select broker API |
| #3 | Design market data architecture |
| #4 | Design order execution engine |
| #5 | Design risk management |
| #6 | Select database |

Each ticket contains the detailed discussion and decisions associated with that particular topic.

Think of it this way:

- Map = High-level project overview.
- Tickets = Detailed decision-making records.

# 3. What is the Destination?

This is one of the most important concepts.

Before starting, Wayfinder asks:

**What exactly are we trying to achieve?**

For example, our destination could be:

"Create a complete technical specification for an AI-powered trading platform."

Notice something important.

The destination doesn't necessarily have to be a finished application.

It could be:

1. A detailed technical specification.
2. A finished software product.
3. A completed course.
4. Any other large project.

For software engineering, the recommended destination is a **Specification (Spec)**.

Why?

Because once all decisions are resolved, agents can use the specification to implement the application.

The overall workflow is:

```text
       WAYFINDER
           |
           v
      Resolve Tickets
           |
           v
        /to-spec
           |
           v
       Specification
           |
           v
       /to-tickets
           |
           v
       Implementation
           |
           v
       Code Review
```

This is important because Wayfinder handles planning, while the subsequent workflow handles implementation.

# 4. Understanding Fog of War

This is probably the most interesting concept in the entire lesson.

Imagine you're planning the trading platform.

Initially, you identify three decisions:

- Broker integration.
- Market data architecture.
- Database selection.

However, while investigating broker integration, you discover additional questions:

- How do we authenticate?
- How do we handle token expiration?
- Does the broker support WebSockets?
- How do we handle rejected orders?

These are new decisions that weren't visible initially.

This is called **Fog of War**.

It means you cannot see the entire project or all its decisions at the beginning.

As you resolve existing decisions, new decisions become visible.

For example:

```text
Initial Map
    |
    |-- Ticket #1: Broker API
    |
    |-- Ticket #2: Market Data
    |
    |-- Ticket #3: Database
                |
                v
       Investigate Broker API
                |
                v
       Discover New Decisions
                |
                |-- Ticket #4: Authentication
                |
                |-- Ticket #5: WebSocket
                |
                |-- Ticket #6: Order Failures
```

This is why Wayfinder doesn't attempt to generate the entire project plan upfront.

It allows the plan to evolve as knowledge increases.

# 5. Blocking Relationships and Frontier

Now let's understand how Wayfinder determines which tickets can be worked on.

Consider this example:

```text
       Ticket #2
           |
           |
           v
       Ticket #4
           ^
           |
       Ticket #3
```

Ticket #4 depends on both Ticket #2 and Ticket #3.

Therefore:
- Ticket #2 must be completed.
- Ticket #3 must be completed.
- Only then can Ticket #4 be started.

Wayfinder uses GitHub's native issue dependency tracking.

A ticket is considered unblocked when all its blocking tickets are closed.

### What is Frontier?

This is an important definition.

**Frontier = Open + Unblocked + Unclaimed tickets.**

For example:

| Ticket | Status | Frontier? |
|---|---|---|
| #2 | Open, unblocked, unclaimed | Yes |
| #3 | Open, unblocked, claimed | No |
| #4 | Open, blocked | No |

Only Ticket #2 is available to work on.

Why is this useful?

Because multiple agents can work on independent decisions simultaneously without interfering with each other.

# 6. The Four Types of Tickets

This is another critical concept.

Wayfinder provides four ticket types.

| Ticket Type | Purpose | Who handles it? |
|---|---|---|
| Grilling | Resolve questions through discussion | You + Agent |
| Prototype | Build something rough to evaluate | You + Agent |
| Research | Gather external information | Sub-agent |
| Task | Perform manual activities | You |

Let's understand each.

## A. Grilling

Grilling is a structured conversation between you and an AI agent.

For example:

You want to select a database for your trading platform.

The agent might ask:

1. What is your expected transaction volume?
2. Do you need time-series storage?
3. Do you require high availability?
4. What is your expected data retention?
5. Do you need real-time queries?

You answer these questions.

The agent uses your answers to arrive at a decision.

It is essentially a decision-making session.

The lesson mentions `/grill-with-docs` as the standard grilling workflow.

## B. Prototype

Sometimes discussing something isn't enough.

Consider this question:

"Which dashboard layout should we use?"

You can discuss it for hours, but you won't know which design is better until you see it.

This is where prototypes come in.

The agent creates a cheap, rough implementation that you can inspect.

For example:
- Dashboard layout A.
- Dashboard layout B.
- Different chart arrangements.
- Different interaction patterns.

You examine them and decide which direction to follow.

**Use Prototype when the main question is how something should look or behave.**

The corresponding skill is:

`/prototype`

## C. Research

Suppose you want to integrate Dhan's trading API.

You need information about:
- Authentication.
- Rate limits.
- WebSocket support.
- Order types.
- API restrictions.

The agent cannot simply guess these details.

It needs to research external documentation.

This is where Research tickets come in.

The interesting part is that research tickets are handled by sub-agents.

The workflow:

```text
Create Research Ticket
         |
         v
Assign to Sub-agent
         |
         v
Sub-agent researches
         |
         v
Findings attached to ticket
         |
         v
Research ticket resolved
```

You don't need to participate in this process.

The lesson calls this AFK (Away From Keyboard).

## D. Task

Some decisions cannot be made until you perform manual activities.

For example:
- Create a broker developer account.
- Generate API credentials.
- Provision a cloud environment.
- Obtain database access.
- Upload sample market data.

The agent cannot necessarily perform these activities.

Therefore, it creates a checklist for you.

You complete the tasks, and the dependent decision can proceed.

# 7. How Do We Invoke Wayfinder?

There are two ways.

### First: Create a new map

```bash
/wayfinder Build an AI-powered trading platform
```

The agent starts a grilling session with you.

It tries to understand:
- What you're building.
- What the destination is.
- What decisions need to be made.

It then creates the map and associated tickets.

### Second: Work on an existing ticket

```bash
/wayfinder #2
```

This starts a Wayfinder session for ticket #2.

You can resolve that particular decision.

Once completed, the ticket is closed, potentially unblocking other tickets.

---

# 8. Putting Everything Together

Let's look at the complete architecture.



The complete process looks like this:

| Stage | Activity | Result |
|---|---|---|
| 1 | Define destination | Clear project objective |
| 2 | Create map | Parent GitHub issue |
| 3 | Identify decisions | Child tickets |
| 4 | Resolve tickets | Decisions documented |
| 5 | Discover unknowns | New tickets added |
| 6 | Identify frontier | Work that can proceed |
| 7 | Resolve all tickets | Complete decision map |
| 8 | Run `/to-spec` | Detailed specification |
| 9 | Run `/to-tickets` | Implementation tickets |
| 10 | Implement and review | Finished software |

The key idea is that **planning and implementation are separate phases**.

You first establish what needs to be built and why. Then you hand that information to implementation agents.

---

# 9. Five Important Takeaways

I would particularly remember these concepts:

| Concept | What you should remember |
|---|---|
| Map | Parent issue containing the overall project plan |
| Decision Tickets | Individual decisions required to reach the destination |
| Fog of War | Unknown decisions become visible as existing decisions are resolved |
| Frontier | Open, unblocked and unclaimed tickets |
| Four Ticket Types | Grilling, Prototype, Research and Task |

### My interpretation

The most important architectural idea behind Wayfinder is that it treats software planning as a **dynamic dependency graph rather than a static checklist**.

A static checklist assumes you already know everything that needs to be done.

Wayfinder assumes you don't.

As agents investigate, research and discuss individual decisions, the graph evolves.

This makes it particularly useful for large projects where the requirements and technical decisions aren't completely understood at the beginning.

---

# 10. Test Your Understanding

The document has five quiz questions. Let's test whether you understood the concepts.

**Question 1:**

You are building a large software application. You create a parent GitHub issue labelled `wayfinder:map`.

How should the individual decisions be represented?

A. As checklist items inside the parent issue.

B. As separate child GitHub issues under the map.

C. As separate Markdown files.

D. As independent issues without any relationship.

**Question 2:**

You are investigating broker integration. You discover that authentication, WebSocket connectivity and rate limits need separate decisions.

Which Wayfinder concept explains this?

A. Frontier

B. Fog of War

C. Prototype

D. Grilling

Let's start with these two questions.
