---
type: roadmap
status: active
domain: ai
target:
  - computer-vision-engineer
  - robotics
  - lifestyle-app
  - 3d-generation
level: beginner-restart
created: 2026-07-28
updated: 2026-07-28
---

# AI Roadmap

## Profil dan Target

- Level awal: pemula, mengulang dari fondasi.
- Target utama: Computer Vision Engineer.
- Target proyek: robotics, lifestyle application, dan 3D generation.
- Target jangka panjang: siap membangun sistem end-to-end dan melakukan riset Computer Vision.

> [!info] Status saat ini
> Sedang mempelajari **Bagian 0 — Fundamental**, dimulai dari [[Mathematics MOC]] → [[Linear Algebra MOC]] → [[Scalar]].

## Cara Menggunakan Roadmap

- `[ ]` belum dimulai
- `[~]` sedang dipelajari
- `[x]` selesai dan sudah dipraktikkan

Satu bab dianggap selesai jika:

- Bisa menjelaskan intuisi dan konsep dasar.
- Bisa mengerjakan contoh manual jika ada matematika.
- Bisa mengimplementasikan contoh sederhana.
- Sudah mengerjakan latihan atau mini project yang relevan.

## Alur Besar

```text
Fundamental
    ↓
Machine Learning
    ↓
Deep Learning
    ↓
Computer Vision
    ↓
Modern CV + ViT + VLM
    ↓
Video + 3D + Robotics + RL
    ↓
Deployment dan MLOps
    ↓
Research dan Proyek Akhir
```

---

# Bagian 0 — Fundamental (Wajib)

## Bab 0.1 — Matematika untuk AI

Status: **materi lengkap tersedia, sedang dipelajari**

**Index materi:** [[Mathematics MOC]]

### Linear Algebra

- [ ] [[Scalar]]
- [ ] [[Vector]]
- [ ] [[Vector Geometry]]
- [ ] [[Matrix]]
- [ ] [[Tensor]]
- [ ] [[Matrix Multiplication]]
- [ ] [[Transpose]]
- [ ] [[Determinant]]
- [ ] [[Inverse Matrix]]
- [ ] [[Matrix Rank]]
- [ ] [[Eigenvalue and Eigenvector]]

### Probability and Statistics

- [ ] [[Probability]]
- [ ] [[Random Variables and Probability Distributions]]
- [ ] [[Expectation Variance and Covariance]]
- [ ] [[Conditional Probability and Independence]]
- [ ] [[Statistics]]
- [ ] [[Bayes Theorem]]

### Calculus

- [ ] [[Derivative]]
- [ ] [[Partial Derivative]]
- [ ] [[Chain Rule]]
- [ ] [[Gradient]]
- [ ] [[Jacobian and Hessian]]

### Optimization

- [ ] [[Gradient Descent]]
- [ ] [[Convex Optimization]]

**Milestone:** bisa menjelaskan bagaimana data direpresentasikan dan bagaimana gradient dipakai untuk menurunkan loss.

**Mini project:** [[PRJ - Mathematical Foundations Visual Lab]]

## Bab 0.2 — Python untuk AI

Status: **materi lengkap tersedia, belum dimulai**

- [ ] [[Python Fundamentals]]
- [ ] [[Python Variables]]
- [ ] [[Python Data Types]]
- [ ] [[Python Control Flow]]
- [ ] [[Python Functions]]
- [ ] [[Object-Oriented Programming in Python]]
- [ ] [[Python Exception Handling]]
- [ ] [[Python File Handling]]
- [ ] [[Python Virtual Environment]]
- [ ] [[Python Package Management]]
- [ ] [[Python Type Hints]]
- [ ] [[Python Logging]]

**Milestone:** mampu membuat program Python modular dan membaca error dengan mandiri.

**Mini project:** [[PRJ - Command-Line Image Organizer]]

**Index materi:** [[Python MOC]]

## Bab 0.3 — Scientific Computing

Status: **materi lengkap tersedia, belum dimulai**

