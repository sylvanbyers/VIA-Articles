AI Conundrum: Why MCP Security Can't Be Patched Away
 darkreading.com/application-security/mcp-security-patched
Jai Vijayan
March 19, 2026

Source: Umut Hasanoglu via Shutterstock

Organizations rushing to connect their LLM-powered apps to external data sources and services using the Model Context Protocol (MCP) may be inadvertently creating attack surfaces that are fundamentally different from anything their existing security controls can handle.

Making matters worse is that the risks are not the kind a security team can address via patching or configuration changes because they exist at the architectural level in both large language models (LLMs) and in MCP, says Gianpietro Cutolo, cloud threat researcher at Netskope, who is scheduled to highlight the issue at a session next week at the RSAC 2026 Conference in San Francisco

Foundational Problems
The problem has to do with how an LLM behaves when MCP is in the picture, he adds. Typically, when an LLM receives a prompt or an instruction, it generates a response that a user has to review and decide what to do with. The worst that can happen is a hallucinated response.

That dynamic changes completely with MCP because the LLM is no longer just generating a response but is executing real actions on behalf of the user. In an MCP-enabled environment, an LLM can access enterprise data, trigger workflows, call APIs, and make decisions autonomously, Cutolo says.

For example, a user might ask their AI assistant — like Claude or ChatGPT — to schedule a meeting, and the LLM could use a Google Calendar MCP connector to check availability, create a calendar event, and set up a reminder, all without the user needing to do anything manually. It's the model that chooses the specific functions or capabilities that an MCP server might publish — like "fetch emails" or "create calendar event" or "search files." It is also the model that instructs the MCP server what parameters to use when, for instance, fetching emails or creating a calendar event. 

But while MCP connectors allow organizations to extend the capabilities of their LLM-enabled apps and services, they also introduce new risk, Cutolo says. 

One major issue is the fact that LLMs cannot distinguish between content and instructions. When an MCP connector fetches content from an external source like an email or a document, for instance, the LLM processes it all as input. This makes it trivial for an adversary to hide a malicious instruction in content that the model retrieves or processes.

As an example, Cutolo points to an attacker sending an email containing both legitimate content and malicious instructions to a target individual. If the user asked their AI-assistant to summarize the email, the MCP connector would fetch the email from the user's inbox and inject both the legit text and the malicious instructions into the LLM's context. Because the LLM is unable to distinguish content from instruction, it will execute whatever instruction that adversary might have hidden in that email — like exfiltrating files and data for instance or sending emails on the user's behalf without the user ever knowing a thing. The consequences of this kind of indirect prompt injection can be substantial in environments with multiple MCP connectors to local files, Jira tickets, Google Drives, and folders on the hard drive and other services, Cutolo points out. A single poisoned email instructing the agent to exfiltrate folder contents could trigger coordinated actions across all those services in one pass, he notes.

Another way adversaries can take advantage is by tool poisoning. When an LLM connects to an MCP server it asks the server to list all the tools or capabilities that it supports, their names, description, input requirements, and other data. The tool metadata goes directly into the LLM context window. An adversary can plant malicious instructions in the tool metadata that once again the LLM will process as content, because it cannot distinguish content from instruction, he says.

The third attack class that Cutolo plans on highlighting at the conference is Rug Pull, where the creator of an MCP server — or an attacker that might have gained access to it — could maliciously alter it. The protocol currently has no mechanism to notify an MCP client or AI agent of any changes to the server. That means if a legitimate MCP server is subsequently compromised through a malicious update, it could begin serving malicious tool descriptions or instructions that cause the AI agent to take malicious actions, without the agent or MCP client having any way to know the server was tampered with, he says.

Patching Won't Work
Because these are foundational issues in how LLM and MCP work, organizations cannot patch or update their way out of risk, Cutolo warns.

For the indirect prompt injection threat, he recommends organizations take measures like separating MCP servers for private versus public data, as well as scanning for instruction-like patterns, hidden text, and unusual formatting in any context that the agent might process, and keeping humans in the loop for all sensitive actions. Organizations should inventory and vet every MCP server in their environment, enforce least-privilege permissions so each connector has access only to what its specific task requires, log all MCP traffic, and build behavioral baselines that can flag when an agent's activity deviates from expected behavior, he adds. The practical defense for tool poisoning, he says, is to scan tool metadata for malicious instructions before installing any MCP server.

About the Author

Jai Vijayan

Contributing Writer

Jai Vijayan is a seasoned technology reporter with over 20 years of experience in IT trade journalism. He was most recently a Senior Editor at Computerworld, where he covered information security and data privacy issues for the publication. Over the course of his 20-year career at Computerworld, Jai also covered a variety of other technology topics, including big data, Hadoop, Internet of Things, e-voting, and data analytics. Prior to Computerworld, Jai covered technology issues for The Economic Times in Bangalore, India. Jai has a Master's degree in Statistics and lives in Naperville, Ill.
