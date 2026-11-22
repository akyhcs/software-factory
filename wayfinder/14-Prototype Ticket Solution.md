Let's go through the entire **Prototype Ticket Solution** from the beginning. I'll explain it as if we are sitting together and walking through the actual development process.

The important thing is to understand what the developer is doing, what the AI agent is doing, why they are doing it, and what the final outcome is.

# 1. First, understand what we are trying to achieve

In the previous lesson, the developer investigated different notification providers and selected **Knock** as the notification infrastructure.

Now the question is:

**Can we actually integrate Knock into our existing application, Cadence, and make it work as expected?**

Before implementing the integration in production, we want to test it.

Imagine our existing application has the following functionality:

- Students can enroll in courses.
- Students can ask questions.
- Other students can reply to those questions.
- Users should receive notifications when relevant activities happen.

We want Knock to handle these notifications.

For example:

| Event | Notification |
|---|---|
| Sarah replies to Emma's question | Emma receives a notification |
| James enrolls in a course | Marcus receives a notification if opted in |
| Student opts out of enrollment notifications | No enrollment notifications |

We need to verify that Knock supports all these behaviours.

This is where **prototyping** comes in.

# 2. Understand the three Git commits

At the beginning, the document shows three commits.

```text
add-wizard-skill
       |
       v
knock-spike
       |
       v
remove-knock-spike
```

Let's understand each one.

### Commit 1: add-wizard-skill

The developer adds a skill to the repository.

```text
.agents/
   skills/
      wizard/
         SKILL.md
```

This skill teaches the AI agent how to create interactive setup wizards.

For example, instead of manually explaining 15 steps to the developer, the agent can generate a shell script that guides the developer through those steps.

### Commit 2: knock-spike

The agent builds a temporary Knock integration.

It creates a separate branch:

```text
prototype/knock-spike
```

This branch contains experimental code.

The developer can run the application and test whether Knock works.

### Commit 3: remove-knock-spike

After testing, the temporary implementation is removed.

Why?

Because this was only an experiment.

We don't want experimental code to become production code without proper engineering, code review, and implementation.

However, we don't throw away everything.

We preserve:
- What worked.
- What failed.
- Integration requirements.
- Important implementation details.
- Known limitations.

These findings are saved back into the Wayfinder map.

**Important:** We remove the prototype code, not the knowledge gained from the prototype.

---

# 3. The agent claims the prototype ticket

The developer asks the agent to work on the prototype ticket.

The agent does the following:

1. Reads the prototype ticket.
2. Identifies the prototype skill.
3. Loads the instructions from that skill.
4. Reads the Knock documentation.
5. Installs the Knock Node SDK.
6. Creates the experimental implementation.

The architecture looks something like this:

```text
                Developer
                    |
                    | Assigns ticket
                    v
                AI Agent
                    |
          +---------+----------+
          |         |          |
          v         v          v
       Ticket     Skill     Knock Docs
          |         |          |
          +---------+----------+
                    |
                    v
             Prototype Code
                    |
                    v
             Knock Integration
```

Notice something important.

The agent isn't randomly writing code.

It uses three sources of information:

- Ticket: What needs to be achieved.
- Skill: How the agent should approach prototyping.
- Documentation: How Knock actually works.

This is why the Wayfinder map is important.

The agent can retrieve relevant research and decisions instead of relying entirely on whatever happens to be in the current conversation.

# 4. The agent creates a temporary integration

The agent builds the prototype on:

```text
prototype/knock-spike
```

It starts the development server:

```bash
http://localhost:5199
```

Now the developer can open the application in a browser.

The agent also reports that the first check has passed.

Let's understand what that means.

### Check 1: Signed user tokens

The developer wants to ensure that users can access only their own notifications.

For example:

```text
User 1 = Sarah
User 2 = Emma
```

Sarah should see Sarah's notifications.

Emma should see Emma's notifications.

The developer verifies that user 2 can read their own feed using the signed user token.

This check passes.

