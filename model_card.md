# Model Card: Mood Machine

This model card covers both versions of the Mood Machine classifier built in this lab.

---

## 1. Model Overview

**Model type:**
Both models were built and compared: a rule-based classifier (`mood_analyzer.py`) and a machine learning classifier (`ml_experiments.py`).

**Intended purpose:**
Classify short text messages (social media posts, quick notes) into one of four mood labels: `positive`, `negative`, `neutral`, or `mixed`.

**How it works (brief):**
- **Rule-based:** Text is preprocessed (lowercased, punctuation removed, text emojis converted to words), then each token is checked against lists of positive and negative words. A numeric score is computed — negation words like "not" or "never" flip the score of the next word. If both positive and negative words are found, the label is `mixed`; otherwise the score sign determines the label.
- **ML model:** Uses scikit-learn's `CountVectorizer` to convert posts into word-count vectors (bag of words), then trains a `LogisticRegression` classifier on those vectors and the human-assigned labels from `dataset.py`.

---

## 2. Data

**Dataset description:**
The dataset contains 16 short posts in `SAMPLE_POSTS`. The original 6 starter posts were expanded with 10 new posts that include slang, text emojis, sarcasm, negation, and mixed emotions.

**Labeling process:**
Labels were assigned manually using one of four values: `positive`, `negative`, `neutral`, or `mixed`. Several posts required judgment calls:
- `"lowkey stressed but proud of myself ngl"` — labeled `mixed` because it contains both negative stress and positive pride.
- `"whatever I guess it's fine"` — labeled `neutral` because the tone is flat and resigned despite the word "fine."
- `"Oh great, another Monday"` — labeled `negative` because the intent is sarcastic, even though "great" is a positive word.

**Important characteristics:**
- Contains slang (`"lowkey"`, `"ngl"`, `"vibing"`)
- Contains text emojis (`:)`)
- Includes sarcasm (`"Oh great, another Monday"`)
- Includes negation (`"not happy"`, `"not bad"`, `"can't believe"`)
- Several posts express mixed feelings

**Possible issues with the dataset:**
- Only 16 examples — far too small for reliable ML generalization
- Labels reflect one person's interpretation; others might disagree on edge cases
- Heavily English-language and informal; formal or non-English text would be misclassified
- No true held-out test set — both models are evaluated on training data only

---

## 3. How the Rule-Based Model Works

**Scoring rules:**
- Preprocess: lowercase, remove punctuation, convert text emojis (`:)` → `"happy"`, `:(` → `"sad"`)
- For each token: +1 if in `POSITIVE_WORDS`, -1 if in `NEGATIVE_WORDS`
- Negation handling: words like `"not"`, `"never"`, `"no"`, `"can't"` flip the score of the next token
- Label mapping: if both positive and negative words appear → `"mixed"`; else score > 0 → `"positive"`, score < 0 → `"negative"`, score == 0 → `"neutral"`

**Strengths:**
- Fully transparent — every prediction can be explained by pointing to specific tokens and rules
- Consistent: the same input always produces the same output for the same reason
- Negation handling correctly flips `"not happy"` to negative and `"not bad"` to positive
- Text emoji conversion allows `:)` to contribute to scoring

**Weaknesses:**
- Cannot detect sarcasm — `"Oh great, another Monday"` is predicted `positive` because `"great"` scores +1
- Depends entirely on vocabulary coverage — unknown words are ignored
- No understanding of context, tone, or sentence structure
- Slang and informal language only works if manually added to word lists

---

## 4. How the ML Model Works

**Features used:**
Bag of words using `CountVectorizer` — each post is represented as a vector of word counts across the full vocabulary.

**Training data:**
Trained on all 16 posts in `SAMPLE_POSTS` with labels from `TRUE_LABELS`.

**Training behavior:**
The ML model achieved 1.00 (100%) accuracy on the training data. However, this is a sign of overfitting — with only 16 examples and no separate test set, the model has simply memorized the training data rather than learning generalizable patterns.

