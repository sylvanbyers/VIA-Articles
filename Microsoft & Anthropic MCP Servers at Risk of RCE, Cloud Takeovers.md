Microsoft & Anthropic MCP Servers at Risk of RCE, Cloud Takeovers
 darkreading.com/application-security/microsoft-anthropic-mcp-servers-risk-takeovers
Nate Nelson
January 20, 2026

Source: Jansos via Alamy Stock Photo

The most popular trusted model context protocol (MCP) servers on the Web today contain severe cybersecurity vulnerabilities.

The Internet of AI forming all around us is growing larger and more unwieldy by the day. Even just a few years ago, AI apps and services were contained and prescribed. Talking to ChatGPT was like being in a closed room with a smart person — whatever happened there didn't really affect the rest of the world.

Today autonomous agents have infected every software-as-a-service (SaaS) platform, performing largely unmonitored actions that spread data and cyberattacks beyond where their users realize. And even simple chatbot conversations are no longer so simple anymore, because large language models (LLMs) can connect to external data sources using MCP servers.

In the rush to get them to market — and in fear of overly constricting their users — connected AI solutions like MCP servers have often shipped with inadequate guardrails. When Anthropic first created the MCP open standard, it left security up to the user. Now, more than a year later, even the most reliable, brand name MCP servers are rife with security holes, according to security researchers.

Related:Axios NPM Package Compromised in Precision Attack

On Jan. 20, Cyata researcher Yarden Porat revealed an exploit chain that weaponizes Anthropic's own Git and filesystem MCP servers to achieve remote code execution (RCE). Meanwhile, BlueRock principal solutions engineer David Onwukwe disclosed a severe server-side request forgery (SSRF) vulnerability in Microsoft's MarkItDown MCP server. Worse: When they analyzed more than 7,000 MCP servers, they found that the same SSRF exposure might be latent in around 36.7% of all MCP servers on the Web today.

Together these findings paint a picture of an overly pliant, dangerously underestimated threat vector.

MCP Servers Carry SSRF Risks
Microsoft's MarkItDown MCP server is one of the most popular around, as evidenced by its more than 85,000 stars and 5,000 forks on GitHub. It's utterly simple, too: little more than a conversion tool for the world of large language models (LLMs), taking a variety of file types and converting them into Markdown for LLMs. What's the harm in that?

Well, to begin with, the user needs to indicate where the file they want to convert is located. So they feed MarkItDown a URI, and MarkItDown fetches whatever's there. It turns out, though, that MarkItDown doesn't in any way restrict this user input. "It could be a file that is internal; it could be any other file that is accessible via the network, to that given server, [or] any other type of arbitrary call that you would want to make," explains Harold Byun, chief product officer at BlueRock.

Related:AI-Driven Code Surge Is Forcing a Rethink of AppSec

When they discovered this, Byun and his team realized the immense potential for server-side request forgeries (SSRF). For instance, an employee — or an attacker with control of an employee's machine — might escape their privileges and reach resources they otherwise aren't allowed to touch in a company network, simply by asking an overpermissioned MCP server to fetch the information for them.

BlueRock took particular interest in scenarios where the MCP server runs in the cloud, specifically in an Amazon Web Services (AWS) EC2 instance. EC2 instances come with a built-in metadata service at a specific IP that, among other things, contains temporary credentials if that instance has an AWS identity and access management (IAM) role attached. It isn't supposed to be exposed to users, but by feeding MarkItDown its IP, the researchers were able to get to it and retrieve access keys, secret keys, and session tokens.

There is a workaround for this issue in AWS. The newer version of the metadata service — the Instance Metadata Service version 2 (IMDSv2) — is more robust against SSRF abuse than IMDSv1, which Blue Rock exploited. Still, "the reality is the vast majority of instances in Amazon are on Instance Metadata Service v1," Byun says.

Related:F5 BIG-IP Vulnerability Reclassified as RCE, Under Exploitation

As for Microsoft's end of the bargain, a company spokesperson told Dark Reading, "After investigation we found the reported scenario does not create significant risk for our customers, as it requires a user to deliberately employ the feature in a way that is outside its intended design and normal usage patterns."

Byun disagrees. "People are going to take available software and they're going to use it [as they see fit]," he says. "The recommendation that we would have liked to have seen is: 'Yes, this potentially exposes a gap in how MCP is implemented, and developers should use best practices to bound and constrain the calls.'"

MCP's Inventors Created Buggy MCP Servers
The same day Blue Rock published its findings about Microsoft's MarkItDown, researchers from Cyata disclosed three medium-severity vulnerabilities in the official Git MCP server maintained by Anthropic. 

The first, CVE-2025-68145 (CVSS 6.4), is a simple path validation bypass. Admins have a setting that allows them to restrict the MCP server to a specific repository path, but the mechanism for enforcing this rule didn't actually do that.

The server also allowed users to create new repos wherever they wanted in the filesystem, turning any arbitrary directory into a Git repository. This issue earned the label CVE-2025-68143 (CVSS 6.5).

The last issue, CVE-2025-68144 (CVSS 6.3), concerned "git_diff," the command used to show the differences between versions of files. git_diff might be read-only in nature, but the researchers figured out how to use it to empty or overwrite any file that the server could reach.

Individually, these issues might appear rather manageable. The key for Cyata was chaining the first two together, along with the Filesystem MCP server, to create a powerful RCE exploit.

Imagine an attacker plants some kind of malicious content — a slimy README or a compromised webpage — where a user's AI assistant will read it. Through indirect prompt injection (IPI), the file secretly instructs the AI to exploit the Git MCP vulnerabilities. First, it turns a normal directory on the victim's machine into a Git repo. Next, using the Filesystem MCP's legitimate file writing capability, it instructs the AI to write a ".gitattributes" file and overwrite the repo's default config file, effectively creating a filter so that when Git stages new .txt files, it automatically runs attacker-defined shell commands. Finally, the AI writes a malicious payload and a normal text file that, when introduced to the repo, will trigger the payload's execution. The result: arbitrary code execution.

"On its own, each MCP server was relatively safe," Cyata CEO and co-founder Shahar Tal says. "But when combined, the Git MCP server and the Filesystem MCP server cross interaction is what broke our assumptions. I think that's really interesting. Hyperconnected AI agents and more MCP servers is a toxic combination that makes the agents fall."

The company reported its findings to Anthropic last June. Half a year later, in December, Anthropic released the 2025.12.18 version of the Git MCP server, which better enforced path validation (in response to CVE-2025-68145), addressed argument handling (CVE-2025-68144), and completely removed the git_init tool (CVE-2025-68143).

Pointing out the all too obvious irony, Tal says, "If Anthropic gets it wrong — in their official MCP reference implementation for what 'good' should look like — then everyone can get [MCP security] wrong. That's where we are today."

Dark Reading contacted Anthropic for comment but the company did not respond at press time.

About the Author

Nate Nelson

Contributing Writer

Nate Nelson is a journalist and scriptwriter. He writes for "Darknet Diaries" — the most popular podcast in cybersecurity — and co-created the former Top 20 tech podcast "Malicious Life." Before joining Dark Reading, he was a reporter at Threatpost.
