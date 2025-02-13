# TCS Road Intelligence: Capstone Final Project

<img width="468" alt="image" src="https://github.com/user-attachments/assets/f0605a70-b2e2-4639-99e7-877a5f9ed381" />

## Executive Summary

The team was tasked with addressing a critical challenge faced by vehicles navigating busy urban roads, particularly in rapidly developing regions such as India. In these areas, road infrastructure often poses significant risks to drivers due to obstacles like potholes and the unpredictable movement of pedestrians and other vehicles. This project aimed to develop an intelligent system that uses only camera input to identify and estimate the size and severity of potholes, predict the future trajectory of pedestrians and provide real-time assistance to drivers.

The approach leveraged a combination of fundamental computer vision techniques and cutting-edge machine learning models tailored for real-time performance. The solution comprised four main components:

- Object Detection: The team used the YOLOv5 (You Only Look Once) object detection framework for identifying potholes, vehicles, and pedestrians.

- Pothole Distance and Size Estimation: A custom formula was used to estimate the size and severity of potholes, accounting for assumptions regarding the camera's angle, focal length, and other intrinsic properties.

- Trajectory Prediction: The RAFT (Recurrent All-Pairs Field Transform) model was employed for optical flow estimation to understand motion patterns on the road. LSTM (Long Short-Term Memory) models were integrated to predict the future trajectories of pedestrians and vehicles, enabling drivers to make informed decisions.

- Real-time Integration and Optimization: The system was designed to operate efficiently on limited computational resources, with all code written in Python and optimized for lightweight processing.

The solution delivered several key benefits to TCS (Tata Consultancy Services). By building a resource-efficient, camera-only system, the project offered a cost-effective alternative to traditional sensor-intensive techniques such as LIDAR. Furthermore, the system's design aligns with TCS's ongoing efforts to enhance Level 3+ vehicle automation capabilities, bridging the gap between accessible technology and advanced automation.

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

The dataset simulates an urban driving setting, featuring dynamic scenes with moving objects such as pedestrians, vehicles, and potholes. It consists of sequential video frames capturing common scenarios encountered in real-world environments.

<img width="400" alt="image" src="https://github.com/user-attachments/assets/830e8833-e97b-45dc-8abe-89e4332dfcaa" />


## Project Methodology

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

= Optical Flow Estimation with RAFT: RAFT generates a dense flow field, representing pixel-wise motion. The motion features are averaged within bounding boxes to derive object movement.

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
