One-stage detectors skip the "proposal" step entirely for maximum speed.

### YOLO (You Only Look Once)

- **Algorithm**:
    
    1. Divide the image into an $S \times S$ grid.
        
    2. Each grid cell is responsible for predicting $B$ bounding boxes and class probabilities.
        
    3. If the center of an object falls into a cell, that cell is "responsible" for detecting it.
        
- **Architecture**: A single CNN that outputs a huge tensor of shape $(S, S, B \times 5 + C)$.
    

### Key Concepts

- **Anchor Boxes**: Pre-defined shapes (e.g., "tall" for people, "wide" for cars) that help the model predict shapes more accurately.
    
- **Non-Maximum Suppression (NMS)**: The model often predicts many overlapping boxes for the same object. NMS keeps only the box with the highest confidence and removes the others that overlap significantly (based on IoU).