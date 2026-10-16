# mcp-
# Complete MCP Notes for Beginners — 2026 Edition

Based on your uploaded video script and summary • Explained from zero • With simple real-life examples • No pictures

These notes cover the complete video, including the smaller technical points that are easy to miss. I'll explain each concept in simple language first, then show you the technical terms you should know as an aspiring AI Engineer.

# Part 1: What is MCP?

## 1. What does MCP stand for?

MCP = Model Context Protocol.

MCP is an open standard that helps AI applications connect to external tools, applications, and data using a common method.

Let's understand each word:

- Model: The AI model, such as GPT, Claude, or Gemini.
- Context: The information and capabilities the AI needs to perform a task.
- Protocol: A set of rules that two systems follow to communicate.

Think of a protocol as a language that two computers agree to speak.

### A small example

Imagine you tell an AI assistant:

> "Check my Gmail and tell me whether I received an interview invitation."

The AI understands English, but that does not automatically mean it can access your Gmail.

It needs a connection to Gmail, permission to read your emails, and a way to understand the available actions.

MCP provides a standardized way for AI applications to discover and use those actions.

It does not automatically grant access. The application must still connect to an appropriate server and obtain the required authorization.

## 2. Why was MCP introduced?

MCP was introduced by Anthropic on November 25, 2024.

Before MCP, developers could already connect applications using APIs.

For example:

- Gmail API — read or send emails.
- GitHub API — access repositories and issues.
- Google Calendar API — create and read events.
- Stripe API — work with payment information.
- Database APIs — retrieve and update stored data.

So why do we need MCP if APIs already exist?

### Before MCP

Imagine you're building an AI assistant that can:

1. Read Gmail.
2. Check Google Calendar.
3. Search GitHub.
4. Retrieve customer details from a CRM.

Each integration may have different documentation, authentication requirements, request formats, and response structures.

You would need to write and maintain custom integration code and explain each tool's capabilities to the AI.

Different AI frameworks might also describe and call these tools differently.

### With MCP

MCP provides a common way for AI applications to:

- Discover which tools are available.
- Understand what each tool does.
- Learn which arguments the tool needs.
- Invoke the tool.
- Receive structured results.

The underlying APIs can remain completely different.

Important: MCP does not remove the need for integration code. It standardizes the interface between AI applications and the systems providing tools.

## 3. Why did MCP become popular?

The video explains that the rise of Agentic AI accelerated MCP adoption.

A normal AI chatbot might answer:

> "Here is how you can send a reminder email."

An AI agent with appropriate tools and permissions can actually perform the workflow:

> "Find everyone registered for tomorrow's event, prepare a reminder, and send it after confirmation."

Agents need access to real applications to complete real work.

As companies such as OpenAI and Google adopted MCP, interest increased.

The uploaded video's cited September 2026 figures include:

- A reported 98× increase in MCP tool calls by ChatGPT users between January and August 2026.
- Railway reporting more than 500% growth in daily MCP users from July to August 2026.

These are different measurements from different sources, not a single growth rate for the entire MCP ecosystem.

# Part 2: MCP Architecture

## 4. The three important components

MCP uses a Host–Client–Server architecture.

| Component  | Simple meaning                                   | Example             |
| ---------- | ------------------------------------------------ | ------------------- |
| Host       | The main AI application                          | Cursor              |
| MCP Client | The component that communicates with MCP servers | Cursor's MCP client |
| MCP Server | Exposes tools and data through MCP               | GitHub MCP server   |

### Host

The host is the application you interact with.

Examples include:

- ChatGPT
- Claude Code
- Cursor
- Codex

A host may manage the AI model, conversation context, permissions, and tool execution.

Think of the host as a school where many activities are organized.

### MCP Client

The MCP client lives inside the host and communicates with MCP servers.

It handles operations such as discovering capabilities, sending requests, and receiving responses.

Think of the client as the student representative who communicates with another department.

One host can maintain connections to multiple MCP servers, typically through separate clients.

### MCP Server

The MCP server exposes capabilities to the client using MCP conventions.

It can be built by:

- The original application provider.
- A third-party developer.
- Your own development team.

