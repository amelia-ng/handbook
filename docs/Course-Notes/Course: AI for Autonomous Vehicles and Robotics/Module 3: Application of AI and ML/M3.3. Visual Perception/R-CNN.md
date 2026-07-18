# M3.3.2. Regional Convolutional Neural Networks (R-CNN)

<blockquote> 
If the LaTex Math notation are not displayed correctly, please reload the site.
</blockquote>

## 1. Overview

Regional Convolutional Neural Networks (R-CNN) represent a seminal advancement in the field of object detection within computer vision. R-CNN bridges the gap between image classification and object detection by effectively localizing objects within an image and classifying them into predefined categories. It also paved the way for later models such as Fast R-CNN and Faster R-CNN, which enhanced its speed and efficiency.

## 2. Motivation Behind R-CNN

Before R-CNN, object detection methods struggled with balancing accuracy and computational efficiency. Traditional approaches such as sliding window techniques were computationally expensive because they required running a classifier on numerous possible object locations and scales within an image. The key motivations of R-CNN include

- **Efficiency**: Reduce the computational burden by narrowing down potential object locations.
- **Accuracy**: Leverage the power of deep convolutional neural networks (CNNs) for feature extraction and classification.
- **Scalability**: Develop a method that can handle a wide variety of object classes and complex images.

## 3. Core Components of R-CNN

R-CNN combines region proposals with CNNs to achieve object detection. The main components are:

- Region Proposal Generation
- Feature Extraction using CNNs
- Classification using Support Vector Machines (SVMs)
- Bounding Box Regression

### 3.1. Region Proposal Generation

**Selective Search** is the algorithm used in R-CNN to generate region proposals. It aims to generate a small set of high-quality object locations (around 2000 per image), significantly reducing the number of regions to process.

**Selective Search Algorithm Steps**

- **Step 1: Superpixel Generation**
    - The image is segmented into small regions called superpixels using algorithms such as Felzenszwalb's method.
    - Superpixels are groups of pixels with similar color or texture.

- **Step 2: Region Hierarchy Creation**
    - Superpixels are recursively merged based on similarity measures (e.g., color, texture, size, shape compatibility). This process creates a hierarchical grouping of regions.

- **Step 3: Region Proposal Extraction**
    - The algorithm outputs a set of candidate regions that potentially contain objects. These regions vary in size and aspect ratio, capturing objects of different scales.

**Advantages of Selective Search**

- *Efficiency*: Reduces the search space from millions of windows to a few thousand.
- *Quality*: Generates proposals with high object recall..

### 3.2. Feature Extraction using CNNs

Each region proposal is processed individually through a CNN to extract features.

- **Normalization**: Each region proposal is warped to a fixed size (e.g., 227 × 227 pixels) required by the CNN.
- **CNN Architecture**: Typically uses models such as AlexNet pretrained on ImageNet.
- **Feature Extraction**: The CNN processes the input region and outputs a fixed-length feature vector from one of the fully connected layers (e.g., the 4096-dimensional output from the FC7 layer).

**Mathematical Representation**

Let $R_i$ be the $i$-th region proposal. The feature vector $f_i$ is obtained by

$$f_i = CNN(R_i; \theta)$$

| Symbol | Description |
| --------- | ------------ |
| $f_i$ | Feature vector for region $R_i$ |
| $CNN$ | Convolutional Neural Network function |
| $R_i$ | Warped region proposal |
| $\theta$ | Parameters (weights and biases) of the CNN |

### 3.3. Classification Using Support Vector Machines (SVMs)

The extracted feature vectors are used to train class-specific linear SVMs.

- **Training SVMs**: For each object class, a binary SVM classifier is trained to distinguish between that class and the background.
- **Positive examples**: Feature vectors of region proposals that overlap significantly with ground-truth bounding boxes (e.g., Intersection over Union (IoU) > 0.5).
- **Negative examples**: Feature vectors of region proposals with low overlap with any ground-truth boxes (e.g., IoU < 0.3).

**Classification**: During testing, each feature vector $f_i$ is classified by the SVMs to determine the presence of objects.

