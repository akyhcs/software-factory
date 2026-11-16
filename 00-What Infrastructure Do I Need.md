# **Summary: What Infrastructure Do I Need?**

The lesson explains how to build your own **AI-powered Software Factory** by moving away from relying entirely on GitHub Actions and understanding the five infrastructure components required to run software engineering agents.

The central idea is that a software factory needs infrastructure to accept tasks, execute agents, manage code changes, coordinate execution, and monitor performance.

## **1. The five components of a Software Factory**

**1. Ticket Tracker**

Where humans and agents communicate, create tasks, assign work and provide feedback.

Examples: GitHub Issues, Jira, Linear, Trello, Asana

**2. Git Host**

Stores source code, branches, pull requests and code reviews.

Examples: GitHub, GitLab, Bitbucket, Forgejo, Gitea

**3. Runner**

P1 priority

The machine or sandbox where AI agents actually execute their tasks and modify code.

Examples: GitHub Actions, Daytona, E2B, VPS with containers

**4. Orchestrator**

P1 priority

Coordinates runners, triggers agents, monitors external events and handles execution errors.

Examples: GitHub Actions, custom server, Symphony, hosted agent platforms

**5. Observability Platform**

P2 priority

Measures execution costs, success rates and overall factory performance.

Examples: Langfuse, LangSmith, model-provider observability tools

## **2. Understanding each component**

| Component      | Important requirements                            |
| -------------- | ------------------------------------------------- |
| Ticket Tracker | Ticket list, assignment, labels, agent access     |
| Git Host       | Branches, PRs, reviews, code storage              |
| Runner         | Sandboxed, disposable execution environment       |
| Orchestrator   | Trigger runners, listen to events, schedule loops |
| Observability  | Cost tracking, success rates, execution metrics   |

### **Important concept: Disposable runners**

One of the most important architectural principles in this lesson is that runners should be disposable.

For example:

1. An agent starts working inside a runner.
2. It modifies source code.
3. It creates commits, branches or pull requests.
4. It sends progress updates to the ticket tracker.
5. The runner can then be destroyed.

The runner should not be the permanent storage location for work.

**Where should the state go?**

- Code changes → Git host.
- Progress notes and task information → Ticket tracker.
- Execution metrics → Observability platform.

This makes the infrastructure easier to scale and recover.

## **3. Which infrastructure should you decide first?**

The author recommends the following priority:

**Existing infrastructure — reuse it**

Already decided

Ticket Tracker

Already decided

Git Host

**New infrastructure — primary decisions**

P1

Runner

P1

Orchestrator

**Later**

P2

Observability Platform

The reasoning is straightforward:

- Migrating ticket trackers and Git hosts can be a massive effort.
- Runner and orchestrator are likely to require new infrastructure decisions.
- Observability can initially be optional until the software factory proves useful.

## **4. The author's actual infrastructure**

The author provides a real-world example of his setup:

| Component      | Technology    |
| -------------- | ------------- |
| Ticket Tracker | Linear        |
| Git Host       | GitHub        |
| Runner         | Daytona       |
| Orchestrator   | Custom server |
| Observability  | Langfuse      |

He uses Daytona to create sandboxed execution environments and a custom server to schedule work inside them.

## **5. How everything works together**

**1. Linear**

A new ticket is created

**2. Orchestrator**

Monitors tickets and triggers a runner

**3. Daytona Runner**

Creates sandbox and executes the AI agent

**4. GitHub**

Agent creates a branch and pull request

**5. Langfuse**

Tracks costs and execution performance

## **6. Important takeaways**

There are five concepts you should retain from this lesson:

1. A software factory is more than an AI agent. It requires supporting infrastructure.
2. Ticket Tracker and Git Host provide the interface for managing work and code.
3. Runner provides an isolated environment for agent execution.
4. Orchestrator manages execution, scheduling, parallelism and errors.
5. Observability helps measure whether the factory is actually working efficiently.

## **7. What you should understand before moving forward**

I would particularly focus on these three architectural questions:

- **Runner vs Orchestrator:** Why should the machine executing an agent be separate from the service coordinating agents?
- **State management:** Why should runners be disposable instead of maintaining their own persistent state?
- **Build vs Buy:** When should you use GitHub Actions or Daytona, and when should you build your own orchestration server?

**My recommendation:** Don't focus on memorising the product names (Daytona, E2B, Langfuse, etc.). Understand the responsibilities and boundaries of the five components. Those architectural concepts are the real learning outcome of this lesson.
