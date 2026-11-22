This lesson demonstrates how the **Wizard Skill is actually used to complete the Knock integration setup**, without exposing sensitive API credentials to the AI agent.

Let's break down what is happening, step by step, and why the developer has designed it this way.

# 1. Where are we before starting?

In the previous lessons, we:
- Researched notification vendors.
- Compared different vendors.
- Selected **Knock** as our notification provider.
- Created tickets describing the work required.
- Added the Wizard Skill to `.agents/skills/wizard/SKILL.md`.

Now we have a ticket that requires us to configure Knock locally.

The agent needs four things:

1. Knock Feed Channel ID.
2. Knock Public API Key.
3. Knock Secret API Key.
4. Knock Signing Key.

Normally, we would manually navigate through Knock's dashboard, find these values, and configure our `.env` file.

The developer wants to automate this process.

# 2. How does the Wizard Skill get triggered?

This is probably the most important concept in this lesson.

Remember, we did not explicitly tell the agent:

"Please use the wizard skill."

Instead, we gave it a ticket.

The agent:
1. Reads the ticket.
2. Understands that Knock needs to be configured.
3. Checks the available skills.
4. Finds the Wizard Skill.
5. Determines that this skill is relevant.
6. Uses the skill to generate a setup wizard.

**Why does this happen?**

Because the skill's description contains information that helps the agent recognize when it should be used.

For example, imagine the skill description says:

"When a task requires configuring an external service, collecting API keys, or setting up credentials, use this skill to create an interactive terminal wizard."

When the agent encounters the Knock setup ticket, it recognizes the match.

This is called **automatic skill invocation**.

The agent decides when to use the skill based on its description and the current task.

# 3. What exactly does the agent build?

The agent creates a Bash script:

```text
scripts/
    knock-setup-wizard.sh
```

This script is an interactive terminal application.

Think of it as a small installation assistant.

Instead of asking the AI agent to collect credentials, the script interacts directly with you.

The overall architecture looks like this:

```text
                  USER
                    |
                    |
              Gives Ticket
                    |
                    v
              AI AGENT
                    |
                    |
             Reads Ticket
                    |
                    v
           Finds Wizard Skill
                    |
                    |
             Builds Script
                    |
                    v
       knock-setup-wizard.sh
                    |
                    |
              USER RUNS IT
                    |
                    v
             BASH TERMINAL
                    |
           +--------+--------+
           |        |        |
           v        v        v
        Browser  User Input  .env
           |        |
           |        |
           v        v
       Knock Dashboard
           |
           |
      User Copies Keys
           |
           |
           v
      Bash Script
           |
           |
           v
       Local .env
```

Notice something important:

**The AI agent creates the wizard, but the user runs the wizard independently.**

# 4. Why does the developer run the script outside the agent?

This is the most important security concept in this lesson.

The developer runs:

```bash
bash scripts/knock-setup-wizard.sh
```

Instead of asking the agent to execute it.

Why?

Because the wizard collects sensitive information.

Consider the following:

```text
KNOCK_PUBLIC_API_KEY
KNOCK_SECRET_API_KEY
KNOCK_SIGNING_KEY
```

If the agent collects these credentials, they might become part of the agent's context and potentially be sent to an external AI provider.

That is an unnecessary security risk.

Instead:

```text
                 AI AGENT
                    |
                    | Creates script
                    v
             BASH SCRIPT
                    |
                    | Runs independently
                    v
                  USER
                    |
                    | Enters credentials
                    v
                LOCAL .env
```

The credentials never need to enter the AI conversation.

**Important:** Running a script outside the agent does not automatically guarantee security. The script itself must be inspected to ensure it doesn't transmit or log secrets.

# 5. Understanding the five stages

Let's understand what the wizard actually does.

### Stage 1: Create the Knock account

The wizard opens:

`dashboard.knock.app`

The user performs the following actions:

- Creates an account.
- Creates an organization named Cadence.
- Selects the free Developer plan.
- Switches to the Development environment.

Why Development?

Because we don't want to configure production credentials while building and testing the application locally.

### Stage 2: Create an in-app feed channel

The wizard asks the user to create a channel named:

```text
Cadence feed
```

What is a channel?

In Knock, a channel represents a delivery mechanism for notifications.

For example:

```text
              Knock
                |
       +--------+--------+
       |        |        |
       v        v        v
     Email    In-app    Push
              Feed
```

Our application needs an in-app feed channel.

Once created, Knock provides a Channel ID.

The wizard asks the user to paste that ID.

### Stage 3: Copy API keys

The user navigates to:

```text
Platform
   |
   v
API Keys
```

There are two important keys:

**Public API Key**

Used for client-side operations where Knock permits public-key access.

**Secret API Key**