- [ ] [[Scientific Computing Fundamentals]]
- [ ] [[NumPy Fundamentals]]
- [ ] [[NumPy Broadcasting and Vectorization]]
- [ ] [[Pandas Fundamentals]]
- [ ] [[Matplotlib Fundamentals]]
- [ ] [[Pillow Fundamentals]]
- [ ] [[OpenCV Fundamentals]]
- [ ] [[Scikit-Learn Fundamentals]]

**Milestone:** mampu memuat, memanipulasi, menganalisis, dan memvisualisasikan data numerik serta gambar.

**Mini project:** [[PRJ - Image Dataset Explorer]]

**Index materi:** [[Scientific Computing MOC]]

## Bab 0.4 — Dasar Machine Learning

Status: **materi lengkap tersedia, belum dimulai**

- [ ] [[Introduction to Machine Learning]]
- [ ] [[Supervised Learning]]
- [ ] [[Unsupervised Learning]]
- [ ] [[Semi-Supervised Learning]]
- [ ] [[Self-Supervised Learning]]
- [ ] [[Dataset and Data Quality]]
- [ ] [[Data Splitting and Leakage]]
- [ ] [[Feature Engineering and Preprocessing]]
- [ ] [[Overfitting and Underfitting]]
- [ ] [[Cross-Validation and Model Selection]]
- [ ] [[Machine Learning Evaluation Metrics]]
- [ ] [[End-to-End Machine Learning Baseline]]

**Milestone:** mampu membangun dan mengevaluasi baseline ML tanpa data leakage.

### Algoritma Classical ML

- [ ] [[Linear Regression]]
- [ ] [[Logistic Regression]]
- [ ] [[k-Nearest Neighbors]]
- [ ] [[Decision Tree]]
- [ ] [[Random Forest and Gradient Boosting]]
- [ ] [[Support Vector Machine]]
- [ ] [[K-Means Clustering]]
- [ ] [[Principal Component Analysis]]

**Mini project:** [[PRJ - End-to-End ML Baseline]]

**Index materi:** [[Machine Learning Foundations MOC]]

## Pendukung — Software Engineering untuk AI

Status: **materi lengkap tersedia, belum dimulai**

- [ ] [[Git and GitHub Fundamentals]]
- [ ] [[Linux Command Line]]
- [ ] [[Bash Fundamentals]]
- [ ] [[AI Project Structure]]
- [ ] [[Clean Code for AI]]
- [ ] [[Testing AI Code with Pytest]]
- [ ] [[Configuration Management for AI]]
- [ ] [[Python Packaging]]

**Index materi:** [[Software Engineering for AI MOC]]

---

# Bagian 1 — Computer Vision Fundamental

- [ ] Bab 1 — Pengenalan Computer Vision
- [ ] Bab 2 — Representasi Digital Image
  - Pixel
  - Resolution
  - Channel
  - Bit depth
- [ ] Bab 3 — Color Space
  - RGB
  - BGR
  - HSV
  - LAB
  - YCrCb
  - Grayscale
- [ ] Bab 4 — Image I/O
  - Membaca gambar
  - Menampilkan gambar
  - Menyimpan gambar
- [ ] Bab 5 — Image Geometry
  - Resize
  - Crop
  - Rotate
  - Flip
  - Translate
  - Perspective transform
- [ ] Bab 6 — Drawing and Annotation
  - Line
  - Circle
  - Rectangle
  - Polygon
  - Text
- [ ] Bab 7 — Image Processing
  - Histogram
  - Histogram equalization
  - Thresholding
  - Adaptive threshold
  - Contrast enhancement
- [ ] Bab 8 — Filtering
  - Gaussian blur
  - Median blur
  - Bilateral filter
  - Sharpening
  - Edge detection
- [ ] Bab 9 — Morphological Operations
  - Erosion
  - Dilation
  - Opening
  - Closing
  - Morphological gradient
  - Top hat
  - Black hat
- [ ] Bab 10 — Image Segmentation Klasik
  - Contour
  - Connected components
  - Watershed
- [ ] Bab 11 — Feature Extraction
  - Harris Corner
  - FAST
  - ORB
  - SIFT
  - SURF
  - HOG
- [ ] Bab 12 — Classical Object Detection
  - Haar Cascade
  - Viola–Jones
  - Template matching
