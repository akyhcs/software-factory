# Task Ticket – Complete Explanation

In this lesson, we are moving from **Decision Tickets to Task Tickets** in the Wayfinder workflow.

The previous lessons focused on researching, discussing, and making decisions. Now we are going to give the agent a task that requires interaction with an external service.

The example used here is **signing up for Knock and provisioning development API keys**.

Let's understand everything step by step.

---

# 1. What is the objective of this lesson?

The ticket says:

**Sign up for the notification vendor and provision dev keys.**

We want to integrate a notification service called Knock into our application.

Before we can integrate Knock into our code, we need to:
1. Create an account on Knock.
2. Create or configure a development environment.
3. Generate the required API keys.
4. Configure environment variables in our project.
5. Make the credentials available to the application.

The problem is that some of these activities require human interaction.

For example:
- Signing up using your email.
- Verifying your email.
- Accepting terms and conditions.
- Navigating the Knock dashboard.
- Creating credentials.
- Copying secret keys.

An AI agent might not have access to your browser, email, or the necessary authentication.

**So, how do we handle this?**

We introduce a Wizard Skill.

---

# 2. What is a Task Ticket?

Until now, we have worked with Decision Tickets.

Let's understand the difference.

| Decision Ticket | Task Ticket |
|---|---|
| Requires making a decision | Requires completing an activity |
| Research and discussion are important | Execution is important |
| Resolved when a decision is made | Resolved when the work is completed |
| Example: Which notification provider should we use? | Example: Sign up for Knock and provision dev keys |

Think about it this way.

Suppose we are building a notification system.

First, we need to decide which notification provider to use.

We research:
- Knock
- Novu
- Courier

After discussion, we select Knock.

That is a Decision Ticket.

Now we need to create a Knock account and obtain credentials.

That becomes a Task Ticket.

**Important:** A task ticket is not completed simply because the agent has created a plan. The actual work must be completed.

---

# 3. Why do we need a Wizard Skill?

This is the most important concept in this lesson.

Imagine asking an AI agent:

"Please sign up for Knock and configure the development API keys."

The agent might be able to:
- Read the documentation.
- Understand the signup process.
- Identify the required credentials.
- Write configuration files.
- Generate scripts.

But it might not be able to:
- Verify your email.
- Complete human authentication.
- Accept legal agreements.
- Access an authenticated dashboard.

These activities require human involvement.

Instead of asking the agent to perform everything, we divide the responsibilities.

| Responsibility | Who performs it? |
|---|---|
| Understand the requirements | AI Agent |
| Research the signup process | AI Agent |
| Identify the required credentials | AI Agent |
| Generate an interactive wizard | AI Agent |
| Execute the wizard | Human |
| Complete signup and authentication | Human |
| Provide development credentials | Human |
| Configure the required environment variables | Wizard, with human input |

This is where the Wizard Skill becomes useful.

### What exactly is a Wizard?

A wizard is an interactive Bash script that guides a human through a sequence of steps.

For example:

```text
Welcome to the Knock Setup Wizard!

Step 1: Open Knock signup page.

Please create your Knock account.

Have you completed signup? (y/n):
```

After you enter `y`:

```text
Step 2: Configure your development environment.

Please open the Knock dashboard.

Navigate to Settings → API Keys.

Enter your Development API Key:
```

You provide the key.

The wizard can then save it into the appropriate environment configuration.

For example:

```env
KNOCK_API_KEY=your_development_api_key
KNOCK_API_URL=https://api.knock.app
```

The exact variables depend on the project's requirements and the wizard's implementation.

**The important point is that the AI creates the instructions and automation, while you perform the actions that require human access.**

---

# 4. Understanding the Wizard Skill

The next step is:

```bash
npm run reset add-wizard-skill
```

What does this command do?

It adds a new skill to your repository.

You should see something similar to:

```text
.agents/
    skills/
        wayfinder/
            SKILL.md

        prototype/
            SKILL.md

        research/
            SKILL.md

        wizard/
            SKILL.md
```

The new file is:

```text
.agents/skills/wizard/SKILL.md
```

### What is SKILL.md?

It contains instructions that explain to the agent:
- When to use the wizard skill.
- What kind of problems it solves.
- How to generate an interactive Bash wizard.
- How to guide humans through external setup processes.

Think of it as a reusable capability.

Instead of explaining how to generate a setup wizard every time, we provide those instructions once.

The agent can refer to them whenever required.

### What does model-invoked mean?

This is another important concept.

There are two ways a skill can be activated.

**1. Explicit invocation**

You directly ask the agent to use a skill.

For example:

```text
Use the wizard skill to create a setup wizard.
```

**2. Model invocation**

The agent independently determines that a particular skill is relevant.

For example:

You give the agent this task:

```text
Sign up for Knock and provision development API keys.
```

The agent reads the available skill descriptions.

It recognizes that:
- The task requires external service configuration.
- Some steps require human interaction.
- The wizard skill is designed for exactly this situation.

It decides to use the wizard skill.

You don't need to explicitly tell it to use `/wizard`.

That's what model-invoked means.

---

# 5. Running Wayfinder on the Ticket