For example, a GitHub MCP server might expose tools to search repositories, read files, or create issues.

The server knows how to translate an MCP request into the underlying application's API operation.

Think of it as the department that provides the requested service.

## 5. Complete example: Sending an email

Suppose you tell Cursor:

> "Send a reminder email to everyone registered for tomorrow's AI workshop."

Here is the process:

1\. User

"Send the workshop reminder."

2\. Host — Cursor

Receives your instruction and coordinates the AI model.

3\. MCP Client

Sends the structured tool request.

4\. Gmail MCP Server

Translates the MCP tool request into a Gmail API request.

5\. Gmail API

Performs the authorized email operation.

The result travels back through the MCP server and client to the host.

For example, the server could expose a tool described as:

```
{
  "name": "send_email",
  "description": "Send an email",
  "inputSchema": {
    "type": "object",
    "properties": {
      "to": {"type": "string"},
      "subject": {"type": "string"},
      "body": {"type": "string"}
    },
    "required": ["to", "subject", "body"]
  }
}
```

This is an illustrative tool definition.

The AI can understand the description and provide arguments like:

```
{
  "to": "student@example.com",
  "subject": "AI Workshop Reminder",
  "body": "Your workshop starts tomorrow."
}
```

The MCP server then performs the operation through Gmail's API.

Remember: The AI chooses and supplies arguments for a tool, but the host, client, and server software actually handle communication and execution.

# Part 3: Local MCP vs Remote MCP

## 6. Two ways an MCP server can run

### Local MCP server

A local server runs on your own computer.

For example, you're developing a Python project on your MacBook and want Cursor to access files through an MCP server.

The host can launch the server as a local program.

A common communication method is stdio, meaning standard input and standard output.

Imagine two programs passing written messages to each other on the same computer.

### Remote MCP server

A remote server runs on infrastructure managed by a company or another provider.

Your AI application connects to it over HTTP.

For example, Cursor connects to a remotely hosted GitHub MCP server.

| Feature          | Local MCP                  | Remote MCP                  |
| ---------------- | -------------------------- | --------------------------- |
| Where it runs    | Your computer              | Remote infrastructure       |
| Common transport | stdio                      | HTTP                        |
| Who operates it  | You or your local software | Provider or server operator |
| Credentials      | May be held locally        | May be managed remotely     |
| Example          | Local file tools           | Hosted SaaS integration     |

One subtle point from the video: A local MCP server does not necessarily mean everything happens offline.

A local Gmail MCP server may still call Gmail's API over the internet.

The difference is where the MCP server runs, not necessarily where the actual data lives.

# Part 4: Original MCP Lifecycle

## 7. The four original lifecycle concepts

The video describes four important parts of the earlier MCP lifecycle.

### Step 1: Initialization

The MCP client and server introduce themselves.

They agree on the protocol version and exchange supported capabilities.

It's like two people saying:

> "Hello, which language do you speak, and what can you do?"

### Step 2: Discovery

The client asks:

> "What tools or capabilities do you provide?"

For a Gmail MCP server, the response might describe:

- `search_email`
- `read_email`
- `send_email`

Each tool can include a description and required input arguments.

The AI can then understand which operation matches the user's request.

### Step 3: Request and response

The client sends a structured request to invoke a capability.

The server processes it and returns a structured result.

Example:

Request:

```
{
  "to": "student@example.com",
  "subject": "Reminder",
  "body": "Your event is tomorrow."
}
```

Illustrative response:

```
{
  "status": "success",
  "message": "Email sent"
}
```

### Step 4: Session management

Earlier MCP implementations could use session IDs.

A session ID is like a temporary visitor pass.

Suppose you visit a hospital:

- At reception, you receive token 45.
- At each counter, you show token 45.
- Staff can associate your requests with the same visit.

Similarly, an MCP server could assign a session ID and use it to associate later requests with an ongoing interaction.

This becomes important when we discuss the 2026 changes.

# Part 5: APIs vs MCP vs Skills vs Plugins

These four concepts are related, but they are not the same.

## 8. Understanding the difference