- [ ] Bab 13 — Video Processing
  - Webcam
  - Video stream
  - Frame processing
  - Background subtraction

**Milestone:** mampu menjelaskan dan membangun classical CV pipeline tanpa Deep Learning.

**Mini projects:** document scanner, coin counter, lane detection, dan panorama stitching.

---

# Bagian 2 — Deep Learning Fundamental

- [ ] Bab 14 — Artificial Neural Network
- [ ] Bab 15 — Perceptron
- [ ] Bab 16 — Forward Propagation
- [ ] Bab 17 — Backpropagation
- [ ] Bab 18 — Activation Function
  - ReLU
  - Leaky ReLU
  - Sigmoid
  - Tanh
  - Softmax
- [ ] Bab 19 — Loss Function
  - MSE
  - Cross-entropy
  - Focal loss
- [ ] Bab 20 — Optimizer
  - SGD
  - Momentum
  - RMSProp
  - Adam
  - AdamW
- [ ] Bab 21 — Regularization
  - Dropout
  - Batch normalization
  - Weight decay
  - Early stopping
- [ ] Bab 22 — Training Pipeline
  - Epoch
  - Batch size
  - Learning rate
  - Scheduler
  - Checkpoint
  - TensorBoard

## PyTorch Fundamentals

- [ ] Tensor dan device
- [ ] Autograd
- [ ] `Dataset` dan `DataLoader`
- [ ] `nn.Module`
- [ ] Training loop
- [ ] Custom dataset
- [ ] Custom loss
- [ ] Custom layer
- [ ] Mixed precision
- [ ] Distributed training dasar

**Milestone:** mampu menulis training loop PyTorch, melakukan debugging gradient, dan membaca learning curve.

**Mini project:** neural network classifier dari nol dan menggunakan PyTorch.

---

# Bagian 3 — Convolutional Neural Network

- [ ] Bab 23 — Dasar CNN
- [ ] Bab 24 — Convolution Layer
- [ ] Bab 25 — Pooling Layer
- [ ] Bab 26 — Padding
- [ ] Bab 27 — Stride
- [ ] Bab 28 — Transfer Learning
- [ ] Bab 29 — Fine-Tuning
- [ ] Bab 30 — Data Augmentation
- [ ] Bab 31 — Image Classification

**Milestone:** mampu menghitung shape feature map, melatih classifier, dan melakukan error analysis.

**Mini projects:** MNIST, CIFAR-10, food classification, dan plant disease detection.

---

# Bagian 4 — Modern Computer Vision

- [ ] Bab 32 — Object Detection
  - R-CNN
  - Fast R-CNN
  - Faster R-CNN
  - SSD
  - RetinaNet
  - YOLO
- [ ] Bab 33 — YOLO
  - YOLOv5
  - YOLOv8
  - YOLO11
  - Training
  - Fine-tuning
  - Custom dataset
- [ ] Bab 34 — Semantic Segmentation
  - FCN
  - U-Net
  - DeepLab
- [ ] Bab 35 — Instance Segmentation
  - Mask R-CNN
  - YOLO Segmentation
- [ ] Bab 36 — Pose Estimation
- [ ] Bab 37 — Object Tracking
  - SORT
  - DeepSORT
  - ByteTrack
- [ ] Bab 38 — OCR
- [ ] Bab 39 — Face Detection
- [ ] Bab 40 — Face Recognition
- [ ] Bab 41 — Image Retrieval

## Evaluation Metrics CV

- [ ] Precision dan Recall
- [ ] IoU
- [ ] AP dan mAP
- [ ] AP50 dan AP75
- [ ] Dice
- [ ] mIoU
- [ ] Tracking metrics

**Milestone:** mampu menyiapkan custom dataset, memilih metric, melatih model, dan menganalisis failure case.

**Mini projects:** helmet detection, fitness pose coach, OCR, dan smart CCTV.

---

# Bagian 5 — Vision Transformer

- [ ] Bab 42 — Attention Mechanism
- [ ] Bab 43 — Transformer
- [ ] Bab 44 — Vision Transformer
- [ ] Bab 45 — Swin Transformer
- [ ] Bab 46 — DETR
- [ ] Bab 47 — Segment Anything Model

