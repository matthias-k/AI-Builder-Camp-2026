# Hints: how much is a prompt worth?

Optional, and mildly spoiler-y. These are nudges for when a session stalls on
[the task](prompt-sensitivity.md) — skip them if you'd rather hit the surprises yourself.
They're the things sessions tend to trip on.

- **Answer extraction is where accuracy quietly leaks.** The shared extractor has to be
  validated against the raw responses, not just "it runs". A model that replies "The answer
  is B." or reasons for a sentence first will fool a naive parser and sink an otherwise good
  prompt — so the parser's failures look like the prompt's failures. Spot-check what it pulls
  out of a handful of real responses before you trust any score.

- **If you reach for GEPA (the optimizer direction):** the reflector model you pick and
  keeping prompts short matter as much as the search itself. The default reflection setup can
  balloon the prompt into a multi-page instruction, or let the optimizer quietly game the
  metric instead of getting better at the task. Read the prompts it actually produces, and
  budget your metric calls before you start — the search spends them fast. (Grounded in real
  runs in this lab.)

- **Put an error bar on every number.** With ~150 questions, a few points of difference is
  often just noise. A binomial confidence interval on each accuracy tells you whether a gap
  between two prompts (or two models) is real before you spend jobs chasing it.
