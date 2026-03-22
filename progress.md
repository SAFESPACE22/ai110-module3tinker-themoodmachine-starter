# Mood Machine - Lab Progress

## Part 1: Build the Dataset
- [x] Fork the repo and open in VS Code
- [x] Read through `dataset.py` and understand `SAMPLE_POSTS` / `TRUE_LABELS`
- [x] Add 5-10 new posts with realistic language (slang, emojis, sarcasm, mixed)
- [x] Add matching labels to `TRUE_LABELS` (one per post)
- [x] Run `python main.py` and confirm no length errors

**Checkpoint:** Program runs without errors and expanded dataset loads correctly.

---

## Part 2: Engineer the Rule-Based Brain
- [ ] Open `mood_analyzer.py` and review `preprocess`, `score_text`, `predict_label`
- [ ] Improve `preprocess` (e.g. remove punctuation, handle basic emojis)
- [ ] Implement `score_text` — loop tokens, +1 for positive words, -1 for negative
- [ ] Add at least ONE enhancement (negation handling, word weighting, emoji signals)
- [ ] Implement `predict_label` — map score to "positive", "negative", "neutral", or "mixed"
- [ ] Run `python main.py` and test a few obvious cases
- [ ] Print intermediate values (tokens, scores) if predictions look wrong

**Checkpoint:** Rule-based classifier produces reasonable predictions for simple cases.

---

## Part 3: Stress Test and Break the Model
- [ ] Run `python main.py` on current rule-based system
- [ ] Create a small set of "breaker" sentences:
  - Sarcasm: e.g. "I love getting stuck in traffic"
  - Slang: e.g. "that movie was sick"
  - Emojis: e.g. "I'm fine :/"
  - Mixed: e.g. "I'm exhausted but proud of myself"
- [ ] Run each breaker and note which words affected the score
- [ ] Identify one clear failure pattern
- [ ] Pick ONE failure to fix (update `dataset.py` vocab or `mood_analyzer.py` logic)
- [ ] Re-run to confirm the fix helped without breaking other examples

**Checkpoint:** Can show one misclassified sentence, explain why it failed, and show the targeted fix.

---

## Part 4: Evaluate Your System
- [ ] Confirm every post in `SAMPLE_POSTS` has a label in `TRUE_LABELS`
- [ ] Run rule-based model: `python main.py`
- [ ] Study the mismatches — what patterns do you see in the failures?
- [ ] Choose one incorrect prediction and analyze it in detail
- [ ] Decide: fix it now, or document it as a known limitation?
- [ ] Run ML model: `python ml_experiments.py`
- [ ] Compare ML vs rule-based — do they fail in the same places?
- [ ] Add a few more labeled posts and observe how each model changes

**Checkpoint:** Can describe what each model gets right/wrong and why rule-based vs ML behave differently.

---

## Part 5: The Model Card
- [ ] Open `model_card.md`
- [ ] Fill in **Section 1** — model type, intended purpose, how it works
- [ ] Fill in **Section 2** — dataset description, labeling process, characteristics
- [ ] Fill in **Section 3** — rule-based scoring rules, strengths, weaknesses
- [ ] Fill in **Section 4** — ML model features, training behavior, strengths/weaknesses
- [ ] Fill in **Section 5** — evaluation results, correct/incorrect prediction examples
- [ ] Fill in **Section 6** — known limitations with specific examples
- [ ] Fill in **Section 7** — ethical considerations
- [ ] Fill in **Section 8** — ideas for improvement
- [ ] Add ML vs rule-based comparison based on `ml_experiments.py` results

**Checkpoint:** A reader can open the repo, read the model card, and clearly understand how the Mood Machine works, what data shaped it, where it succeeds, and where it fails.