However, checks 2–4 require additional configuration inside Knock.

So the agent cannot complete everything automatically.

---

# 5. The developer asks the agent to create a wizard

This is an interesting part of the lesson.

The developer tells the agent:

> Could you create a wizard for me to set up the thing I need for checks two to four?

What does that mean?

Instead of manually explaining every configuration step, the developer asks the agent to generate a shell script.

The agent creates:

```text
scripts/
   prototype-knock-wizard.sh
```

The developer executes:

```bash
bash scripts/prototype-knock-wizard.sh
```

The script guides the developer through the configuration.

Think of it as an interactive installation assistant.

For example:

```text
Step 1: Open Knock dashboard.

Step 2: Create a workflow.

Step 3: Enter workflow name.

Step 4: Select notification channel.

Step 5: Configure notification template.

Step 6: Commit workflow.
```

The developer follows these steps.

### Important distinction

There are two different things here:

| Component | Purpose |
|---|---|
| Wizard skill | Instructions that teach the agent how to create a wizard |
| Wizard script | Actual executable script generated for this particular Knock prototype |

The wizard skill is reusable.

The script is specific to this project and this prototype.

---

# 6. Stage one: Create the discussion-reply workflow

Now we reach the actual Knock configuration.

The developer needs to create a workflow called:

```text
discussion-reply
```

What is a workflow?

A workflow defines what should happen when a particular event occurs.

For example:

```text
Someone replies to a discussion
              |
              v
       discussion-reply
              |
              v
       Generate notification
              |
              v
       Send to in-app feed
```

The developer creates this workflow inside Knock.

The wizard provides the following instructions:

1. Create a workflow named `discussion-reply`.
2. Add a category named `discussion`.
3. Add a channel step.
4. Select the In-app feed channel.
5. Select the Cadence feed channel.
6. Configure the notification template.
7. Save and commit to Development.

### What is the notification template?

The document provides:

```text
**{{ data.replierName }}** replied to your question in {{ data.lessonTitle }}
```

Let's understand this.

Suppose Sarah replies to Emma's question in a lesson called SQL Fundamentals.

The application sends the following data:

```json
{
  "replierName": "Sarah",
  "lessonTitle": "SQL Fundamentals"
}
```

Knock uses the template to generate:

```text
Sarah replied to your question in SQL Fundamentals
```

The `{{ }}` syntax represents dynamic variables.

The values are populated at runtime.

### Why do we commit the workflow?

This is important.

Creating and saving a workflow doesn't necessarily make it available for triggering.

The developer must commit it to the Development environment.

The document later highlights this as an important lesson:

A workflow's category exists at runtime only after the workflow is committed.

If you try to trigger an uncommitted workflow, Knock returns:

```text
404 - Workflow not found
```

So committing is an important step.

---

# 7. Stage two: Create the new-enrollment workflow

The second workflow is:

```text
new-enrollment
```

Its purpose is to notify users about new course enrollments.

The notification template is:

```text
**{{ data.studentName }}** enrolled in {{ data.courseTitle }}
```

Suppose James enrolls in a course called Data Engineering.

The application sends:

```json
{
  "studentName": "James",
  "courseTitle": "Data Engineering"
}
```

Knock generates:

```text
James enrolled in Data Engineering
```

The developer saves and commits this workflow.

At this point, we have two workflows:

```text
                 KNOCK
                   |
          +--------+--------+
          |                 |
          v                 v
   discussion-reply    new-enrollment
          |                 |
          v                 v
    Discussion event    Enrollment event
          |                 |
          v                 v
     In-app feed        In-app feed
```

Both workflows are configured.

But there is another requirement.

We need to control which users receive enrollment notifications.

---

# 8. Stage three: Understand preferences and opt-in

This is probably the most confusing part of the entire lesson.

The requirement is:

**Enrollment notifications should be disabled by default. Users must explicitly opt in to receive them.**

For example:

```text
Marcus
   |
   v
Enrollment notification preference
   |
   v
OFF
```

