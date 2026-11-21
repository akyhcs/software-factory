# Chart The Map – Complete Explanation

This lesson demonstrates how to use the **Wayfinder skill to convert an unclear software requirement into a structured engineering roadmap**.

The example project is building an in-app notification system for an application called Cadence.

The most important thing to understand is that the agent does not immediately start writing code. Instead, it:
1. Understands the requirement.
2. Asks questions to clarify the destination.
3. Identifies unknowns.
4. Creates research and implementation tickets.
5. Delegates research to background agents.
6. Updates the project map with research findings.
7. Prepares the project for the next stage of planning.

Let's understand the entire process.

---

# 1. What is the objective of this lesson?

The developer wants to implement notifications inside Cadence.

The initial requirement is:

> I want to build in-app notifications inside Cadence. I want a real third-party service to handle this. I want to figure out what kind of notifications I want to send. We'll need to figure out what it actually looks like in the application.

This looks like a relatively simple requirement.

But think about the engineering questions hidden inside it.

- What types of notifications should we send?
- Who should receive notifications?
- Should notifications appear inside the application?
- Should we send emails?
- Should we support browser push notifications?
- Which third-party service should we use?
- Should we build our own notification infrastructure?
- How should users control notification preferences?
- How should notifications appear in the UI?

There are many unanswered questions.

**The problem is that we know what we want to build, but we don't yet know exactly what building it involves.**

This is where Wayfinder comes in.

# 2. What is Wayfinder?

Wayfinder is a skill that helps an AI coding agent navigate from an initial requirement to a well-defined destination.

Think of it as a planning and discovery mechanism.

Instead of asking an AI:

"Build a notification system."

We ask it to help us figure out:

- What exactly needs to be built?
- What decisions must be made?
- What needs investigation?
- What needs prototyping?
- What work can be delegated?

The Wayfinder skill coordinates this process.

### Understanding the initial workflow

```text
                 Developer
                     |
                     |
          Initial Requirement
                     |
                     v
              /wayfinder
                     |
                     v
          Load Relevant Skills
                     |
          +----------+----------+
          |          |          |
          v          v          v
       Grilling  Domain      Wayfinder
                 Modeling
                     |
                     v
            Ask Clarifying Questions
                     |
                     v
             Establish Destination
                     |
                     v
              Create Project Map
                     |
                     v
             Create GitHub Issues
                     |
                     v
             Delegate Research
                     |
                     v
              Update Project Map
```

Notice something important:

Wayfinder doesn't immediately produce implementation code.

It first establishes what success should look like.

The document mentions that the agent loads three relevant skills:

- `wayfinder`
- `grilling`
- `domain-modeling`

The lesson describes the interaction as essentially a `/grill-with-docs` session with additional wayfinding.

Let's understand why.

---

# 3. Round One of Grilling

Grilling means asking questions until the requirements become sufficiently clear.

It is not random questioning.

The agent asks questions that influence the architecture, scope and implementation.

### Question 1: What does done look like?

The agent asks whether the expected outcome is:

1. A specification.
2. A working implementation.
3. A set of decisions.

The developer chooses:

**A specification.**

Why is this important?

Because the destination determines the amount of work required.

Consider these three possibilities:

| Destination | Expected outcome |
|---|---|
| Specification | Document requirements, architecture and decisions |
| Working build | Implement and test the actual feature |
| Decisions | Resolve open architectural and product questions |

If the destination is a specification, the agent doesn't need to implement the complete notification system.

It needs to gather enough information to create a reliable specification.

This prevents unnecessary work.

### Question 2: What counts as in-app notifications?

The developer clarifies:

- Definitely an in-app notification feed.
- Push notifications are also desirable.
- Email could be explored.

There is an important distinction here.

An in-app feed and an out-of-app notification are different delivery channels.

For example:

**In-app notification:**

A student logs into Cadence and sees:

"You have a new lesson available."

**Email notification:**

The student receives an email about the new lesson.

**Browser push notification:**

The student receives a browser notification even when Cadence isn't currently open.

The developer also clarifies that Cadence does not have a native mobile application.

That introduces another question:

