Programming has been an act of translation: understand the problem in human terms, design a solution in abstract terms, then render it in syntax a machine can execute.
For decades, the developer's primary interface with the machine has been syntax: curly braces, semicolons, type annotations, 
and the precise grammar of programming languages. That era is ending.

A new paradigm has arrived in which developers express what they want to build rather than how to build it. The machine handles implementation. 
The human provides intent, architecture, and judgment.

This shift didn't happen overnight. It began with autocomplete - simple token prediction in the editor. Then came inline code suggestions that could 
complete entire functions. Next, chat-based interfaces allowed developers to describe features in natural language and receive working implementations. 
Now, fully autonomous agents can clone repositories, plan multi-file changes, 
execute them in sandboxed environments, run tests, and submit pull requests - all without a human typing a single line of code.


Agent

An AI agent is a software system that perceives a goal, plans steps to reach it, takes actions through tools, observes the results, 
and iterates until the goal is met or it hits a stopping condition.

Every agent, however simple or sophisticated, is built from five parts.
• The model is the reasoning engine. It reads the current context, decides what should happen next, and produces the next thought, 
  the next tool call, or the next message.
• Tools connect the model to the world. They include APIs the agent can call, code it can execute, databases it can query, and other agents it can delegate to.
• Memory is the state. It allows the agent to recall past interactions, retrieve projec tspecific rules, and retain context across sessions 
 so it never starts from a blank slate.
• Orchestration is the code that runs the loop. It assembles the context for each model call, dispatches tool calls, captures their results, 
and decides whether to continue.
• Deployment is what turns the prototype into a service: hosting, identity, observability, and the production infrastructure the agent runs on.

Agentic Engineering

https://github.com/santoshkumar-devop/SDLC_With_Vibe_Coding/blob/main/vibe_coding.png

The single biggest differentiator between the two ends is how outputs get verified. 
In vibe coding, verification is optional; the developer runs the code and checks if it seems right. 

In agentic engineering, two mechanisms work together. Tests verify the deterministic parts of the system: a function given this input produces that output. 
Evaluations, or evals, verify the parts that are not deterministic: did the agent take the right trajectory of steps, choose the right tools, 
and produce a final response that meets the quality bar. Tests are checked by code; evals are checked by labelled datasets, scoring rubrics, and LM judges. 
Without both, the practice is always vibe coding, regardless of how sophisticated the prompts are.


Context engineering

The quality of AI-generated code depends less on the cleverness of your prompts and more on the quality of the context provided. 
This realization has given rise to the concept of context engineering, the practice of providing AI agents with rich, structured information about your codebase, 
architecture, conventions, and intent

Developers must consider six primary types of context:
• Instructions: The agent's core role, goals, and operational boundaries.
• Knowledge: Retrieved documents, architectural diagrams, and domain-specific data.
• Memory: Short-term session logs (what just happened) and long-term persistent state(what the project is).
• Examples: Few-shot behavioral demonstrations and codebase reference patterns.
• Tools: The precise definitions of the APIs, scripts, and external services the agent can invoke.
• Guardrails: Hard constraints, formatting rules, and safety validations.

In AI code generation, context engineering involves carefully balancing which of these six elements the agent possesses upfront versus what it can retrieve 
on demand. This creates a critical separation between static and dynamic context.

Static context is always loaded: system instructions, rule files (AGENTS.md, CLAUDE.md, GEMINI.md), global memory, and persona definitions. 
It defines who the agent is and how it behaves. Static context is expensive because every token is present in every interaction, regardless of relevance.

Dynamic context is loaded on demand: skill instructions triggered by task matching, tool results retrieved during execution, documents fetched from RAG pipelines,
and windowed session history. Dynamic context is efficient because the agent pays the token cost only when the information is needed.

https://github.com/santoshkumar-devop/SDLC_With_Vibe_Coding/blob/main/context_engineering.png

Context engineering is the bridge between vibe coding and agentic engineering.


New SDLC

https://github.com/santoshkumar-devop/SDLC_With_Vibe_Coding/blob/main/AI_driven_SDLC.png


The factory model: building the system that builds software

The mental model that ties these transformations together is what we call the factory model.
In this model, the developer's primary output is not code - it's the system that produces code. This system includes:
• Specifications and context that define what needs to be built
• Agents that translate specifications into implementation
• Tests and quality gates that verify correctness
• Feedback loops that route failures back to agents for correction
• Guardrails that constrain agents to safe, predictable behavior

If the developer is the factory manager, the AI model is merely the raw engine on the factory floor. An engine on its own cannot manufacture a car; it needs belts, gears, safety sensors, and an assembly line. In the context of AI-assisted development, this surrounding machinery is known as the Harness.

https://github.com/santoshkumar-devop/SDLC_With_Vibe_Coding/blob/main/factory_model.png

Harness Engineering: What surrounds the model

Agent = Model + Hareness
https://github.com/santoshkumar-devop/SDLC_With_Vibe_Coding/blob/main/Hareness.png


What's in the harness

Concretely, a harness includes:
• Instructions and Rule Files: The text that defines who the agent is, what it cares about,
and what it is forbidden from doing. This includes AGENTS.md, CLAUDE.md, GEMINI.md, skill files, and sub-agent prompts.
• Tools: The functions, MCP servers, and APIs the agent can call, plus the prose around them that tells the model when and how to call them.
• Sandboxes and execution environments: Where the agent's code actually runs, what it has access to, what it cannot reach.
• Orchestration logic: Sub-agent spawning, model routing, hand-offs between specialists, and the rules that govern when each one fires.
• Guardrails or Hooks: Deterministic code that runs at specific lifecycle points: before a
tool call, after a file edit, before a commit. Hooks are the place for things the agent should never forget but often does.
• Observability: Logs, traces, evaluations, cost and latency metering. Without
observability, there is no way to tell whether the agent is doing well or quietly drifting.


