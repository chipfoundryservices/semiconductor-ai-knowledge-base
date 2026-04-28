# Snorkel

**Keywords**: snorkel,weak supervision,label

---

**Snorkel** is a **programmatic data labeling framework that enables teams to create large labeled training datasets without manual annotation — using weak supervision theory to combine noisy, imprecise labeling functions written in Python into high-quality probabilistic labels** — making it possible to label millions of examples in hours instead of months.

**What Is Snorkel?**

- **Definition**: An open-source Python framework (Stanford AI Lab, now Snorkel AI) that operationalizes weak supervision — the technique of using multiple noisy, imperfect labeling signals (heuristics, knowledge bases, external models, crowdsourced labels) and combining them using a learned generative model to produce unified probabilistic labels.
- **Labeling Functions (LFs)**: The core abstraction in Snorkel — Python functions that look at an example and either return a label or ABSTAIN (when the function doesn't apply). LFs encode domain knowledge as code: keyword patterns, regular expressions, distant supervision from databases, pretrained model outputs, or any arbitrary logic.
- **Label Model**: Snorkel's probabilistic model learns the accuracy and correlation structure of all labeling functions automatically — combining their noisy outputs into a single high-quality probabilistic label for each example without requiring any ground truth labels.
- **Slicing Functions**: Identify important data subsets (slices) for targeted evaluation and model improvement — define slices as Python functions applied to the dataset, then use Slice-Based Learning to ensure the model performs well on critical slices.
- **The Weak Supervision Paradigm**: Instead of "label your data manually," Snorkel's paradigm is "encode your domain knowledge as labeling functions" — much faster and more scalable for large datasets and frequently changing labeling schemas.

**Why Snorkel Matters**

- **Labeling Cost Reduction**: Manual labeling at $0.10/example costs $100,000 for one million examples — Snorkel labeling functions written by a domain expert in a week can label the same dataset for near-zero cost.
- **Schema Flexibility**: When labeling requirements change (new categories, refined definitions), update the labeling functions — no need to re-label thousands of examples manually.
- **Programmatic Consistency**: Human annotators are inconsistent — annotator agreement rates of 70-80% are common for complex tasks. Labeling functions are perfectly consistent, improving the signal-to-noise ratio of the generated labels.
- **Enterprise Adoption**: Snorkel has powered labeled dataset creation at Google, Intel, Apple, and Stanford Medicine — used for clinical NLP, content moderation, document classification, and scientific literature triage.
- **Foundation Model Era**: Snorkel's programmatic approach complements LLM-based labeling — use GPT-4 as a labeling function alongside rule-based functions, combining LLM semantic understanding with domain heuristics.

**Core Snorkel Workflow**

**Step 1: Define Labeling Functions**:
```python
from snorkel.labeling import labeling_function
POSITIVE, NEGATIVE, ABSTAIN = 1, 0, -1

@labeling_function()
def lf_keyword_positive(x):
    return POSITIVE if "excellent" in x.text.lower() else ABSTAIN

@labeling_function()
def lf_keyword_negative(x):
    return NEGATIVE if "terrible" in x.text.lower() else ABSTAIN

@labeling_function()
def lf_short_review(x):
    # Very short reviews tend to be negative
    return NEGATIVE if len(x.text.split()) < 3 else ABSTAIN

@labeling_function()
def lf_sentiment_model(x):
    # Use a pretrained model as a labeling function
    score = sentiment_analyzer(x.text)
    if score > 0.8: return POSITIVE
    if score < 0.2: return NEGATIVE
    return ABSTAIN
```

**Step 2: Apply and Analyze LFs**:
```python
from snorkel.labeling import PandasLFApplier, LFAnalysis

applier = PandasLFApplier(lfs=[lf_keyword_positive, lf_keyword_negative, lf_short_review, lf_sentiment_model])
L_train = applier.apply(df=train_df)

analysis = LFAnalysis(L=L_train, lfs=[...])
print(analysis.lf_summary())
# Coverage: what % of examples does each LF label?
# Conflicts: where do LFs disagree?
# Overlaps: where do LFs agree?
```

**Step 3: Train Label Model**:
```python
from snorkel.labeling.model import LabelModel

label_model = LabelModel(cardinality=2)
label_model.fit(L_train=L_train, n_epochs=500, lr=0.001)
probs_train = label_model.predict_proba(L=L_train)
# probs_train: N x 2 matrix of probabilistic labels
```

**Step 4: Train End Model**:
```python
from sklearn.linear_model import LogisticRegression

# Filter uncertain examples and train on high-confidence labels
filter_mask = (probs_train.max(axis=1) > 0.85)
X_filtered = X_train[filter_mask]
y_filtered = probs_train[filter_mask].argmax(axis=1)

model = LogisticRegression().fit(X_filtered, y_filtered)
```

**Snorkel Use Cases**

- **Clinical NLP**: Label electronic health records for disease classification using ICD codes, medication lists, and clinical heuristics as labeling functions — impossible to label manually at scale.
- **Content Moderation**: Label millions of social media posts using keyword lists, user report history, and a distilled moderation model as weak supervision sources.
- **Document Routing**: Classify incoming legal documents using contract clause patterns, entity recognition, and document metadata as labeling functions.
- **Scientific Literature**: Triage research papers for systematic reviews using title keywords, MeSH terms, and abstract patterns — replaces months of manual reviewer time.

**Snorkel vs Alternatives**

| Feature | Snorkel | Manual Labeling | Cleanlab | LLM Labeling |
|---------|---------|---------------|---------|-------------|
| Scalability | Excellent | Poor | N/A | Good |
| Cost at scale | Very low | Very high | Low | Medium |
| Label quality | High (with good LFs) | Gold standard | Cleaned labels | Variable |
| Domain encoding | Programmatic | Human intuition | N/A | Prompt |
| Open source | Yes | N/A | Yes | Varies |
| Schema flexibility | Excellent | Low | N/A | Excellent |

Snorkel is **the framework that makes large-scale programmatic data labeling practical by transforming domain expertise into code** — for teams facing the fundamental bottleneck of insufficient labeled training data, Snorkel provides the infrastructure to create production-quality labeled datasets at a fraction of the time and cost of manual annotation.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/snorkel) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
