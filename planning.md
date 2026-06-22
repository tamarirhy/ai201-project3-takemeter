# TakeMeter Planning

## Community

### Chosen Community: r/nba

I chose r/nba because it is one of the largest and most active sports communities on Reddit, with thousands of daily posts and comments. The community contains a wide range of discourse styles, including statistical analysis, strategic discussion, emotional reactions, controversial opinions, and open-ended debates.

This makes it a strong classification task because discourse quality and style vary significantly. Community members frequently distinguish between thoughtful analysis, reactionary posting, and unsupported hot takes, making these distinctions meaningful and recognizable.

---

# Label Taxonomy

## Label 1: Analysis

### Definition

A post that supports its claim with evidence, statistics, historical comparisons, tactical reasoning, or detailed explanation.

### Example Posts

Example 1:
"Jokic's assist percentage and efficiency numbers show why he remains the most impactful offensive player in the league."

Example 2:
"The Celtics improved defensively because they switched more aggressively and reduced opponents' corner three-point attempts."

---

## Label 2: Hot Take

### Definition

A bold or controversial opinion expressed confidently without substantial supporting evidence.

### Example Posts

Example 1:
"Anthony Edwards is already better than prime Dwyane Wade."

Example 2:
"The Lakers should rebuild immediately and trade everyone except Luka."

---

## Label 3: Reaction

### Definition

An emotional response to a game, player performance, trade, injury, or news event that contains little or no reasoning.

### Example Posts

Example 1:
"WHAT A SHOT!"

Example 2:
"I can't believe we blew that lead."

---

## Label 4: Discussion Question

### Definition

A post whose primary purpose is to invite opinions, debate, or conversation from other users.

### Example Posts

Example 1:
"Who would you build around for the next decade: Wembanyama or Edwards?"

Example 2:
"What is the greatest Finals performance in NBA history?"

---

# Hard Edge Cases

## Edge Case

A post that contains both a strong opinion and a small amount of evidence.

Example:

"LeBron is overrated because his playoff record against top-seeded teams is below .500."

## Why It Is Difficult

The post includes evidence, which may suggest Analysis, but the evidence may simply be supporting a provocative opinion rather than forming a reasoned argument.

## Decision Rule

A post will only be labeled Analysis if evidence forms the foundation of a broader argument. If the evidence appears cherry-picked or mainly serves to strengthen a provocative opinion, the post will be labeled Hot Take.

Additional difficult cases include:

* Discussion questions that reveal the author's opinion.
* Emotional reactions that contain one sentence of reasoning.
* Short comments with minimal context.

In all cases, classification will be based on the post's primary purpose.

---

# Data Collection Plan

## Data Source

Examples will be collected from public posts and comments on r/nba.

## Target Dataset Size

Total examples: 200

Target distribution:

* Analysis: 50
* Hot Take: 50
* Reaction: 50
* Discussion Question: 50

## Collection Process

1. Browse recent discussion threads and highly active game threads.
2. Copy comments into a spreadsheet.
3. Label each example manually according to the taxonomy.
4. Review labels periodically to ensure consistency.

## If Labels Become Imbalanced

If one category becomes underrepresented:

* Search specifically for discussion threads, post-game threads, or opinion posts.
* Continue collecting examples until each category contains at least 40 examples.
* If necessary, collect additional examples beyond 200 to balance the dataset.

---

# Evaluation Metrics

Accuracy alone is not sufficient because a model could achieve high accuracy by over-predicting the most common class.

The following metrics will be used:

## Accuracy

Measures overall percentage of correct predictions.

## Precision

Measures how often a predicted label is correct.

This is important because falsely labeling Hot Takes as Analysis would reduce trust in the system.

## Recall

Measures how many true examples of a label are successfully identified.

This is important because missing legitimate Analysis posts would reduce the usefulness of the classifier.

## F1 Score

Balances precision and recall.

Since all labels are important, F1 provides a more complete evaluation than accuracy alone.

## Confusion Matrix

A confusion matrix will help identify which labels are commonly confused with one another.

This is particularly important for distinguishing Analysis and Hot Take posts.

---

# Definition of Success

The classifier will be considered successful if it achieves:

* At least 75% overall test accuracy
* Macro F1 score of at least 0.70
* No individual class F1 score below 0.60
* Better performance than the zero-shot Llama baseline

For a real-world deployment scenario, I would consider the model useful if users can generally trust distinctions between Analysis, Hot Take, Reaction, and Discussion Question posts.

If the model consistently confuses Analysis and Hot Take posts, it would not be reliable enough for deployment.

---

# AI Tool Plan

## Label Stress-Testing

Before annotation begins, ChatGPT will be used to generate 5–10 examples that intentionally sit at the boundary between Analysis and Hot Take, Analysis and Reaction, and Hot Take and Discussion Question.

If examples cannot be classified consistently using the current rules, the label definitions will be revised before data collection begins.

---

## Annotation Assistance

I may use ChatGPT to suggest preliminary labels for batches of examples.

However:

* Every label will be manually reviewed.
* Final labels will be assigned by me.
* Any AI-assisted examples will be documented in the project's AI usage section.

The AI will serve as a productivity tool rather than the final annotator.

---

## Failure Analysis

After evaluating the model, incorrect predictions will be reviewed manually and with AI assistance.

ChatGPT will be asked to identify possible patterns such as:

* Confusion between Analysis and Hot Take
* Difficulty with short comments
* Difficulty with sarcastic posts
* Difficulty with posts containing statistics

Any proposed pattern will be verified manually by reviewing the actual misclassified examples before being included in the final report.

---

# Expected Risks

1. Subjectivity when distinguishing Analysis and Hot Take posts.
2. Dataset imbalance.
3. Very short comments providing insufficient context.
4. Sarcasm and humor that obscure the true intent of the post.

These risks will be monitored throughout annotation and evaluation.

---

# Success Checklist

Before training begins:

* Community selected
* Label taxonomy finalized
* Edge cases documented
* Data collection strategy defined
* Evaluation metrics selected
* Success criteria established
* AI usage plan documented

Once these requirements are met, annotation of the 200-example dataset can begin.
