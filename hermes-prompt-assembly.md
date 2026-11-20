# How Hermes Agent assembles a model request

Research note, November 20, 2026.

In Hermes Agent, *composer* refers to the terminal message editor. It holds the draft, preserves multiline input, and lets the user edit queued messages. When the user submits a draft, the submission code routes it. The Python backend assembles the context that accompanies the message to the model. [Terminal UI source guide](https://github.com/NousResearch/hermes-agent/blob/main/ui-tui/README.md) · [Agent architecture](https://hermes-agent.nousresearch.com/docs/developer-guide/architecture)

## The reusable system prompt

Hermes assembles three tiers in order. The order keeps commonly reused instructions toward the beginning of the request, which helps providers reuse a cached prefix. [Prompt assembly guide](https://hermes-agent.nousresearch.com/docs/developer-guide/prompt-assembly)

| Order | Tier | What Hermes includes |
| --- | --- | --- |
| 1 | Stable | Agent identity from `SOUL.md` or the fallback identity, tool and model guidance, and general instructions for coding work. |
| 2 | Context | Caller-supplied system instructions, applicable project files such as `AGENTS.md`, a Git workspace snapshot, operator instructions, and platform hints. |
| 3 | Volatile | An index of available skills, snapshots of `MEMORY.md` and `USER.md`, an optional external memory-provider block, and session details such as model, provider, time, and working directory. |

The skills index tells the model which skills it can look up. The memory and profile snapshots give the session a consistent starting point. Hermes can also add temporary guidance for a particular model call while preserving its reusable system prompt. The code that orders the tiers lives in `agent/system_prompt.py`; the helpers that build individual pieces live in `agent/prompt_builder.py`. [Tier assembly](https://github.com/NousResearch/hermes-agent/blob/main/agent/system_prompt.py) · [Builder helpers](https://github.com/NousResearch/hermes-agent/blob/main/agent/prompt_builder.py)

## Example model call

Suppose the user types “Fix the failing test.” That sentence becomes the user message. If the repository’s `AGENTS.md` specifies a test command, Hermes can supply that instruction through the project tier. Earlier conversation turns supply the context for which test the user means. The enabled shell tool’s schema separately tells the model which arguments it can supply when requesting execution. The backend formats these pieces for the selected provider. [Prompt assembly guide](https://hermes-agent.nousresearch.com/docs/developer-guide/prompt-assembly) · [CLI reference](https://hermes-agent.nousresearch.com/docs/reference/cli-commands)

## Inspect a local installation

Run `hermes prompt-size` from the repository of interest for an offline byte breakdown of the fixed request content for a fresh session, including the prompt tiers and enabled tool schemas. Use `hermes prompt-size --json` to compare configurations programmatically. Conversation history and future user messages fall outside this measurement. [CLI command reference](https://hermes-agent.nousresearch.com/docs/reference/cli-commands)
