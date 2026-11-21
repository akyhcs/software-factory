Here is the ASCII representation of the diagram:

```text
                  +----------------------+
                  |       #1 MAP         |
                  |   (Parent Issue)     |
                  +----------------------+
                           |
             +-------------+-------------+
             |             |             |
             v             v             v
       +-----------+ +-----------+ +-----------+
       | #2 Ticket | | #3 Ticket | | #4 Ticket |
       | Decision 1| | Decision 2| | Decision 3|
       +-----------+ +-----------+ +-----------+
             |             |             |
             +-------------+-------------+
                           |
                    All Tickets
                     Resolved
                           |
                           v
                  +----------------+
                  |    /to-spec    |
                  |  Transformation|
                  +----------------+
                           |
                           v
                  +----------------+
                  |    #5 SPEC     |
                  | Final Output   |
                  +----------------+
```

### Simplified representation (as shown in the original image)

```text
+-----------+          /to-spec          +-----------+
|  #1 MAP   | --------------------------> |  #5 SPEC  |
+-----------+                             +-----------+
     |
     +------ [✓] #2 Ticket
     |
     +------ [✓] #3 Ticket
     |
     +------ [✓] #4 Ticket
```

**Remember:**
- #1 Map = Overall planning structure.
- #2, #3, #4 = Individual decision tickets.
- Green checkmarks = Decisions resolved.
- `/to-spec` = Converts the resolved planning information into a specification.
- #5 Spec = Consolidated specification used for the next stage of development.






This diagram illustrates an important transition in the Wayfinder workflow: **converting the decisions made during planning into a formal Software Specification (Spec).**

Let's break it down.

## 1. What are #1, #2, #3 and #4?

These represent the planning phase.

- **#1 Map:** The parent issue containing the overall project plan.
- **#2 Ticket:** First decision.
- **#3 Ticket:** Second decision.
- **#4 Ticket:** Third decision.

The green checkmarks indicate that all these tickets have been resolved.

For example, imagine you're building an AI trading platform.

| Issue | Decision |
|---|---|
| #1 Map | Build AI Trading Platform |
| #2 Ticket | Select programming language |
| #3 Ticket | Design market data architecture |
| #4 Ticket | Design order execution architecture |

At this point, you have resolved all the important decisions.

## 2. What does `/to-spec` mean?

This is the interesting part.

Once all the decision tickets are resolved, you invoke:

```bash
/to-spec
```

This command takes the information gathered during Wayfinder planning and produces a formal specification.

In other words, it consolidates:
- The original project destination.
- Decisions made in individual tickets.
- Technical discussions.
- Requirements.
- Architectural decisions.

It transforms the planning information into a structured specification.

## 3. What is #5 Spec?

This is the output of `/to-spec`.

Think of it as the master document describing what needs to be built.

For our trading platform, the specification might contain:

| Section | Content |
|---|---|
| Requirements | What the application should do |
| Architecture | Overall system architecture |
| Technology Stack | Java, Python, databases, etc. |
| Market Data | How market data is collected |
| Order Execution | How orders are placed |
| Risk Management | How trading risks are controlled |
| API Integration | Broker API integration |
| Error Handling | Failure handling mechanisms |

The specification becomes the reference document for implementation agents.

## 4. Understanding the complete flow

```text
       WAYFINDER
           |
           v
       #1 MAP
           |
           +------ #2 Ticket
           |
           +------ #3 Ticket
           |
           +------ #4 Ticket
           |
           v
    All decisions resolved
           |
           v
        /to-spec
           |
           v
       #5 SPEC
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

### 5. The most important distinction

Don't confuse the Map with the Spec.

| Map | Spec |
|---|---|
| Planning structure | Formal specification |
| Contains decision tickets | Consolidates resolved decisions |
| Tracks unknowns and dependencies | Describes what needs to be built |
| Used during planning | Used as input for implementation |

**My interpretation of the diagram:**

Wayfinder doesn't directly build your software. It first helps you understand the problem, discover unknowns, resolve decisions and produce a detailed specification.

That specification is then handed over to the implementation workflow.

**The fundamental idea is:**

Don't start coding with incomplete requirements. First resolve the decisions, then generate the specification, and finally implement it.