Can we support push notifications without a mobile application?

This question becomes a research requirement later.

### Question 3: Is the vendor already decided?

The original requirement says:

"I want a real third-party service."

But it doesn't specify which vendor.

The agent asks whether vendor selection should be part of the planning process.

The developer says:

- Use a hosted third-party service.
- Do not self-host the notification infrastructure.
- Vendor selection should be part of the map.

This is an important architectural decision.

There are two broad approaches:

| Approach | Meaning |
|---|---|
| Self-hosted | We operate and maintain the notification infrastructure |
| Hosted service | A third-party provider manages the notification infrastructure |

The developer prefers the second approach.

### Question 4: Production-grade or prototype?

Cadence currently uses a development UI user switcher instead of real authentication.

The agent asks whether the specification should target production-grade functionality.

The developer confirms that it should.

This matters because authentication and authorization cannot be treated as optional.

For example:

If user A logs into Cadence, user A should only see user A's notifications.

User B must not be able to access them by changing an identifier in an API request.

Therefore, notification authentication becomes an important vendor-selection criterion.

### What have we achieved in Round One?

We now know:

| Area | Decision |
|---|---|
| Project | In-app notifications |
| Primary channel | In-app feed |
| Additional channels | Push and possibly email |
| Vendor | Hosted third-party provider |
| Hosting | No self-hosting |
| Quality | Production-grade |
| Final deliverable | Specification |

But several questions remain unanswered.

That's why we need Round Two.

---

# 4. Round Two of Grilling

Round Two focuses on more specific product and engineering decisions.

### Question 1: How should we decide additional notification channels?

The in-app feed is already confirmed.

But what about push notifications and email?

There are three possible approaches:

1. Decide everything immediately.
2. Research the available options.
3. Include all three channels in the specification without further investigation.

The developer agrees with the recommendation to create a research ticket.

Why?

Because web push capabilities depend on browser support, device capabilities and notification infrastructure.

We should investigate before committing to an architecture.

This demonstrates an important principle:

**An AI agent should recognize when it doesn't have enough information and create an investigation task instead of inventing an answer.**

### Question 2: Which user roles are supported?

The application glossary already contains four roles:

1. Student
2. Instructor
3. Platform Admin
4. Team Admin

The developer confirms that all four roles should be included in the notification catalogue.

Why does this matter?

Because different roles receive different notifications.

For example:

```text
                   Cadence
                      |
              Notification System
                      |
       +--------------+--------------+
       |              |              |
       v              v              v
    Student       Instructor      Admin
       |              |              |
       v              v              v
  New lesson     New question    Refund request
  Enrollment     New rating      Platform event
  Comments       New comment     Admin updates
```

This is just an illustrative example. The actual notification catalogue still needs to be decided.

The important point is that the existing domain model helps the agent ask better questions.

### Question 3: What should the notification vendor support?

The agent asks about React components and development requirements.

The developer responds:

- No need for a prebuilt React notification component.
- Building the frontend ourselves is acceptable.
- The vendor should have a usable free development tier.

This is an interesting architectural decision.

The developer wants to control the frontend experience rather than depend on the vendor's UI.

That means the vendor should provide APIs that allow Cadence to build its own notification feed.

This is commonly described as a **headless integration**.

A headless notification service provides backend functionality through APIs while allowing the application to control the user interface.

For example:

```text
              Cadence Frontend
                     |
                     |
             Custom React UI
                     |
                     v
              Cadence Backend
                     |
                     |
                     v
              Knock API
                     |
          +----------+----------+
          |          |          |
          v          v          v
       Store      Delivery   Preferences
    Notifications Channels     Management
```

This is a conceptual architecture, not the final architecture from the lesson.

### Question 4: Should we test multiple vendors?

The agent asks whether the developer wants to:

- Read documentation and select a vendor.
- Sign up for two vendors and test both.

The developer prefers signing up for as few services as possible.

The proposed process is:

1. Research the available vendors.
2. Narrow down the candidates.
3. Select one sensible vendor.
4. Create a prototype using that vendor.

This is a good example of avoiding unnecessary experimentation.