If James enrolls in a course, Marcus should not receive a notification.

However, if Marcus opts in:

```text
Marcus
   |
   v
Enrollment notification preference
   |
   v
ON
```

Now Marcus should receive enrollment notifications.

The developer initially tries to configure this through the Knock dashboard.

He explores:

- Platform preferences.
- Preference center.
- Enrollment category.
- Channel settings.
- Default preferences.

But he gets confused.

Why?

Because Knock has different configuration concepts:

1. Workflow categories.
2. Preference options.
3. Channel preferences.
4. Default preferences.

They are related, but they aren't necessarily the same thing.

The developer cannot figure out how to configure the default state correctly.

So he asks the agent for help.

---

# 9. The important architectural decision: Cadence manages defaults

The agent finds a workaround.

Instead of configuring the default preference inside Knock, the existing application, Cadence, will manage it.

This is an important architectural decision.

Let's understand it.

Whenever Cadence synchronizes an instructor/user with Knock, it sets:

```json
{
  "enrollment": "off"
}
```

This means the user's enrollment notification preference is disabled.

If the user explicitly opts in, the preference changes to:

```json
{
  "enrollment": "on"
}
```

Now Knock can use this preference when deciding whether to send the notification.

The architecture becomes:

```text
             CADENCE
                |
                | User synchronization
                |
                v
              KNOCK
                |
                v
       User Preferences
                |
        +-------+-------+
        |               |
        v               v
    enrollment       discussion
        |               |
        v               v
       OFF             ON
```

The important point:

**Cadence controls the initial preference state. Knock handles notification delivery according to the configured preferences.**

This means we don't need to struggle with Knock's default preference configuration.

The developer decides to skip stage three of the wizard.

This is a good example of prototyping.

Sometimes the prototype reveals that the original approach is unnecessarily complicated and a different architecture is simpler.

---

# 10. Why does the prototype contain bad code?

The document shows this code:

```typescript
const existing = await knock.users
  .getPreferences(id, "default")
  .catch(() => null);
```

The developer explains that this is intentionally simplified code.

Let's understand it.

Suppose we want to retrieve the user's existing preferences.

```typescript
await knock.users.getPreferences(id, "default")
```

This operation might fail.

For example:
- User doesn't exist.
- API request fails.
- Network error occurs.
- Knock returns an error.

The developer uses:

```typescript
.catch(() => null)
```

This means:

If anything goes wrong, ignore the error and return `null`.

Why is this potentially dangerous?

Because different errors are treated identically.

For example:

```text
User has no preferences
          |
          v
        null
```

But also:

```text
Knock API is unavailable
          |
          v
        null
```

These are two completely different situations.

In production, we would want to distinguish them.

But this is a prototype.

The objective is to validate functionality, not build production-quality error handling.

**Remember: Prototype code is experimental code, not production-ready code.**

---

# 11. Operating the prototype

Now comes the most important part.

Until now, the developer has configured Knock and built the integration.

But how do we know that it actually works?

We test it.

The agent provides a prototype interface:

```text
/prototype/knock
```

The page contains:

- Buttons to trigger discussion notifications.
- Buttons to trigger enrollment notifications.
- Ability to switch between users.
- Notification feed.
- Mark as read.
- Mark as unread.
- Mark all seen.
- Opt-in enrollment.

This is not a UI design prototype.

It is a functional prototype.

The purpose is to verify the backend behaviour.

---

# 12. Testing discussion notifications

The developer starts testing the first scenario.

The expected behaviour:

Sarah replies to Emma's question.

Emma should receive a notification.

The testing sequence is:

```text
1. Login as Sarah.
        |
        v
2. Reply to Emma's question.
        |
        v
3. Knock receives the event.
        |
        v
4. Knock executes discussion-reply.
        |
        v
5. Notification appears in Emma's feed.
```

The developer expects the notification to appear within approximately three seconds.

Initially, the test fails.

The developer cannot identify which button actually triggers the reply.

