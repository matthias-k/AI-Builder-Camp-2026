# How much is a prompt worth?

> An example task for the AI Builders Camp — see all tasks in [the task index](./README.md).

LLM benchmark scores look precise: "model X gets 71.3 %". But the same model on the same
questions can score quite differently depending on how you ask. How fragile are these
numbers, and how much can you gain by optimizing the prompt, or the harness around the
model? You get a detailed starting point, then choose where to take it. Everything is
tracked in r3.

**Setup:** r3 (see the [tutorial](https://kalliope2.matthias-k.org/bethgelab/r3-tutorial/playbook.html), §1) and an API key for the lab-hosted models
(`deepseek-ai/DeepSeek-V4.1-Flash`, `google/gemma-4-31B-it-qat-w4a16-ct`,
`Qwen/Qwen3.6-35B-A3B`). They are served through an OpenAI-compatible LiteLLM proxy, so the
`openai` Python package works with the base URL and key you get from us. Quarto comes from your compute-environment job, so you don't install it yourself.

**Thinking mode.** Qwen and Gemma think before they answer by default, which takes many
seconds per question. Switch it off per request:

```python
client.chat.completions.create(
    model="Qwen/Qwen3.6-35B-A3B", messages=messages, max_tokens=16,
    extra_body={"chat_template_kwargs": {"enable_thinking": False}},
)
```

DeepSeek answers directly by default. Thinking on or off is a setting like any other:
record it in the query job's `config.yaml`.

**Rules of the game**

- Every result comes from a committed r3 job. Each analysis ends in a **Quarto report,
  committed as an r3 job**.
- **Query and score in separate jobs.** A query job sends the prompts and stores the raw
  responses in its output. A scoring job depends on it and computes the accuracy. Keep the
  answer extraction in one place that all scoring jobs share.
- Put the model name and sampling settings in the query job's `config.yaml`. **Never
  commit your API key**: read it from an environment variable.
- Tune on the dev set only. Use the test set once, for final numbers.
- Try a job on a few questions before you commit it, and remove (`r3 remove`) or tag
  jobs that failed, so they don't end up in your queries.
- The models run on a shared cluster: keep outputs short, and don't re-run queries you
  already have.

## Starting point

**Step 1 — Environment and data.** Make your compute environment a job too: a job that
builds a Python virtual environment (`venv`) with e.g. `openai datasets pyyaml pandas
matplotlib jupyter` in its output (`jupyter` lets Quarto render your reports inside the
same environment). All later jobs depend on it and run inside it, so every result
records exactly which packages produced it. Then a dataset job that downloads the
multiple-choice tasks of [BIG-Bench Hard](https://huggingface.co/datasets/lukaemon/bbh)
(the tasks whose questions end in an `Options:` list) and draws a fixed random subset,
e.g. 150 dev and 150 test questions. Split the answer options out of the question text,
so you can format them differently later.

**Step 2 — Baseline.** One query job per model: every dev question as a multiple-choice
prompt, "answer with the letter only", temperature 0, a small `max_tokens`. Store per
question: id, the full prompt (all messages), raw response, token counts. Then one
scoring job per query job.

*Report:* dev accuracy per model, with error bars. How big a difference could be pure
noise with 150 questions?

**Step 3 — Does the format matter?** Write about 6 prompt formats that *shouldn't*
matter. Each changes one surface detail of the baseline and keeps its instruction
("answer with the letter only"): option labels (A/B/C vs. 1/2/3), separators and
whitespace, an extra "Answer:" line at the end, the instruction in a system prompt,
shuffled option order. Generate one query
job per format × model with a small script, like the sweep in the tutorial (§11.3).

*Report:* the accuracy range per model across formats. Is the spread bigger than the
noise? Does the ranking of the models change with the format?

**Step 4 — Optimize the prompt.** Choose one model and improve its prompt on the dev set:
better instructions, few-shot examples (from the dev set only), a different answer format.
Every attempt is a committed job. Then run both the baseline prompt and your best prompt
on the test set, once, for all three models, so you compare like with like.

*Report:* how much did you gain on dev, and how much of it holds up on test (baseline vs.
best prompt on the same test questions)? Does your prompt also help the other two models?

## Where to take it

Pick any:

- **Optimize the harness.** Go beyond a single call: switch thinking on, let the model
  reason before it answers, sample several answers and take a majority vote, or have the
  model check its own answer. Count the tokens. Which harness gives the most accuracy per
  token? Thinking costs hundreds of tokens and up to ~20 s per question, so start with
  at most ~50 questions.
- **Let your coding agent optimize.** Give your coding agent the dev set, your scoring
  setup and a budget (e.g. 20 query jobs), and let it search on its own. Afterwards, use
  the r3 graph to reconstruct what it tried. Did any of its jobs ever depend on the test
  set?
- **Let an optimizer search the prompt.** Instead of hand-tuning, use a dedicated prompt
  optimizer. GEPA ("Genetic-Pareto") is one: it evolves prompts by having an LLM reflect
  on execution traces and keeps a Pareto front of candidates, and it's available in DSPy
  (`dspy.GEPA`). Run it on the dev set and compare its best prompt against your Step 4
  prompt, counting the tokens it spends — and, as with the other options, reconstruct from
  the r3 graph what it tried.