You don't need to build prototypes for five different vendors if research can eliminate four of them.

### Question 5: Can users control notifications?

The agent asks whether users should be able to configure their notification preferences.

For example:

A student might want:

- Enrollment notifications through email.
- New lesson notifications through the in-app feed.
- Comments through both channels.

The developer agrees with controlling preferences at two levels:

- Notification category.
- Delivery channel.

Conceptually:

| Notification category | In-app | Email | Push |
|---|---|---|---|
| Enrollment | Enabled | Enabled | Disabled |
| New lessons | Enabled | Disabled | Disabled |
| Comments | Enabled | Enabled | Enabled |

This is an example of a category × channel preference model.

### Question 6: What terminology should we use?

The agent asks what the notification feature should be called.

It recommends:

- Notification
- Inbox

But the developer prefers:

- Notification
- Notification Feed

The terminology is added to the application glossary.

This is important because software engineering requires consistent domain language.

If one developer calls it an Inbox, another calls it an Activity Feed and another calls it Notifications, communication becomes confusing.

The glossary establishes a common vocabulary.

---

# 5. Understanding GLOSSARY.md

The agent updates the existing `GLOSSARY.md` file.

The new section looks like this:

```markdown
### Notifications

**Notification**:
A message Cadence sends a person to tell them something
that concerns them has happened.

_Avoid_: Alert, Message, Activity

**Notification Feed**:
The place inside Cadence where a person sees their Notifications.

_Avoid_: Inbox, Activity feed
```

Let's understand the significance.

### Notification

A notification is an individual message about an event.

Example:

"You have received a new comment."

### Notification Feed

The notification feed is the location inside Cadence where users see their notifications.

Example:

A bell icon in the application header opens a list of notifications.

### Why maintain a glossary?

Imagine we have five developers and three AI agents working on the same application.

Without a shared glossary:

- Agent A implements an Inbox.
- Agent B implements an Activity Feed.
- Agent C implements Notifications.

They might all be implementing the same concept differently.

A glossary reduces this ambiguity.

**The glossary acts as a shared domain-language reference for both developers and AI agents.**

---

# 6. What does Shared Understanding mean?

This is one of the most important concepts in the lesson.

After both rounds of grilling, the developer and agent have reached an understanding of the destination.

But notice that not everything is decided.

Some things are known, and others are still unknown.

For example:

| Known | Unknown |
|---|---|
| We need an in-app feed | Which vendor should we use? |
| We need production-grade functionality | What is the complete notification catalogue? |
| We need four user roles | How far does web push reach? |
| We prefer hosted services | What does the UI look like? |
| Users control preferences | What exact events trigger notifications? |

The agent doesn't try to resolve everything immediately.

Instead, it creates a map containing tickets for the remaining work.

This is the transition from conversation to actionable engineering work.

---

# 7. Understanding the Wayfinder Map

The map is the central planning artifact.

According to the lesson, it contains the following sections.

## 7.1 Destination

The destination describes the final outcome.

In this example:

A production-grade notification specification for Cadence using one hosted third-party vendor.

It establishes what success looks like.

## 7.2 Notes

Notes capture important information discovered during grilling.

For example:

- Cadence has four user roles.
- The application doesn't have a native mobile app.
- The developer prefers a hosted provider.
- The frontend will be custom-built.

These notes preserve the context behind the decisions.

## 7.3 Decisions so far

This section records decisions that have already been made.

Initially, this section is empty because the map has just been created.

As research and grilling progress, decisions get added.

For example:

- Use a hosted notification provider.
- Build a custom React notification feed.
- Support category × channel preferences.

## 7.4 Not yet specified (Fog)

This is particularly important.

Fog represents areas where we don't yet have sufficient information.

For example:

- Which vendor?
- Which notification events?
- Which events should trigger emails?
- How should browser push work?
- What should the UI look like?

The agent doesn't pretend these decisions have already been made.

Instead, it identifies the unknowns and creates tickets to resolve them.

This is a major benefit of Wayfinder.

## 7.5 Out of scope

This section explicitly records what will not be built.

In this example:

- No native mobile application.
- No self-hosted notification vendor.
- No custom notification delivery infrastructure.

Why is this important?

Because software projects tend to expand.

Suppose someone suggests building a native Android application just to support push notifications.

That would violate the agreed scope.

Out-of-scope items help prevent unnecessary expansion.

---

# 8. The Four Ticket Types

This is probably the most important practical part of the lesson.

The map contains approximately seven tickets.

All four ticket types are represented.

| Ticket type | Purpose |
|---|---|
| Research | Investigate unknown technical or product questions |
| Grilling | Resolve requirements and make decisions |
| Task | Perform a concrete action |
| Prototype | Build an experiment to validate an idea |

Let's examine each.

## 8.1 Research tickets

Research tickets are created when the agent doesn't have enough information to make a reliable decision.

The lesson creates two research tickets.

**Research ticket 1: Notification vendors**

Question:

Which hosted notification vendor is suitable for Cadence?

The agent needs to investigate:

- Vendor capabilities.
- Free-tier limitations.
- Authentication.
- Notification feeds.
- Preference management.
- Web push support.

**Research ticket 2: Web push reach**

Question:

Can we support browser push notifications without a native mobile application?

The research needs to establish:

- Browser compatibility.
- Device limitations.
- Delivery mechanisms.
- Whether a mobile application is required.

Notice that these are investigations, not implementation tasks.

The output should be research findings and a recommendation.

## 8.2 Grilling tickets

Grilling tickets are created when a decision requires further discussion with the developer.

The example in the document is:

**What is the notification catalogue?**

The ticket asks us to determine:

For each role:

- Student
- Instructor
- Platform Admin
- Team Admin

Which events should generate notifications?

Who should receive them?

What should the notification communicate?

The ticket also provides existing application events to investigate:

- Refund Requests and their outcomes.
- Refunds.
- Enrollments.
- Claimed Seats.
- Comments.
- Questions.
- Ratings.
- New lessons.

This is a product and domain-modeling exercise.

The agent should not arbitrarily decide which events belong in version one.

It needs to discuss them with the developer.

## 8.3 Task tickets

Task tickets represent concrete actions.

The example:

**Sign up for the selected notification vendor.**

This is different from research.

Research answers:

"Which vendor should we use?"

The task answers:

"Now that we have selected the vendor, create an account."

A task has a clear completion condition.

## 8.4 Prototype tickets

A prototype validates whether an idea works in practice.

The example:

Build a notification feed prototype in Cadence.

Why is this necessary?

Because reading vendor documentation doesn't guarantee that the integration will work smoothly.

A prototype can validate:

- API integration.
- Authentication.
- Notification retrieval.
- Frontend rendering.
- User preferences.

It reduces implementation risk before committing to the full feature.

### Comparing all four tickets

Let's use a simple example.

Suppose we want to implement payments.

| Ticket | Example |
|---|---|
| Research | Compare Stripe, Razorpay and Adyen |
| Grilling | Decide which payment methods to support |
| Task | Create a Stripe account |
| Prototype | Test a payment flow in the application |

The four ticket types solve different problems.

**Research discovers information. Grilling makes decisions. Tasks perform actions. Prototypes validate feasibility.**

---

# 9. Creating the Map and GitHub Issues

Once shared understanding is achieved, the developer approves the map.

The prompt is:

```text
yes, go ahead and create the map
```

The agent then:

1. Creates the Wayfinder map.
2. Creates the associated tickets.
3. Assigns appropriate labels.
4. Identifies dependencies.
5. Starts background research agents.

The lesson uses GitHub Issues as the tracking mechanism.

The map is not simply a document containing ideas.

It is connected to actionable engineering work.

### Understanding the workflow

```text
                  Wayfinder Agent
                        |
                        v
                   Create Map
                        |
                        v
                 Create GitHub Issues
                        |
          +-------------+-------------+
          |             |             |
          v             v             v
       Research      Grilling        Task
       Tickets       Tickets         Tickets
          |
          v
    Background Agents
          |
          v
    Research Findings
          |
          v
    Update GitHub Issues
          |
          v
    Update Wayfinder Map
```

---

