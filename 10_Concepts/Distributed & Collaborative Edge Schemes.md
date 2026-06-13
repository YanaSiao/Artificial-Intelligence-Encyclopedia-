### A. Distributed CNNs for IoT

- Instead of executing a heavy model on a single node, layers of a deep neural network are **partitioned and mapped** across an ecosystem of heterogeneous IoT units.
    
- Optimization targets prioritize minimizing total **end-to-end latency** spanning from the initial sensor capture timestamp down to final classification at the central sink node.
    

### B. Federated Learning (FL)

- **Core Principle**: A privacy-focused distributed training protocol where raw data sets never leave local host memory boundaries. Devices train local model parameters independently and only transmit **model weight updates** up to a central coordinating coordinator node.
    
- **Architectural Alignments**:
    
    1. _Horizontal FL_: Local host datasets share the identical feature space structure but contain completely unique individual user sample sets (e.g., Gboard next-word typing prediction profiles).
        
    2. _Vertical FL_: User sample spaces overlap heavily between participants, but individual clients hold entirely unique feature spaces.
        
    3. _Federated Transfer Learning_: Participating client nodes share minimal overlap in both sample groups and feature space tracking layout.
        
- **Federated Averaging (FedAVG) Protocol Loop**:
    
    1. Central server initializes global baseline parameters and broadcasts them to active edge client nodes.
        
    2. Local clients compute localized gradient steps or weight updates on their private data assets.
        
    3. Clients transmit their updated parameter vectors back up to the central aggregation server.
        
    4. The server aggregates these updates using a weighted or _simple mathematical average_:
        
        $$\min_{w} F(w), \quad \text{where } F(w) \coloneqq \sum_{k=1}^{K} p_k F_k(w)$$
        
    5. The new global parameter baseline is calculated, pushed back out to edge devices, and the cycle repeats.