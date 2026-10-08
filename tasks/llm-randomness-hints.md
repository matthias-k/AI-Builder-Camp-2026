# Hints: how random is an LLM?

Optional, and mildly spoiler-y. These are nudges for when a session stalls on
[the task](llm-randomness.md) — skip them if you'd rather hit the surprises yourself.
They're the things sessions tend to trip on.

- **Make it emit only the number.** A tiny `max_tokens`, a strict instruction, and
  defensive parsing keep "Sure, I'll go with 7!" out of your histogram. Anything you fail to
  parse is a silent hole in the distribution, not a neutral drop — decide what happens to it.

- **You need a lot of draws.** It takes thousands of samples to pin down "it loves 7";
  a few hundred won't separate a real bias from chance. Put an error bar on each bar of the
  histogram, so you can see which favorites are solid and which are sampling jitter.

- **Asking for a shape often backfires.** Request a *specific* distribution and the model may
  ignore it outright, or collapse to near-deterministic output on one or two values. Compare
  what you get to the target with KL or chi-square, and recover the empirical mean and
  variance, to see how far off it really is rather than eyeballing the bars. (Seen in real
  digit-distribution experiments in this lab.)

- **If you use the logprobs route, read it correctly.** Confirm that each option (each digit)
  is a single token for the tokenizer in use, and mind the chat template around your prompt.
  Otherwise you are reading a distribution over the wrong tokens — leading whitespace, a
  multi-token "10", a formatting token — and it will look cleaner than sampling while being
  quietly wrong.