# 10. Background Agents

This is another important engineering concept.

When the developer approves the map, two research agents are launched.

One investigates notification vendors.

The other investigates web push capabilities.

These agents work independently in the background.

Why is this useful?

Imagine the main agent had to perform all research itself.

It would need to:

1. Research vendor A.
2. Research vendor B.
3. Research vendor C.
4. Investigate web push.
5. Compare findings.
6. Update tickets.

This can be time-consuming.

Instead, the work can be delegated.

### Parallel execution

```text
                  Main Agent
                      |
                      v
                 Wayfinder Map
                      |
             +--------+--------+
             |                 |
             v                 v
        Research Agent 1   Research Agent 2
             |                 |
             v                 v
        Vendor Research    Web Push Research
             |                 |
             v                 v
        Vendor Findings    Push Findings
             |                 |
             +--------+--------+
                      |
                      v
                Update Issues
                      |
                      v
                Update Map
```

This is an example of multi-agent orchestration.

The main agent coordinates the work, while specialized agents investigate separate questions.

An important detail: the research agents don't need to implement the feature. Their responsibility is to resolve the research questions.

---

# 11. Understanding the GitHub Issue Labels

The lesson shows that tickets have labels such as:

`wayfinder:grilling`

This label identifies the ticket type.

For example:

```text
GitHub Issue
     |
     +-- wayfinder:research
     |
     +-- wayfinder:grilling
     |
     +-- wayfinder:task
     |
     +-- wayfinder:prototype
```

Why use labels?

Because the system can distinguish different kinds of work.

A grilling ticket requires an interactive decision-making session.

A research ticket can be delegated to a research agent.

A prototype ticket requires an experimental implementation.

This classification helps the agent understand what action to take next.

---

# 12. Understanding the Notification Catalogue Ticket

Let's examine the actual question from the lesson.

```markdown
## Question

What is the notification catalogue?

For each of Student, Instructor, Platform Admin and Team Admin:

which events produce a Notification,
who exactly receives it,
and what it says at a high level.

Start from events that already exist in the code:

- Refund Requests and their outcomes
- Refunds
- Enrollments
- Claimed Seats
- Comments
- Questions
- Ratings
- New lessons

Decide which make the cut for v1.
```

This ticket is very important because it translates the broad requirement into a domain-level specification.

Consider an example:

A student enrolls in a course.

Questions:

1. Should the student receive a notification?
2. Should the instructor receive a notification?
3. Should the team admin receive one?
4. Should the platform admin receive one?
5. Should it appear in the notification feed?
6. Should an email also be sent?

These are business rules.

The notification catalogue defines those rules.

We can think of it as a matrix:

| Event | Student | Instructor | Team Admin | Platform Admin |
|---|---|---|---|---|
| New enrollment | ? | ? | ? | ? |
| New lesson | ? | ? | ? | ? |
| New comment | ? | ? | ? | ? |
| Refund request | ? | ? | ? | ? |
| New rating | ? | ? | ? | ? |

The question marks represent decisions that still need to be made.

The actual notification rules are not provided in this lesson.

That is why the grilling ticket exists.

---

# 13. Understanding the Research Results

Now we reach the second half of the lesson.

The two background agents finish their research.

The results update the original tickets.

## 13.1 Vendor research

The research agent compares:

- Knock
- Novu Cloud
- MagicBell

The final recommendation is **Knock**.

Why?

According to the research in the lesson, Knock was the only candidate that met every specified criterion on its free plan.

The criteria included:

- Free development tier.
- Signed user tokens.
- Vendor-stored preferences.
- Headless feed API.

Let's understand these.

### Free development tier

The developer wants to experiment without immediately paying for the service.

A usable free tier makes prototyping easier.

### Signed user tokens

This is a security requirement.

Consider:

```text
User A logs in
     |
     v
Cadence Backend
     |
     v
Generate signed token for User A
     |
     v
Knock
     |
     v
User A's notifications
```

The signed token allows the notification service to authenticate the user.

It helps prevent users from accessing another user's notification feed.

### Vendor-stored preferences

Instead of maintaining all notification preferences ourselves, the vendor can manage them.

