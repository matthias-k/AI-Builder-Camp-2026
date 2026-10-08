# AI Builders Camp

This camp is about agentic science: driving your own AI coding agent through a real research project, hands-on, and seeing how far that gets you. We also want to learn together where this way of working helps and where it breaks. Research done this way is only useful if you can trust and reproduce what the agent did. The fastest way to stay in that loop is low-tech: read the reports your agent writes and look at the figures — that is where a wrong turn shows up first.

That is what r3 is for: a framework for making research results tracked, provenanced, and reliable. Ever had a vague memory of an analysis you did a while back, only to find you can't locate it anymore, or it no longer runs? r3 organizes and tracks all your experiments and results, tells you for each number and each plot in the final paper exactly where it came from, and helps you and your agents keep an overview of everything you did. It scales from quick-and-dirty early experiments to large, long-running ones, project pivots, and the resulting paper, without getting in your way.

## Before you arrive

- Bring a **coding agent** — Claude Code, Codex, or similar. This is what you'll drive.
- Install the **r3 toolchain**: https://github.com/matthias-k/r3-tooling. It also installs the two repos it builds on, worth a look:
  - **r3** — the provenance engine itself: https://github.com/mtangemann/r3
  - **foreman** — for browsing jobs, their dependency graph, and reports: https://github.com/matthias-k/foreman-ai-builder-camp
- Skim the **r3 tutorial**: https://kalliope2.matthias-k.org/bethgelab/r3-tutorial/playbook.html
- **If you're interested in the ellamind harness evaluation:** install **Docker** — it runs its evaluation environments locally in Docker.

## What we provide

API keys for lab-hosted open-weight LLMs, served through the Tübingen MLCloud **LiteLLM** proxy (OpenAI-compatible). We hand out keys at the start of the first work session. They power some of the example tasks and ellamind's harness session.

## At the camp

We start with the r3 tutorial. After that the work sessions are yours: bring your own research project, or pick one of the **example tasks** below. Whatever you work on, track it in r3.

ellamind also runs a **"build your own harness"** tutorial: you run evaluation tasks on a subset of public [Terminal-Bench](https://www.tbench.ai) tasks with their Harbor harness — seeing how an environment is built, how tasks run, and how results are analyzed — then build and extend your own agent harness. It uses the same lab-hosted models.

## Example tasks

Each gives a concrete starting point and ideas for where to take it, plus a short "stuck?" companion of hints.

- [What makes a good model of natural images?](tasks/natural-image-statistics.md) — classic natural-image-statistics models (PCA/ICA/whitening/…), runs on a laptop CPU.
- [How much is a prompt worth?](tasks/prompt-sensitivity.md) — how fragile LLM benchmark scores are to prompt formatting; uses the lab-hosted models.
- [How random is an LLM?](tasks/llm-randomness.md) — can an LLM give you the distribution you ask for? Randomness is the easy case; uses the lab-hosted models, runs on a laptop.

See also [going further with r3](tasks/going-further.md) — cross-cutting things to try on top of r3, whatever you work on — and [datasets to explore](tasks/datasets.md), laptop-sized open datasets to bring your own question to.
