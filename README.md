# Sandboxed Agents

**How the Industry Runs Untrusted AI Code Safely**

A technical deep dive into microVMs, gVisor, Firecracker, and the infrastructure behind AI agents that write and execute code. Covering isolation technologies, real-world architectures from E2B, Modal, OpenAI, and Anthropic, and the security models that underpin them.

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