For example:

```text
User A
  |
  +-- Enrollment
  |      |
  |      +-- In-app: ON
  |      +-- Email: ON
  |
  +-- New lessons
         |
         +-- In-app: ON
         +-- Email: OFF
```

This reduces the amount of preference-management infrastructure Cadence needs to build.

### Headless feed API

This is especially relevant because the developer wants to build a custom React interface.

The vendor provides APIs, but Cadence controls the UI.

## 13.2 Vendor comparison

The lesson includes the following comparison:

| Capability | Knock | Novu Cloud | MagicBell |
|---|---|---|---|
| Per-user feed authentication | RS256 signed user token | Static HMAC with no expiry | User JWT signed with project secret |
| Web push | Via FCM only | Via FCM, OneSignal and others | Native VAPID |
| Vendor-stored preferences | Category × channel | Workflow × channel only | Category × channel |

A few important observations:

**Authentication:** The vendors use different token mechanisms.

**Web push:** Their browser-push integration approaches differ.

**Preferences:** Knock and MagicBell support category × channel preferences, while Novu's model is workflow × channel according to the research.

The developer ultimately selects Knock based on the criteria established during grilling.

Important: These are the findings reported in the lesson's research, not an independently verified comparison of current vendor capabilities.

---

# 14. Research on Web Push Notifications

The second research agent investigates:

Can web push work without a native mobile application?

The conclusion is:

> Email should be Cadence's default out-of-app channel. Web push is credible only as an extra channel people opt into, and everything it sends should also land in the Notification Feed.

Let's understand the reasoning.

There are three different notification destinations:

| Channel | Where notification appears |
|---|---|
| In-app | Inside Cadence |
| Email | User's email account |
| Web push | Browser/device notification area |

The application does not have a native mobile app.

Therefore, relying entirely on push notifications would be risky.

Email provides an alternative out-of-app delivery channel.

Web push can be offered as an additional opt-in capability.

### Resulting conceptual architecture

```text
                  Cadence
                     |
                     v
              Notification Event
                     |
                     v
             Notification Service
                     |
          +----------+----------+
          |          |          |
          v          v          v
      In-App       Email     Web Push
       Feed                    (Opt-in)
          |          |          |
          v          v          v
       Cadence    Email       Browser
        UI        Account    Notification
```

One important design principle emerges:

Regardless of whether an email or browser push notification is sent, the notification should also be recorded in the in-app Notification Feed.

This gives users a central place to review their notifications.

---

# 15. Understanding Research on a Throwaway Branch

There is another interesting engineering detail.

The research agent doesn't put the entire research report directly into the GitHub issue.

Instead:

1. It creates a research document.
2. Saves it in a temporary research branch.
3. Adds a summary to the GitHub issue.
4. Links the full research document.
5. Preserves the original question and ticket.

Why?

Because research reports can become large.

A GitHub issue should provide a concise summary of the findings and the decision.

The detailed research can live in a separate file.

For example:

```text
GitHub Repository
       |
       +-- Main Branch
       |
       +-- Research Branch
                |
                +-- docs/
                     |
                     +-- research/
                          |
                          +-- notification-vendors.md
                          |
                          +-- web-push-reach.md
```

The research branch is temporary, so the detailed reports don't necessarily become permanent project documentation.

The research results are summarized in the issues and incorporated into the map.

This is useful because the team can inspect the original evidence without cluttering the main project.

---

# 16. What happens after the research?

After the agents finish:

- Vendor research is resolved.
- Web push research is resolved.
- Knock is selected.
- Email becomes the default out-of-app channel.
- Web push becomes an optional additional channel.

The map now contains more decisions than it did initially.

Compare the two stages:

| Before research | After research |
|---|---|
| Vendor unknown | Knock selected |
| Push capability unknown | Web push is optional |
| Email role unknown | Email is default out-of-app channel |
| In-app feed confirmed | In-app feed remains primary |

The important thing is that the map evolves as information becomes available.

It isn't a static planning document.

It is a living representation of the project's current understanding.

---

# 17. Complete End-to-End Architecture of the Wayfinder Process