**Strengths:**
- Learns patterns from labeled examples automatically without hand-coded rules
- Correctly labeled the sarcasm case (`"Oh great, another Monday"` → `negative`) — but only because it memorized the label, not because it understood the tone

**Weaknesses:**
- 100% training accuracy on 16 examples is overfitting, not real performance
- Would likely fail on new sentences not seen during training
- No ability to generalize to unseen slang, emojis, or phrasing
- Highly sensitive to the labels provided — a mislabeled example has outsized impact

---

## 5. Evaluation

**How the models were evaluated:**
Both models were evaluated on the same 16 labeled posts in `dataset.py`. Accuracy was computed as the fraction of posts where the predicted label matched the true label.

| Model | Accuracy |
|---|---|
| Rule-based | 0.94 (15/16) |
| ML model | 1.00 (16/16) — overfitted |

**Examples of correct predictions (rule-based):**
- `"I love this class so much"` → `positive` — "love" is in `POSITIVE_WORDS`, no negative signals
- `"I am not happy about this"` → `negative` — negation handling flips "happy" from +1 to -1
- `"Feeling tired but kind of hopeful"` → `mixed` — "tired" (negative) and "hopeful" (positive) both found

**Examples of incorrect predictions (rule-based):**
- `"Oh great, another Monday"` → `positive` (true: `negative`) — sarcasm; the model sees "great" and scores +1 with no awareness of tone

**How their failures differed:**
The ML model had no failures on training data (overfitting). The rule-based model failed only on the sarcasm case. If tested on new, unseen sentences, the ML model would likely fail more broadly — especially on any phrasing not present in the 16 training examples.

---

## 6. Limitations

- **Sarcasm is not detectable.** The rule-based model will always misclassify sarcastic positives like `"Oh great, another Monday"` as positive. No keyword-based approach can reliably fix this without understanding context.
- **Dataset is too small for ML.** 16 examples cannot train a model that generalizes. The ML model's 100% accuracy is misleading — it reflects memorization.
- **Vocabulary coverage is manual.** The rule-based model only knows the words explicitly added to `POSITIVE_WORDS` and `NEGATIVE_WORDS`. Novel slang or informal language is silently ignored.
- **No test set.** Both models are evaluated on their training data. Real accuracy on unseen posts is unknown.
- **Mixed label is fragile.** The rule-based model only predicts `mixed` when both a positive and a negative word appear. Nuanced posts without recognized vocabulary are labeled `neutral` instead.

---

## 7. Ethical Considerations

- **Misclassifying distress:** A post like `"I'm not okay"` could be predicted `positive` (negation flipping "okay") or `neutral` (if "okay" is not in the vocab). Using this system to screen for emotional wellbeing in real applications would be unreliable and potentially harmful.
- **Language bias:** The dataset reflects one person's informal English. Slang, dialects, or cultural expressions not in the vocabulary are invisible to the model. This means the system works best for whoever built it and may misinterpret others.
- **Privacy:** Analyzing personal messages for mood without consent raises privacy concerns, even with a simple keyword model.
- **False confidence:** A 94% accuracy number on 16 training examples can feel impressive but is not a reliable signal for real-world deployment.

---

## 8. Ideas for Improvement

- **Add more labeled data.** Even 100-200 diverse examples would make the ML model meaningfully more robust.
- **Use TF-IDF instead of CountVectorizer.** This would down-weight common words and give more signal to distinctive ones.
- **Add a real test set.** Split data into train/test so accuracy reflects actual generalization, not memorization.
- **Improve sarcasm handling.** One approach: flag sentences with positive words followed by negative context phrases (e.g. "great... another Monday") as potentially sarcastic.
- **Expand emoji support.** Map Unicode emojis (😂, 🥲, 💀) to sentiment tokens the same way text emojis are handled.
- **Use a small transformer model.** A pretrained model like DistilBERT fine-tuned on sentiment data would handle sarcasm and context far better than either approach here.
- **Improve the `mixed` label.** Track positive and negative hit counts separately in `score_text` and use the ratio to assign `mixed` more precisely.