**Milestone:** mampu menjelaskan patch embedding, self-attention, positional encoding, dan perbedaan CNN vs ViT.

**Mini project:** fine-tuning ViT untuk image classification dan membandingkannya dengan CNN.

---

# Bagian 6 — Generative AI

- [ ] Bab 48 — Pengenalan Generative AI
- [ ] Bab 49 — Generative Adversarial Network
- [ ] Bab 50 — DCGAN
- [ ] Bab 51 — Conditional GAN
- [ ] Bab 52 — CycleGAN
- [ ] Bab 53 — Pix2Pix
- [ ] Bab 54 — Variational Autoencoder
- [ ] Bab 55 — Latent Space
- [ ] Bab 56 — Diffusion Model
- [ ] Bab 57 — Stable Diffusion
- [ ] Bab 58 — ControlNet
- [ ] Bab 59 — LoRA
- [ ] Bab 60 — Image Inpainting
- [ ] Bab 61 — Image Upscaling
- [ ] Bab 62 — Image-to-Image
- [ ] Bab 63 — Text-to-Image

**Evaluation:** FID, LPIPS, CLIP score, dan human evaluation.

**Mini projects:** conditional image generation, image inpainting, dan domain translation.

---

# Bagian 7 — Vision Language Model

- [ ] Bab 64 — Embedding
- [ ] Bab 65 — CLIP
- [ ] Bab 66 — BLIP
- [ ] Bab 67 — Florence
- [ ] Bab 68 — Qwen-VL
- [ ] Bab 69 — LLaVA
- [ ] Bab 70 — Grounding DINO
- [ ] Bab 71 — SAM 2
- [ ] Bab 72 — Image Captioning
- [ ] Bab 73 — Visual Question Answering

**Milestone:** memahami alignment vision–language dan mampu membangun multimodal pipeline.

**Mini projects:** visual search engine dan AI vision assistant.

---

# Bagian 8 — Video AI

- [ ] Bab 74 — Video Classification
- [ ] Bab 75 — Action Recognition
- [ ] Bab 76 — Video Object Detection
- [ ] Bab 77 — Video Segmentation
- [ ] Bab 78 — Video Tracking
- [ ] Bab 79 — Video Generation

**Milestone:** mampu menangani dimensi temporal, sampling frame, tracking, dan evaluasi video.

**Mini project:** action recognition atau traffic analysis.

---

# Bagian 9 — 3D Computer Vision

- [ ] Bab 80 — Stereo Vision
- [ ] Bab 81 — Depth Estimation
- [ ] Bab 82 — Point Cloud
- [ ] Bab 83 — LiDAR
- [ ] Bab 84 — Structure from Motion
- [ ] Bab 85 — Neural Radiance Field
- [ ] Bab 86 — Gaussian Splatting

**Dataset penting:** KITTI, nuScenes, Waymo, ScanNet, dan ShapeNet.

**Milestone:** memahami camera geometry, pose, depth, point cloud, dan novel-view rendering.

**Mini projects:** multi-view reconstruction, NeRF, dan Gaussian Splatting.

---

# Bagian 10 — Reinforcement Learning

- [ ] Bab 87 — Dasar Reinforcement Learning
- [ ] Bab 88 — Markov Decision Process
- [ ] Bab 89 — Policy
- [ ] Bab 90 — Reward
- [ ] Bab 91 — Value Function
- [ ] Bab 92 — Q-Learning
- [ ] Bab 93 — SARSA
- [ ] Bab 94 — Deep Q-Network
- [ ] Bab 95 — Policy Gradient
- [ ] Bab 96 — Actor–Critic
- [ ] Bab 97 — PPO
- [ ] Bab 98 — SAC
- [ ] Bab 99 — TD3
- [ ] Bab 100 — Vision-Based Reinforcement Learning
- [ ] Bab 101 — Sim2Real

**Milestone:** mampu memformulasikan MDP, melatih agent, dan menganalisis reward serta sample efficiency.

**Mini projects:** visual navigation dan robotic control di simulator.

---

# Bagian 11 — Deployment dan MLOps

