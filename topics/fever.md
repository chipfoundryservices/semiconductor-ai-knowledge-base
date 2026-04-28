# FEVER (Fact Extraction and VERification)

**Keywords**: fever, fever, evaluation

---

**FEVER (Fact Extraction and VERification)** is the **large-scale fact verification benchmark requiring models to retrieve evidence from Wikipedia and classify claims as SUPPORTS, REFUTES, or NOT ENOUGH INFO** — serving as the primary standard benchmark for automated fact-checking, misinformation detection, and hallucination evaluation systems that must cite their sources and verify claims against a trusted knowledge base.

**Task Definition**

FEVER presents:
- **Claim**: A factual statement about the world.
- **Wikipedia**: The full English Wikipedia as the evidence corpus.
- **Task**: Retrieve relevant Wikipedia sentences, then classify the claim as:
  - **SUPPORTS**: The claim is verifiable and correct based on Wikipedia evidence.
  - **REFUTES**: The claim is verifiable and incorrect based on Wikipedia evidence.
  - **NOT ENOUGH INFO**: Wikipedia does not contain sufficient evidence to verify or refute the claim.

**Example 1 — SUPPORTS**:
Claim: "William Shakespeare was born in Stratford-upon-Avon."
Evidence: Wikipedia sentence: "William Shakespeare was an English playwright, born in Stratford-upon-Avon, Warwickshire, in April 1564."
Label: SUPPORTS.

**Example 2 — REFUTES**:
Claim: "The Eiffel Tower was built in the 20th century."
Evidence: "The Eiffel Tower is a wrought-iron lattice tower... constructed from 1887 to 1889."
Label: REFUTES (1887–1889 is the 19th century).

**Example 3 — NOT ENOUGH INFO**:
Claim: "Nikola Tesla preferred cats to dogs."
Evidence: No Wikipedia sentence establishes this preference.
Label: NOT ENOUGH INFO.

**Dataset Construction**

FEVER was constructed through a rigorous multi-stage process to ensure claim diversity and difficulty:

**Step 1 — Claim Generation**: Crowdworkers were shown sentences from Wikipedia and asked to write claims by:
- Mutating the original sentence (changing a fact to make it false).
- Paraphrasing (different wording, same meaning).
- Generalizing (broader claim from a specific fact).
- Specializing (specific claim from a general fact).

**Step 2 — NOT ENOUGH INFO Generation**: Some claims were specifically written to require inference beyond available Wikipedia evidence, preventing models from treating "hard to find" as "supported."

**Step 3 — Evidence Annotation**: For SUPPORTS and REFUTES claims, annotators identified the specific Wikipedia sentences (evidence set) that justify the label. Claims often require 1–5 sentences from potentially different Wikipedia articles.

**Dataset Scale**: 185,445 claims split across training (145k), development (19k), and test (19k) sets. Human performance: ~89% label accuracy.

**The Full Pipeline Challenge**

FEVER requires a complete reasoning pipeline — not just classification:

**Stage 1 — Document Retrieval**: Given the claim, identify relevant Wikipedia articles. The full Wikipedia corpus has ~5 million articles; efficient retrieval must narrow candidates without losing relevant documents.

**Stage 2 — Sentence Selection**: From retrieved articles, select the specific sentences that contain evidence relevant to the claim. Claims may require sentences from multiple different Wikipedia articles.

**Stage 3 — Natural Language Inference**: Classify the claim as SUPPORTS, REFUTES, or NOT ENOUGH INFO given the retrieved evidence sentences.

Each stage introduces errors that compound: a retrieval failure means no correct evidence can support the subsequent classification, regardless of the classifier's quality. FEVER's primary metric, FEVER Score, requires correct label prediction AND correct evidence identification simultaneously.

**Evaluation Metrics**

**Label Accuracy**: Fraction of claims correctly classified into SUPPORTS / REFUTES / NOT ENOUGH INFO, regardless of evidence quality.

**FEVER Score (Primary)**: A claim is "correctly verified" only if:
1. The label is correct AND
2. The predicted evidence set contains at least one full evidence set from the ground truth annotation.

FEVER Score penalizes models that achieve correct labels via incorrect reasoning paths (lucky guesses without finding the right evidence).

**Model Performance**

| System | FEVER Score |
|--------|------------|
| TF-IDF retrieval + BERT NLI | 71.3 |
| DrKIT + RoBERTa | 79.2 |
| DPR + T5 | 84.1 |
| Human | ~89 |

**FEVER for Hallucination Evaluation**

FEVER's most significant modern application is evaluating factual grounding and hallucination in language models:

**FactScore**: Decomposes LLM-generated text into atomic claims and verifies each against a knowledge source (Wikipedia or retrieval-augmented context) using a FEVER-style pipeline. Produces a "factual precision" score measuring what fraction of generated claims are supported by evidence.

**RAG Faithfulness Evaluation**: In RAG systems, FEVER-style classification determines whether model outputs are faithful to retrieved documents — detecting when models generate claims not supported by their context.

**Claim-Evidence Linking**: FEVER trains models to link claims to supporting evidence, a capability directly useful for explainable AI systems that must cite sources for their assertions.

**Misinformation Detection Applications**

FEVER-trained models are deployed in:
- **News fact-checking**: Classifying news article claims against Wikipedia evidence.
- **Social media moderation**: Flagging posts that make verifiable false claims.
- **Scientific claim verification**: Checking whether paper abstracts are supported by cited evidence.
- **Medical claim validation**: Verifying health claims against clinical evidence databases.

**NOT ENOUGH INFO and Epistemic Calibration**

The NOT ENOUGH INFO class is crucial for calibrated fact-checking: a system should abstain rather than confabulate a verdict when evidence is absent. FEVER trains models to recognize the limits of available evidence — preventing the false confidence that produces dangerous misinformation corrections when the evidence base is simply inadequate.

FEVER is **the automated fact-checker's training ground** — the benchmark that established the full pipeline from claim to evidence retrieval to entailment classification, training AI systems to cite their sources, recognize the limits of available evidence, and verify the truth of written claims against a trusted corpus rather than relying on parametric memory alone.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/fever) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
