PlanetDetection-Robotics
=====================

## Overview
This repository features the development of an autonomous system capable of navigating a spacecraft and identifying celestial bodies such as Earth, Moon, Mars, and Mercury. The robot utilizes image processing, machine learning, and LaserScan data to detect windows, stitch images, and calculate planetary distances, ultimately determining the spacecraft’s current location.

## Team Members

Ayesha Rahman

Sandra Guran

Natalie Leung

Geeyoon Lim

## Aim
The primary aim of the system is to:
- Enable autonomous navigation inside a spacecraft using predefined coordinates.
- Identify celestial objects through spacecraft windows and estimate distances between them.
- Perform image stitching to create panoramic views of the detected planets and calculate their positions.
  
## Tasks Breakdown

1. Navigation & Obstacle Avoidance
 ![Alt text](https://github.com/sc21samg/Navigation_PlanetaryDetection_Robot/blob/main/1.1%20git.png)
- Input Coordinates: The robot navigates to specific locations within the spacecraft using target coordinates provided in a YAML file. The system dynamically adapts to different environments based on these coordinates. The navigation sequence leverages the ROS 2 navigation stack, sending goals to move between the spacecraft’s predefined locations.
- Wall Navigation: The robot uses LaserScan data to navigate along walls and avoid obstacles. The system employs PID controllers (Proportional, Integral, Derivative) for precise control of the robot's lateral and longitudinal movements. The real-time feedback from the LaserScan data helps the robot adjust its distance from walls and avoid obstacles while navigating the spacecraft.
- Grid Navigation & Heuristics: A heuristic-based navigation strategy predicts window locations based on common room layouts, such as a rectangular structure with windows on opposite walls. This improves the robot's exploration efficiency by focusing its search in likely window locations before visual confirmation.

2. Sign Detection
- Red and Green Sign Detection: The robot uses computer vision techniques to detect red and green signs at the entrances of different rooms within the spacecraft. These signs determine whether the robot should enter or bypass a room. A green sign indicates that the robot should enter and continue its navigation within the room, while a red sign signals that the robot should avoid the area. The detection process uses color filtering and contour analysis to recognize the signs, and the robot reacts accordingly by adjusting its route.

3. Window Detection & Image Stitching
 ![Alt text](https://github.com/sc21samg/Navigation_PlanetaryDetection_Robot/blob/main/1.3%20git.png)
- Window Detection: After entering a room, the robot identifies windows using shape detection techniques, specifically searching for rectangular objects with distinct white borders and predominantly black interiors. The window detection algorithm checks for these geometric and color properties to distinguish windows from other similar objects.
- Screenshot Capture: Once a window is detected, the robot aligns itself with the window using PID control to ensure it is positioned optimally. The robot then captures multiple images of the view through the window, which are subsequently stitched together to form a panoramic image of the outside environment. The stitched panorama is used in the later stages for planetary detection and measurement.

4. Planet Detection & Measurement
 ![Alt text](https://github.com/sc21samg/Navigation_PlanetaryDetection_Robot/blob/main/1.4%20git.png)
- Planet Detection: Using a machine learning model, the robot detects planets such as Earth, Moon, Mars, and Mercury within the stitched panorama. The detection algorithm applies a ResNet18-based convolutional neural network (CNN) that has been trained specifically on planetary images. Once planets are detected, they are marked with bounding boxes in the panoramic image.
- Distance Calculation: After the planets are detected, the system calculates the distance between them. The pixel height of each detected planet, along with its known real-world diameter, is used to estimate its distance from the spacecraft. This information helps determine the spacecraft’s current location relative to the detected planets. The distances between planets are computed using Euclidean distance formulas, based on their coordinates in the panoramic image.

5. Machine Learning Model for Planet Detection
- Model Architecture: The planet detection task is handled by a custom ResNet18-based convolutional neural network. This CNN architecture was chosen for its strong performance in image classification tasks. The model has been fine-tuned to detect celestial bodies using a dataset of planetary images, focusing on distinguishing between Earth, Moon, Mars, and Mercury.
- Training & Accuracy: The model was trained on a dataset sourced from GitHub and other internet repositories, achieving a classification accuracy of 92.59% on the test set. The dataset was split into training and validation subsets to ensure robustness and generalization, with augmentation techniques applied to handle variations in image quality and lighting conditions.

6. Simulation & Real-World Testing
- Simulation Worlds: The robot was tested in a variety of simulated environments to validate its navigation, window detection, and planetary detection capabilities. These environments ranged in difficulty from simple room layouts to more complex configurations with obstacles and challenging lighting conditions. Each simulation world (Easy, Moderate, Hard, and Custom) was designed to stress-test different aspects of the robot’s capabilities.
- Real Robot Testing: After completing the simulation tests, the system was deployed on a real robot. The tests focused on validating the performance of the navigation system, window detection, and planetary detection in real-world conditions. The LaserScan data used in real-world testing posed additional challenges, such as noise and data inconsistency, which were addressed through adaptive algorithms for better handling of sensor input.

## Project Structure
- src/: Source code for robot control, image processing, and navigation.
- models/: Pre-trained ResNet18 model used for planet detection.
- worlds/: Simulation worlds used for testing the system.
- scripts/: Helper scripts for window detection, image stitching, and planet distance calculation.
- requirements.txt: Required Python libraries for running the project.

## Results & Limitations
- Achievements: The system successfully navigates through rooms, detects windows, and performs planetary detection with accurate distance measurement.
- Challenges: In real-world scenarios, the system encounters difficulties with LaserScan data consistency and low-resolution images, affecting the accuracy of planetary detection.

## Future Work
Potential enhancements include:
- Implementing "LaserScan Stretching" to compensate for gaps in sensor data during navigation.
- Improving planetary detection accuracy in challenging real-world environments.
