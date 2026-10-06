# Datasets to explore

Bring your own question — these are starting points, not instructions. Each is openly
downloadable, laptop-sized, and has something concrete to measure or model, so the work maps
cleanly onto r3's committed jobs → score → report. Pick one, ask something you actually want
to know, and track the whole trail.

## Neuroscience & clinical

*(These complement the [natural-image-statistics task](natural-image-statistics.md), which
already covers visual neuroscience — pick a different signal.)*

- **[MIT-BIH Arrhythmia Database](https://physionet.org/content/mitdb/)** — 48 half-hour
  two-channel ECG records at 360 Hz with ~110k cardiologist-labeled beats; ~100 MB total.
  *Ask:* how far does a simple morphology-based beat classifier get, and which arrhythmias
  does it reliably miss?
- **[Gait in Neurodegenerative Disease](https://physionet.org/content/gaitndd/)** —
  stride-interval time series from Parkinson's, Huntington's, ALS, and controls (64
  records); a few MB.
  *Ask:* does stride-to-stride timing variability separate the three diseases from each
  other, not just from healthy gait?
- **[Oxford Parkinson's Voice](https://archive.ics.uci.edu/dataset/174/parkinsons)** — 195
  sustained-vowel recordings from 31 people (23 with PD), 23 dysphonia features; a ~40 KB
  CSV.
  *Ask:* which voice measures actually carry the signal, and does accuracy survive an honest
  per-speaker split instead of per-recording?
- **[Sleep-EDF](https://physionet.org/content/sleep-edf/)** — whole-night EEG/EOG/EMG with
  expert sleep-stage hypnograms; ~315 MB for the original set (the "expanded" version is 8
  GB, so stay with a few nights).
  *Ask:* how well can you stage sleep from a single EEG channel, and which stages are the
  ones that blur together?
- **[PubMedQA](https://huggingface.co/datasets/qiaojin/PubMedQA)** — 1k expert-labeled
  biomedical questions answered yes/no/maybe over their PubMed abstracts; a few MB. *(lab
  LLM optional)*
  *Ask:* does handing a lab-hosted LLM the abstract beat answering from the question alone,
  and where does "maybe" trip it up?

## Other curiosities

- **[SILSO Sunspot Number](https://sidc.be/SILSO/datafiles)** — the monthly sunspot record
  since 1749 (daily since 1818); one small CSV.
  *Ask:* measure the ~11-year cycle — are its length and amplitude actually constant, or do
  they wander?
- **[USGS Earthquake Catalog](https://earthquake.usgs.gov/fdsnws/event/1/)** — every
  catalogued earthquake, queryable by region, time, and magnitude; CSV out, you pick the
  size.
  *Ask:* fit the Gutenberg-Richter law and estimate the b-value — does it hold across
  regions, and below what magnitude does the catalog quietly go incomplete?
- **[Gravitational-wave strain (GWOSC)](https://gwosc.org)** — public LIGO/Virgo detector
  strain around events like GW150914; seconds of data are a few MB.
  *Ask:* can you pull the chirp out of the noise and recover its rising frequency without
  the official pipeline?
- **[LIAR](https://huggingface.co/datasets/ucsbnlp/liar)** — 12.8k short political claims
  rated for truthfulness by PolitiFact editors, with speaker/context metadata; a few MB.
  *(lab LLM optional)*
  *Ask:* can an LLM (or a plain bag-of-words baseline) predict the six-way truth rating, and
  does the metadata beat the words?

---

These are seeds, not a catalogue — bring your own dataset and question too. See
[going further with r3](going-further.md) for directions that apply whatever you pick.
