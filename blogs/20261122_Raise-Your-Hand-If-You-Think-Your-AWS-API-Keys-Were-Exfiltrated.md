# Raise Your Hand If You Think Your AWS API Keys Were Exfiltrated by the OpenRouter Attack That Happened Last Week!

**Published**: November 22, 2026

**Reading Time**: 8 minutes

**Author**: Taariq Lewis, SerenAI

Last week, it was US banks, the NSA, & US airlines all coming to a halt or experiencing stoppages and attacks. Could it be related to the OpenRouter action against a number of Model providers?

At Seren, we suspect the correlation between the rash of stoppages and OpenRouter dropping model providers and their Zero Retention labels is compelling. Banks, NSA, and others, or their employees, may have been using OpenRouter for agentic work. OpenRouter model providers were having your prompts read and intercepted by threat actors. Because agents were using AWS API keys to manage DevOps, many systems, including ours, were compromised.

Based on what we know, the AWS key exfiltrators were using Asia-based Compute providers they hijacked with LLMs and had the LLMs run the attacks. The LLMs were simply aiming to find every AWS and GitHub API Key they could find and use. Once attacked and successfully exfiltrated, they would use AWS API Keys to access...NEW COMPUTE...for their LLMs to continue to attack other sites and exfiltrate AWS keys as well as crypto private keys.

At Seren, we found the signatures of the attackers. They lead to GitHub open-source project Artex as the attackers' preferred codebase of choice. ARTEX - AI autonomous penetration testing system (English translation; upstream: Autumn-27/ARTEX, AGPL-3.0). You can star and clone the repo here: [https://github.com/BaaRai/ARTEX](https://github.com/BaaRai/ARTEX).

Today, anyone can have their AI agent find a GitHub repo with an AI penetration testing system and set it to work against anyone else. We recommend you star that repo and have your AI agents check the signatures against your own codebase. This is the new age of AI hacking.

We recommend that everyone using AWS tell your LLMs that you're switching over to IAM accounts for all your users and agents with required passkey/authenticator/yubikey auth. Then remove all AWS Keys from your system. I think today is the day we will mark as the end of use of AWS root keys. The risk is simply too great that your AI agent will accidentally leak in your file-systems or that your inference prompts will end up somewhere in North Korea.

Next: Canaries everywhere. At Seren, we run a 24-hour, lights-out, code factory. Now, we include canaries in the slop cannon. Canaries are alerts that fire when abnormal behavior hits your codebase and databases. Your AI runners will be handling thousands of commits everyday, so they will be the first line of canaries to detect when an intruder is deploying code changes into your system that you did not authorize.

Every agent must be a canary. Every service must have a canary. Canaries everywhere.

Finally: Ban GitHub Fine Grained Personal Access Tokens from your organizations. Kill them with fire by changing your GitHub organizations to block their use or only let Admins grant use on a case-by-case basis. AI attackers are scouring the web and inference prompts for these tokens and API keys and then using them to grant themselves more access to decrypt and read everything.

If you've read up to here, here are some more remediations that we took that you can feed into your AI Agent to explore implementing into your own systems for additional safety.

## Security Remediation for Agentic Software Development

### 0. Assume exposure: revoke, rotate, and hunt for persistence.

If an AWS key, GitHub token, deploy key, private key, webhook URL, OAuth credential, agent workspace, or prompt history may have entered an untrusted system, treat it as exposed. Monitoring it is not enough.

- Revoke and replace exposed AWS keys, GitHub tokens, deploy keys, OAuth credentials, webhook secrets, package-registry tokens, database credentials, signing keys, and wallet keys.
- Search for persistence: new IAM users and roles, access keys, OIDC trust-policy changes, GitHub Apps, deploy keys, organization owners, self-hosted runners, webhooks, scheduled jobs, cloud workloads, and unfamiliar public ingress.
- Review CloudTrail, GitHub audit logs, CI logs, VPC Flow Logs, and billing across the possible exposure window for new compute, unusual regions, new API calls, snapshots, KMS-policy changes, unusual egress, and secret reads.
- Preserve evidence before cleanup: audit events, host identities, process logs, network logs, and artifact digests.

**Rotation without an investigation leaves persistence behind. Investigation without rotation leaves the attacker logged in. Do both.**

### 1. Egress is a privilege. Default-deny every agent, runner, and workload.

Your agent does not need the open internet. Give it only the destinations and ports it needs: your artifact registry, source-control API, cloud endpoints, package mirrors, and an approved outbound proxy. Deny everything else by default—including direct access to cloud metadata services.