Harness in SDLC

1. Requirements, Planning, & Architecture (Configuring the Harness)
This is where the harness is configured and calibrated. Before the AI writes any production code, the developer must set up the agent's environment.
• Harness Configuration: Providing the Instructions and Rule Files (e.g., creating theAGENTS.md and defining architectural constraints)
that the harness will load and make available to the model.
• The Action: The developer defines the tools the agent will have access to (like specific APIs or database schemas) and sets the fundamental rules the agent cannot break.

2. Implementation (Running the Harness)
During active coding, the harness acts as the boundary that keeps the AI model focused, secure, and productive.
• Harness Components Used: Sandboxes, Execution Environments, and Tools.
• The Action: As the model generates code, it executes it within the harness's isolated sandbox. If the model needs to read a file or search the web, it uses the tools provided by the harness.

3. Testing & QA (The Feedback Loop)
Testing in an agentic workflow relies heavily on the harness to facilitate autonomous self-correction.
• Harness Components Used: Orchestration Logic and Guardrails.
• The Action: When the agent writes a function, the harness provides the execution environment (such as a sandboxed terminal) that allows the automated tests to be
executed. If a test fails, the orchestration logic captures the error output from that environment and routes it back to the model, asking it to try again. The harness is what creates this automated 'think -> act -> observe' loop."

4. Code Review, Deployment, & Maintenance (Observing the Harness)
Even after the code is written, the harness ensures the agent behaves safely in live or near-live environments.
• Harness Components Used: Hooks and Observability.
• The Action: The harness runs deterministic hooks (e.g., blocking a commit if the agent tries to push a hard-coded password). Furthermore, the observability layer tracks token costs, latency, and agent drift, allowing human engineers to audit exactly why an agent made a specific deployment decision.

https://github.com/santoshkumar-devop/SDLC_With_Vibe_Coding/blob/main/two_modes.png


The conductor: hands-on, real-time direction

In conductor mode, a developer works in real-time with an AI pair-programmer. They're in the IDE, watching code appear, guiding the AI with prompts and corrections, and maintaining fine-grained control over what gets written. The AI is a powerful instrument, but the developer is actively directing every movement.

This mode is typical when working on complex logic, debugging tricky issues, or working in unfamiliar codebases where the developer needs to understand each change as it's made. Tools like GitHub Copilot, Google's Gemini Code Assist, Cursor, and Windsurf primarily support this mode through inline completions, chat interfaces, and edit-in-place capabilities.

The conductor mode is natural for developers who come from traditional engineering backgrounds. It preserves the sense of understanding and control that many engineers value. The risk is that it can also become a bottleneck - if the developer is personally directing every keystroke, the throughput improvement from AI is limited.

The orchestrator: async, multi-agent delegation

In orchestrator mode, the developer operates at a higher level of abstraction. They define goals, assign them to agents, and review results - but they're not watching code appear line by line. Agents may be working in the background, in parallel, on different parts of a codebase. The developer checks in periodically, reviews output, and provides course corrections.

This mode is typical for well-defined tasks like bug fixes, feature implementations against established patterns, codebase migrations, and test generation. Tools like Google's Jules, GitHub Copilot's agent mode, Cursor's background agents, and Claude Code support this mode through async task execution, often working in sandboxed environments with full access to the repository, build tools, and test suites.

The orchestrator mode requires a different skill set. Instead of deep expertise in syntax and language idioms, it demands strong skills in:
• Specification: Defining tasks precisely enough that an agent can execute them without ambiguity
• Decomposition: Breaking large tasks into appropriately sized units for agent execution
• Evaluation: Quickly assessing whether agent output meets quality standards
• System design: Designing the constraints, tests, and feedback loops that keep agents productive


The Economics of AI Development

When evaluating the impact of AI on the software development life cycle, the conversation often begins and ends with developer velocity: how fast can we write code? However, for engineering leaders, the more critical metric is the Total Cost of Ownership (TCO).

To understand the true cost of AI-assisted development, we must look at how different workflows shift the financial and operational burdens between Capital Expenditure (CapEx)— the upfront investment to build something—and Operational Expenditure (OpEx)—the ongoing cost to run, fix, and maintain it. Crucially, in the AI era, OpEx is heavily dictated by the token economy.


For individual developers

1. Set up an AGENTS.md (or equivalent) for the project. Pick the convention that matches the coding agent of choice. Start with ten lines: stack, conventions, hard rules, workflow. Add a rule every time the agent does something it should not do again.
2. Install a set of skills for your coding agents (like Agents CLI) to build, evaluate, deploy and optimize agents.
3. Pick one repetitive workflow and make it the first agent. A research workflow, a code review process, a recurring report, a piece of content produced regularly. Use a coding agent for the prototype, and graduate it to a production agent through Agents CLI when it earns its keep. Building one agent end to end teaches more than reading about a hundred.
4. Write the tests and evals before generating the code. Together they are the contract with the AI. A well-written test and eval suite communicates intent more precisely than any natural-language prompt, and turns AI-assisted development from vibe coding into agentic engineering.
5. Review every line the agent produces that is going to ship. Be skeptical of anything that looks clever. Check imports for real packages. Verify that error handling covers realistic failure modes. Code that the team does not understand becomes debugging cost the team cannot afford.
6. Maintain your developer skills. AI handles the routine so the developer can focus on the challenging. That arrangement only works if the foundational skills, debugging, system design, intuition for performance and correctness, stay sharp. Treat AI as a way to apply expertise at a greater scale, not as a substitute for it. Regular practice with complex debugging, code review of AI output, and architecture discussions stay essential to growing as an engineer.