Used for privileged server-side API operations.

Never expose the secret key in frontend code.

### Stage 4: Generate signing key

This is another important security mechanism.

The user generates an Application Signing Key.

The signing key is used to generate signed tokens that establish trust when communicating with Knock.

For example:

```text
Your Application
       |
       | Generate signed token
       v
 Signing Key
       |
       v
 Signed Token
       |
       v
    Knock
       |
       v
 Verify Signature
       |
       v
 Accept / Reject
```

The signing key must remain confidential.

The developer specifically asks for its single-line Base64-encoded representation.

### Stage 5: Enable Enhanced Security Mode

The wizard asks the user to enable enhanced security mode.

This is a security configuration within Knock.

The developer notices that Knock's documentation doesn't clearly identify the toggle's location.

Instead of pretending to know, the wizard honestly suggests checking:

- API Keys page.
- Environment settings.

The user finds the setting and enables it.

# 6. What happens after completing the wizard?

The script writes four values into the project's `.env` file.

```env
KNOCK_FEED_CHANNEL_ID=xxxxxxxx

KNOCK_PUBLIC_API_KEY=xxxxxxxx

KNOCK_SECRET_API_KEY=xxxxxxxx

KNOCK_SIGNING_KEY=xxxxxxxx
```

Now our application has the configuration required to communicate with Knock.

But there is another important detail.

### Why is `.env` gitignored?

Because we don't want credentials committed to GitHub or Bitbucket.

For example:

```text
Project
  |
  +-- src/
  |
  +-- scripts/
  |     |
  |     +-- knock-setup-wizard.sh
  |
  +-- .env
  |
  +-- .gitignore
```

Inside `.gitignore`:

```gitignore
.env
```

This prevents Git from tracking the `.env` file if it isn't already tracked.

Remember: `.gitignore` alone doesn't remove a file that Git already tracks.

# 7. Why does the agent verify the configuration afterward?

Now the developer returns to the AI agent and says:

```text
Okay, successfully run the wizard.
Can you just verify that everything looks good locally?
```

The agent checks whether the configuration is valid.

For example:

| Configuration | Verification |
|---|---|
| Feed Channel ID | Checks whether it exists |
| Public API Key | Checks format |
| Secret API Key | Checks format and API access |
| Signing Key | Checks format and signing functionality |

The agent should avoid printing or exposing the actual secret values.

The developer discovers something interesting.

**The signing key was truncated.**

This means the entire key wasn't written correctly into `.env`.

The wizard had a bug when handling the long Base64-encoded value.

Instead of running the entire wizard again, the developer manually copies the signing key into `.env`.

Then asks the agent to verify again.

This time, the agent successfully performs a real signed-token API call to Knock.

That is stronger evidence than simply checking whether the environment variables exist.

It confirms that the credentials actually work.

# 8. What does recording the resolution mean?

Remember our earlier Wayfinder discussions?

We have a map that tracks the progress of our project.

Initially:

```text
Knock Integration
       |
       v
Configure Knock
       |
       v
     Pending
```

After successful verification:

```text
Knock Integration
       |
       v
Configure Knock
       |
       v
    Completed
       |
       v
Ready for Prototyping
```

The agent updates the corresponding ticket and records the resolution on the Wayfinder map.

This is important because future agents can understand:

- What work has been completed.
- What configuration has been done.
- Which vendor has been selected.
- Whether the local environment is ready.

---

# 9. The bigger engineering lesson

There are three important concepts demonstrated here.

| Concept | What it achieves |
|---|---|
| Automatic Skill Invocation | Agent identifies and uses relevant skills without explicit instructions. |
| Human-in-the-loop Wizard | User performs sensitive configuration through an interactive script. |
| Post-setup Verification | Agent validates that the configuration actually works. |

The architecture is particularly interesting:

```text
             SOFTWARE ENGINEERING WORKFLOW

                    AI AGENT
                       |
              Understands the task
                       |
                Uses Wizard Skill
                       |
                Creates Bash Script
                       |
                       v
                 HUMAN OPERATOR
                       |
                Runs Bash Script
                       |
                Enters Credentials
                       |
                       v
                  LOCAL SYSTEM
                       |
                  Stores .env
                       |
                       v
                    AI AGENT
                       |
                Verifies Integration
                       |
                       v
                Updates Wayfinder
                       |
                       v
                 NEXT TICKET
```

### My biggest takeaway

The developer is demonstrating that **AI agents don't need to perform every operation themselves.**

Sometimes the best approach is:

1. Let the agent understand the task.
2. Let the agent generate the necessary automation.
3. Let the human execute sensitive operations.
4. Let the agent verify the result.
5. Let the agent record the completed work.

This is a practical example of combining AI automation with human control and security.