He clicks all the reply buttons, but no notification appears.

He reports the issue to the agent.

The agent investigates and provides an updated testing sequence.

The new sequence is:

```text
Sarah
  |
  | Sends notification to Olivia
  v
Knock
  |
  | Processes event
  v
Olivia's notification feed
```

Now it works.

Olivia receives the notification.

This is an example of manual QA with AI assistance.

The developer tests the feature, reports unexpected behaviour, and the agent investigates the prototype.

---

# 13. Testing read, unread and seen behaviour

The prototype provides three operations:

- Mark as read.
- Mark as unread.
- Mark all seen.

Initially, the developer thinks that Mark all seen is broken.

He reports the issue to the agent.

The agent investigates and explains something important about Knock.

**Seen and Read are different states.**

Look at the table:

| State | Meaning | Behaviour |
|---|---|---|
| Seen | User has seen the notification | Doesn't necessarily mark it as read |
| Read | User has marked the notification as read | Notification appears dimmed |

For example:

Imagine Olivia receives three notifications.

Initially:

```text
Notification 1: Unread
Notification 2: Unread
Notification 3: Unread
```

She clicks Mark all seen.

Now:

```text
Notification 1: Seen
Notification 2: Seen
Notification 3: Seen
```

But they aren't necessarily read.

When she clicks Mark as read on notification 1:

```text
Notification 1: Read
Notification 2: Seen
Notification 3: Seen
```

The prototype dims only the read notification.

That's why the developer initially thought the functionality wasn't working.

The underlying behaviour was correct.

This is an important finding to preserve for future implementation.

---

# 14. Testing the enrollment opt-in preference

Now we test the second workflow.

The requirement:

Enrollment notifications must be disabled by default.

Let's walk through the scenario.

### Test 1: User has not opted in

Olivia enrolls in a course and selects Marcus as the notification recipient.

Marcus should receive nothing.

Expected result:

```text
Olivia enrolls
      |
      v
new-enrollment workflow
      |
      v
Check Marcus's preference
      |
      v
enrollment = OFF
      |
      v
No notification
```

The developer tests this.

It works.

### Test 2: User explicitly opts in

Now Marcus opens the application and clicks:

```text
Opt-in Enrollment
```

His preference changes:

```json
{
  "enrollment": "on"
}
```

Now James enrolls in a course and selects Marcus.

The application triggers the enrollment workflow.

Knock checks Marcus's preference.

Since it is enabled, Marcus receives the notification.

The complete flow:

```text
                  JAMES
                    |
                    | Enrolls
                    v
                 CADENCE
                    |
                    | Trigger event
                    v
                  KNOCK
                    |
                    v
          Check Marcus preference
                    |
                    v
             enrollment = ON
                    |
                    v
          Send notification
                    |
                    v
                  MARCUS
```

This confirms that the opt-in requirement works.

---

# 15. Saving the learnings back to Wayfinder

This is one of the most important concepts in the entire lesson.

After completing testing, the developer tells the agent:

> Let's save our learnings back to the map.

Why?

Because the prototype code will eventually be removed.

If we only remove the code, the next agent won't know:

- What was tested.
- What worked.
- What failed.
- What architectural decisions were made.
- What special cases need attention.

So the agent updates the Wayfinder map and closes the prototype ticket with a resolution.

The main decision is:

```text
Knock passes. We commit to it.

Off by default is set by Cadence,
not by a Knock setting.
```

The agent also records important pitfalls.

Let's understand each one.

### Pitfall 1: Workflow category exists only after commitment

A workflow may have a category configured, but the category becomes available at runtime only after the workflow is committed.

### Pitfall 2: Uncommitted workflows return 404

If you trigger a workflow that hasn't been committed:

```text
404 - Workflow not found
```

### Pitfall 3: Seen is not read

Marking notifications as seen does not necessarily update the unread count.

### Pitfall 4: setPreferences replaces the entire preference set

This is particularly important from a coding perspective.

Suppose a user has:

```json
{
  "discussion": "on",
  "enrollment": "off",
  "marketing": "off"
}
```

Now the application wants to enable enrollment.

A careless update might send:

```json
{
  "enrollment": "on"
}
```

If the API replaces the entire preference set, the other preferences may be lost.

The correct approach is to retrieve existing preferences, merge the new value, and then update.

Conceptually:

```typescript
const existing = await getPreferences(userId);

const updated = {
  ...existing,
  enrollment: "on"
};

await setPreferences(userId, updated);
```

This preserves the other preferences.

---

# 16. What happens to the prototype after this?

We now have:

1. A working Knock integration prototype.
2. Successful functional tests.
3. Identified limitations.
4. A confirmed architectural decision.
5. Documentation of implementation pitfalls.

The developer closes the prototype ticket.

The experimental branch is removed.

```text
                 MAIN BRANCH
                      |
                      |
             Prototype ticket
                      |
                      v
              Experimental branch
             prototype/knock-spike
                      |
                      v
              Build Knock integration
                      |
                      v
                Test everything
                      |
                      v
              Record the findings
                      |
                      v
               Remove prototype
                      |
                      v
             Implement production
```

The next implementation agent has access to:

- The original research.
- The prototype findings.
- The final architectural decision.
- The known pitfalls.

Therefore, it doesn't have to rediscover everything.

---

# 17. The complete Wayfinder development lifecycle

Let's connect this lesson with the bigger picture.

The process is:

```text
                  WAYFINDER
                      |
                      v
              Research the problem
                      |
                      v
                Grill the ticket
                      |
                      v
             Understand requirements
                      |
                      v
             Create prototype ticket
                      |
                      v
              Build temporary code
                      |
                      v
             Configure Knock manually
                      |
                      v
                Test functionality
                      |
                      v
              Identify bugs and issues
                      |
                      v
             Record findings in the map
                      |
                      v
              Close prototype ticket
                      |
                      v
             Remove temporary code
                      |
                      v
             Create implementation ticket
                      |
                      v
             Build production solution
                      |
                      v
                 Code review
                      |
                      v
                   Deploy
```

## 18. Five concepts you should take away from this lesson

| Concept | What it actually means |
|---|---|
| Prototype / Spike | Build temporary code to prove whether an approach works |
| Wizard skill | Reusable instructions that help an agent generate setup wizards |
| Manual QA | Developer tests the prototype and reports unexpected behaviour |
| Wayfinder map | Stores research, decisions, and learnings for future agents |
| Production implementation | Rebuild the solution properly using the knowledge gained from prototyping |

## 19. Why is this approach useful?

Consider two approaches.

**Approach A: Direct implementation**

```text
Requirement
    |
    v
AI writes production code
    |
    v
Integration problems
    |
    v
Repeated modifications
    |
    v
More bugs
```

**Approach B: Wayfinder approach**

```text
Requirement
    |
    v
Research
    |
    v
Grill
    |
    v
Prototype
    |
    v
Test
    |
    v
Document findings
    |
    v
Production implementation
```

Approach B reduces uncertainty before committing to a production implementation.

You discover integration problems early, validate whether the external service actually supports your requirements, and give the production agent concrete implementation knowledge.

---

## 20. My interpretation of the most important lesson

There is one sentence that captures the entire purpose of this exercise:

**The prototype is disposable, but the knowledge gained from the prototype is permanent project knowledge.**

The developer isn't trying to build the final notification system here.

He is trying to answer:

1. Can Knock support our notification requirements?
2. Can we integrate it with Cadence?
3. Can we support user preferences?
4. Can we handle notification states correctly?
5. What problems should the production developer know about?

Once these questions are answered, the prototype has served its purpose.

The production implementation becomes much easier because the difficult unknowns have already been investigated.

**One distinction to remember:** The document demonstrates a prototype of the Knock integration, not the complete production migration of Cadence's notification infrastructure. The final production implementation is the next stage, not something completed in this lesson.
