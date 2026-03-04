## Foundations & Paradigms 
- [[Introduction to Deep Learning vs Classical ML]] 
- [[The Perceptron]] 
- [[Neural Networks as Universal Approximation Tools]] 
## Training & Optimization 
- [[Backpropagation and Training Algorithms]] 
- [[Vanishing and Exploding Gradients]] 
- [[Best Practices. Overfitting and Regularization]] (Early Stopping, Weight Decay, Dropout) 
## Unsupervised Models 
- [[Foundations of Language Modeling]]
- [[Neural Net Language Model (NNLM)- Bengio et al. (2003)]]
- [[Word2Vec and Data Embedding]] 
- [[Autoencoders and Sparse Autoencoders]]
## Sequence & Structural Learning 
- [[Recurrent Neural Networks (RNN)]] (Basics, BPTT, and Gradient Issues)
- [[Gated Architectures (LSTM & GRU)]] (Solving Vanishing Gradients)
- [[Sequence-to-Sequence Models (Seq2Seq)]] (Encoder-Decoder & Decoding)
- [[Attention Mechanisms]] (Overcoming the Representation Bottleneck)
## Computer Vision (CV) 
- [[Digital Images & Foundations]] (RGB, Videos, Tensors)
- [[Spatial Filtering & Correlation]] (Linear filters, Kernels)
- [[Linear Classifiers & Template Matching]] (Weights as templates, Geometric view)
- [[Feature Extraction]] (SIFT/HOG vs. Data-Driven)
- [[CNN Anatomy. The FEN]] (Params, Stride, Padding, Dilation)
- [[CNN Anatomy. The Classifier Head]] (GAP, Flattening, Softmax)
- [[The Evolution of Famous CNN Architectures]] (AlexNet to EfficientNet)
- [[Data Scarcity, Augmentation & Regularization]] 
- [[CNN Explainability & Visualization]] (Saliency Maps, Grad-CAM, Occlusion)
## Advanced CV & Segmentation 
- [[Optimization & Training Stability]] (Pre-processing, Batch Norm)
- [[The Quest for Image Segmentation]] (Semantic vs. Instance, FCN, U-Net)
- **[[Object Localization & Regression]]** (Bounding Box $(x, y, w, h)$, Regression Loss, Multi-task Learning (Class + Box).)
- **[[Object Detection. Region Proposals (R-CNN)]]** (Selective Search, R-CNN, Fast R-CNN, Faster R-CNN (RPN).)
- **[[Object Detection. One-Stage Detectors (YOLO)]]** (YOLO, Anchor Boxes, Non-Maximum Suppression (NMS))
- **[[Detection Metrics: IoU & mAP]]** (Intersection over Union, Mean Average Precision.)
- [[Transfer Learning & Fine-Tuning]] (Freezing, Adaptation)
---

- [[CNN Layer Reference. Dimensions & Parameters]] 

---
## Unsupervised & Self-Supervised Learning

- [[Autoencoders and Sparse Autoencoders]]
- **[[Self-Supervised Learning Practices for Pre-training]]**
- **[[Metric-Based Learning & Triplet Loss]]**
- **[[Zero-Shot Learning & Knowledge Distillation]]**
- **[[DL for Anomaly Detection & Image Restoration]]**

## The Transformer Revolution

- [[Attention Mechanisms]] (The Bottleneck Problem)
- **[[The Transformer Architecture]]** (Self-Attention, Positional Encoding, Multi-Head)
- **[[Vision Transformers (ViT)]]** (Patch-based Embedding, Attention in CV)
- **[[Contrastive & Multimodal Learning]]** (CLIP, DINO, ALIGN)

## Generative AI

- **[[Variational Autoencoders (VAEs)]]** (Latent Space & Reparameterization)
- **[[Generative Adversarial Networks (GANs)]]** (Minimax Game, DCGAN, StyleGAN)
- **[[Normalizing Flows]]** (Change of Variables Formula)
- **[[Diffusion Models]]** (Forward/Reverse Process, DDPM, Stable Diffusion)
- **[[Text-to-Image Synthesis]]** (DALL-E, Guided Diffusion)

## Geometric & 3D Deep Learning

- **[[3D Data Representations]]** (Voxels, Point Clouds, Meshes)
- **[[Volumetric Learning (Conv3D)]]**
- **[[Point-Based Architectures]]** (PointNet, PointNet++)
- **[[Graph Neural Networks (GNN)]]** (Message Passing, GCN, Graph Attention)
- **[[Applications: Medical Imaging & Graph Processing]]**

## Model Investigation & Security

- [[CNN Explainability & Visualization]] (Grad-CAM, Occlusion)
- **[[Explainability Tools for DNNs]]** (LRP, Integrated Gradients, Saliency Maps)
- **[[Adversarial Attacks]]** (FGSM, PGD, Black-box vs White-box)
- **[[Defense Mechanisms & Robustness]]** (Adversarial Training, Defensive Distillation)