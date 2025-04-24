---
title: What is MCP? Integrate AI Agents with Databases &amp; APIs - YouTube
origin_url: https://www.youtube.com/watch?v=eur8dUO9mvE
date: 2025-04-24T02:46:17.313Z
---

# Model Context Protocol (MCP) Explained by Roy Derks

[Watch on YouTube](https://www.youtube.com/watch?v=eur8dUO9mvE)

---

## Certification Offer

Ready to become a certified Architect on Cloud Pak?  
Register now and use code **IBMTechYT20** for **20% off** your exam → [ibm.biz/BdGhLq](https://ibm.biz/BdGhLq)

---

## Learn More

- **AI Agents** → [ibm.biz/BdGhLf](https://ibm.biz/BdGhLf)  
- **Monthly AI Newsletter** → [ibm.biz/BdGhLP](https://ibm.biz/BdGhLP)

---

## Video Description

Unlock the secrets of MCP! 🚀 Dive into the world of Model Context Protocol and learn how to seamlessly connect AI agents to databases, APIs, and more. Roy Derks breaks down its components, from hosts to servers, and showcases real-world applications. Gain the knowledge to revolutionize your AI projects! 🧠

---

## Transcript

```plaintext
0:00 If you're building AI agents, you've probably heard about MCP or Model Context Protocol.
0:05 MCP is a new open source standard to connect your agents to data sources such as databases or APIs.
0:11 MCP consists of multiple components.
0:13 The most important ones are the host, the client, and the server.
0:17 So let's break it down.
0:19 At the very top you would have your MCP host.
0:22 Your MCP host will include an MCP client.
0:25 And it could also include multiple clients.
0:28 The MCP host could be an application such as a chat app.
0:33 It could also be a code assistant in your IDE, and much more.
0:38 The MCP host will connect to an MCP server.
0:41 It can actually connect to multiple MCP servers as well.
0:48 It doesn't matter how many MCP servers you connect to your MCP host or client.
0:53 The MCP host and servers will connect over each other through the MCP protocol.
0:57 The MCP protocol is a transport layer in the middle.
1:04 Whenever your MCP host or client needs a tool, it's going to connect to the MCP server.
1:09 The MCP server will then connect to, for example, a database.
1:12 And it doesn't matter if this is a relational database or a NoSQL database.
1:17 It could also connect to APIs.
1:20 And also the API standard doesn't really matter.
1:23 Finally, it could also connect to data sources such as a local file type or maybe code.
1:30 This is especially useful when you're building something like a code assistant in your IDE.
1:36 Let's look at an example of how to use MCP in practice.
1:40 We still have the three components.
1:41 We would have our MCP host and client,
1:46 of course, we also have a large language model,
1:53 and finally, we have our MCP servers,
1:56 and these could be multiple MCP servers or just a single one.
2:03 Let's assume our MCP client and host is a chat app,
2:07 and you ask a question such as, what is the weather like in a certain location or how many customers do I have?
2:13 The MCP host will need to retrieve tools from the MCP server.
2:18 The MCP server will then conclude and tell which tools are available.
2:23 From the MCP host, you would then have to connect to the large language model
2:26 and send over your question plus the available tools.
2:30 If all is well, the LLM will reply and tell you which tools to use.
2:38 Once the MCP host and client knows which tools to use, it knows which MCP servers to call.
2:43 So when it calls the MCP server in order to get a tool result,
2:48 the MCP server will be responsible for executing something that goes to a database, to an API, or a local piece of code,
2:59 and of course, there could be subsequent calls to MCP servers.
3:02 The MCP server will apply with a response, which you can send back to the LLM.
3:07 And finally, you should be able to get your final answer based on the question that you asked in the chat application.
3:15 If you are building agents, I'd really advise you to look at MCP protocol.
3:18 The MCP protocol is a new standard which will help you to connect your data sources via MCP server to any agent.
3:25 Even though you might not be building agents, your client might be building agents.
3:29 And if you enjoyed this video, make sure to like and subscribe.
