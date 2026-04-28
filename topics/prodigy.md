# Prodigy

**Keywords**: prodigy,annotation,active

---

**Prodigy** is a **scriptable annotation tool from Explosion AI (the creators of spaCy) that combines active learning, rapid micro-task annotation, and programmatic customization** — enabling NLP engineers to collect high-quality training data efficiently by having machine learning models select the most valuable examples for human review, maximizing annotation ROI while producing custom datasets for NER, text classification, dependency parsing, and computer vision tasks.

**What Is Prodigy?**

- **Definition**: A commercial annotation tool (one-time perpetual license, ~$490) built by Explosion AI — designed for developer-practitioners rather than annotation managers, with a scriptable Python interface, built-in active learning loop, and a rapid binary annotation UI optimized for speed and focus.
- **Active Learning Core**: Prodigy's defining feature — instead of presenting examples in random order, the underlying model scores unlabeled examples by uncertainty, presenting the most informative ones first. Each labeled example immediately updates the model, making subsequent selections smarter.
- **Micro-Task Design**: Rather than showing annotators complex full documents to label end-to-end, Prodigy decomposes annotation into the smallest possible decisions — "Is this span an organization? YES/NO" — enabling annotation rates of 1,000+ examples per hour.
- **Recipe System**: Annotation workflows are defined as Python "recipes" — customizable scripts that control data loading, model selection, UI presentation, and data storage. Dozens of built-in recipes cover common NLP tasks; custom recipes can implement any annotation workflow.
- **spaCy Integration**: Seamless pipeline with spaCy — annotate with Prodigy, train with spaCy, evaluate with spaCy — the same data format and model architecture throughout the workflow.

**Why Prodigy Matters**

- **Active Learning Efficiency**: Random sampling annotation wastes time on easy examples. Prodigy's uncertainty sampling routes annotator time to the examples the model is most confused about — empirically requiring 3-5x fewer labeled examples to reach the same accuracy as random annotation.
- **Developer Control**: Unlike SaaS annotation platforms designed for annotation managers, Prodigy is designed for engineers — Python scripts control everything, data is stored locally in JSONL files, and the entire workflow is reproducible and versionable.
- **Rapid Iteration**: Bootstrap a new NER model in an afternoon — start with zero labels, annotate 200 examples, train a model, and use that model to pre-annotate the next batch (corrective annotation rather than from-scratch labeling).
- **Local Data Ownership**: All annotated data stays on your machine — critical for proprietary, sensitive, or regulated data (medical records, financial documents, legal contracts) that cannot be sent to third-party labeling platforms.
- **Multi-Task Support**: Single tool covers NER, text classification, relation extraction, dependency parsing, image segmentation, image classification, audio transcription, and coreference resolution.

**Core Prodigy Recipes**

**Named Entity Recognition (from scratch)**:
```bash
python -m prodigy ner.manual my_dataset blank:en data.jsonl --label PERSON,ORG,GPE
# Annotate spans — click to highlight, select label, press Enter to accept
```

**NER with Active Learning (model in the loop)**:
```bash
python -m prodigy ner.correct my_dataset en_core_web_md data.jsonl --label ORG,PRODUCT
# Model pre-annotates, human corrects errors — much faster than from scratch
```

**Text Classification (binary)**:
```bash
python -m prodigy textcat.manual my_dataset data.jsonl --label POSITIVE,NEGATIVE
# Press A (Accept/Positive), X (Reject/Negative), Space (skip) — 1000+ per hour
```

**Prodigy Annotation UI Philosophy**

- **Single decision per screen**: Each annotation is one decision — no multi-step workflows, no form filling, no context-switching.
- **Keyboard shortcuts only**: Accept (A), Reject (X), Ignore (Space), Undo (U) — no mouse required, maximizing throughput.
- **Progress indicators**: Running accuracy against a held-out validation set updates after each batch — annotators see their work improving the model in real time.
- **Immediate feedback**: Accepted examples are written to the database immediately — no batch submit, no risk of losing work.

**Custom Recipe Example**

```python
import prodigy
from prodigy.components.loaders import JSONL

@prodigy.recipe("custom-classify")
def custom_recipe(dataset, source):
    def get_stream():
        for eg in JSONL(source):
            eg["options"] = [
                {"id": "urgent", "text": "Urgent"},
                {"id": "normal", "text": "Normal"},
                {"id": "low", "text": "Low Priority"}
            ]
            yield eg

    return {
        "dataset": dataset,
        "stream": get_stream(),
        "view_id": "choice",
    }
```

Run: `python -m prodigy custom-classify my_tickets data.jsonl`

**Prodigy vs Alternatives**

| Feature | Prodigy | Label Studio | Scale AI | Labelbox |
|---------|---------|-------------|---------|---------|
| Active learning | Built-in | Plugin | No | Limited |
| Developer-oriented | Excellent | Good | Limited | Limited |
| Pricing | One-time ~$490 | Free (open source) | Usage-based | Subscription |
| Data ownership | Full (local) | Full (self-hosted) | Shared | Cloud |
| spaCy integration | Native | Good | No | Limited |
| Custom workflows | Python recipes | Templates | No | Limited |
| Annotation speed | Very high | High | High | High |

**When to Choose Prodigy**

- Building NLP models with spaCy and need efficient, local annotation.
- Working with sensitive data that cannot leave your infrastructure.
- Small-to-medium datasets (10,000 - 500,000 examples) where active learning provides significant advantage.
- Developer-led annotation where engineering time is the bottleneck.
- Need fully custom annotation workflows beyond pre-built templates.

Prodigy is **the annotation tool of choice for NLP engineers who prioritize efficiency, data ownership, and programmatic control over labeling workflows** — by combining active learning's sample efficiency with a micro-task UI optimized for speed and a fully scriptable recipe system, Prodigy enables practitioners to collect the exact training data their models need in a fraction of the time required by traditional annotation approaches.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/prodigy) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