- [ ] Bab 102 — PyTorch Production
- [ ] Bab 103 — TensorFlow
- [ ] Bab 104 — ONNX
- [ ] Bab 105 — TensorRT
- [ ] Bab 106 — OpenVINO
- [ ] Bab 107 — TensorFlow Lite
- [ ] Bab 108 — FastAPI
- [ ] Bab 109 — Docker
- [ ] Bab 110 — Kubernetes
- [ ] Bab 111 — Cloud Deployment
- [ ] Bab 112 — MLflow
- [ ] Bab 113 — DVC
- [ ] Bab 114 — Model Monitoring
- [ ] Bab 115 — Model Drift

## Tools Pendukung

- [ ] Weights & Biases
- [ ] Hugging Face Hub
- [ ] Triton Inference Server
- [ ] Lightning
- [ ] Accelerate
- [ ] Ray

**Milestone:** mampu men-deploy model reproducible dengan monitoring latency, resource, dan kualitas prediksi.

**Mini project:** deploy YOLO API dan edge inference benchmark.

---

# Bagian 12 — Research dan Paper Reading

- [ ] Bab 116 — Cara Membaca Paper AI
- [ ] Bab 117 — Reproduksi Paper
- [ ] Bab 118 — Benchmark Dataset
- [ ] Bab 119 — Ablation Study
- [ ] Bab 120 — Menulis Paper AI

## Research Workflow

- [ ] Literature search dan screening
- [ ] Three-pass paper reading
- [ ] Literature note dan synthesis
- [ ] Research gap mapping
- [ ] Research question dan hypothesis
- [ ] Fair baseline selection
- [ ] Experiment design
- [ ] Multiple seeds dan reproducibility
- [ ] Error analysis
- [ ] Ablation study
- [ ] Paper writing
- [ ] Submission dan review process

## Target Venue

- CVPR
- ICCV
- ECCV
- WACV
- NeurIPS
- Workshop yang relevan

**Milestone:** mampu membaca, mengkritik, dan mereproduksi paper serta merancang eksperimen yang fair.

---

# Bagian 13 — Proyek Akhir

## Fundamental dan Classification

- [ ] Image Classification
- [ ] Defect Detection
- [ ] Agriculture Vision
- [ ] Medical Imaging

## Detection, Segmentation, dan Tracking

- [ ] Object Detection
- [ ] Semantic Segmentation
- [ ] Smart CCTV
- [ ] Crowd Counting
- [ ] Traffic Analysis
- [ ] Human Pose Estimation

## Document dan Identity

- [ ] OCR
- [ ] Face Recognition
- [ ] License Plate Recognition
- [ ] Document AI

## Lifestyle Applications

- [ ] Fitness Pose Coach
- [ ] Food Recognition
- [ ] Smart Wardrobe
- [ ] Retail AI

## Robotics

- [ ] Line-Following Vision
- [ ] Object Picking
- [ ] Visual Servoing
- [ ] Obstacle Detection
- [ ] Drone Vision
- [ ] Autonomous Driving

## 3D dan Multimodal

- [ ] 3D Reconstruction
- [ ] NeRF Application
- [ ] Gaussian Splatting
- [ ] Visual Search Engine
- [ ] AI Vision Assistant
- [ ] Multimodal AI Assistant

## Syarat Project Portfolio

- Problem statement jelas.
- Dataset dan license terdokumentasi.
- Baseline tersedia.
- Experiment tracking dan versioning aktif.
- Metric sesuai task.
- Error analysis dan limitation ditulis jujur.
- Inference atau deployment dapat didemonstrasikan.
- README dan reproducibility instruction lengkap.

---

# Dataset dan Benchmark Penting

- [ ] ImageNet
- [ ] CIFAR-10
- [ ] COCO
- [ ] Pascal VOC
- [ ] Open Images
- [ ] LVIS
- [ ] Cityscapes
- [ ] KITTI
- [ ] nuScenes
- [ ] Waymo Open Dataset
- [ ] ScanNet

# Navigasi

- [[AI Dashboard]]
- [[AI Knowledge Map]]
- [[Mathematics MOC]]
- [[Projects MOC]]
- [[Research MOC]]
