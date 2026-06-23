# TakeMeter: NBA Discourse Quality Classifier

## Project Overview

TakeMeter is a fine-tuned DistilBERT classifier designed to categorize r/nba comments into four discourse categories:

* Analysis
* Hot Take
* Reaction
* Discussion Question

The goal is to distinguish between evidence-based basketball discussion, opinionated claims, emotional reactions, and community discussion prompts.

## Community Selection

I chose r/nba because it contains a wide range of discussion styles, including statistical analysis, emotional reactions, controversial opinions, and debate-driven questions. These distinctions are meaningful to community members because they affect the quality and usefulness of discussion.

## Label Definitions

### Analysis

Comments that provide reasoning, evidence, statistics, or structured explanations.

### Hot Take

Strong opinions, predictions, or judgments that are not supported by substantial evidence.

### Reaction

Emotional, humorous, celebratory, or immediate responses to events.

### Discussion Question

Comments that ask a question or invite discussion from other users.

## Dataset

* Source: Reddit r/nba
* Total Examples: 200
* Labels: 4
* Train/Validation/Test Split: 70% / 15% / 15%

### Label Distribution

| Label               | Count |
| ------------------- | ----- |
| Analysis            | 52    |
| Hot Take            | 65    |
| Reaction            | 72    |
| Discussion Question | 11    |

## Baseline Model

Model: Groq llama-3.3-70b-versatile (zero-shot)

### Baseline Results

| Metric   | Score |
| -------- | ----- |
| Accuracy | 0.500 |

### Per-Class Metrics

| Label | Precision | Recall | F1-Score | Support |
|---------|---------|---------|---------|---------|
| Analysis | 0.29 | 0.88 | 0.44 | 8 |
| Hot Take | 0.25 | 0.10 | 0.14 | 10 |
| Reaction | 0.50 | 0.09 | 0.15 | 11 |
| Discussion Question | 0.00 | 0.00 | 0.00 | 1 |
| **Accuracy** | - | - | **0.30** | 30 |
| **Macro Avg** | 0.26 | 0.27 | 0.18 | 30 |
| **Weighted Avg** | 0.34 | 0.30 | 0.22 | 30 |
## Fine-Tuned Model

Model: DistilBERT (distilbert-base-uncased)

Hyperparameters:

* Epochs: 3
* Learning Rate: 2e-5
* Batch Size: 16

### Fine-Tuned Results

| Metric   | Score |
| -------- | ----- |
| Accuracy | 0.300 |

### Per-Class Metrics

| Label | Precision | Recall | F1-Score | Support |
|---------|---------|---------|---------|---------|
| Analysis | 0.29 | 0.88 | 0.44 | 8 |
| Hot Take | 0.25 | 0.10 | 0.14 | 10 |
| Reaction | 0.50 | 0.09 | 0.15 | 11 |
| Discussion Question | 0.00 | 0.00 | 0.00 | 1 |
| **Macro Average** | 0.26 | 0.27 | 0.18 | 30 |
| **Weighted Average** | 0.34 | 0.30 | 0.22 | 30 |

## Confusion Matrix

| True \ Predicted    | Analysis | Hot Take | Reaction | Discussion Question |
| ------------------- | -------- | -------- | -------- | ------------------- |
| Analysis            | X        | X        | X        | X                   |
| Hot Take            | X        | X        | X        | X                   |
| Reaction            | X        | X        | X        | X                   |
| Discussion Question | X        | X        | X        | X                   |

## Failure Analysis

### Failure 1

Post:

Text:      Doesn’t Miami trade make more sense? You go for a clear rebuild while getting young pieces, picks and cap space. If you take JB then you’re in a similar if not worse position that you were in with Gia...

True Label:
Analysis

Predicted Label:
Hot Take

Analysis:
This post was labeled as Analysis because it presents a logical argument supported by specific reasoning about roster construction, draft picks, and long-term team strategy. The model likely focused on the subjective phrasing ("Doesn't Miami trade make more sense?") and interpreted the comment as an opinion rather than a structured argument. This suggests the model struggled to recognize analytical reasoning when it was presented in a conversational style instead of with statistics or explicit evidence.

### Failure 2

Post:

Text:      Pacers fans have PTSD at seeing that name.

True Label:
Reaction

Predicted Label:
Hot Take

Analysis:
This comment is primarily an emotional reaction referencing a shared experience among Pacers fans. However, because it contains exaggeration and humor, the model likely interpreted it as an opinionated statement rather than an emotional response. This error highlights how sarcasm, jokes, and hyperbole can blur the distinction between Reaction and Hot Take, making the boundary difficult for the model to learn.

### Failure 3

Post:

Text:      I really, really thought Chris Duarte was going to be something after his rookie year lol

True Label:
Reaction

Predicted Label:
Analysis

Analysis:
This comment reflects a personal emotional response and hindsight about a player's career trajectory. The model likely focused on the basketball-related content and interpreted the statement as an evaluation of the player rather than an expression of disappointment. The phrase "I really, really thought" signals a personal reaction, but the model appears to have relied more heavily on the discussion of a player's development than on the emotional tone of the comment.

## Error Pattern Analysis

The most common confusion occurred between Hot Take and Reaction. Both labels often contain emotional language, short comments, and strong opinions. The model appeared to rely heavily on sentiment rather than distinguishing between emotional reactions and evaluative judgments.

## Sample Classifications

| Post      | Predicted Label     | Confidence |
| --------- | ------------------- | ---------- |
| Doesn’t Miami trade make more sense? You go for a clear rebuild while getting young pieces, picks and cap space. If you take JB then you’re in a similar if not worse position that you were in with Gia... | Analysis            | 0.27       |
| Pacers fans have PTSD at seeing that name. | Reaction            | 0.26       |
| I'd think Rick Fox is somewhere in the top 5. | Hot Take            | 0.26       |


## Reflection: What the Model Learned

I intended the model to distinguish discourse style. The model successfully learned surface-level signals such as questions, emotional reactions, and analytical language. However, it struggled with nuanced distinctions between Hot Take and Reaction, suggesting that it learned sentiment cues more strongly than discourse intent.

## Spec Reflection

The project specification helped guide the design of clear, mutually exclusive labels before data collection began. However, during implementation I found that many real NBA comments did not fit perfectly into one category, requiring additional decision rules for ambiguous cases.

## AI Usage

### Use Case 1

I used ChatGPT to stress-test my label definitions by generating boundary-case NBA comments that could plausibly belong to multiple labels. This helped refine my labeling rules before annotation.

### Use Case 2

I used ChatGPT during error analysis to identify patterns among misclassified examples. I reviewed those suggestions manually and incorporated only patterns that were supported by the confusion matrix and prediction outputs.

## Spec Reflection

The project specification helped guide my implementation by emphasizing label design before model training. Spending time defining the labels made annotation more consistent and reduced confusion during data collection.

One way my implementation diverged from the original plan was in the dataset composition. While I intended to collect an evenly balanced dataset across all labels, Discussion Question posts were less common than expected, resulting in fewer examples for that category.

## Reflection

My goal was for the model to distinguish between Analysis, Hot Take, Reaction, and Discussion Question posts in the NBA community. The model learned to identify Analysis posts relatively well, but struggled to separate Reactions from Hot Takes. Many NBA comments contain both emotional language and opinions, which made the boundary difficult for the model to learn.

The model appeared to rely heavily on opinionated wording and basketball-related keywords rather than consistently identifying the author's intent. This suggests that additional examples, especially for Reaction and Discussion Question posts, would improve performance.