Turning on network-policy enforcement will expose unexpected database routes from your workloads to public addresses. You do not know what your AI agents can reach until you enforce the fence and watch the denial logs.

**Default deny. Explicit allow. Log every refusal.**

### 2. Fence your control planes: AWS, Kubernetes, and admin consoles should not be reachable from everywhere.

The AWS console, AWS API sessions, Kubernetes API server, Argo CD, database administration interfaces, and internal admin applications are not public websites. They all go behind a VPN, identity-aware proxy, or fixed approved network ranges from need-to-access workstations. Require phishing-resistant MFA—passkeys or hardware security keys—and use short-lived sessions. No agent needs 24-hour access.

Restrict your cloud and cluster control-plane access to explicitly approved addresses and audit the policy. Test the policy from explicitly authorized and unauthorized test paths in an environment you control, and alert on denied access.

**No public admin plane. No long-lived admin session. No unlogged exception.**

### 3. Assume every in-cluster identity will eventually be stolen. Scope secrets so one agentic workload cannot read everything in the rest of the software factory.

Privileged in-cluster roles that can read secrets far beyond the workload that needed them must be audited and secured.

Have an architect agent give every agentic workload its own cloud identity and its own narrowly scoped secret permissions. Your agents can set this up easily so that all agents use workload identity/OIDC and short-lived credentials instead of shared environment variables or account-wide "external secrets" reader roles. Put resource policies on crown-jewel secrets so that only the exact runtime principal can retrieve them. Rotate CI OAuth client secrets, webhook secrets, deploy credentials, signing keys, and wallet keys—not only AWS credentials.

**One service. One identity. One small set of secrets. One short-lived credential.**

### 4. Treat webhooks as production credentials. Eliminate tokens from URLs.

Agents are notorious for leaving these in, and when you're publishing hundreds or thousands, agentic drift can make them bleed into your system. Create rules for your agents to avoid this practice.

A webhook URL is not "just an integration." If it contains a bearer token, it is a production credential that can trigger builds, deploys, or internal workflows.

Inventory every organization and repository webhook now. Delete anything unused; rotate the secret for everything retained. Never put bearer tokens in URLs. Use signed webhook payloads with a dedicated secret in a secret manager. Restrict webhook endpoints to known source IP ranges where the provider supports it. Alert on webhook creation, modification, deletion, delivery failures, and changes to the CI endpoint.

**If your CI can be triggered by a URL, that URL is a key. Protect it like one.**

### 5. Only deploy verified artifacts. Pin image digests and make privilege-boundary tests release blockers.

When agents are deploying and scaling containers, they don't automatically check for valid artifacts. They are automatons. They just follow the code. Attackers compromising one of your containers can allow them to compromise others.

Deploy containers by immutable digest, not latest or a mutable version tag. Require build provenance and signatures for production artifacts. Maintain a software bill of materials for each release. Add tests that specifically prove tenant, agent, plugin, and workflow code cannot execute as an administrative identity. Block releases when authorization-boundary regression tests fail—even if the application's normal functional tests pass. A green build does not mean a safe build. Prove that untrusted code cannot become privileged code.

### 6. Make security telemetry tamper-resistant—and alert on control-plane changes, not just outages.

Or in plain English: Logs, logs, everywhere, with agents monitoring them for anomalies, canaries, and alerts. It's our view that log collection and log management are going to be the new table-stakes for agentic factory security.

Your logging costs will rise, so get ready for more retention tooling and storage. Get ready for more email alerts. Collect CloudTrail, CI/CD, source-control, cloud, network-flow, load-balancer, administrative-session, and agent audit logs. Create new alerts on IAM and KMS policy changes, new credentials, role assumptions, secret reads, public-ingress changes, webhook changes, image changes, and disabled logging. Time to go to Log-Party-Town!

Use agents to inventory, test, draft changes, and open pull requests—but require a qualified owner to review production changes to IAM, network policy, secret stores, deletion actions, and deployment permissions.

## Conclusion

All this does not require that anyone hire a new agentic cyber-security company or bring on new Forward Deployed Engineers. It does not require you to wait even 5 minutes. You can feed this blog post into your agent/LLM and it will audit my claims against your codebase, check for exploitation signatures from Artex, and set up the agentic factory-software security plan to get you going.

Your agents will be able to use this blog to start agentic security audits, test, then propose to remediate your agentic gaps against an Artex attack. Start today. The number of agentic penetration attacks is only going to increase.

---

*Published: November 22, 2026*

*Tags: #AIsecurity #AgenticAI #AWS #IAM #GitHub #ARTEX #OpenRouter #Canaries #DevSecOps #SerenAI*
