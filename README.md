# TCS Road Intelligence: Capstone Final Project

<img width="468" alt="image" src="https://github.com/user-attachments/assets/f0605a70-b2e2-4639-99e7-877a5f9ed381" />

- [Executive Summary](#executive-summary)
- [Project Objectives](#project-objectives)
- [Value and Business Impact](#value-and-business-impact)
- [Project Methodology](#project-methodology)
- [Lessons Learned](#lessons-learned)
- [Future Work](#suggestions-for-future-work)
- [Conclusions](#conclusions-and-recommendations)

## Executive Summary

Urban road infrastructure in developing regions presents significant challenges for drivers, including potholes, unpredictable pedestrians, and moving vehicles. This project, developed in collaboration with Tata Consultancy Services (TCS), introduces a camera-based Road Intelligence System designed to enhance driver safety using real-time hazard detection and trajectory prediction.

Leveraging computer vision and deep learning, the system detects road hazards with YOLOv8, estimates pothole size and severity using camera-based depth estimation, and predicts pedestrian/vehicle movement through RAFT optical flow and LSTM-based trajectory forecasting. Unlike sensor-heavy approaches (e.g., LiDAR), this solution is lightweight, cost-effective, and optimized for real-time performance on resource-constrained devices.

This proof of concept supports TCS’s push toward Level 3+ vehicle automation, offering a scalable and affordable alternative for smart transportation in urban environments. Future improvements include enhanced depth estimation, Transformer-based trajectory models, and deployment optimizations for edge devices.

## Project Objectives

- Hazard Detection: Accurately identify and classify road hazards, including potholes, pedestrians, and vehicles, using a camera-based solution that is both efficient and cost-effective.

- Pothole Size and Severity Estimation: Develop a robust methodology to calculate the size and severity of potholes based on camera input, leveraging intrinsic properties like focal length and viewing angle.
 
- Trajectory Prediction: Predict the future trajectories of pedestrians and vehicles on the road to enable real-time driver assistance and improve situational awareness.

- Real-Time Performance: Ensure that the system operates in real-time with minimal latency, making it suitable for deployment on resource-constrained devices and in real-world driving scenarios.

- Scalability and Affordability: Design a solution that eliminates the need for expensive hardware, such as LIDAR, making the system accessible and scalable for use in developing regions.

- Alignment with Automation Goals: Support TCS’s broader goal of advancing Level 3+ vehicle automation capabilities by providing a lightweight, camera-only solution compatible with autonomous driving systems.

## Value and Business Impact

- Advancing Automation Capabilities: The project aligns with TCS's goal of advancing towards Level 3+ automation, positioning TCS at the forefront of automotive technology.

- Innovative Technological Approach: The solution demonstrates a novel camera-based methodology, setting it apart from traditional sensor-heavy approaches.

- Cost-Effectiveness and Efficiency: Designed for efficiency, the solution is both cost-effective and lightweight, reducing implementation costs and making advanced autonomous features more accessible.

- Proof of Concept: This project serves as a successful proof of concept, paving the way for further enhancements and potential product offerings in the autonomous vehicle sector.

## Data Explanation

The dataset used in this project simulates real-world urban driving scenarios, capturing dynamic interactions between vehicles, pedestrians, and road hazards such as potholes. It consists of sequential video frames processed to extract motion patterns and object positions.

Key Data Components:
Raw Video Frames: Captured from a forward-facing vehicle camera, providing a continuous stream of road conditions.
Object Detection Annotations: Bounding box labels for potholes, pedestrians, and vehicles, enabling supervised learning for hazard identification.
Optical Flow Data: Motion vectors extracted using RAFT, allowing precise tracking of object movement between frames.
Depth and Size Estimations: Derived from monocular image analysis, estimating pothole depth and distance without requiring expensive LiDAR sensors.
Trajectory Sequences: Processed with LSTMs to predict future movement of pedestrians and vehicles based on historical motion patterns.
This dataset structure ensures robust model training and validation, enabling the system to detect hazards, estimate severity, and provide real-time trajectory forecasts to enhance driver safety.

<img width="400" alt="image" src="https://github.com/user-attachments/assets/830e8833-e97b-45dc-8abe-89e4332dfcaa" />


## Project Methodology

<img width="264" alt="image" src="https://github.com/user-attachments/assets/5b6247c6-220d-461c-958e-3b1f5a3462c4" />


### - Component 1: Pothole Distance, Size, and Depth Estimation


- Object Detection Using YOLOv8: A pre-trained YOLOv8 model detects potholes in images. The model predicts bounding boxes for potholes, providing coordinates, confidence scores, and class labels. Bounding box dimensions (width and height) are used to estimate pothole size.

- Estimation of Pothole Size: Bounding box dimensions in pixels are converted into real-world units (cm) based on camera intrinsic parameters.

  <img width="400" alt="image" src="https://github.com/user-attachments/assets/52c77883-c72e-4da2-88e1-e7cfa27f2de0" />

- Estimation of Distance to Potholes: A formula based on bounding box position and camera parameters estimates pothole distance.

 <img width="468" alt="image" src="https://github.com/user-attachments/assets/c9d2eb9b-9fc5-402d-8b42-bcebab2dcd5f" />

- Estimation of Pothole Depth: Structure from Motion (SfM) photogrammetric technique constructs a 3D point cloud. The RANSAC algorithm determines the best-fit plane for depth calculation. The deepest point inside the pothole is calculated based on plane differences.
  
  <img width="367" alt="image" src="https://github.com/user-attachments/assets/ff331aca-91d8-4cf0-9c28-674420193ff1" />

  <img width="204" alt="image" src="https://github.com/user-attachments/assets/cc00ee06-20a1-4965-b60c-e6d69b69e6bb" />

- Visualization and Annotation: Detected potholes are annotated with labels, confidence scores, estimated size, and distance from the camera.
  
  <img width="492" alt="image" src="https://github.com/user-attachments/assets/6703197b-ad58-4cc0-b79e-0d8b87e2d32d" />

  <img width="468" alt="image" src="https://github.com/user-attachments/assets/e08c4daa-45cb-4b25-a85c-3faab0070be6" />


### - Component 2: Trajectory Prediction

- Data Preparation: YOLOv5 extracts object bounding boxes from video frames.
  
  <img width="290" alt="image" src="https://github.com/user-attachments/assets/f10d5ee4-cb38-49ba-a966-68ba9939b065" />

- Optical Flow Estimation with RAFT: RAFT generates a dense flow field, representing pixel-wise motion. The motion features are averaged within bounding boxes to derive object movement.

<img width="468" alt="image" src="https://github.com/user-attachments/assets/9929be59-3403-4165-80db-7c955230966b" />

- Feature Engineering: The pipeline calculates feature vectors comprising normalized bounding box coordinates and averaged optical flow values.

- Sequence Matching and Tracking: Bounding boxes are matched between consecutive frames to maintain object continuity.

- Trajectory Prediction Model Architecture: A lightweight LSTM network processes sequential input features. The LSTM learns temporal dependencies and predicts future bounding boxes.
  
  <img width="324" alt="image" src="https://github.com/user-attachments/assets/39e6b5ee-b014-442c-a057-b0b68143e0bf" />

- Training and Evaluation: The model is trained using sequences of input features and ground truth bounding boxes. Mean Squared Error (MSE) loss function is used to optimize the model.

- Inference and Visualization: The model predicts future trajectories, visualized as bounding boxes overlaid on video frames.
  
  <img width="468" alt="image" src="https://github.com/user-attachments/assets/70cdce0e-2dad-4f0a-999e-01c34e8370c6" />

  <img width="270" alt="image" src="https://github.com/user-attachments/assets/b26ea82f-342e-4596-b0df-c04129721339" />


## Lessons Learned

- Importance of Lightweight Models for Edge Deployment: The need for optimization to ensure real-time performance on resource-constrained devices.

- Challenges in Depth Estimation: Limitations in depth accuracy due to monocular camera assumptions.

- Real-World Variability: Environmental factors such as lighting, occlusions, and road texture impact model performance.

## Suggestions for Future Work

- Enhanced Depth Estimation: Integrate stereo vision or deep learning-based depth estimation.

- Improved Trajectory Prediction: Use Transformer-based architectures for more accurate movement predictions.

- Edge Deployment Optimization: Further optimize the model for real-time processing on low-power hardware.

- Multi-Modal Sensor Integration: Combine camera input with other sensors for higher accuracy and robustness.

## Conclusions and Recommendations

This project successfully developed a camera-based system for pothole detection and trajectory prediction, demonstrating the feasibility of a lightweight, cost-effective solution. While initial results were promising, further optimization and model refinement will be necessary for real-world deployment. Future work should focus on improving prediction accuracy, optimizing computational efficiency, and ensuring scalability to different urban environments. The approach aligns well with TCS's automation goals and presents an opportunity for continued research and development in autonomous driving solutions.
