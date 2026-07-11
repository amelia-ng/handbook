# Convolutional Neural Networks

## 1. Overview

Convolutional Neural Networks (CNNs) are a class of deep learning models designed to process data with a grid-like topology, such as images.

They are particularly effective for visual recognition tasks and have become the cornerstone of modern computer vision applications.

## 2. Core Concept

### 2.1. Convolution Operation

- **Definition**: A mathematical operation tha combines two functions to produce a third function expressing how the shape of one is modified by the other.
- **Purpose in CNNs**: To extract features from the input data by applying filters (kernels).
- **Mathematical Representation**: For a 2D input image $I$ and a filter $K$:

$$(I * K)(i,j) = \sum_m \sum_n I(i-m, j-n)\cdot K(m,n)$$

in which:

- $(I * K)(i,j)$: Result of the convolution at position $(i, j)$
- $I(i-m, j-n)$: Pixel value at position $(i-m, j-n)$ in the input image.
- $K(m,n)$: Value at the kernel at position $(m, n)$.
- $\sum_m \sum_n$: Summations over the kernel dimensions.

### 2.2. Filters/Kernels
- Small matrices (e.g., 3 x 3, 5 x 5) used to detect specific features.
- The values in the filters are learned during training.

### 2.3. Feature Maps

- The outputs of convolution operations.
- Represent the presence of features (edges, textures) at various locations.

### 2.4. Activation Functions

- Introduce nonlinearity into the network.
- Common functions:
    - **ReLU (Rectified Linear Unit)**: 

        $$\operatorname{ReLU}(x)=\max(0,x)$$

    - **Sigmoid Function**:

        $$\sigma(x)=\frac{1}{1+e^{-x}}$$
    
    - **Tanh Function**

        $$\tanh(x)=\frac{e^x-e^{-x}}{e^x+e^{-x}}$$
- Components:
    - $x$: Input to the activation function.
    - $e$: Euler's number, base of natural logarithms.

### 2.5. Pooling Layers
- Reduced the spatial dmensions of feature maps. 
- Type:
        
    - **Max Pooling**: Takes the maximum value in each window.
    - **Average Pooling**: Computes the averag value.

- Max Pooling Math Rep:

$$Y(i,j) = \max_{0 \le m < p} \max_{0 \le n < p} X(p \cdot i + m,\; p \cdot j + n)$$

| Component | Description |
|----------|-------------|
| $Y(i,j)$ | Output after pooling at position $(i,j)$. |
| $X$ | Input feature map. |
| $p$ | Pooling window size. |
| $\max$ | Maximum function. |

### 2.6. Fully Connected Layers
- Neurons are fully connected to all activations in the previous layer.
- Serve as classifiers in the network

## 3. CNN Architecture Components

- **Input Layer**: Receives raw pixel data (e.g., for a color image, dimensions are height × width × 3).

- **Convolutional Layers**: Apply filters to extract features.

- **Activation Layers**: Apply non-linear functions such as ReLU.

- **Pooling Layers**: Downsample feature maps.

- **Flattening Layer**: Converts 2D feature maps into a 1D vector.

- **Fully Connected Layers**: Perform classification based on extracted features.

## 4. How CNNs Work

### 4.1. Feature Extraction

- Convolutional and pooling layers act as feature extractors.
- Early layers capture low-level features (edges, corners).
- Deeper layers capture high-level features (objects, shapes).

### 4.2. Classification
Fully connected layers analyze features to classify the input.

## 5. Training a CNN
- **Forward Propagation**: Input data is passed through the network to generate an output.
- **Loss Function**: Measures the difference between the predicted and true values.

    - **Cross-Entropy Loss** for classification

    $$L=-\sum_i y_i\log(p_i)$$

    | Component | Description |
    |----------|-------------|
    | $L$ | Loss |
    | $y_i$ |True lable (1 if class $i$ is correct, 0 otherwise). |
    | $p_i$ | Predicted probability for class $i$ |

- **Back propagation**
    - Calculate gradients for the loss function with respect to weights.
    - Update weights using gradient descent algorithms.

- **Optimization Algorithms**
    - **Stochastic Gradient Descent (SGD)**
    
    $$w \leftarrow w-\alpha\nabla L(w)$$

    | Component | Description |
    |----------|-------------|
    | $w$ | Weights of the network |
    | $\alpha$ |Learning rate. |
    | $\nabla L(w)$ | Gradint of the loss with respect to $w$|

    - **Adaptive Methods**: Adaptive Moment Estimation (Adam) and Root Mean Square Propagation (RMSprop) are popular methods to adjust learning rates during training.

## 6. Popular CNN Architectures
| Architecture | Architecture Description | Key Innovation | Advantages | Limitations | Best Use Cases |
|--------------|--------------------------|----------------|------------|-------------|----------------|
| **LeNet-5** (Mainly for historical/educational) | The first successful convolutional neural network designed for handwritten digit recognition. Introduced convolution, pooling, and fully connected layers. (7 layers)| First practical CNN architecture | Simple, lightweight, easy to understand, fast training | Very limited capacity; unsuitable for complex images | Learning CNN fundamentals, MNIST digit recognition, education |  |
| **AlexNet** (Mainly for historical/educational)| A breakthrough CNN that won the 2012 ImageNet competition and demonstrated the effectiveness of deep learning for computer vision. (8 layers) | ReLU activation, Dropout, GPU training, Data Augmentation | Much higher accuracy than previous methods, relatively simple architecture | Large model (~60M parameters), computationally expensive | Learning modern CNNs, transfer learning on small datasets, historical benchmark |
| **VGG16 / VGG19** (Quite common for transfer learning adn education)| Deep CNN that stacks many small 3×3 convolution filters to improve feature learning while maintaining a simple architecture. (16/19 layers) | Uniform use of 3×3 convolutions | Excellent feature extraction, simple design, widely available pretrained models | Extremely large model (~138M parameters), slow inference, high memory usage | Feature extraction, transfer learning, visualization, medical imaging |
| **GoogLeNet (Inception V1)** (Less common than ResNet but still influential) | Deep CNN that processes multiple convolution kernel sizes in parallel using Inception modules to capture multi-scale features efficiently. (22 layers)| Inception Module (multi-scale convolutions) | parameters than VGG, strong accuracy | Complex architecture, harder to implement and customize | Efficient image classification, embedded vision systems |
| **ResNet** (Popular industry standard backbone)| Deep residual network that introduces shortcut (skip) connections, allowing extremely deep models to be trained effectively. (50–152+ layers) | Residual (Skip) Connections | Solves vanishing gradients, excellent accuracy, easy optimization, strong pretrained models | Higher computational cost than lightweight CNNs | Image classification, object detection, segmentation, autonomous driving, medical imaging |
| **DenseNet** (Popular in research and specialized applications) | Connects every layer to all subsequent layers, encouraging feature reuse and improving gradient flow. (121–264 layers) | Dense Connections | Better feature reuse, fewer parameters than ResNet, strong gradient propagation | High memory usage due to dense connectivity | Medical imaging, fine-grained recognition, feature extraction |
| **EfficientNet** (Very popular for efficient vision models)| CNN family that scales depth, width, and image resolution together using a compound scaling strategy to maximize accuracy and efficiency. (≈82–813 layers depending on variant) | Compound Scaling | Excellent accuracy with relatively few parameters, fast inference, highly efficient | More complex scaling strategy, less interpretable architecture | Mobile AI, edge devices, transfer learning, image classification | 

## 7. Application of CNNs
Applications of CNNs

- Image classification
- Object detection
- Semantic segmentation
- Facial recognition
- Medical image analysis
- Video analysis