| Concept | What it does                                                   | Simple analogy           |
| ------- | -------------------------------------------------------------- | ------------------------ |
| API     | Lets software interact with an application                     | Kitchen equipment        |
| MCP     | Standardizes how AI applications discover and use capabilities | Standard ordering system |
| Skill   | Teaches an agent how to complete a workflow                    | Recipe                   |
| Plugin  | Packages capabilities together for installation                | Complete cooking kit     |

Let's use the same email example.

### API — Performs the actual operation

The Gmail API provides the underlying capability to send emails.

It is the software interface used to communicate with Gmail.

### MCP — Exposes that capability in a standard way

The Gmail MCP server describes a `send_email` tool.

The AI application can discover and invoke it through MCP.

### Skill — Explains how to perform the workflow

A skill might tell the agent:

"When preparing an event reminder, include the event name, location, joining link, preparation instructions, and a polite closing."

The skill provides reusable instructions, procedures, examples, or scripts.

Skills teach the agent how to do a job. MCP provides a standardized way to use external capabilities.

### Plugin — Packages related capabilities

A plugin might bundle:

- An email integration.
- A reminder-writing skill.
- An MCP server.
- Other workflow resources.

The user installs one package rather than configuring every component separately.

The exact contents depend on the application.

Importantly, skills and plugins do not have to use MCP.

### Putting them together

Imagine you install an event-management plugin.

You say:

> "Send reminders to all attendees for tomorrow's event."

The plugin supplies the relevant integration and workflow instructions. The skill teaches the agent how to prepare the reminder. The MCP connection provides access to the sending tool, and the Gmail API performs the operation.

That is how the different layers can cooperate.

# Part 6: The Four Major MCP Changes in 2026

This is the most important section of the video.

## 9. Change 1 — Stateless MCP

### First, what does state mean?

State means information a system remembers about an ongoing interaction.

Imagine you call a restaurant:

> "I want to order pizza."

The employee remembers your order while you continue talking.

That remembered information is state.

### Stateful MCP

In the earlier session-based model, a server could remember information associated with a session ID.

For example:

- User A sends a request to Server 1.
- Server 1 stores session information.
- User A sends another request.
- The system needs the appropriate session state to continue.

This can become complicated when a service has thousands of users and many server instances.

### Why was this a problem?

Suppose a company runs seven copies of its MCP server.

Your first request reaches Server 1, but your next request reaches Server 7.

Server 7 may not know what Server 1 remembered.

The company could solve this by keeping you connected to the same server or sharing session state between servers, but both approaches add infrastructure complexity.

### Stateless MCP in 2026

The video describes the July 28, 2026 protocol revision moving toward stateless requests.

The initialization handshake and MCP session ID were removed from the protocol-level design described in that revision.

Each request carries the protocol information required to process it rather than depending on a previous protocol session.

Simple meaning: Every request should be understandable on its own at the protocol level.

This does not mean that applications cannot store information or that users cannot have ongoing conversations. It means the protocol no longer requires the same kind of remembered MCP session.

### Benefits

- Easier horizontal scaling.
- Less session-management complexity.
- Requests can be handled by different server instances.
- Better suitability for large production deployments.

Earlier MCP implementations may still use older protocol versions.

## 10. Multi-Round Trip Requests (MRTR)

Stateless requests introduce a new question:

What happens when a server needs additional information before completing the task?

For example, you tell your agent:

> "Send emails to all 500 workshop participants."

The server might need confirmation before sending so many emails.

It should not simply send them without the appropriate approval.

### How MRTR works

Round 1 — Original request

You request that the emails be sent.

Server — Input required

The server responds that it needs confirmation, along with information required to resume the work.

Round 2 — Confirmation

You approve the action. The client sends a new request carrying the necessary continuation information.

Final result

The server completes the operation and returns the result.

### Important technical detail

MRTR is not simply a callback.

A callback usually means a system invokes another system later.

With the MRTR pattern described in the video:

1. The original request finishes.
2. The server returns an input-required response.
3. The response includes continuation state.
4. The client obtains the missing input.
5. The client sends another request.
6. The server resumes processing.

This allows a multi-step interaction without requiring a long-lived MCP session.

