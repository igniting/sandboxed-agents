# Sandboxed Agents

**How the Industry Runs Untrusted AI Code Safely**

A technical deep dive into microVMs, gVisor, Firecracker, and the infrastructure behind AI agents that write and execute code. Today's agents — Claude Code, OpenAI Codex, and others — use code execution, CLI commands, and filesystem access as their primary harness, making every action potentially untrusted. Covering isolation technologies, real-world architectures from E2B, Modal, OpenAI, Anthropic, and AWS AgentCore, and the security models that underpin them.

## Research Links

### Primary Sources & Academic Papers

- [Firecracker: Lightweight Virtualization for Serverless Applications (NSDI '20)](https://www.usenix.org/conference/nsdi20/presentation/agache) — the design behind AWS Lambda's isolation
- [Firecracker NSDI '20 paper (Amazon Science)](https://www.amazon.science/publications/firecracker-lightweight-virtualization-for-serverless-applications)
- [Blending Containers and VMs (ACM)](https://dl.acm.org/doi/10.1145/3381052.3381315) — UW-Madison's comparison of Firecracker vs. gVisor
- [gVisor Architecture Guide](https://gvisor.dev/docs/architecture_guide/) — how Google reimplemented Linux in Go
- [gVisor Linux Syscall Compatibility (amd64)](https://gvisor.dev/docs/user_guide/compatibility/linux/amd64/) — 274 of ~350 Linux syscalls reimplemented
- [gVisor nvproxy: GPU Support for PyTorch & Stable Diffusion](https://gvisor.dev/blog/2023/06/20/gpu-pytorch-stable-diffusion/)

### Security Vulnerabilities & Threat Research

- [CVE-2019-5736](https://nvd.nist.gov/vuln/detail/CVE-2019-5736) — runc flaw allowing host binary overwrite and root access
- [CVE-2024-21626](https://nvd.nist.gov/vuln/detail/CVE-2024-21626) — "Leaky Vessels" fd leak granting full host filesystem access
- [CVE-2025-31133](https://nvd.nist.gov/vuln/detail/CVE-2025-31133), [CVE-2025-52565](https://nvd.nist.gov/vuln/detail/CVE-2025-52565), [CVE-2025-52881](https://nvd.nist.gov/vuln/detail/CVE-2025-52881) — mount-handling flaws bypassing AppArmor and SELinux
- [CVE-2025-53773](https://nvd.nist.gov/vuln/detail/CVE-2025-53773) — command injection in GitHub Copilot
- [CVE-2024-5565](https://nvd.nist.gov/vuln/detail/CVE-2024-5565) — prompt injection executing arbitrary Python
- [Leaky Vessels Disclosure (Snyk)](https://snyk.io/blog/leaky-vessels-docker-runc-container-breakout-vulnerabilities/) — Snyk's writeup of CVE-2024-21626
- [Slack AI Data Exfiltration (PromptArmor)](https://promptarmor.substack.com/p/data-exfiltration-from-slack-ai-via) — prompt injection causing AI to leak private conversations
- [Firecracker Security Advisories](https://github.com/firecracker-microvm/firecracker/security/advisories) — zero known VM escape CVEs
- [OWASP Top 10 for LLM Applications](https://owasp.org/www-project-top-10-for-large-language-model-applications/)
- [GKE Security Bulletin GCP-2024-005](https://cloud.google.com/anthos/clusters/docs/security-bulletins#gcp-2024-005-gke) — gVisor Sandbox clusters not impacted by CVE-2024-21626

### Core Technologies

- [KVM (Kernel-based Virtual Machine)](https://www.linux-kvm.org/page/Main_Page)
- [Hypervisor (Wikipedia)](https://en.wikipedia.org/wiki/Hypervisor)
- [Docker: What is a Container?](https://www.docker.com/resources/what-container/)
- [Linux Namespaces (man7)](https://man7.org/linux/man-pages/man7/namespaces.7.html)
- [Firecracker Jailer](https://github.com/firecracker-microvm/firecracker/blob/main/docs/jailer.md) — combines KVM, chroot, namespaces, cgroups, seccomp, and privilege drop
- [Cloud Hypervisor](https://www.cloudhypervisor.org/)
- [Cloud Hypervisor VFIO (GPU passthrough)](https://github.com/cloud-hypervisor/cloud-hypervisor/blob/main/docs/vfio.md)
- [Kata Containers](https://katacontainers.io/)
- [Kata 3.x runtime-rs (async Rust)](https://github.com/kata-containers/kata-containers/blob/main/src/runtime-rs/README.md)
- [Kata Confidential Containers (SEV-SNP)](https://github.com/kata-containers/kata-containers/blob/main/docs/how-to/how-to-run-kata-containers-with-SNP-VMs.md)
- [Wasmtime](https://wasmtime.dev/)
- [Bubblewrap](https://github.com/containers/bubblewrap)

### The Agent Harness Shift: Code + CLI + Filesystem

All major AI agents have converged on the same pattern: code execution, CLI commands, and filesystem access are now the primary interface — not chat. Tools like Claude Code, OpenAI Codex, and LangChain's Deep Agents use a handful of tools (shell, file read/write, code execution) rather than hundreds of specialized APIs. This "harness" — the scaffolding of system prompts, tool definitions, and orchestration logic around the model — is what turns an LLM into a coding agent.

- [Deep Agents: The Harness Behind Claude Code, Codex, Manus, and OpenClaw (Agent Native)](https://agentnativedev.medium.com/deep-agents-the-harness-behind-claude-code-codex-manus-and-openclaw-bdd94688dfdb) — how code execution + CLI + filesystem became the universal agent interface
- [Using Skills with Deep Agents (LangChain)](https://blog.langchain.com/using-skills-with-deep-agents/) — LangChain's open-source harness: "give agents a computer, not more tools"
- [Unlocking the Codex Harness: How We Built the App Server (OpenAI)](https://openai.com/index/unlocking-the-codex-harness/) — Codex's unified harness across web, CLI, IDE, and macOS
- [OpenAI Codex CLI vs Claude Code: A Practical Harness Comparison (Field Journal)](https://fieldjournal.ai/blog/codex-cli-vs-claude-code/) — side-by-side comparison of how both agents use shell, filesystem, and code execution
- [Codex vs Claude Code vs OpenCode: Three Terminal Coding Agents, Compared (Awesome Agents)](https://awesomeagents.ai/tools/codex-vs-claude-code-vs-opencode/) — the three dominant terminal-first agents
- [Claude Code & Codex CLI: AI Agents Beyond Coding (iTecs)](https://itecsonline.com/post/claude-code-codex-cli-ai-agents-beyond-coding-2026) — how CLI agents moved from coding tools to general-purpose work
- [MCPs, Claude Code, Codex, and the 2026 Workflow Shift (DEV Community)](https://dev.to/austinwdigital/mcps-claude-code-codex-moltbot-clawdbot-and-the-2026-workflow-shift-in-ai-development-1o04) — the shift from "AI writes code" to "AI runs work"
- [Coding Agent Sandbox: Secure Environments for AI-Generated Code (Bunnyshell)](https://www.bunnyshell.com/guides/coding-agent-sandbox/) — survey of sandboxing approaches across major coding agents

### Model Provider Implementations

- [Claude Code Sandboxing (Anthropic)](https://www.anthropic.com/engineering/claude-code-sandboxing) — reduced permission prompts by 84%
- [Claude Code Sandbox Documentation](https://code.claude.com/docs/en/sandboxing)
- [Claude Code on the Web](https://code.claude.com/docs/en/claude-code-on-the-web) — each session runs in an isolated, Anthropic-managed VM
- [Code Execution with MCP (Anthropic)](https://www.anthropic.com/engineering/code-execution-with-mcp) — 98.7% context token reduction (150K to 2K tokens)
- [Anthropic Sandbox Runtime (open-source)](https://github.com/anthropic-experimental/sandbox-runtime)
- [Model Context Protocol (MCP) Announcement](https://www.anthropic.com/news/model-context-protocol)
- [OpenAI Codex (open-source)](https://github.com/openai/codex) — Rust implementation with Landlock + seccomp
- [Modal Security Docs](https://modal.com/docs/guide/security) — gVisor in production at scale
- [Google Agent Sandbox CRD](https://cloud.google.com/blog/products/containers-kubernetes/introducing-agent-sandbox)

### AWS Bedrock AgentCore Runtime

Amazon Bedrock AgentCore provides a serverless, framework-agnostic platform for deploying AI agents with built-in Firecracker microVM isolation. Each user session runs in a dedicated microVM with isolated CPU, memory, and filesystem — terminated and sanitized after completion. Supports sessions up to 8 hours, MCP and A2A protocols, and works with LangGraph, Strands, and CrewAI. However, security researchers have found significant sandbox bypass vectors including DNS-based C2 channels and credential exfiltration via the microVM metadata service.

- [Amazon Bedrock AgentCore](https://aws.amazon.com/bedrock/agentcore/) — product page
- [AgentCore Runtime: How It Works](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/runtime-how-it-works.html) — architecture and microVM provisioning
- [AgentCore Runtime Session Isolation](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/runtime-sessions.html) — one-session-one-microVM model
- [AgentCore Tools Session Isolation (Firecracker)](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/built-in-tools-how-it-works.html) — Firecracker-based isolation for Code Interpreter and Browser Tool
- [Host Agents or Tools with AgentCore Runtime](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/agents-tools-runtime.html) — deploying agents with session management and scaling
- [AgentCore Runtime Lifecycle Settings](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/runtime-lifecycle-settings.html) — idle timeout, max lifetime, health checks
- [Introducing the AgentCore Code Interpreter (AWS Blog)](https://aws.amazon.com/blogs/machine-learning/introducing-the-amazon-bedrock-agentcore-code-interpreter/) — fully managed sandboxed code execution (Python, JS, TS)
- [Amazon Bedrock AgentCore Samples (GitHub)](https://github.com/awslabs/amazon-bedrock-agentcore-samples) — official sample code
- [Building AI Agents on AWS in 2025: Bedrock, AgentCore, and Beyond (DEV Community)](https://dev.to/aws-builders/building-ai-agents-on-aws-in-2025-a-practitioners-guide-to-bedrock-agentcore-and-beyond-4efn) — practitioner's guide
- [AWS Bedrock AgentCore Deep Dive (Medium)](https://joudwawad.medium.com/aws-bedrock-agentcore-deep-dive-6822e4071774) — architecture walkthrough
- [AgentCore Runtime Part 1: Introduction (DEV Community)](https://dev.to/aws-heroes/amazon-bedrock-agentcore-runtime-part-1-introduction-e5i) — getting started guide

#### AgentCore Security Research

- [Sandboxed to Compromised: Credential Exfiltration Paths in AWS Code Interpreters (Sonrai Security)](https://sonraisecurity.com/blog/sandboxed-to-compromised-new-research-exposes-credential-exfiltration-paths-in-aws-code-interpreters/) — MMDS metadata bypass enabling credential exfiltration from sandboxed code interpreters
- [AgentCore: The Overlooked Privilege Escalation Path (Sonrai Security)](https://sonraisecurity.com/blog/aws-agentcore-privilege-escalation-bedrock-scp-fix/) — non-agentic identities invoking code interpreters to assume execution roles
- [Pwning AI Code Interpreters in AWS Bedrock AgentCore (BeyondTrust)](https://www.beyondtrust.com/blog/entry/pwning-aws-agentcore-code-interpreter) — DNS sandbox bypass enabling bidirectional C2 channels (CVSSv3 7.5)
- [AWS Bedrock AgentCore Sandbox Bypass: Covert C2 and Data Exfiltration (CybersecurityNews)](https://cybersecuritynews.com/aws-bedrock-agentcore-sandbox-bypass/) — DNS A/AAAA record exfiltration despite "complete isolation" claim
- [AWS Privilege Escalation: IAM Risks and AI-Driven Bedrock/AgentCore Vectors (Software Secured)](https://www.softwaresecured.com/post/aws-privilege-escalation-iam-risks-service-based-attacks-and-new-ai-driven-bedrock-agentcore-vectors) — new escalation class: AI-managed compute executing code on behalf of privileged service roles

### New Entrants & Emerging Platforms

- [E2B Series A ($32M)](https://e2b.dev/blog/series-a)
- [Modal Series B ($87M)](https://modal.com/blog/announcing-our-series-b) — $1.1B valuation, GPU-first
- [Daytona](https://www.daytona.io/) — sub-90ms creation, $24M Series A
- [Morph Cloud Infinibranch](https://morph.so/blog/infinibranch) — instant VM branching for RL research
- [Deno Sandbox](https://deno.com/blog/introducing-deno-sandbox) — zero secrets in sandbox, placeholder injection
- [Microsoft Wassette](https://github.com/microsoft/wassette) — Wasm Components + MCP for per-tool isolation
- [Fly.io Sprites](https://fly.io/blog/code-and-let-live/) — 100GB NVMe, context continuity for long-running tasks
- [Zeroboot](https://github.com/zerobootdev/zeroboot)

### Industry Moves

- [Gitpod rebranded to Ona](https://ona.com/stories/gitpod-is-now-ona) (Sep 2025)
- [CodeSandbox absorbed by Together AI](https://codesandbox.io/blog) (Dec 2024)
