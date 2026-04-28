# Horizontal Federated Learning

**Keywords**: horizontal federated learning, federated learning

---

**Horizontal Federated Learning** is the standard federated learning setting where **distributed clients have the same features but different samples** — enabling organizations with compatible data schemas but separate user populations to collaboratively train models while keeping data decentralized, the most common federated learning scenario in practice.

**What Is Horizontal Federated Learning?**

- **Definition**: Federated learning where data is partitioned by samples (users/examples).
- **Feature Space**: All clients have same features/columns.
- **Sample Space**: Each client has different samples/rows.
- **Also Known As**: Sample-partitioned federated learning.

**Why Horizontal Federated Learning Matters**

- **Most Common Scenario**: Matches real-world federated deployments.
- **Natural Data Distribution**: Users naturally partitioned across devices/institutions.
- **Privacy Preservation**: Keep user data on local devices/servers.
- **Regulatory Compliance**: Meet data residency and privacy requirements.
- **Scalability**: Train on billions of devices without centralizing data.

**Characteristics**

**Data Distribution**:
- **Same Features**: All clients measure same attributes.
- **Different Samples**: Each client has different users/examples.
- **Example**: Multiple hospitals with same patient measurements but different patients.

**Model Architecture**:
- **Shared Architecture**: All clients use identical model structure.
- **Compatible Parameters**: Model parameters can be directly averaged.
- **Aggregation**: Simple parameter averaging works naturally.

**Contrast with Vertical FL**:
- **Horizontal**: Same features, different samples (user-partitioned).
- **Vertical**: Different features, overlapping samples (feature-partitioned).
- **Example**: Horizontal = multiple banks with same customer data schema; Vertical = bank + retailer with shared customers.

**Standard Algorithms**

**FedAvg (Federated Averaging)**:
- **Most Popular**: De facto standard for horizontal FL.
- **Process**: Clients train locally, server averages parameters.
- **Simple**: Easy to implement and understand.
- **Effective**: Works well in practice despite simplicity.

**FedProx**:
- **Extension**: Adds proximal term to handle heterogeneity.
- **Regularization**: Keeps local updates close to global model.
- **Benefit**: More robust to non-IID data and stragglers.

**FedOpt**:
- **Server Optimization**: Apply adaptive optimizers (Adam, Yogi) at server.
- **Client SGD**: Clients still use SGD locally.
- **Benefit**: Faster convergence, better handling of heterogeneity.

**Applications**

**Mobile Devices**:
- **Use Case**: Next-word prediction, voice recognition, app recommendations.
- **Example**: Google Gboard keyboard training across millions of phones.
- **Data**: Each phone has user's typing patterns, voice samples.
- **Benefit**: Personalized models without uploading sensitive data.

**Healthcare Institutions**:
- **Use Case**: Disease prediction, treatment recommendations, medical imaging.
- **Example**: Multiple hospitals collaborating on diagnosis models.
- **Data**: Each hospital has patient records with same measurements.
- **Benefit**: Larger training dataset without violating HIPAA.

**Financial Organizations**:
- **Use Case**: Fraud detection, credit scoring, risk assessment.
- **Example**: Banks collaborating on fraud detection.
- **Data**: Each bank has transaction records with same features.
- **Benefit**: Better models without sharing customer data.

**IoT Devices**:
- **Use Case**: Predictive maintenance, anomaly detection.
- **Example**: Smart home devices learning usage patterns.
- **Data**: Each device has sensor readings with same schema.
- **Benefit**: Collective intelligence without cloud upload.

**Challenges**

**Non-IID Data**:
- **Problem**: Client data distributions differ significantly.
- **Impact**: Slower convergence, reduced accuracy.
- **Solutions**: FedProx, data augmentation, personalization.

**Communication Efficiency**:
- **Problem**: Frequent communication with many clients is expensive.
- **Impact**: Bandwidth costs, latency, energy consumption.
- **Solutions**: Local SGD, gradient compression, client sampling.

**Stragglers**:
- **Problem**: Slow clients delay training rounds.
- **Impact**: Increased training time, resource waste.
- **Solutions**: Asynchronous updates, timeout mechanisms, client selection.

**Privacy & Security**:
- **Problem**: Model updates may leak information about training data.
- **Impact**: Privacy violations, inference attacks.
- **Solutions**: Secure aggregation, differential privacy, encrypted computation.

**System Heterogeneity**:
- **Problem**: Clients have different computational capabilities.
- **Impact**: Uneven participation, fairness issues.
- **Solutions**: Adaptive model sizes, tiered participation.

**Technical Components**

**Client Selection**:
- **Random Sampling**: Select subset of clients each round.
- **Stratified Sampling**: Ensure diverse client representation.
- **Importance Sampling**: Prioritize clients with more data or higher loss.

**Aggregation Methods**:
- **Simple Average**: θ_global = (1/K) Σ_k θ_k.
- **Weighted Average**: θ_global = Σ_k (n_k/n) θ_k (weight by data size).
- **Robust Aggregation**: Median, trimmed mean to handle outliers.

**Privacy Mechanisms**:
- **Secure Aggregation**: Cryptographic protocol hiding individual updates.
- **Differential Privacy**: Add calibrated noise to updates.
- **Homomorphic Encryption**: Compute on encrypted updates.

**Communication Optimization**:
- **Gradient Compression**: Quantization, sparsification, low-rank.
- **Local Steps**: Multiple local updates before communication (Local SGD).
- **Model Compression**: Distillation, pruning for smaller models.

**Evaluation Metrics**

**Model Performance**:
- **Global Test Accuracy**: Performance on held-out centralized test set.
- **Local Test Accuracy**: Average performance on client test sets.
- **Fairness**: Variance in performance across clients.

**Efficiency Metrics**:
- **Communication Rounds**: Number of server-client communication cycles.
- **Total Communication**: Bytes transferred (upload + download).
- **Training Time**: Wall-clock time to convergence.

**Privacy Metrics**:
- **Privacy Budget**: ε in differential privacy.
- **Membership Inference**: Success rate of privacy attacks.
- **Reconstruction Error**: Ability to recover training data.

**Tools & Frameworks**

- **TensorFlow Federated**: Google's production-grade FL framework.
- **PySyft**: OpenMined's privacy-preserving ML library.
- **Flower**: Flexible and scalable FL framework.
- **FedML**: Comprehensive research and production FL platform.
- **FATE**: Industrial federated learning framework.

**Best Practices**

- **Start Simple**: Begin with FedAvg, add complexity as needed.
- **Monitor Heterogeneity**: Track data distribution differences across clients.
- **Tune Hyperparameters**: Learning rate, local steps, client sampling rate.
- **Implement Privacy**: Use secure aggregation and differential privacy.
- **Handle Failures**: Design for client dropouts and network issues.
- **Evaluate Fairly**: Report both global and per-client metrics.

Horizontal Federated Learning is **the foundation of practical federated systems** — by enabling organizations with compatible data schemas to collaborate without centralizing data, it makes privacy-preserving machine learning at scale a reality, powering applications from mobile keyboards to healthcare to financial services.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/horizontal-federated-learning) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
