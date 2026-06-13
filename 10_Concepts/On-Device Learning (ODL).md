## Foundations

- **Definition**: The shift from static on-device inference to updating model parameters ($\theta$) directly on the embedded device during its _operational life_.
    
- **The "Why" (Drivers for ODL)**: Traditional deployment assumes environmental _stationarity_ (i.i.g. data generation). In real-world edge deployment, models face **non-stationary environments** caused by:
    
    - _Environmental changes_ (e.g., changing weather patterns).
        
    - _User behavior variations_ (evolving interests, vocal patterns).
        
    - _Hardware degradation_ (sensor aging effects, thermal drift, malfunctions).
        
- **Edge Benefits**: Drastically reduces **decision-making latency**, minimizes network **transmission bandwidth**, increases **energy efficiency**, and boosts **privacy/security** by keeping user data local.

## Taxonomy of TinyML Operational Perspectives

TinyML operational capabilities can be broken down from classical centralized pipelines to fully localized autonomous loops:

- **Level 0: Cloud Intelligence**: Both _training and inference_ occur entirely inside the cloud.
    
- **Level 1–2: Cloud Training, Edge Inference**: The network is trained using cloud assets, and a optimized frozen model is pushed to edge architectures or end units for _inference only_.
    
- **Level 3–4: Cloud-Edge Co-Training**: Training and inference tasks are split dynamically between cloud infrastructure and edge aggregation units.
    
- **Level 5: Edge Training & Inference**: The training loop and inference cycle are fully contained within local edge computing architectures.
    
- **Level 6: End Device Training & Inference**: Full standalone localized learning loop executed entirely on the **constrained end device** (e.g., microcontrollers).

## Execution Paradigms: Personalization vs. Continuous Learning

- **On-Device Personalization (Fine-Tuning)**:
    
    - _Mechanism_: Takes a generalized pre-trained global model $\theta'$ (optimized off-device) and executes an _on-device fine-tuning algorithm_ ($\mathcal{L}_f$) using a small, localized user dataset $P$.
        
    - _Focus_: Minimizes user specific validation error (e.g., speaker verification tuning).
        
    - _Memory Footprint Constraint_: Peak runtime RAM usage scales to $\max(\mathcal{M}_{\mathcal{L}f} + \mathcal{M}_P, \mathcal{M}_T + \mathcal{M}_f)$, where execution must strictly adhere to physical constraints (e.g., standard **32KB RAM bounds**).
        
- **Continuous / Incremental Learning**:
    
    - _Mechanism_: Continually updates model states ($\theta_t = \mathcal{L}(f_{\theta_{t-1}}, \mathcal{O}_t)$) in response to a constant incoming data stream $\mathcal{O}_t$ without discarding historical knowledge.
        
    - _Algorithmic Frameworks_: Tools like **TyBox** automate this translation, generating optimized C-code configurations capable of performing resource-constrained incremental adjustments directly on microcontrollers.
        

## Algorithmic Adaptation Under Concept Drift

When dealing with non-stationary data streams, edge devices utilize **Passive**, **Active**, or **Hybrid** update policies to re-align tracking models:

### A. Adaptation Paradigms

- **Passive Adaptation**: Continuously adjusts model parameters instantly as new supervised inputs stream in, completely bypassing drift tracking modules.
    
- **Active Adaptation**: Relies on specialized triggering nodes (**Change Detection Tests - CDTs**) to actively monitor data distributions. If a _concept drift_ state is declared, adaptation modules flush old information and trigger targeted retraining loops.
    
- **Hybrid Adaptation**: Merges constant micro-updates via passive flows while initializing active detection tests to rapidly dump obsolete knowledge bases when severe structural drift events trigger.
    

### B. Structural Mechanism Design

For lightweight edge models encountering non-stationarity, full backpropagation is often too memory-intensive. Instead, systems combine **approximate classifiers** with **knowledge management techniques**:

- _Instance Selection_: Maintains an active temporal memory window over incoming inputs. It can employ _fixed windows_ or _heuristics_ to alter window visibility lengths when structural deviations trigger.
    
- _Instance Weighting_: Retains historical profiles but systematically _scales down individual instance weights_ based on data age or recent performance relevance indices.
    
- _Adaptive k-NN Classifiers_: Utilized as classification heads because they skip explicit backpropagation stages. To bound memory consumption, a **Condensing Algorithm** is introduced to isolate and retain only the minimal subset of high-value training vectors required to map true classification regions safely.
    

## Architectural Trigger Techniques (CDTs)

Active learning models rely on statistical triggers to find the exact historical moment data distribution paths shift:

- **Limit Checking / Binary Thresholds**: Simple boundaries evaluating if an input variable breaches preset safe limits. Extremely light to run but lacks sensitivity to subtle statistical distribution shifts.
    
- **The CUSUM (Cumulative Sum) Test**: A parametric approach measuring statistical log-likelihood ratios to identify structural variations in probability density functions ($p.d.f.$). It keeps a running tally of deviations from a baseline state ($\theta_0 \to \theta_1$) and flags a drift event when the cumulative metric passes a set threshold value ($h$).
    
- **Hierarchical CDTs**: Employs a multi-tier confirmation filter. A _First-Level CDT_ acts as a fast monitor on raw error profiles. If an anomaly flashes, a _Second-Level CDT_ (using advanced _Hotelling $T^2$ statistics_ or _Change-Point Methods_) is initialized to confirm structural drift and accurately estimate the exact timestamp ($T^*$) the shift began. This allows the model to cleanly isolate and discard obsolete historical data.
