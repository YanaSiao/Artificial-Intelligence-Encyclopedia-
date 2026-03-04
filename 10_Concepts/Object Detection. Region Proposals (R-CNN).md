When an image has _multiple_ objects, a simple regression head isn't enough. We need to find "candidate" regions first.

### A. Selective Search & R-CNN

- **Selective Search**: An unsupervised algorithm that groups pixels into ~2,000 "proposals" (blobs that look like objects).
    
- **R-CNN**: Takes every single proposal, warps it to a fixed size, and runs it through a CNN.
    
    - _Problem_: Extremely slow (2,000 forward passes per image).
        

### B. Fast R-CNN

- **Innovation**: Run the CNN _once_ on the whole image to get a feature map. Use **RoI Pooling** (Region of Interest) to extract features for each proposal from that map.
    
    - _Result_: Much faster, but Selective Search is still the bottleneck.
        

### C. Faster R-CNN & RPN

- **Region Proposal Network (RPN)**: A small neural network that replaces Selective Search. It "learns" where objects might be.
    
- **Anchor Boxes**: Fixed-size reference boxes used by the RPN to predict offsets.