Remember: Stateless means independent protocol requests. MRTR allows multiple independent requests to form a longer interaction.

## 11. Change 2 — MCP Tasks

Originally, many MCP operations were relatively short.

For example:

- Search an email.
- Read a file.
- Create a GitHub issue.
- Retrieve a database record.

But modern AI agents can perform much longer jobs.

Imagine telling your AI agent:

> "Analyze 10,000 customer support conversations and prepare a detailed report."

That might take 20 minutes or longer.

It would be inefficient to keep one request open while waiting for the final result.

### What is an MCP Task?

An MCP Task allows long-running work to continue independently of the original request.

The server starts the operation and returns a task handle.

Think of the handle as a tracking number.

For example:

```
{
  "task_id": "TASK-102",
  "status": "working"
}
```

This is an illustrative response, not an exact MCP protocol schema.

Your AI application can use the handle to check what happened to the task.

### Example: Food delivery

When you order food:

1. You place an order.
2. The restaurant gives you an order number.
3. The restaurant continues preparing the food.
4. You check whether it is being prepared or delivered.
5. Eventually, the order is completed.

You do not need to keep the original ordering request open.

MCP Tasks follow a similar idea.

### Task capabilities

The video discusses the ability to:

- Start a long-running task.
- Check its status.
- Provide additional information.
- Request cancellation.
- Retrieve its result.

Two important protocol operations mentioned in the uploaded source are:

`tasks/get` — used for retrieving task status and results.

`tasks/update` — used for providing additional input.

Cancellation is cooperative. Asking to cancel does not guarantee that all underlying work stops immediately.

Also, the Tasks extension must be supported by both the client and server.

### Stateless vs MCP Tasks vs MRTR

| Feature            | Main purpose                                  | Example                               |
| ------------------ | --------------------------------------------- | ------------------------------------- |
| Stateless requests | Avoid relying on stored protocol sessions     | Requests handled by different servers |
| MRTR               | Get additional input across multiple requests | Confirm before sending emails         |
| MCP Tasks          | Handle work that takes a long time            | Analyze 10,000 documents              |

These solve different problems and can complement one another.

## 12. Change 3 — Stronger Security and Authorization

As MCP adoption grew, AI agents gained access to increasingly sensitive systems.

Examples include:

- Company email accounts.
- Internal source code.
- Customer records.
- Private databases.
- CRM systems.
- Financial information.

That creates important security questions:

Who is connecting? What credentials are they using? What are they allowed to do?

### Authentication vs Authorization

These terms are different.

Authentication: Who are you?

Example: You log in using your company account.

Authorization: What are you allowed to access?

Example: You can read customer support tickets but cannot delete customer records.

### Stronger credential verification

The July 2026 specification tightened authorization rules.

The video highlights stronger verification of which authorization server issued a credential and closer binding of credentials to the correct issuer.

Why does this matter?

Imagine someone presents a visitor pass from another company and tries to use it to enter your office.

A secure system should verify that the pass came from an accepted issuer and is valid for the intended purpose.

Similarly, MCP clients and authorization systems must handle credentials carefully.

### Client metadata documents

The video also explains a move away from relying on dynamic client registration with each authorization server.

Instead, client metadata documents provide a more predictable way for authorization systems to identify clients.

Think of a client metadata document as a standardized identification card describing the application.

It helps the authorization system recognize which client is requesting access.

## 13. Enterprise-Managed Authorization

Imagine a company with 5,000 employees.

Each employee might use AI agents connected to approved internal tools.

Without centralized management, employees might have to authorize integrations individually.

That becomes difficult to manage and audit.

Enterprise-Managed Authorization allows a company to provision access centrally through its existing identity provider.

An identity provider is the system a company uses to manage employee logins and identities.

For example, a company may decide:

- Software engineers can use the GitHub MCP server.
- HR employees can use the HR system integration.
- Finance employees can use approved finance tools.
- Other employees cannot access those systems.

Employees use their company identity to access permitted services, subject to organizational policies.

The video states that Enterprise-Managed Authorization became stable in 2026.

### Security risks that still remain

Authorization does not eliminate every security risk.

