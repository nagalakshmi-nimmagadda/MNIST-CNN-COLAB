# MNIST Classification with Progressive CNN Models

[![Python](https://img.shields.io/badge/Python-3.8%2B-blue.svg)](https://www.python.org/downloads/)
[![PyTorch](https://img.shields.io/badge/PyTorch-2.0%2B-red.svg)](https://pytorch.org/)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](https://opensource.org/licenses/MIT)
[![Code Style](https://img.shields.io/badge/code%20style-black-000000.svg)](https://github.com/psf/black)
[![Accuracy](https://img.shields.io/badge/Accuracy-99.54%25-success.svg)]()
[![Parameters](https://img.shields.io/badge/Parameters-<8K-brightgreen.svg)]()
[![AWS](https://img.shields.io/badge/AWS-EC2%2FSageMaker-orange.svg)](https://aws.amazon.com/)

## Project Overview
This project implements a systematic approach to MNIST digit classification using three progressive CNN models, achieving >99.4% accuracy with less than 8000 parameters in ≤15 epochs.

## Project Requirements
- Achieve 99.4% accuracy consistently in final epochs
- Complete training within 15 epochs
- Model size: ≤8000 parameters
- Implementation in 3 progressive steps
- Training deployment on AWS (EC2/SageMaker)

## Model Evolution

### Model 1: Foundation Architecture
**Target:**
- Establish baseline performance with minimal parameters
- Implement basic CNN structure with GAP
- Target accuracy: 96%

**Architecture:**
- 4 Convolutional layers (1→4→8→16→10)
- MaxPooling and GAP for dimensionality reduction
- Basic normalization

**Results:**
- Parameters: 1,674
- Best Training Accuracy: 95.94%
- Best Test Accuracy: 96.58% (Epoch 13)
- Epochs to reach target: 13

**Analysis:**
- Successfully established lightweight architecture
- Good parameter efficiency with basic feature extraction
- GAP implementation helped reduce parameters
- Key features: Basic convolutions, MaxPooling, GAP

### Model 2: Enhanced Architecture
**Target:**
- Optimize model capacity while maintaining parameter budget
- Implement batch normalization and efficient blocks
- Target accuracy: 98.5%

**Architecture:**
- Structured blocks: Input, Convolution, Transition
- Batch normalization after each convolution
- Efficient channel management (8→12→16→8→12→10)

**Results:**
- Parameters: 5,208
- Best Training Accuracy: 99.02%
- Best Test Accuracy: 98.91% (Epoch 12)
- Epochs to reach target: 6

**Analysis:**
- Significant improvement in accuracy with minimal parameter increase
- Batch normalization greatly improved training stability
- Better feature extraction through structured blocks
- Key features: BatchNorm, Block architecture, Efficient transitions

### Model 3: Final Optimization
**Target:**
- Achieve consistent 99.4%+ accuracy
- Implement advanced training techniques
- Optimize model dynamics

**Architecture:**
- Advanced feature processing blocks
- Strategic dropout (0.008)
- Optimized channel progression (8→10→12→16→16→10)

**Results:**
- Parameters: 6,188
- Best Training Accuracy: 99.40%
- Best Test Accuracy: 99.54% (Epoch 12)
- Consistent >99.4% accuracy from Epoch 11-15

**Analysis:**
- Successfully achieved target accuracy
- OneCycleLR scheduler proved crucial for optimization
- Image augmentation helped in generalization
- Key features: Advanced augmentation, Dropout, OneCycleLR

## Training Techniques
1. Data Augmentation:
   - Random rotation (-5° to 5°)
   - Random affine transforms
   - Normalization (mean=0.1307, std=0.3081)

2. Optimization:
   - Adam optimizer with weight decay
   - OneCycleLR scheduler
   - Dropout regularization

## Detailed Architecture Analysis

### Model 1: Foundation Architecture (Basic CNN)
**Design Philosophy:**
- Minimalist approach focusing on essential CNN components
- Progressive channel expansion (1→4→8→16→10)
- Early dimensionality reduction with MaxPool

**Architecture Breakdown:**
1. Initial Feature Extraction:
   - Conv2d(1, 4, 3): Initial feature detection
   - Output: 26x26x4
   
2. Feature Enhancement:
   - Conv2d(4, 8, 3): Increased feature channels
   - Output: 24x24x8
   - MaxPool2d(2, 2): Spatial reduction
   - Output: 12x12x8

3. Feature Refinement:
   - Conv2d(8, 16, 3): Deep feature extraction
   - Output: 10x10x16

4. Classification Head:
   - Conv2d(16, 10, 1): Channel reduction
   - GAP: Spatial summarization
   - Output: 1x1x10

**Key Insights:**
- Simple architecture achieved 96.58% accuracy
- GAP eliminated need for dense layers
- Limited parameter count: 7,432
- Showed signs of underfitting, indicating room for optimization

### Model 2: Enhanced Architecture (Structured CNN)
**Design Philosophy:**
- Block-based architecture for better feature organization
- Efficient information flow through transition blocks
- Balanced channel expansion and compression

**Architecture Breakdown:**
1. Input Block:
   - Conv2d(1, 8, 3) + BatchNorm + ReLU
   - Output: 26x26x8

2. Convolution Block 1:
   - Two Conv layers with BatchNorm
   - Channel progression: 8→12→16
   - Output: 24x24x16

3. Transition Block:
   - MaxPool + 1x1 Conv
   - Channel compression: 16→8
   - Output: 12x12x8

4. Convolution Block 2:
   - Two Conv layers with BatchNorm
   - Channel progression: 8→12→12
   - Output: 8x8x12

5. Output Block:
   - 1x1 Conv + GAP
   - Output: 1x1x10

**Key Improvements:**
- BatchNorm stabilized training
- Structured blocks improved feature hierarchy
- Channel compression reduced parameters
- Better accuracy (98.91%) with minimal parameter increase

### Model 3: Final Optimization (Advanced CNN)
**Design Philosophy:**
- Focus on regularization and training dynamics
- Strategic feature processing
- Advanced training techniques integration

**Architecture Breakdown:**
1. Initial Feature Extraction:
   - Conv2d(1, 8, 3) + BatchNorm
   - Output: 26x26x8

2. Feature Processing Block 1:
   - Conv2d(8, 10, 3) + BatchNorm
   - MaxPool + Dropout(0.008)
   - Output: 12x12x10

3. Feature Processing Block 2:
   - Two Conv layers (10→12→16)
   - BatchNorm after each Conv
   - Output: 8x8x16

4. Transition Block:
   - MaxPool + Dropout
   - Output: 4x4x16

5. Final Processing:
   - Conv2d(16, 16, 3)
   - GAP + Dropout
   - Linear(16, 10)

**Advanced Features:**
1. Training Optimizations:
   - OneCycleLR scheduler
   - Adam optimizer with weight decay
   - Strategic dropout placement

2. Data Augmentation:
   - Random rotation
   - Random affine transforms
   - Normalized inputs

**Evolution Highlights:**
- Consistent >99.4% accuracy
- Efficient parameter usage (7,984)
- Better generalization through regularization
- Robust against overfitting

### Architecture Evolution Summary
1. **Parameter Efficiency:**
   - Model 1: 1,674 parameters
   - Model 2: 5,208 parameters
   - Model 3: 6,188 parameters
   - Gradual increase with significant accuracy gains

2. **Feature Extraction:**
   - Model 1: Basic sequential approach
   - Model 2: Structured block-based approach
   - Model 3: Advanced processing with regularization

3. **Training Dynamics:**
   - Model 1: Basic training
   - Model 2: Added BatchNorm
   - Model 3: Comprehensive training strategy

4. **Accuracy Progression:**
   - Model 1: 96.58%
   - Model 2: 98.91%
   - Model 3: 99.54%
   - Consistent improvement while maintaining parameter efficiency
