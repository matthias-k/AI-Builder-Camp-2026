# What makes a good model of natural images?

> An example task for the AI Builders Camp — see all tasks in [the task index](./README.md).

Natural image patches are far from random. How well do simple models capture their
statistics, and what does each model add? You will build up the answer step by step,
and track everything in r3: environment, data, code, results, and reports.

Work with your coding agent or by hand. Everything runs on a laptop CPU.

**Setup:** r3 (see the [tutorial](https://kalliope2.matthias-k.org/bethgelab/r3-tutorial/playbook.html), §1). Your compute-environment job (Step 1) provides Quarto and the Python packages, so you don't install those yourself.

**What good agentic r3 work looks like**

- Your agent should produce every result from a committed r3 job, never from
  uncommitted code — core r3 workflow; watch that it holds to it.
- Each step should end with a **Quarto report, committed as an r3 job**, built from
  the committed results — your agent should reach for this by default.
- **Score every model the same way:** average negative log-likelihood of held-out
  patches in **bits per dimension**. If a model transforms the data, include the
  Jacobian (log |det|) of the transform.
- Before committing a job, your agent should smoke-test it in a dev-checkout, and remove
  or tag jobs that failed so they don't pollute your queries. This is standard r3 workflow
  — watch whether your agent does it on its own.

---

## Step 1 — Environment

Your compute environment is itself an r3 job: all later jobs depend on it and run their
code inside it, so every result records exactly which packages produced it. Build it with
`numpy`, `scipy`, `scikit-learn`, `matplotlib`, and `jupyter` (so Quarto can render your
reports in the same environment). Watch whether your agent sets this up as a committed job
rather than a throwaway environment.

## Step 2 — Data

Make the first 20 images of the **van Hateren image set** a dataset job. It is a
classic dataset for natural image statistics, because it stores calibrated brightness
values rather than just RGB values. Download the linearized `.iml` files from
[Paul Ivanov's mirror](https://pirsquared.org/research/vhatdb/) (`imk00001.iml`,
`imk00002.iml`, …; about 3 MB each: 1536 × 1024 pixels, 16-bit big-endian, linear in
luminance). If the wifi struggles, ask us for a local copy.

Then add a preprocessing job that depends on it:

- convert to log intensities
- sample 8×8 patches
- remove the mean brightness (DC component) of each patch
- split the patches into train and test sets

## Step 3 — PCA vs. ICA

Fit two linear models of the same shape: assume the patch can be linearly transformed
into a basis whose components are independent, and model each component with a flexible
1-D density (e.g. a generalized Gaussian). The two differ only in the basis:

- the **PCA** basis
- the **ICA** basis (e.g. FastICA)

*Report:* the learned filters and the scores. How do the filters differ? How much
better is ICA?

## Step 4 — Retina, LGN, V1

Early vision can be read as a sequence of bases for the same image:

- the **pixel basis**, like the retina
- **symmetric whitening**, whose filters look like center-surround receptive fields,
  like the LGN
- **ICA**, whose filters look like oriented simple cells, like V1

Score all three in the same way as in Step 3. Add **PCA** and a **random whitening
basis** for comparison.

*Report:* one table and one figure with all five bases. How much does each stage gain
over the one before? How much of ICA's gain would any whitening basis already give you?

## Stretch goals

- **Deep models:** can a neural network do better? You need a model that gives you log
  likelihoods, e.g. a normalizing flow; small ones train fine on a CPU. Does it beat ICA?
- **No filters at all:** whiten, then model only the length ‖z‖ of each whitened patch
  (e.g. with a gamma distribution). How does this model compare to ICA?
- **Larger patches** (12×12, 16×16): does the gap between the models grow or shrink?
