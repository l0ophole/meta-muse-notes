# Meta Muse research findings

November 20, 2026

## Harness and influence

Meta identifies the Hatch daemon as Muse's core agent harness. It runs alongside user tools and files in a Debian systemd-nspawn container. Separate host services manage credentials and permissions. Sentinel evaluates connector actions and outbound network requests. Muse Spark was the model identified at launch. Muse Code is Meta's separately documented terminal coding agent. [Engineering account](https://muse.ai/blog/security-and-safety-for-ai-agents-our-approach-with-muse) · [Launch announcement](https://about.fb.com/news/2026/09/introducing-muse-personal-ai-agent/) · [Muse Code](https://dev.meta.ai/docs/muse-code)

Nat Friedman said Meta's team used OpenClaw and deliberately adopted some of its product and workspace conventions. He said Meta built Muse from scratch. The shared filenames make the design influence visible. No public evidence reviewed establishes that Hatch runs OpenClaw, Pi, or Muse Code internally. A researcher found Codex CLI 0.149.0 in the image and traced use of its bundled bubblewrap sandbox for media processing. They found no invocation of the CLI as a coding agent. [Friedman's statement](https://techcrunch.com/2026/09/22/meta-admits-muses-likeness-to-openclaw-isnt-a-coincidence/) · [Firsthand filesystem inspection](https://mouse.dev/blog/muse-runtime-export)

## Filesystem comparisons

Independent reports place the agent home at `/home/hatch`, skills and runtime tools under `/opt/hatch`, and bundled software under `/opt/hatch-image`. Paths below are relative to the supplied partial directory listing. A listing verifies names and nesting. Runtime behavior needs separate evidence. [Filesystem report](https://mouse.dev/blog/muse-runtime-export)

| Area | Muse evidence | Closest verified comparison |
| --- | --- | --- |
| Memory | `MEMORY.md`, `memory/YYYY-MM-DD.md`, and `memory/bank/` containing `world.md`, `experience.md`, `opinions.md`, and `reflections.md` | An OpenClaw research proposal uses the same daily-log pattern and groups the first three bank filenames together. Its proposed bank sits beside the memory directory. It credits Hindsight and Letta/MemGPT as design influences. |
| Dreams | Dated Markdown in `dreams/`. Under `dreams/alignment/derived/` the listing contains `ALIGNMENT_STATE.yaml`, `ALIGNMENT_SYNTHESIS.md`, and `PROGRESSION_HISTORY.yaml`. The repair directory contains `REPAIR_THREADS.yaml`. | Independent Muse inspections corroborate the synthesis and repair names. OpenClaw documents phase reports at `memory/dreaming/<phase>/YYYY-MM-DD.md`. |
| Hooks | `hooks/definitions/`, `hooks/scripts/`, `hooks/logs/`, `hooks/state/`, `hooks/runtime/poll-schedule/`, and `hooks/runtime/hatch_hook_runtime.sh` | A separate inspection of Muse reports the same runtime script. No unrelated implementation with this exact collection of paths was verified. |

[OpenClaw memory proposal, archived copy](https://openclawdocs.sitemirror.store/experiments/research/memory/) · [OpenClaw dreaming paths](https://docs.openclaw.ai/cli/memory) · [Independent Muse architecture notes](https://github.com/sys-dissect/meta-muse-sandbox-architecture/blob/main/part3-the-mind/README.md)

## Behavior and remaining questions

- **Dreams.** A nightly review produces dated reflections and guidance for later conversations. One inspected build marked the raw nightly prose as excluded from automatic prompt insertion. Demonstrating improved behavior would require testing the same situation in a later conversation. [Runtime report](https://mouse.dev/blog/muse-runtime-export)
- **Hooks.** A publicly mirrored Muse guide describes lightweight polling scripts that wake the agent when a condition matches. Crons schedule agent work. The listing includes per-watcher definitions, logs, scripts, and state, but does not establish that a hook initiates the dream pass. To trace one wake-up, compare the same watcher's definition, script, state, and timestamped log. [Scheduling and watching guide](https://github.com/levifrsn63/system-prompt-collection/blob/main/Meta/muse-agent/docs/chat/scheduling-and-watching.md)
- **Initiative.** Muse documents goal follow-ups as a built-in behavior. OpenClaw's default heartbeat avoids reactivating old tasks from chat, and recurring checks can be configured explicitly. No controlled same-task, same-permissions comparison was identified in the sources reviewed. [Muse overview](https://ai.meta.com/muse/) · [OpenClaw heartbeat](https://github.com/openclaw/docs/blob/main/docs/gateway/heartbeat.md)
