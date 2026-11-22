Let's understand what is happening in this lesson, step by step, without getting lost in the theory.

# Prototype Ticket – What are we actually doing?

In the previous lessons, we:
1. Created the notification vendor research.
2. Compared Knock with Cadence.
3. Decided to proceed with Knock.
4. Created the `wizard` skill to help with the Knock signup process.
5. Provisioned the Knock API keys.

**Now we're going to build a working prototype using Knock to verify whether our decision was correct.**

The main question is:

> Does Knock actually work well for Cadence's notification requirements?

## 1. Understand the three commits

There are three important commits in this lesson.

| Commit | What happens |
|---|---|
| `add-wizard-skill` | Starting point. The wizard skill already exists. |
| `knock-spike` | The agent builds the Knock prototype. |
| `remove-knock-spike` | Prototype code is removed, but the decision remains. |

Notice something important:

**We're not planning to merge the prototype into the actual application.**

We're building it to experiment and validate our technical decision.

## 2. Set up the repository

If you're following Path 1, execute:

First:

```bash
npm run seed-wayfinder-issues 04-task-ticket
```

This prepares the GitHub issues for this lesson.

It marks the previous signup ticket as completed.

Next:

```bash
npm run reset add-wizard-skill
```

This resets your repository to the `add-wizard-skill` commit.

Your repository is now ready for the prototype exercise.

Remember: You must already have these four environment variables configured:

```env
KNOCK_SECRET_API_KEY=
KNOCK_PUBLIC_API_KEY=
KNOCK_FEED_CHANNEL_ID=
KNOCK_SIGNING_KEY=
```

These come from your Knock account.

## 3. Open the prototype ticket

Go to GitHub Issues.

Find:

**Does Knock hold up against Cadence?**

It should have this label:

`wayfinder:prototype`

This ticket contains four checks that the agent needs to validate.

The ticket is essentially the requirement document for our experiment.

## 4. Clear the agent's context

Execute:

```text
/clear
```

Why?

Because we're starting a new, relatively large task.

The agent should not carry unnecessary conversation history from previous research and implementation sessions.

It will independently explore the repository and understand the prototype requirements.

## 5. Start Wayfinder

Now execute:

```text
/wayfinder <URL-of-your-ticket>
```

For example:

```text
/wayfinder https://github.com/your-org/your-repo/issues/123
```

What happens now?

1. Wayfinder receives the prototype ticket.
2. It reads the requirements.
3. It explores the existing Cadence codebase.
4. It understands how notifications currently work.
5. It starts building a Knock integration prototype.
6. It validates the four checks specified in the ticket.

The important point is that **the agent is now moving from research to actual experimentation.**

## 6. Why does the prototype need a separate branch?

This is probably the most important concept in this lesson.

Imagine your repository:

```text
main
  |
  |--- Existing Cadence application
  |
  |--- Existing notification implementation
```

Now the agent wants to experiment with Knock.

We don't want experimental code mixed with production application code.

Instead:

```text
main
  |
  |--- Existing Cadence application
  |
  |
  └── prototype/knock-spike
          |
          |--- Knock integration
          |--- Notification workflows
          |--- Feed implementation
          |--- Experimental code
```

The agent can freely modify the prototype without affecting the actual application.

This is similar to creating a throwaway Git branch for experimentation.

**Why is this useful?**

Because the prototype might contain:
- Incomplete implementations.
- Temporary code.
- Hardcoded values.
- Experimental architecture.
- Code that we eventually decide not to use.

We don't want these changes accidentally becoming part of `main`.

## 7. Iterate on the prototype (Vibe Coding)

This is where most of the work happens.

The agent builds something.

You inspect it.

Suppose it creates a notification UI.

You might tell it:

- I don't like this UI. Make it simpler.
- The notification feed should support unread counts.
- Change how notifications are displayed.
- Improve the integration architecture.
- Test what happens when Knock API calls fail.

The agent modifies the prototype based on your feedback.

You repeat this process until you're satisfied.

This is why the instructor says to spend tokens freely.

The objective isn't to minimize token usage. It's to get a high-quality prototype and validate the decision.

## 8. What happens when we're finished?

Once the prototype is complete, compare your implementation with the instructor's solution:

```text
knock-spike
```

Then the final commit:

```text
remove-knock-spike
```

removes the experimental implementation.

However, the important findings and architectural decision remain.

Your Git history effectively becomes:

```text
add-wizard-skill
       |
       v
knock-spike
       |
       v
remove-knock-spike
```

The prototype existed, was tested, and was removed.

The decision informed by that experiment remains part of the project.

---

## The bigger picture: What is this lesson teaching?

There is an important software engineering practice here.

| Activity | Purpose |
|---|---|
| Research | Identify the best notification vendor. |
| Decision | Select Knock over Cadence. |
| Prototype | Verify whether Knock actually works. |
| Iteration | Improve the prototype based on feedback. |
| Validation | Confirm the four requirements. |
| Cleanup | Remove experimental code while retaining the decision. |

**My interpretation:** The instructor is teaching you not to blindly trust research or architectural decisions.

Even after selecting a technology, build a working prototype to validate it before committing to the actual production implementation.

That's the real purpose of this lesson.


-----



Yes! You've understood the idea correctly, with one important distinction.

**We are experimenting with a different notification integration (Knock) against the existing notification system (Cadence).**

Here's what happens:

1. **Existing system:** Cadence already has its notification functionality.
2. **Reference:** The agent explores the existing Cadence code to understand how notifications currently work.
3. **New integration:** The agent builds a prototype using Knock.
4. **Comparison:** It checks whether Knock can support the same requirements as the existing notification system.
5. **Iteration:** We refine the Knock implementation based on what we observe.

Think of the architecture like this:

```text
       Existing Cadence Application
                  |
                  v
          Notification Interface
                  |
          Existing Implementation
                  |
                  v
              Cadence


       Prototype Experiment

       Existing Cadence Application
                  |
                  v
          Notification Requirements
                  |
                  v
             Knock Integration
                  |
                  v
             Knock Platform
```

**One important thing:** We are not replacing the existing notification system in `main` yet.

We're building a separate prototype to determine whether Knock is a suitable replacement.

If the experiment succeeds, the next step would be to implement the actual integration into the application.

So, the existing notification implementation acts as a reference, while Knock is the technology being evaluated.
