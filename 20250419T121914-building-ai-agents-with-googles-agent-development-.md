---
title: Building AI Agents with Google’s Agent Development Kit (ADK) as MCP Client — A Deep Dive (Full Code)
origin_url: https://medium.com/google-cloud/building-ai-agents-with-googles-agent-development-kit-adk-as-mcp-client-a-deep-dive-full-54d683713afe
date: 2025-04-19T12:19:23.255Z
---

# Building AI Agents with Google’s Agent Development Kit (ADK) as MCP Client — **A Deep Dive (Full Code)**

Google Cloud Next ’25 unveiled several groundbreaking announcements, and I had the privilege of attending the event in person. It was an inspiring experience filled with innovation, hands-on demos, product showcases, and insightful discussions.

While last year’s spotlight was on GenAI (chatbots) and Vertex AI, this year’s theme was loud and clear — **Agents , Agents and more Agents** .From [Agent Development Kit (**ADK** )](https://google.github.io/adk-docs/get-started/installation/) to Agent to Agent (**A2A**) Protocol and **AgentSpace** along with Google Flagship LLM — Gemini 2.5 Pro Preview

In my [**previous article**](/google-cloud/model-context-protocol-mcp-with-google-gemini-llm-a-deep-dive-full-code-ea16e3fac9a3), I explored how to integrate the Gemini LLM using the **Model Context Protocol (MCP)** acting as **MCP client**, enabling structured, tool-augmented interactions with external APIs and systems. We also discussed the core Concept of MCP , use-case and along with demo.

In this article, our focus will be leveraging **existing MCP Servers** through **ADK agent** acting as **MCP client** using Gemini LLM , enabling tool invocation using capabilities provided by external MCP-powered tools. We will discuss the core concepts of **ADK** before diving into code implementation.

# **What is ADK — Agent Development Kit ?**

**Agent Development Kit (ADK)** is an open-source, code-first Python toolkit for building, evaluating, and deploying intelligent AI agents.

> ADK enables developers to create agentic workflows — from simple single-agent tasks to complex multi-agent orchestration — all within a modular and extensible framework.

# **What are Agents in ADK ?**

An **Agent** is an autonomous, self-contained execution unit designed to achieve specific goals. Agents can

1.  Perform tasks,
2.  Interact with users,
3.  Leverage external tools, and
4.  Collaborate with other agents to complete complex workflows.

![image](https://github.com/user-attachments/assets/12f4695a-8769-4db2-92ba-f54064d2f84d)

[https://google.github.io/adk-docs/agents/](https://google.github.io/adk-docs/agents/)

# **Core Agent Categories**

ADK offers three primary agent types to support

*   **LLM Agents** _(e.g., LlmAgent, Agent)_: Use LLMs to understand, reason, plan, and act — ideal for dynamic, language-driven tasks.
*   **Workflow Agents** _(e.g., SequentialAgent, ParallelAgent, LoopAgent)_: Orchestrate other agents in predictable patterns without relying on an LLM for flow control — best for structured, repeatable processes.
*   **Custom Agents**: Built by extending BaseAgent to enable custom logic, specialized workflows, or unique tool integrations — perfect for advanced, tailor-made solutions.

In this article we will be using [**LLM Agents**](https://google.github.io/adk-docs/agents/llm-agents/) **type** with **MCPTools**

# **What is Tools in context with ADK ?**

A **Tool** represents a specific capability granted to an AI agent, allowing it to perform actions and interact with the external world beyond basic text generation and reasoning.

> A tool is usually a modular code component — such as a Python function, class method, or even another agent — designed to carry out a defined task.

# How Agents Use Tools ?

Agents dynamically leverage tools through **function-calling mechanisms**, where the LLM reasons over context, selects and invokes the appropriate tool with generated inputs, observes the result, and integrates the output into its next action or response.

# Tool Types in ADK

ADK supports several types of Tool type

1.  **Function Tools**: Custom tools built specifically for our application’s unique logic and workflows.

*   **Functions/Methods**: Standard synchronous Python functions (def) or class methods registered as tools.
*   **Agents-as-Tools**: Use specialized agents as callable tools within a parent agent for modular behavior.
*   **Long-Running Function Tools**: Tools designed for asynchronous or time-intensive operations.

**2\. Built-in Tools**: Predefined tools included in the framework for tasks like web search, code execution, or RAG.

**3\. Third-Party Tools**: Easily integrate tools from popular ecosystems like LangChain or CrewAI.

# Architecture

We will be using same architecture as earlier article and in addition , we will be using ADK here

![image](https://github.com/user-attachments/assets/8259a252-ab1e-4d82-8a1f-92aebaaf3045)

Image by Author

# Implementation

Let us dive into building this pipeline with ADK + MCP + Gemini AI by breaking down into key implementation steps

# Pre-Requisites

1.  **Python 3.8+** installed

2\. Google [**Gemini**](https://makersuite.google.com/app) Generative AI access via API key

3\. A valid [**SerpAPI key**](https://serpapi.com/) (used to fetch live flight data)

# Step 1 : Setup virtual environment

## **Install the dependancies using**

\# Setup virtual environment (mac or Unix )  
python -m venv venv && source venv/bin/active   
  
\# Install agent development kit  
pip install google-adk  
  
\# Install MCP server   
pip install mcp-flight-search  
  
\# Install GenAI Python SDK  
pip install google-genai

**google-sdk** : Google’s Agent Development Kit for building agents.

**google-genai** : Google’s library for interacting with Generative AI models (like Gemini).

**mcp-flight-search** : MCP server which uses SerpAPI to fetch flight using MCP library

![image](https://github.com/user-attachments/assets/2433a7d6-94ed-47a7-88c7-b7826abc8dc1)

**Set Environment variables :**

Note the difference here : instead of GEMINI\_API\_KEY , it will be GOOGLE\_API\_KEY in ADK

export GOOGLE\_API\_KEY="your-google-api-key"  
export SERP\_API\_KEY="your-serpapi-key"

# Step 2: Install the MCP Server — mcp-flight-search

To enable Gemini to interact with real-world APIs, we’ll use an MCP-compliant server

For this article, we’ll use [**mcp-flight-search**](https://github.com/arjunprabhulal/mcp-flight-search) — a lightweight MCP Server built using [FastMCP](https://github.com/jlowin/fastmcp) which exposes a tool that searches real-time flight data using the SerpAPI.

Verify the installed MCP server package [https://pypi.org/project/mcp-flight-search/](https://pypi.org/project/mcp-flight-search/)

\# Install from PyPI ( Already Installed from Step 1)  
pip install mcp-flight-search

# Step 3 : Understanding ADK as MCP Client

from google.adk.agents.llm\_agent import LlmAgent  
from google.adk.runners import Runner  
from google.adk.sessions import InMemorySessionService  
from google.adk.tools.mcp\_tool.mcp\_toolset import MCPToolset, StdioServerParameters

**LlmAgent** — is a core component in ADK, acting as the “thinking” part of our application which leverages the power of a Large Language Model (LLM) for reasoning, understanding natural language, making decisions, generating responses, and interacting with tools.

![image](https://github.com/user-attachments/assets/c6f15d2f-52f3-4f48-bd7c-91b6348b8070)

**Runner** is responsible for coordinating the interactions between various components during an agent’s lifecycle.Runner uses in-memory implementations for artifact, session, and memory services, providing a lightweight and self-contained environment for agent execution.

![image](https://github.com/user-attachments/assets/c263a827-5992-4a1e-81dd-d02d8a7a199a)

**InMemorySessionService** is an implementation of ADK’s SessionService interface that stores all session data — such as conversation history, state, and metadata — directly in the application’s memory. This means that all session information is lost when the application stops or restarts.

When a user interacts with an AI agent, a Session object is created to track the conversation.

![image](https://github.com/user-attachments/assets/ba1e0a92-9c1d-400b-930f-acf6356360d6)

**MCPToolSet —** is a class within ADK that enables our AI agents to connect with external MCP servers. These servers expose tools — such as APIs or services — that agents can utilize to perform specific tasks. By using MCPToolset, agents can discover, invoke, and manage these external tools seamlessly.

![image](https://github.com/user-attachments/assets/8834a2e0-8a8a-4baf-9863-7520c32d076b)

**StdioServerParameters** is a configuration class used to specify how the agent should connect to an MCP server via standard input/output streams. This is particularly useful when the MCP server is a local process that communicates through the console.

**How They MCPToolSet and StdioServerParameters Work Together ?**

When combined, MCPToolset and StdioServerParameters allow an ADK agent to:

1.  **Establish a Connection**: Using StdioServerParameters, define the command and arguments needed to start the MCP server process.
2.  **Discover Available Tools**: MCPToolset connects to the MCP server and retrieves a list of available tools that the agent can use.
3.  **Integrate Tools into the Agent**: The discovered tools are adapted into a format compatible with ADK agents, allowing seamless invocation during agent execution.
4.  **Manage the Connection Lifecycle**: MCPToolset handles the setup and teardown of the connection to the MCP server, ensuring resources are managed appropriately.

# **Step 4 : Connecting to MCP Server**

StdioServerParameters defines the MCP configuration to list and listen async using MCPToolSet

\# --- Step 1: Get tools from MCP server ---  
async def get\_tools\_async():  
    """Gets tools from the Flight Search MCP Server."""  
    print("Attempting to connect to MCP Flight Search server...")  
    server\_params = StdioServerParameters(  
        command="mcp-flight-search",  
        args=\["--connection\_type", "stdio"\],  
        env={"SERP\_API\_KEY": os.getenv("SERP\_API\_KEY")},  
    )  
      
    tools, exit\_stack = await MCPToolset.from\_server(  
        connection\_params=server\_params  
    )  
    print("MCP Toolset created successfully.")  
    return tools, exit\_stack

# Step5 : Agent creation from ADK

As mentioned above , we are using Llm agent which is thinking part of application

\# --- Step 2: Define ADK Agent Creation ---  
async def get\_agent\_async():  
    """Creates an ADK Agent equipped with tools from the MCP Server."""  
    tools, exit\_stack = await get\_tools\_async()  
    print(f"Fetched {len(tools)} tools from MCP server.")  
      
    # Create the LlmAgent matching the example structure  
    root\_agent = LlmAgent(  
        model=os.getenv("GEMINI\_MODEL", "gemini-2.5-pro-preview-03-25"),  
        name='flight\_search\_assistant',  
        instruction='Help user to search for flights using available tools based on prompt. If return date not specified, use an empty string for one-way trips.',  
        tools=tools,  
    )  
      
    return root\_agent, exit\_stack

# **Step 6 : Integrating Agent creation , session management and orchestration with runner**

async def async\_main():  
    # Create services  
    session\_service = InMemorySessionService()  
  
    # Create a session  
    session = session\_service.create\_session(  
        state={}, app\_name='flight\_search\_app', user\_id='user\_flights'  
    )  
  
    # Define the user prompt  
    query = "Find flights from Atlanta to Las Vegas 2025-05-05"  
    print(f"User Query: '{query}'")  
      
    # Format input as types.Content  
    content = types.Content(role='user', parts=\[types.Part(text=query)\])  
  
    # Get agent and exit\_stack  
    root\_agent, exit\_stack = await get\_agent\_async()  
  
    # Create Runner  
    runner = Runner(  
        app\_name='flight\_search\_app',  
        agent=root\_agent,  
        session\_service=session\_service,  
    )  
  
    print("Running agent...")  
    events\_async = runner.run\_async(  
        session\_id=session.id,   
        user\_id=session.user\_id,   
        new\_message=content  
    )  
  
    # Process events  
    final\_content = None  
    async for event in events\_async:  
        print(f"Event received: {event}")  
  
      
    # Always clean up resources  
    print("Closing MCP server connection...")  
    await exit\_stack.aclose()  
    print("Cleanup complete.")

# Step 7 : Demo

User Query in MCP Client :

query = "Find flights from Atlanta to Las Vegas 2025-05-05"

**Demo with Standard Logging**

![image](https://github.com/user-attachments/assets/b7832637-db22-43fd-876c-471b402bc7b3)


**Demo with Debug logging**

![image](https://github.com/user-attachments/assets/9e1d0b82-bebd-4876-8c09-6d3eaa36ada6)


# **Key Considerations for Integrating MCP with ADK**

1.  **MCP vs. ADK**

*   **MCP is an** open protocol that standardizes how AI models interact with external tools and data sources.
*   **ADK is a** Python-based framework for building and deploying AI agents.
*   **MCPToolset**: Bridges MCP and ADK by enabling ADK agents to consume tools exposed by MCP servers.

> To build an MCP server in Python, use the model-context-protocol library.

2\. **Tool Types and Integration**

*   **ADK Tools**: Python objects (e.g., BaseTool, FunctionTool) designed for direct use within ADK agents.
*   **MCP Tools**: Capabilities exposed by MCP servers, which **MCPToolset** adapts for use within ADK agents.
*   **Third-Party Tools**: Libraries like **LangChain and CrewAI** offer tools that can be integrated into **ADK using wrappers like LangchainTool and CrewaiTool.**

3\. **Asynchronous Architecture**

*   Both ADK and the MCP Python library are built on Python’s asyncio framework.
*   Tool implementations and server handlers should be asynchronous (async def) to ensure non-blocking operations.

4\. **Stateful Sessions in MCP**

*   MCP establishes persistent, stateful connections between clients and servers, unlike typical stateless REST APIs.
*   This statefulness allows for context retention across interactions but requires careful session management.

5\. **Deployment Considerations**

*   The persistent nature of MCP connections can pose challenges for scaling and deployment, especially for remote servers handling multiple users.
*   Infrastructure considerations include load balancing and session affinity to maintain connection stability.

6\. **Managing MCP Connections in ADK**

*   MCPToolset manages the lifecycle of MCP connections within ADK.
*   Using an exit\_stack ensures that connections are properly terminated when the agent’s execution completes.

**Troubleshooting :**

1.  By default , adk library expects GCP Project Vertex AI , location and VertexAI Configs . Ensure to use GOOGLE\_API\_KEY instead of GEMINI\_API\_KEY as the error message may not clearly indicate missing env. value

**Solution** : Ensure to set variable as **GOOGLE\_API\_KEY**

ValueError: Missing key inputs argument! To use the Google AI API,provide (\`api\_key\`) arguments. To use the Google Cloud API, provide (\`vertexai\`, \`project\` & \`location\`) arguments.

2\. Frequent 429 rate-limit error and 500 internal server .

**Solution** : Switch to Gemini 2 Flash as _“_[**_The Gemini API “free tier” is offered through the API service with lower rate limits for testing purposes_**](https://ai.google.dev/gemini-api/docs/pricing)**_”_**

google.genai.errors.ClientError: 429 RESOURCE\_EXHAUSTED. {'error': {'code': 429, 'message': 'You exceeded your current quota, please check your plan and billing details. For more information on this error, head to: https://ai.google.dev/gemini-api/docs/rate-limits.', 'status': 'RESOURCE\_EXHAUSTED', 'details': \[{'@type': 'type.googleapis.com/google.rpc.QuotaFailure', 'violations': \[{'quotaMetric': 'generativelanguage.googleapis.com/generate\_content\_free\_tier\_requests', 'quotaId': 'GenerateRequestsPerDayPerProjectPerModel-FreeTier', 'quotaDimensions': {'model': 'gemini-2.0-pro-exp', 'location': 'global'}, 'quotaValue': '25'}\]}, {'@type': 'type.googleapis.com/google.rpc.Help', 'links': \[{'description': 'Learn more about Gemini API quotas', 'url': 'https://ai.google.dev/gemini-api/docs/rate-limits'}\]}, {'@type': 'type.googleapis.com/google.rpc.RetryInfo', 'retryDelay': '27s'}\]}}  
  
An error occurred during execution: 500 INTERNAL. {'error': {'code': 500, 'message': 'An internal error has occurred. Please retry or report in https://developers.generativeai.google/guide/troubleshooting', 'status': 'INTERNAL'}}

# **Gemini API Cost Billing**

> [**Gemini 2.5 Pro Preview**](https://ai.google.dev/gemini-api/docs/pricing) **—** As per google docs , Gemini API “free tier” is offered through the API service with lower rate limits for testing purposes. The Gemini API “paid tier” comes with [higher rate limits](https://ai.google.dev/gemini-api/docs/rate-limits), additional features, and different data handling.

# Official Documentation References

*   [**Agent Development Kit (ADK) Documentation**](https://google.github.io/adk-docs/) — Complete guide to the open-source AI agent framework integrated with Gemini and Google.
*   [**LLM Agents in ADK**](https://google.github.io/adk-docs/agents/llm-agents/) — Detailed documentation on implementing LLM Agents, including defining identity, instructions, tools, and advanced configuration.
*   [**MCP Tools with ADK**](https://google.github.io/adk-docs/tools/mcp-tools/#1-using-mcp-servers-with-adk-agents-adk-as-an-mcp-client) — Specific guidance on using ADK as an MCP client to connect to MCP servers.

# GitHub Repository:

You can access all the code used in this tutorial in my GitHub:

[

## GitHub - arjunprabhulal/adk-python-mcp-client

### Contribute to arjunprabhulal/adk-python-mcp-client development by creating an account on GitHub.

github.com

](https://github.com/arjunprabhulal/adk-python-mcp-client?source=post_page-----54d683713afe---------------------------------------)

# Conclusion

In this article, we explored how to use open source ADK to build an agent setup assistant using Model Context Protocol (MCP) with Google Gemini LLM as MCP Client”.