Now we have:
- A task ticket.
- A wizard skill.
- Instructions describing when and how to use the skill.

We can invoke Wayfinder.

```text
/wayfinder #<ticket-number>
```

Replace `<ticket-number>` with the actual GitHub issue number.

For example:

```text
/wayfinder #42
```

What happens next?

The agent starts processing the ticket.

Its workflow will look something like this:

```text
             GitHub Task Ticket
                      |
                      |
                      v
               Wayfinder Agent
                      |
                      |
              Reads the ticket
                      |
                      |
              Understands task
                      |
                      |
          Identifies external setup
                      |
                      |
             Reads wizard skill
                      |
                      |
              Generates script
                      |
                      |
              setup-knock.sh
                      |
                      |
              Human interaction
                      |
                      |
          Knock development credentials
                      |
                      |
             Environment configured
```

Notice something important.

The agent is not directly signing up for Knock.

It is generating a script that enables you to complete the signup process.

---

# 6. Why must we run the wizard in a separate terminal?

The instructions specifically mention:

> Open a new terminal and run the wizard there.

Why?

Because the wizard is interactive.

Consider this command:

```bash
./setup-knock.sh
```

The script might ask:

```text
Enter your Knock API key:
```

It waits for your input.

This is an interactive process.

Your AI agent's Bash environment is generally designed to execute commands and receive their output.

An interactive script that waits for human input is better executed in your actual terminal.

For example:

**Terminal 1 – AI Agent**

The agent generates:

```text
scripts/setup-knock.sh
```

**Terminal 2 – Your terminal**

You execute:

```bash
bash scripts/setup-knock.sh
```

The script prompts you for the required information.

You complete the steps.

This separation allows the agent to continue doing development work while you handle the interactive setup.

---

# 7. Understanding the Complete Architecture

Let's put everything together.

```text
                  GITHUB
                     |
                     |
             Task Ticket Created
                     |
                     v
               WAYFINDER
                     |
                     |
              AI Coding Agent
                     |
                     |
             Reads SKILL.md
                     |
                     v
                WIZARD SKILL
                     |
                     |
          Generates Bash Script
                     |
                     v
             setup-knock.sh
                     |
                     |
              HUMAN EXECUTION
                     |
                     v
              Knock Dashboard
                     |
                     |
            Create Development
                 API Keys
                     |
                     |
              Enter Credentials
                     |
                     v
             Bash Wizard Saves
               Environment
                     |
                     v
             .env / Environment
                Variables
                     |
                     v
             Application Ready
```

The key architectural idea is:

**The agent automates the process, but it does not need to automate every individual action.**

It can delegate the steps that require human participation through a guided workflow.

---

# 8. Where else can we use Wizard Skills?

This is not specific to Knock.

The same pattern is useful in many engineering situations.

| Scenario | What the wizard can do |
|---|---|
| AWS setup | Guide a developer through account and credential configuration |
| Database migration | Guide a developer through migration prerequisites and execution |
| CI/CD setup | Help configure GitHub Actions secrets |
| Third-party API integration | Guide the developer through API key generation |
| Cloud infrastructure | Walk through resource provisioning |
| Production cutover | Guide a human through a controlled deployment |
| OAuth configuration | Help configure client IDs, secrets and redirect URLs |

For example, imagine we want to integrate Stripe.

The agent can generate a wizard that asks you to:

1. Open Stripe Dashboard.
2. Create a development account.
3. Navigate to API keys.
4. Copy the publishable key.
5. Copy the secret key.
6. Configure the project's environment variables.

The same skill can support this workflow.

---

# 9. What is the actual learning from this lesson?

There are four major concepts.

**1. Task Tickets**

A task ticket is resolved through execution, not merely through discussion or decision-making.

**2. Agent Skills**

Skills provide reusable instructions that extend how an AI agent handles particular types of work.

**3. Model Invocation**

The agent can independently recognize when a skill is relevant and use it without you explicitly invoking it.

**4. Human-in-the-loop automation**

The agent can automate the preparation and orchestration while allowing humans to complete activities that require authentication, authorization or external interaction.

---

## 10. The practical steps you need to perform

In order:

| Step | Action |
|---|---|
| 1 | Select Path 1 or Path 2 |
| 2 | Open the next Wayfinder task ticket |
| 3 | Verify the `wayfinder:task` label |
| 4 | Run `npm run reset add-wizard-skill` |
| 5 | Read `.agents/skills/wizard/SKILL.md` |
| 6 | Execute `/wayfinder #<ticket-number>` |
| 7 | Review the generated wizard script |
| 8 | Open a separate terminal |
| 9 | Execute the wizard |
| 10 | Complete Knock signup and provision development keys |

### My key takeaway

The most interesting part of this lesson is the shift from **AI doing everything to AI orchestrating work between itself and humans.**

A good AI coding agent shouldn't blindly attempt actions it cannot perform.

Instead, it should:
1. Understand what needs to be done.
2. Identify which actions it can perform.
3. Identify which actions require human intervention.
4. Generate the necessary automation.
5. Guide the human through the remaining steps.

This is an important design pattern for building reliable AI-powered software engineering workflows.
