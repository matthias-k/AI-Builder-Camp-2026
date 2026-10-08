# AI Builders Camp

This camp is about agentic science: driving your own AI coding agent through a real research project, hands-on, and seeing how far that gets you. We also want to learn together where this way of working helps and where it breaks. Research done this way is only useful if you can trust and reproduce what the agent did. The fastest way to stay in that loop is low-tech: read the reports your agent writes and look at the figures — that is where a wrong turn shows up first.

That is what r3 is for: a framework for making research results tracked, provenanced, and reliable. Ever had a vague memory of an analysis you did a while back, only to find you can't locate it anymore, or it no longer runs? r3 organizes and tracks all your experiments and results, tells you for each number and each plot in the final paper exactly where it came from, and helps you and your agents keep an overview of everything you did. It scales from quick-and-dirty early experiments to large, long-running ones, project pivots, and the resulting paper, without getting in your way.

## Before you arrive

- Bring a **coding agent** — Claude Code, Codex, or similar. This is what you'll drive.
- Install the **r3 toolchain**: https://github.com/matthias-k/r3-tooling
- Skim the **r3 tutorial**: https://kalliope2.matthias-k.org/bethgelab/r3-tutorial/playbook.html
- **If you're interested in the ellamind harness evaluation:** install **Docker** — it runs its evaluation environments locally in Docker.

## What we provide

API keys for lab-hosted open-weight LLMs, served through the Tübingen MLCloud **LiteLLM** proxy (OpenAI-compatible). We hand out keys at the start of the first work session. They power one of the example tasks ("How much is a prompt worth?") and ellamind's harness session.

## At the camp

We start with the r3 tutorial. After that the work sessions are yours: bring your own research project, or pick one of the [example tasks](tasks/README.md). Whatever you work on, track it in r3.

ellamind also runs a **"build your own harness"** tutorial: you run evaluation tasks on a subset of public [Terminal-Bench](https://www.tbench.ai) tasks with their Harbor harness — seeing how an environment is built, how tasks run, and how results are analyzed — then build and extend your own agent harness. It uses the same lab-hosted models.

## Repo map

- `tasks/` — the example tasks; start at [`tasks/README.md`](tasks/README.md).
