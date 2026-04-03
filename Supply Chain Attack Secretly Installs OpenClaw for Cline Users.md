Supply Chain Attack Secretly Installs OpenClaw for Cline Users
 darkreading.com/application-security/supply-chain-attack-openclaw-cline-users
Rob Wright
February 19, 2026

Senior News Director,Dark Reading
February 19, 2026

3 Min Read


Source: Rokas Tenys via Alamy stock photo

The rapid spread of OpenClaw wasn't going fast enough for someone.

Cybersecurity vendors this week noticed an odd trend when the npm package for version 2.3.0 of Cline, a widely used open source AI coding tool, began installing an apparent stowaway program: OpenClaw. For approximately eight hours, users who downloaded Cline received a poisoned version of the tool that, while not carrying traditional malware, still made unauthorized installations on their systems.

It's unclear who perpetrated this odd supply chain attack, and what the ultimate motivation is beyond forced installations of OpenClaw. But the attack marks the latest red flag for the fast-growing AI framework, which security researchers have expressed concerns about since its explosion onto the technology landscape last month.

A PoC Leads to a Poisoned NPM Package
The supply chain attack stemmed from a vulnerability disclosed earlier this month by security researcher Adnan Khan. Exploitation of the vulnerability, which had no assigned CVE at press time, can lead to an attacker obtaining secrets such as release tokens.

Related:Axios NPM Package Compromised in Precision Attack

"Between Dec. 21, 2025, and Feb. 9, 2026, a prompt injection vulnerability in Cline’s (now removed) Claude Issue Triage workflow allowed any attacker with a GitHub account to compromise production Cline releases on both the Visual Studio Code Marketplace and OpenVSX and publish malware to millions of developers!" Khan wrote in a blog post.

Khan said his attempts to contact Cline were initially "fruitless," and the company quickly patched the vulnerability shortly after his research was published. Unfortunately, someone took advantage of Khan's research, stole an npm publish token, and tricked the latest version of Cline into also installing OpenClaw.

Henrik Plate, security researcher with Endor Labs, explained in a blog post that version 2.3.0 of the Cline CLI npm package used a post-install hook to silently download OpenClaw to the same system. While the impact is considered low because OpenClaw isn't malicious, he noted that "this event emphasizes the need for package maintainers to not only enable trusted publishing, but also disable publication through traditional tokens — and for package users to pay attention to the presence (and sudden absence) of corresponding attestations."

In an update to his blog post, Khan stressed that he was not behind the supply chain attack and that he didn't conduct testing of his proof-of-concept (PoC) exploit on Cline's repository. "I conducted my PoC on a mirror of Cline to confirm the prompt injection vulnerability. A different actor found my PoC on my test repository and used it to directly attack Cline and obtain the publication credentials," he wrote.

Related:AI-Driven Code Surge Is Forcing a Rethink of AppSec

Cline published an advisory on GitHub and released version 2.4.0 while removing the previous, tainted npm package. "The compromised token has been revoked and npm publishing now uses OIDC provenance via GitHub Actions," the company said. 

OpenClaw Not Malicious, But Risky
StepSecurity said the compromised Cline package was downloaded approximately 4,000 times over an eight-hour stretch before version 2.3.0 was deprecated. And while the short-lived supply chain attack didn't deploy malware, that doesn't mean it didn't present serious risk.

Sai Likhith Paradarami, software engineer with StepSecurity, explained in a blog post that OpenClaw is a "dangerous payload" because it had broad permissions as well as full disk access on a system in order to execute tasks on the user's behalf. OpenClaw also establishes a persistent Gateway daemon that runs quietly in the background as a WebSocket server.

"This design makes it an exceptionally high-value implant for an attacker," Paradarami, wrote, adding that a silently installed version of OpenClaw could give a threat actor a persistent foothold on a targeted system with the ability to steals secrets and credentials as well as tamper with development environments. 

Related:F5 BIG-IP Vulnerability Reclassified as RCE, Under Exploitation

Along with updating their systems to version 2.4.0, Paradarami urged Cline users to review their environments for any unwanted installations of OpenClaw.

About the Author

Rob Wright

Senior News Director, Dark Reading

Rob Wright is a longtime reporter with more than 25 years of experience as a technology journalist. Prior to joining Dark Reading as senior news director, he spent more than a decade at TechTarget's SearchSecurity in various roles, including senior news director, executive editor and editorial director. Before that, he worked for several years at CRN, Tom's Hardware Guide, and VARBusiness Magazine covering a variety of technology beats and trends. Prior to becoming a technology journalist in 2000, he worked as a weekly and daily newspaper reporter in Virginia, where he won three Virginia Press Association awards in 1998 and 1999. He graduated from the University of Richmond in 1997 with a degree in journalism and English. A native of Massachusetts, he lives in the Boston area. 
