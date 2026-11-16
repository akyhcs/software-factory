# **Choosing Your Infrastructure — Explained from the Beginning**

This lesson is about making an important architectural decision: **Which tools and services will power your Software Factory?**

In the previous lesson, you identified the infrastructure required to build a Software Factory. Now you need to select actual products for each infrastructure component.

Let's understand the reasoning before looking at individual tools.

## **1. First, understand what we are building**

A Software Factory is an automated software development system where agents can take a task, understand it, write code, test it, and eventually submit a Pull Request for human review.

For that to work, we need five infrastructure components.

[Agentic AI in Enterprises: Scaling Autonomous Systems](https://images.openai.com/static-rsc-4/Zff2huvbyZSR9Q4BrYrvMtvFRjllKgbFBVbebEkdkzNcNBY9i8FDxjZhf-CdQTzCaPA3zR_c8NhVLOTT5czYl3B2v9s--dvFDd6WlMr56za1BKLSG8HRJ4ZlDJVa2JPHy32RHBOu-t1li0JQRPGpQ0ltOz623h3_W2W4PMmJLvU?purpose=inline)

[truefoundry.com](https://www.truefoundry.com/blog/agentic-ai-in-enterprise?utm_source=chatgpt.com)

| Component      | Responsibility                                    | Example products     |
| -------------- | ------------------------------------------------- | -------------------- |
| Ticket Tracker | Stores development tasks                          | Linear, Jira         |
| Git Host       | Stores source code and PRs                        | GitHub, GitLab       |
| Runner         | Provides an environment where agents execute code | GitHub Actions, E2B  |
| Orchestrator   | Coordinates agents and workflow execution         | Temporal, Inngest    |
| Observability  | Tracks execution, failures, costs and logs        | Langfuse, Braintrust |

Let's understand each one.

## **2. Ticket Tracker**

[New issue creation UI – Changelog](https://images.openai.com/static-rsc-4/Rx28C4mic3L1ekg37fPO6-QQNx3LRhkaLfgrnGDhnDYSHxdnLn2wTjgMNISu7TBK7kQvA50AXUYiWQMat8d-pe0-I58w4--I125gJuVcwd5lZgw-vsxdEkFWNBEw9jMj_bH75XTt4unIuLJje_UiwU6muJbuVixJKR3jlwGUQWM?purpose=inline)

[linear.app](https://linear.app/changelog/2021-03-10-new-issue-creation-ui?utm_source=chatgpt.com)

Think of this as the entry point into your Software Factory.

Suppose someone creates this ticket:

**Ticket: Implement user authentication API**

The ticket contains:

- Description of the requirement.
- Acceptance criteria.
- Priority.
- Assigned team or person.
- Status.

Your Software Factory needs to read this ticket and understand what needs to be implemented.

Common options:

- Linear
- Jira
- GitHub Issues

**Decision:** You probably already have a preferred ticket tracker. There is no need to change it just because you're building a Software Factory.

## **3. Git Host**

[Developing Software In a Team: Code Review – Intermediate Research Software Development](https://images.openai.com/static-rsc-4/TD8DSReKWvxCRJPWjuyISMn06SNO5d2Q9oZ_n9RnKHvr8pCTaN4UOzyfdpboyO_8StA9hqi55fTGVAAU0OodQ-2oa1LOYLtz2Y085MTHcJDYB0UJ_1zS5Xb0M8eKgG0rPHmppaV5wMsh_tiQS4APRnOf1_5WfMR0WzwDLHWzWAI?purpose=inline)

[softwaresaved.github.io](https://softwaresaved.github.io/python-intermediate-development/41-code-review/index.html?utm_source=chatgpt.com)

The Git Host is where your source code lives.

For example:

1. Agent receives the authentication ticket.
2. Agent clones the repository.
3. Agent creates a new branch.
4. Agent implements authentication.
5. Agent commits its changes.
6. Agent creates a Pull Request.

Common options:

- GitHub
- GitLab
- Bitbucket

Again, this is generally a straightforward decision.

## **4. Runner — One of the important decisions**

[Base editor overview  |  Cloud Workstations  |  Google Cloud Documentation](https://images.openai.com/static-rsc-4/oUDjfcks1XCtPAHQ-FZGvZ4XwDRAbSuqmVF0Eoa4c8AsG69_ZfEdc0IJFvM66f4PC4aQPn5feWmT_G91bBGFduXsUmHf3CImnMq7ZIHprH4Hk-wKDj19w0VPi8J4SPInwsrqWuf0J6_QrWL9Nrpj9vsh43Il8CsoxfyUClVPrCQ?purpose=inline)

[docs.cloud.google.com](https://docs.cloud.google.com/workstations/docs/base-editor-overview?utm_source=chatgpt.com)

This is where things become interesting.

**A runner is the environment in which your coding agent actually executes.**

Imagine an agent receives a ticket to implement an API.

It needs to:

- Access the repository.
- Read the existing code.
- Modify files.
- Install dependencies.
- Execute tests.
- Run terminal commands.
- Generate commits.

Where will all these activities happen?

Inside a runner.

The lesson identifies four types of runners.

| Runner type            | Meaning                                                    |
| ---------------------- | ---------------------------------------------------------- |
| CI Runner              | Uses continuous integration infrastructure to execute jobs |
| MicroVM API            | Creates isolated virtual machines on demand                |
| Bundled Agent Platform | Provides the agent and its execution environment together  |
| Bring-your-own Worker  | You manage your own machine or worker infrastructure       |

Let's understand the difference.

**A. CI Runner**

Examples: GitHub Actions and GitLab CI.

You trigger a workflow, and a machine executes the required commands.

Advantages:

- Familiar infrastructure.
- Existing CI/CD integration.
- Convenient for automated builds and tests.

Limitations:

- CI workflows aren't necessarily designed for long-running interactive agents.
- Managing persistent agent sessions can be more complicated.

**B. MicroVM API**

Examples: E2B and Daytona.

You request an isolated environment through an API.

For example:

```
Agent receives ticket
       |
       v
Request sandbox
       |
       v
Sandbox created
       |
       v
Clone repository
       |
       v
Execute coding agent
       |
       v
Run tests
       |
       v
Create Pull Request
```

The advantage is that you don't need to manage an entire fleet of machines yourself.

**C. Bundled Agent Platform**

These platforms provide a more integrated agent execution experience.

Instead of assembling every component independently, you use a platform that already provides several agent-related capabilities.

**D. Bring-your-own Worker**

You manage your own infrastructure.

For example, you could run agents inside Docker containers on your own cloud VMs.

You have greater control, but you are responsible for:

- Provisioning machines.
- Security and isolation.
- Scaling.
- Monitoring.
- Resource management.

The important takeaway: A runner is not the agent itself. It provides the execution environment for the agent.

## **5. Orchestrator — The coordinator**

This is another extremely important component.

Imagine that your Software Factory has multiple activities:

1. Read a ticket.
2. Create an execution environment.
3. Start a coding agent.
4. Wait for implementation.
5. Execute tests.
6. Retry if tests fail.
7. Create a Pull Request.
8. Notify the reviewer.

Who coordinates these steps?

**The orchestrator.**

[What Is an Orchestration Layer? Understanding Its Importance in System Architecture — GoClaw Blog](https://images.openai.com/static-rsc-4/-qf6E1Of45YmErCsPhMUDiVHgtKhwpeJH38rzgJw7nSfrNc0S-Kz4eGlrRP-b11LFDpQBzfyK40hqQsvQAGSRngWQLswxba8M_-vtIkKpAxfIV4qt6POz_WGcUHz99jZ3y20kQa6e_v6SWhB_y75d5zZKOBzT2l5UaO9ZUzVGgs?purpose=inline)

[goclaw.sh](https://goclaw.sh/blog/what-is-an-orchestration?utm_source=chatgpt.com)

Consider this example:

```
Ticket Created
      |
      v
Orchestrator
      |
      v
Create Runner
      |
      v
Start Coding Agent
      |
      v
Wait for Completion
      |
      v
Run Tests
      |
      +------ Tests Failed
      |            |
      |            v
      |       Retry Agent
      |
      v
Create Pull Request
      |
      v
Human Review
```

The orchestrator manages the workflow's execution and state.

Some examples worth investigating:

- Temporal
- Inngest
- Trigger.dev

One important distinction:

An agent decides how to accomplish a task. An orchestrator manages when and how different tasks and agents execute.

## **6. Observability Platform**

[Production-Grade Observability: Agent Kernel Integrates Langfuse and OpenLLMetry | Agent Kernel](https://images.openai.com/static-rsc-4/KtnrjeQQ2DbCmuyBmNPmoYAkC-dg7DosilbU5f8ahz2B2VGzILWGOJKwB0XrNdL8_xnVkD2DbFjCtrDq4BTsWdCmAkoPeGFTtIZ1ytJzMzD6xM61hFRw_RQqJxP0C4kma8cxLd1P9gYDgSYIaIwxDo8KHsMjBdj8lOp1pHWB4Hc?purpose=inline)

[kernel.yaala.ai](https://kernel.yaala.ai/blog/blog/observability-support-langfuse-openllmetry?utm_source=chatgpt.com)

Suppose your coding agent runs for 20 minutes and fails.

You need answers to questions like:

- Which step failed?
- Which model was used?
- How many tokens were consumed?
- How much did the execution cost?
- What tools did the agent invoke?
- How long did each step take?
- Why did the agent fail?

This is where observability comes in.

Some options:

- Langfuse
- Braintrust
- LangSmith

Think of observability as the monitoring and debugging layer of your Software Factory.

## **7. How all five components work together**

Let's put everything together.

**1. Ticket Tracker**

Receives development requirements

**2. Orchestrator**

Coordinates the workflow

**3. Runner**

Provides an isolated execution environment for the agent

**4. Coding Agent**

Reads code, implements changes and runs tests

**5. Git Host**

Stores commits and Pull Requests

**Observability Platform**

Monitors the entire execution lifecycle

Observability collects information across the workflow; it is not necessarily a sequential step.

## **8. What exactly does this lesson expect you to do?**

You are not supposed to provision these services yet.

You are supposed to make five infrastructure decisions.

The lesson asks you to:

1. Add the `/software-factory` skill to your repository.
2. Read its reference documents.
3. Understand the available product categories.
4. Start a `/grill-me` session with your coding agent.
5. Let the agent ask you one question at a time.
6. Decide which products you want for each infrastructure component.
7. Consider pricing, usage and operational complexity.

The important point about the `/grill-me` approach is that you shouldn't simply ask an agent to select five products.

Instead, it should understand your requirements and help you make informed decisions.

## **9. My recommendation for how we should approach this lesson**

I would not immediately select products.

First, understand the architectural responsibilities:

| Component      | Question we need to answer                       |
| -------------- | ------------------------------------------------ |
| Ticket Tracker | Where do development tasks originate?            |
| Git Host       | Where does source code and PR management happen? |
| Runner         | Where will coding agents execute?                |
| Orchestrator   | Who coordinates the execution lifecycle?         |
| Observability  | How will we debug and monitor executions?        |

Then compare actual products, their pricing, limitations and integration capabilities.

**One particularly important distinction to understand before proceeding:**

- Runner = Where the agent works.
- Orchestrator = Who manages the workflow.
- Agent = Who performs the actual development work.
- Observability = How we understand what happened.

If these four concepts are clear, the rest of this lesson becomes considerably easier.

**Let's take the next step by understanding runners in more depth**, because choosing between CI runners, MicroVMs, bundled agent platforms and your own workers will influence the architecture of the entire Software Factory.


---

Yes. I missed an important part of the lesson: **the&#x20;**`/software-factory`**&#x20;skill and its supporting reference documents.** I focused too much on the five infrastructure components and didn't explain how skills fit into the decision-making process.

Let's correct that.

## **1. What is the&#x20;**`/software-factory`**&#x20;skill?**

The lesson asks you to add a skill to your repository:

```
npm run cherry-pick add-software-factory-skill
```

Or, if your repository isn't at the expected starting commit:

```
npm run reset add-software-factory-skill
```

This adds:

```
.agents/
  skills/
    software-factory/
      SKILL.md
      references/
        TICKET-TRACKERS.md
        GIT-HOSTS.md
        RUNNERS.md
        OBSERVABILITY-PLATFORMS.md
```

The exact directory structure may vary, but these are the documents the lesson tells you to inspect.

**What is the purpose of this skill?**

It provides shared terminology and reference material to your coding agent.

For example, when you tell your agent:

> Help me choose a runner for my Software Factory.

The agent should understand what a runner means in the context of this architecture, rather than interpreting it as just any background process.

## **2. Understanding the reference documents**

The lesson specifically asks you to read these files.

| File                         | What you learn                                        |
| ---------------------------- | ----------------------------------------------------- |
| `SKILL.md`                   | Common terminology and concepts of a Software Factory |
| `TICKET-TRACKERS.md`         | Available ticket tracking products                    |
| `GIT-HOSTS.md`               | Available Git hosting products                        |
| `RUNNERS.md`                 | Different runner architectures and products           |
| `OBSERVABILITY-PLATFORMS.md` | Monitoring and observability options                  |

One important observation: these documents are surveys, not authoritative, permanently updated product databases.

The skill itself tells the agent to research current information rather than blindly trust potentially outdated product details.

## **3. Why does the lesson introduce skills before infrastructure selection?**

This is the interesting part.

Consider two situations.

**Without the skill:**

You ask an agent:

"Help me select infrastructure for my Software Factory."

The agent might misunderstand terminology, miss architectural distinctions or make assumptions.

**With the skill:**

The agent has a shared vocabulary and reference material.

It can reason about:

- Ticket trackers.
- Git hosts.
- Runners.
- Orchestrators.
- Observability platforms.
- Their relationships.

The skill establishes a common understanding before the agent starts asking you architectural questions.

## **4. The&#x20;**`/grill-me`**&#x20;session**

This is the next important part I missed.

The lesson asks you to run:

```
/grill-me about creating a new /software-factory. I want to move off this GitHub Actions-based setup into new infrastructure. Let's not worry about any new tasks yet, let's just think about the services. Ask one question at a time.
```

The idea is to have the agent interview you.

For example:

1. Which ticket tracker do you currently use?
2. Which Git host do you prefer?
3. What type of runner do you need?
4. How frequently will agents execute?
5. What is your infrastructure budget?
6. Do you prefer managed services or self-hosted infrastructure?
7. What observability information do you need?

These are illustrative questions; the actual agent should ask one at a time based on your answers.

## **5. The complete sequence of this lesson**

1. **Add the software-factory skill**

   Run the cherry-pick command.
2. **Understand SKILL.md**

   Learn the shared vocabulary.
3. **Read the reference documents**

   Study ticket trackers, Git hosts, runners and observability platforms.
4. **Understand runner classifications**

   CI runner, MicroVM API, bundled agent platform and bring-your-own worker.
5. **Start&#x20;**`/grill-me`

   Let the agent interview you one question at a time.
6. **Make infrastructure decisions**

   Evaluate pricing, usage, operational complexity and capabilities.
7. **Finalize all five components**

   Prepare the decisions for the next lesson's architecture diagram.

**One distinction worth remembering:** A skill is not an infrastructure component. It is knowledge and instructions supplied to the agent to help it perform a task consistently.

Going forward, let's follow the actual course sequence rather than jumping straight to product recommendations. We should understand the skill and its reference files first, then work through the `/grill-me` exercise.
