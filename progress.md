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
- [x] Open `mood_analyzer.py` and review `preprocess`, `score_text`, `predict_label`
- [x] Improve `preprocess` (removes punctuation, maps text emojis like ":)" to words)
- [x] Implement `score_text` — loop tokens, +1 for positive words, -1 for negative
- [x] Add at least ONE enhancement (negation handling: "not happy" flips the score)
- [x] Implement `predict_label` — maps score to "positive", "negative", "neutral", or "mixed"
- [x] Run `python main.py` — accuracy: 0.88 (14/16 correct)
- [x] Two failures identified: sarcasm ("Oh great, another Monday") and missing vocab ("finished")

**Checkpoint:** Rule-based classifier produces reasonable predictions for simple cases. ✓

---

## Part 3: Stress Test and Break the Model
- [x] Run `python main.py` on current rule-based system
- [x] Identified breaker sentences from existing dataset
- [x] Failure 1 — Sarcasm: "Oh great, another Monday" → positive (true: negative). "great" scores +1 with no context awareness.
- [x] Failure 2 — Missing vocab: "I'm exhausted but I finally finished it" → negative (true: mixed). "finished" wasn't recognized as positive.
- [x] Fixed failure 2: added "finished", "accomplished", "done" etc. to POSITIVE_WORDS → now predicts mixed correctly
- [x] Re-ran: accuracy went from 0.88 → 0.94. Sarcasm still fails (documented as known limitation).

**Checkpoint:** Can show one misclassified sentence, explain why it failed, and show the targeted fix. ✓

---

## Part 4: Evaluate Your System
- [x] Confirmed SAMPLE_POSTS and TRUE_LABELS are aligned (16 each)
- [x] Rule-based model: 0.94 accuracy — only failure is sarcasm ("Oh great, another Monday")
- [x] ML model: 1.00 accuracy on training data — but this is overfitting, not true generalization
- [x] ML "got" the sarcasm case by memorizing the label; rule-based failed because "great" always scores positive
- [x] ML would likely fail on new sarcastic sentences it hasn't seen before

**Checkpoint:** Can describe what each model gets right/wrong and why rule-based vs ML behave differently. ✓

---

## Part 5: The Model Card
- [x] Fill in **Section 1** — both models described (rule-based + ML)
- [x] Fill in **Section 2** — 16 posts, labeling decisions documented, edge cases noted
- [x] Fill in **Section 3** — scoring rules, negation handling, emoji conversion, strengths/weaknesses
- [x] Fill in **Section 4** — bag-of-words + logistic regression, overfitting noted
- [x] Fill in **Section 5** — rule-based 0.94, ML 1.00 (overfitted), examples of correct/incorrect predictions
- [x] Fill in **Section 6** — sarcasm, small dataset, no test set, vocabulary coverage
- [x] Fill in **Section 7** — distress misclassification, language bias, privacy, false confidence
- [x] Fill in **Section 8** — more data, TF-IDF, test set, transformer model, emoji expansion
- [x] ML vs rule-based comparison included in sections 4 and 5

**Checkpoint:** A reader can open the repo, read the model card, and clearly understand how the Mood Machine works, what data shaped it, where it succeeds, and where it fails. ✓
