# Which harness powers Meta Muse?

Research date: 2026-11-20

Meta identifies the **Hatch daemon** as the core agent harness for Muse, its personal AI agent. Hatch runs alongside the workspace and executable tools in a `systemd-nspawn` container within Muse Secure VM. [Meta engineering, How We Built Safety Into Muse](https://muse.ai/blog/security-and-safety-for-ai-agents-our-approach-with-muse)

Meta named **Muse Spark** as the model powering the personal agent at launch. The model and the harness have different roles. [Meta's Muse announcement](https://about.fb.com/news/2026/09/introducing-muse-personal-ai-agent/)

**Muse Code** is Meta’s separately documented coding agent for the terminal and CI. The sources reviewed identify Hatch at the personal-agent level and leave open whether Hatch delegates particular coding tasks to another agent. [Muse Code documentation](https://dev.meta.ai/docs/muse-code)