The video specifically mentions:

Prompt injection: Malicious instructions hidden in content an AI agent reads.

Example: A document contains text telling the agent to ignore its original task and disclose private information.

Tool poisoning: Malicious or misleading tool descriptions or outputs that manipulate how an agent behaves.

Data exfiltration: Sensitive information being transferred to an unauthorized destination.

Inappropriate tool use: An agent performs an action that it should not have performed, even if it technically has access to the tool.

The uploaded source also mentions NSA security guidance published in May 2026 for MCP-based automation.

The important lesson is that authentication and authorization are necessary, but they are not complete AI-agent security solutions.

An AI Engineer must also consider permission boundaries, tool trust, prompt injection, validation, and approval for sensitive operations.

## 14. Change 4 — Production Readiness

Production readiness means making MCP suitable for real applications used by many people, not just small demonstrations.

The video describes four important improvements.

### A. Formal extension system

An extension is an optional capability added to a system.

Imagine buying a basic smartphone and installing extra applications only when you need them.

Similarly, MCP can keep a core protocol while optional capabilities evolve separately.

Examples include:

- MCP Tasks.
- MCP Apps.
- Enterprise-Managed Authorization.

This means every client and server does not have to implement every optional feature.

It also allows new functionality to develop without constantly expanding the core protocol.

### B. MCP Apps

Previously, many MCP integrations primarily returned text or structured data.

MCP Apps allow servers to provide richer, interactive interfaces that compatible hosts can display.

For example, suppose you ask:

> "Show me the sales performance for the last three months."

A conventional tool might return:

```
{
  "july": 120,
  "august": 150,
  "september": 180
}
```

With an MCP App, a supporting host could display an interactive sales dashboard instead.

Other examples from the video include:

- Interactive dashboards.
- Forms for user input.
- Customer views inside CRM workflows.

Important: The host must support the relevant MCP Apps capability. A server cannot assume every AI application will render its interface.

### C. Caching

Caching means temporarily keeping reusable information so that it does not have to be fetched repeatedly.

Imagine an MCP server exposes 100 tools.

The client asks for the tool catalogue.

If the catalogue has not changed, repeatedly downloading the same list wastes time and resources.

With caching information, a client can reuse suitable previously retrieved information.

Benefits include:

- Fewer unnecessary requests.
- Reduced overhead.
- Faster repeated discovery.
- Better scalability.

Caching must still respect freshness and access permissions.

### D. Easier request handling

The 2026 changes also made request information easier for infrastructure to inspect.

For example, systems handling MCP traffic can more clearly identify details such as:

- The request method.
- The tool being called.

This helps infrastructure route, observe, and manage traffic.

It is especially useful when many agents are making requests to many tools.

### E. Formal deprecation policy

Deprecation means a feature is being phased out.

Imagine a programming library announces that a function will be removed in a future release.

Developers need time to update their applications.

The video explains that MCP introduced a formal deprecation policy with a minimum 12-month window before deprecated functionality is removed.

This gives companies time to plan migrations and reduces the risk of sudden breaking changes.

# Part 7: One Complete Real-World Example

Let's combine almost everything you learned.

Imagine you build an AI Internship Assistant.

You ask it:

> "Check my Gmail for interview invitations, find the related company on GitHub, and prepare a reminder in my calendar."

The application could work as follows.

Host: Cursor or another MCP-capable AI application.

MCP Clients: The components managing connections to Gmail, GitHub, and Calendar MCP servers.

MCP Servers: Services exposing email, repository, and calendar capabilities.

APIs: Gmail API, GitHub API, and Google Calendar API perform the underlying operations.

Skill: A reusable workflow explains how to identify interview emails, extract dates, avoid duplicate reminders, and ask for confirmation.

Plugin: An optional installable package could bundle the integration and skill.

Discovery: The AI application learns which tools are available and which arguments they need.

Request and response: The client calls the relevant tools and receives their results.

Stateless operation: Requests do not depend on an MCP protocol session being maintained on a particular server.

MRTR: If confirmation is required before creating a calendar event, the interaction can continue through another request.