Let's combine everything into one diagram.

```text
                    DEVELOPER
                        |
                        v
                Initial Requirement
                        |
                        v
                   WAYFINDER
                        |
                        v
              Load Relevant Skills
                        |
              +---------+---------+
              |         |         |
              v         v         v
           Grilling  Domain   Wayfinder
                    Modeling
              |
              v
       ROUND 1: GRILLING
              |
              +-- Define destination
              +-- Define scope
              +-- Vendor approach
              +-- Production requirements
              |
              v
       ROUND 2: GRILLING
              |
              +-- Notification channels
              +-- User roles
              +-- Vendor requirements
              +-- Prototype strategy
              +-- User preferences
              +-- Domain terminology
              |
              v
        SHARED UNDERSTANDING
              |
              v
         CREATE WAYFINDER MAP
              |
      +-------+-------+-------+
      |       |       |       |
      v       v       v       v
  Destination Notes Decisions Fog
              |
              v
       CREATE GITHUB ISSUES
              |
     +--------+--------+--------+
     |        |        |        |
     v        v        v        v
  Research Grilling  Task   Prototype
     |
     v
  BACKGROUND AGENTS
     |
     +---------------------+
     |                     |
     v                     v
 Vendor Research      Web Push Research
     |                     |
     v                     v
 Knock Selected       Email Default
     |                     |
     +----------+----------+
                |
                v
         UPDATE GITHUB ISSUES
                |
                v
         UPDATE WAYFINDER MAP
                |
                v
         SHARED UNDERSTANDING
                |
                v
        READY FOR NEXT GRILLING
                |
                v
       DETAILED SPECIFICATION
```

---

# 18. What is the actual outcome of this lesson?

At the end, we have not implemented the notification system.

We have accomplished something different.

We have transformed an ambiguous requirement into an actionable engineering plan.

The current state is:

| Area | Status |
|---|---|
| Project destination | Defined |
| Notification terminology | Added to glossary |
| User roles | Identified |
| Vendor selection | Knock selected |
| Web push research | Completed |
| Email strategy | Default out-of-app channel |
| Notification catalogue | Pending grilling |
| Vendor signup | Task identified |
| Notification UI | Prototype identified |
| Detailed specification | Not yet completed |

The next step is to continue grilling on the remaining tickets, particularly the notification catalogue.

The goal is to resolve the remaining unknowns and eventually produce the specification.

---

# 19. The Most Important Engineering Concepts to Take Away

I would focus on these eight concepts from this lesson.

| Concept | What you should understand |
|---|---|
| Wayfinding | Navigate from an unclear requirement to a defined destination |
| Grilling | Ask questions to establish shared understanding |
| Domain modeling | Use consistent domain concepts and terminology |
| Fog | Explicitly represent what is not yet known |
| Out of scope | Prevent unnecessary expansion |
| Ticket classification | Separate research, decisions, tasks and experiments |
| Background agents | Delegate independent work in parallel |
| Living map | Continuously update the project plan as new information arrives |

## 20. How this fits into Agentic Software Engineering

The bigger idea behind this lesson is that an AI coding agent should not simply behave like a code generator.

It should behave more like an engineering collaborator.

Consider the difference:

**Traditional AI coding workflow**

```text
Developer
   |
   v
Give requirement
   |
   v
AI generates code
   |
   v
Developer reviews code
```

**Wayfinder-based workflow**

```text
Developer
   |
   v
Give requirement
   |
   v
AI investigates requirement
   |
   v
AI asks questions
   |
   v
Developer and AI establish scope
   |
   v
AI creates engineering map
   |
   v
AI delegates research
   |
   v
AI resolves unknowns
   |
   v
AI prepares implementation plan
   |
   v
Implementation
   |
   v
Testing and review
```

The second workflow is more deliberate.

It separates understanding the problem from solving the problem.

**My biggest takeaway from this lesson:** Don't let an AI agent start implementing a feature when the requirements are still unclear. First establish the destination, identify the fog, create tickets to resolve the unknowns, and then move towards implementation.

That is the fundamental purpose of Chart The Map.
