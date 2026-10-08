# Going further with r3

Cross-cutting directions to try *on top of* r3, independent of which task you pick. Each
is a thing to try, not a prescription, and each stays tracked in r3 like everything else.

- **Auto-research / auto-science.** Let your agent run a research loop largely on its
  own — propose sub-experiments, run each as an r3 job, and build toward a strong
  "topline" result — then use the r3 graph to reconstruct what it tried and whether the
  trail holds up. An autonomous loop and r3's provenance are a natural fit: the graph is what lets you
  trust and reconstruct what the loop actually did, not just what it claims it did. We ran
  one before the camp and it held up well — worth a try.
- **Agentic exploratory data analysis.** Point your agent at a dataset (see [datasets to
  explore](datasets.md)) and let it explore open-endedly, with each analysis a committed job
  plus a short report, so the exploration stays reproducible.
- **Report review loops.** Have independent reviewer agents critique a report (each taking
  a single stance), collect the issues, pick the real ones, and revise — mapped onto the
  r3 version chain.

This is a living list. Bring your own ideas.