MCP Task: If the agent needs to process thousands of emails, a supported Tasks extension could manage the long-running operation.

Authorization: The agent can access only the data and actions permitted by the relevant credentials and policies.

MCP Apps: A compatible host could display an interactive review interface for proposed calendar events.

Caching: Reusable tool catalogue information can be cached when appropriate.

This example demonstrates how the concepts fit together in an actual AI application.

# Part 8: Interview Preparation

## 15. Important MCP questions and answers

1\. What is MCP?

MCP is an open standard that allows AI applications to discover and use external tools and data through a consistent protocol.

2\. Does MCP replace APIs?

3\. What is the difference between an MCP host, client, and server?

4\. What is the difference between local and remote MCP servers?

5\. What is MCP discovery?

6\. What is stateless MCP?

7\. What is MRTR?

Multi-Round Trip Requests allow an interaction to continue across separate requests when additional input is needed.

8\. What are MCP Tasks?

Tasks are an optional mechanism for long-running operations. A server can return a task handle that a client uses to check progress and retrieve results.

9\. How are skills different from MCP?

10\. What are MCP Apps?

11\. What is Enterprise-Managed Authorization?

12\. What are the main security risks?

13\. What changed in MCP in 2026?

The major changes covered in the video are stateless requests and MRTR, long-running Tasks, stronger authorization, and production features such as extensions, MCP Apps, caching, and deprecation policies.

# Part 9: Test What You Learned

MCP Knowledge Check

1 of 7 answered

1\. Which component actually exposes tools through MCP?

Host

MCP Client

MCP Server

AI model

2\. Does MCP replace Gmail's API?

Yes

No

Only for remote servers

Only in 2026

3\. What problem does MRTR primarily solve?

Training models

Handling additional input across requests

Encrypting databases

Caching tool lists

4\. Which feature is most appropriate for a two-hour document analysis?

MCP Tasks

Tool discovery

Client metadata

stdio

5\. What does a skill primarily provide?

An API server

A database

Reusable workflow instructions

An authorization token

6\. What does Enterprise-Managed Authorization help organizations do?

Train LLMs

Manage access centrally

Remove all security risks

Replace APIs

7\. What is the purpose of MCP caching?

Avoid repeatedly fetching unchanged information

Store all private emails permanently

Replace authorization

Train the agent

Check answers

# Part 10: Final Revision Sheet

| Term                     | What to remember                                                          |
| ------------------------ | ------------------------------------------------------------------------- |
| MCP                      | Standardized connection between AI applications and external capabilities |
| Host                     | Main AI application                                                       |
| Client                   | Communicates with MCP servers                                             |
| Server                   | Exposes capabilities through MCP                                          |
| API                      | Underlying application interface                                          |
| Skill                    | Reusable instructions for a workflow                                      |
| Plugin                   | Installable bundle of capabilities                                        |
| stdio                    | Common local-server transport                                             |
| HTTP                     | Common remote-server transport                                            |
| Initialization           | Earlier protocol version/capability handshake                             |
| Discovery                | Finding available capabilities                                            |
| Request/response         | Invoking capabilities and receiving results                               |
| Session ID               | Earlier mechanism for associating requests                                |
| Stateless                | No required ongoing MCP protocol session                                  |
| MRTR                     | Additional input across multiple requests                                 |
| Tasks                    | Long-running operations with task handles                                 |
| Authorization            | Controls what access is permitted                                         |
| Client metadata          | Standardized client identification information                            |
| Enterprise authorization | Centralized organizational access management                              |
| Extensions               | Optional protocol capabilities                                            |
| MCP Apps                 | Interactive interfaces in compatible hosts                                |
| Caching                  | Avoid repeated unnecessary catalogue retrieval                            |
| Deprecation policy       | Planned removal with a minimum notice window                              |

## The most important takeaway

MCP originally focused on making it easier for AI applications to connect to external tools and data.

In 2026, the focus expanded to making those connections suitable for large-scale, long-running, secure, production-grade AI systems.

For your AI Engineer learning, the practical goal is to understand not just how to connect an MCP server, but how tool discovery, authorization, request handling, security, and long-running operations work together.

