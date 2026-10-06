# How random is an LLM?

> An example task for the AI Builders Camp — see all tasks in [the task index](./README.md).

Ask an LLM to "pick a number" and it has favorites: 7, 37, and 42 come up far more often than
chance. The sharper question behind "how random is it?" is whether it can give you *any*
distribution you ask for. Uniform randomness is the easy entry point; matching a distribution
you *specify* — a Gaussian of given mean and variance, say — is the deeper version. LLMs are
revealingly bad at both, because they pattern-match rather than compute: a small model piles
most of its mass on a couple of digits whatever you ask — a GPT-2-class model puts ~35–40 % on
"0" and "1" regardless of the target, a raw token prior, not a calculation. Phrasing moves the
result, and the model *ranking* can flip with it. Everything is tracked in r3.

**Setup:** r3 (see the [tutorial](https://kalliope2.matthias-k.org/bethgelab/r3-tutorial/playbook.html), §1) and an API key for the lab-hosted models
(`deepseek-ai/DeepSeek-V4.1-Flash`, `google/gemma-4-31B-it-qat-w4a16-ct`,
`Qwen/Qwen3.6-35B-A3B`). They are served through an OpenAI-compatible LiteLLM proxy, so the
`openai` package works with the base URL and key you get from us. Quarto comes from your
compute-environment job. It all runs on a laptop CPU; the only cost is API calls, so keep
prompts short and `max_tokens` tiny, don't re-sample what you already have, and remember the
models sit on a shared cluster.

**Thinking mode.** Qwen and Gemma think before they answer by default, which is slow when you
want thousands of one-token answers. Switch it off per request:

```python
client.chat.completions.create(
    model="Qwen/Qwen3.6-35B-A3B", messages=messages, max_tokens=4,
    extra_body={"chat_template_kwargs": {"enable_thinking": False}},
)
```

DeepSeek answers directly. Thinking on or off is a setting like any other: record it in the
query job's `config.yaml`.

**Rules of the game**

- Every result comes from a committed r3 job, and each step ends in a **Quarto report,
  committed as an r3 job**.
- **Score every configuration the same way:** a distance from the target distribution.
- Put the model and sampling settings in the query job's `config.yaml`. **Never commit your
  API key**: read it from an environment variable.
- Smoke-test on a few samples before committing, and remove (`r3 remove`) or tag failed jobs
  so they don't pollute your histograms. Watch whether your agent does this without being told.

## Starting point

**Step 1 — Environment.** Make your compute environment a job: a `venv` with e.g. `openai
numpy scipy pandas matplotlib jupyter` in its output (`jupyter` lets Quarto render reports in
the same environment). All later jobs depend on it. Watch whether your agent reaches for a
committed job rather than a throwaway.

**Step 2 — The basic probe.** Sampling is the dependable route, and works through any
LiteLLM endpoint. Pick one simple ask — uniform over a small range, or a digit 0–9 — sample a
lot, histogram the answers, measure the distance from uniform (KL divergence, a chi-square
goodness-of-fit, entropy), and report the model's favorite values.

**Step 3 — What moves it?** Vary one thing at a time — temperature, prompt phrasing, thinking
on/off, model — each as its own query job. *Report:* the distance-to-uniform across
configurations. Does it improve? Do the three lab models share the same favorites, and does
the ranking flip with the phrasing?

**Step 4 — Ask for a shape.** Request a *specified* distribution: a discretized Gaussian with
a given mean and variance over a small range. Measure the KL to the target, recover the
empirical mean and variance, and check the autocorrelation along a sequence — are the answers
even independent? *Report:* how close can it get, and does a better-worded prompt close the gap?

## Where to take it

- **Debias it.** Wrap the model in a sampler that corrects its bias (rejection sampling, or
  reweighting by the measured histogram) into a near-uniform generator, and measure the
  overhead in model calls per accepted sample.
- **Widen the range.** Push from 0–9 to 1–100 or wider. Do the favorites survive, and does the
  distance grow?
- **One call vs. thousands.** If the endpoint exposes token logprobs (`logprobs` /
  `top_logprobs`), you can read the whole next-token distribution in a single call instead of
  sampling. Compare that readout to your sampled estimate — how many samples does it take to
  agree?
- **Connect it to prompt optimization.** The sibling task [*How much is a prompt
  worth?*](./prompt-sensitivity.md) optimizes a prompt against a score; its GEPA direction
  would optimize one against exactly this distance-to-target.