**Mathematical Representation**: The decision function for class $c$ is

$$ s^c_i = w^T_c f_i + b_c $$

| Symbol | Description |
| --------- | --------------- |
| $s_i^c$ | Score for region $R_i$ belonging to class $c$ |
| $w_c$ | Weight vector for the SVM of class $c$ |
| $b_c$ | Bias term for the SVM of class $c$ |
| $f_i$ | Feature vector for region $R_i$ |

### 3.4. Bounding Box Regression

To refine the predicted bounding boxes, R-CNN employs bounding box regression. The objective is to correct the localization errors by predicting adjustments to the coordinates of the region proposals.

**Regression Model**: A linear regression model is trained for each class to predict the offsets between the proposed boxes and the ground-truth boxes. The regression model minimizes the difference between the predicted box and the ground-truth box.

**Parameterization**:

For a region proposal with coordinates $(x, y, w, h)$ and ground-truth box $(x^*, y^*, w^*, h^*)$, the target offsets are

$$
t_x = \frac{x^* - x}{w}, t_y = \frac{y^* - y}{h}, t_w = \log({\frac{w^*}{w}}), t_h = \log({\frac{h^*}{h}})
$$

| Component | Description |
| ------------- | -------------- |
| $x, y$ | Center coordnates of the region proposal |
| $w, h$ | Width and height of the region proposal |
| $(x^*, y^*, w^*, h^*)$ | Corresponding parameters of the ground-truth bounding box |
| $t_x, t_y, t_w, t_h$ | Regression targets representing the adjustments needed |

**Prediction**:

The regression model predicts ($\Delta x, \Delta y, \Delta w, \Delta h$) such that the refined bounding box coordinates  $(x', y', w', h')$ are

$x' = x + w \cdot \Delta x$. $y' =y + h \cdot \Delta y$, $w' = w \cdot e^{\Delta w}$, $h' = h \cdot e^{\Delta h}$

**Training**:

The regression weights are learned by minimizing the loss between the predicted offsets ($\Delta x, \Delta y, \Delta w, \Delta h$) and the target offsets ($t_x, t_y, t_w, t_h$).

**Loss Function**: Typically uses the Smooth $L_1$ Loss:

$$L_{reg}(t_i, t^*_i) = \Sigma_{j \in {x, y, w, h}} smooth _{L_1}(t_i^j - t_j^j*)$$

where

$$
\operatorname{smooth_{L_1}}(x) =
\begin{cases}
0.5x^2, & \text{if } |x| < 1, \\
|x| - 0.5, & \text{otherwise}.
\end{cases}
$$

| Component | Description |
| ------------ | ----------- |
| $L_{reg}$ | Regression Loss |
| $t_i$ | Predicted offsets for region $i$ | 
| $t_i^*$ | Ground-truth offsets for region $i$ | 
| $j$ | Index over the coordinates $x, y, w, h$ | 

## 4. Training Procedure of R-CNN

- Training R-CNN involves multiple stages:

    **1. Pretraining the CNN**: The CNN is pretrained on a large dataset such as ImageNet for image classification. This step initializes the network with good feature representations.

    **2. Fine-tuning the CNN**: The CNN is fine-tuned for object detection using the region proposals. Each region proposal is labeled as one of the object classes or background. The CNN is retrained (fine-tuned) using these labeled regions to adapt the features for detection.

    **3. Training SVMs**: Extract feature vectors from the CNN for each region proposal. Train one binary SVM per class using these features.

    **4. Training Bounding Box Regressors**: Use the features from the CNN to train linear regression models for bounding box refinement. Separate regressors are trained for each class.

- Notes on Training:

    - **Multi-Stage Process**: Training is complex because it requires separately training the CNN, SVMs, and regressors.
    - **Data Handling**: Requires careful preparation of training data, including labeling and balancing positive and negative examples.
    - **Computation Time**: The process is time-consuming due to the need to process each region proposal through the CNN.

## 5. Testing and Inference

During testing, the R-CNN follows these steps:

- **Step 1. Generate Region Proposals**: Use Selective Search to obtain candidate regions.
- **Step 2. Extract Features**: Warp each region to the required input size. Pass each region through the CNN to get feature vectors.
- **Step 3. Classify Regions**: Use the trained SVMs to classify each region as an object class or background. Obtain classification scores.
- **Step 4. Bounding Box Regression**: Apply the bounding box regressors to refine the predicted bounding boxes
- **Step 5. Post-Processing**
    - Non-Maximum Suppression (NMS):  
        - Suppresses overlapping boxes to reduce duplicates.
        - Retains boxes with the highest classification scores.
    - Non-Maximum Suppression Algorithm: For each class:
        - Sort all detection boxes by their scores in descending order.
        - Select the box with the highest score and remove all other boxes with an IoU greater than a threshold (e.g., 0.3).
        - Repeat the process with the remaining boxes.

## 6. Limitations of R-CNN

Despite its groundbreaking contributions, R-CNN has several limitations.

- **Computational Inefficiency**: *Per-Region CNN Computation*: Each of the ~2000 region proposals per image is processed independently through the CNN, leading to significant computational overhead.
- **Storage Requirements**: *Feature Caching*: Extracted features are often cached to disk due to memory constraints, consuming substantial storage space.
- **Multi-Stage Training Pipeline**: *Complexity*: Separate training of the CNN, SVMs, and regressors complicates the pipeline and increases the chance of errors.
- **Slow Inference**: *Not Real-Time*: The slow processing makes R-CNN unsuitable for real-time applications.

## 7. Improvements Over R-CNN

To address the limitations of R-CNN, several models introduce improvements.

### 7.1. Fast R-CNN

- **Shared Computation**: Processes the entire image once to generate a feature map, reducing redundant CNN computations.
- **RoI Pooling Layer**: Extracts fixed-size feature maps for each region proposal directly from the shared feature map.
- **Single-Stage Training**: Integrates classification and bounding box regression into a single network
- **Speed Improvements**: Significantly faster training and inference compared to R-CNN.

### 7.2. Faster R-CNN

- **Region Proposal Network (RPN)**: Introduces a network to generate region proposals, eliminating the need for Selective Search.
- **Fully Integrated Network**: Combines the RPN and Fast R-CNN into a unified architecture.
- **Real-Time Performance**: Achieves near real-time object detection speeds.

### 7.3. Mask R-CNN

- **Instance Segmentation**: Extends Faster R-CNN by adding a branch for predicting object masks
- **RoIAlign Layer**: Improves upon RoI Pooling by preventing quantization errors, leading to better mask predictions.

## 8. Impact and Applications of R-CNN

R-CNN has significantly impacted the field of object detection:

- **Benchmark Performance**: Achieved state-of-the-art results on datasets such as PASCAL VOC
- **Foundation for Future Models**: Paved the way for more efficient models (Fast R-CNN, Faster R-CNN)
- **Widespread Adoption**: Influenced numerous applications requiring object localization and recognition.

## 9. Applications

- Autonomous Vehicles: Detecting pedestrians, vehicles, and obstacles.
- Surveillance Systems: Monitoring environments for security purposes.
- Robotics: Enabling robots to recognize and interact with objects.
- Medical Imaging: Identifying regions of interest in scans (e.g., tumors).
- Retail Analytics: Understanding customer behavior through object detection.

## 10. Challenges and Considerations

While R-CNN advanced object detection, it also highlighted challenges.

- Need for Speed: Real-world applications often require real-time detection.
- Data Requirements: High-quality annotated datasets are necessary for training.
- Computational Resources: Training and deploying R-CNN models require significant computational power.

R-CNN represents a major advancement in computer vision, demonstrating that deep learning could be effectively applied to object detection. By integrating region proposals with CNNs and traditional machine learning methods such as SVMs, R-CNN has achieved high accuracy in detecting and classifying objects within images. Understanding R-CNN provides valuable insights into the evolution of object detection techniques and the foundational concepts that continue to influence modern architectures.