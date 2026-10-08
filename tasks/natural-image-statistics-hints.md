# Hints: what makes a good model of natural images?

Optional, and mildly spoiler-y. These are nudges for when a session stalls on
[the task](natural-image-statistics.md) — skip them if you'd rather find the surprises
yourself. They're the things sessions tend to trip on, framed as questions rather than
answers.

- **Read the metric right.** The score is an average *negative* log-likelihood,
  `−⟨log₂ p(x)⟩ / d` in bits per dimension: **lower is better**. Most of that large
  number is the fixed cost of coding raw intensities, shared by every model — only the
  *gap* between models carries any signal. Put the formula on the plot, so "shouldn't
  more bits be better?" never bites mid-session.

- **Beautiful filters, tiny payoff.** The ICA filters come out looking like textbook
  oriented V1 cells, which makes them the obvious "winner" by eye. In held-out bits,
  though, that oriented recoding adds almost nothing over plain whitening — the big jump
  is the earlier second-order decorrelation (retina→LGN), not the V1-style step on top.
  Worth noticing how far a model's visual appeal can run ahead of what it actually buys.

- **The real leftover is contrast.** Look at the whitened marginals: strongly
  heavy-tailed. That points at per-patch contrast/variance — structure the linear codes
  leave on the table. Model the *length* of each whitened patch (a radial / spherical
  density) and you can beat the best linear code by several times the whole ICA-over-PCA
  gap. (This is the hook behind the "no filters at all" stretch goal.)

- **Deep models want data.** A normalizing flow has the capacity to beat the linear codes,
  but on a small sample (a hundred images, a limited patch budget) it overfits before that
  capacity pays off — give it enough data and a plain flow does pull ahead of PCA and ICA.
  Reaching the best *contrast* model that way takes far more data still, so building the
  known structure in first — a flow on the whitened / contrast residual — is the cheaper
  route.

- **Two habits that save you.** Probe small and fast before you commit a job. Nothing here
  needs to run long, so a quick probe mostly catches a broken or mis-sized setup before it
  eats your limited time — and the habit matters far more at full scale, where a bad run can
  churn for hours. And make each report *show* the evidence for its claims — plot the actual
  marginal — rather than citing a summary statistic and asserting the rest.
