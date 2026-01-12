# DualNav-Online-Planning-for-Zero-Shot-Robot-Navigation

## Introduction
We present DualNav, a zero-shot mapless navigation framework that enables mobile robots to follow natural language instructions in unknown and unstructured environments, without relying on pre-existing maps. The system is built upon an asynchronous parallel architecture that integrates (i) vision-language perception and active memory, (ii) dual-frequency closed-loop control, (iii) orientation-constrained local planning, and (iv) timestamp-based state synchronization into a unified framework.

Asynchronous Dual-Stream Architecture
To resolve the frequency mismatch between slow semantic reasoning and fast reactive control, our approach decouples high-level perception and planning from low-level motion execution. We employ a timestamp-based state synchronization mechanism to compensate for foundation-model inference latency, eliminating stop-and-go behavior and ensuring smooth, continuous motion.

Active Visual-Semantic Memory & Goal-Aware Planning
Instead of relying on dense geometric maps, we construct a lightweight sparse semantic database through multi-strategy DBSCAN clustering and open-vocabulary detection, enabling robust target localization and zero-shot semantic queries. Furthermore, an explicit angle-maintenance cost is incorporated into the local planner to preserve continuous visual alignment with the target during obstacle avoidance.

Performance & Generalization
Extensive experiments in Gazebo and large-scale factory simulations demonstrate that DualNav achieves a navigation success rate of 86.7%, surpassing serial baselines by 33.4%, with a terminal positioning error of 0.091 m. The system also exhibits strong generalization in perceptually aliased and repetitive environments, validating its applicability in real-world unstructured settings.

## Installation
The project was tested with Ubuntu 20.04 and ROS Noetic. We assume that you have already installed ROS Noetic and CUDA (if using GPU acceleration for local perception, though this project relies heavily on API calls).

1. Create a ROS Workspace & Clone
Set up a catkin workspace and clone the repository into the source directory.

Bash

mkdir -p ~/mapless_nav_ws/src
cd ~/mapless_nav_ws/src
# Replace <your-repo-url> with your actual repository URL
git clone git@github.com:YourUsername/Mapless-Nav-VLM.git
2. Environment Setup
We recommend using Conda to manage Python dependencies to avoid conflicts with the system Python, but you must ensure rospkg is installed to interface with ROS.

Bash

conda create --name vlm_nav python=3.8
conda activate vlm_nav

# Install necessary Python libraries
# We utilize sklearn for DBSCAN clustering and OpenAI for VLM interaction
pip install numpy opencv-python matplotlib scikit-learn openai rospkg pyyaml
Note: If you encounter cv_bridge issues with Python 3 in ROS Noetic within Conda, you may need to build cv_bridge from source or use the system python (/usr/bin/python3) instead of Conda.

3. Install System Dependencies
Install the required ROS navigation and simulation packages used in the code (MoveBase, Gazebo, TF2).

Bash

sudo apt-get update
sudo apt-get install ros-noetic-navigation \
                     ros-noetic-move-base \
                     ros-noetic-gazebo-msgs \
                     ros-noetic-tf2-ros \
                     ros-noetic-cv-bridge \
                     ros-noetic-image-transport
4. Build and Configure
Make the Python scripts executable and build the workspace.

Bash

# Grant execution permissions to the scripts
cd ~/mapless_nav_ws/src/Mapless-Nav-VLM/src/
chmod +x *.py

# Build the workspace
cd ~/mapless_nav_ws
catkin_make
source devel/setup.bash
5. Configuration (API Key)
The system uses the Qwen-VL model via the DashScope (Aliyun) compatible OpenAI API. You must configure your API key.

Open test_ctrl.py.

Locate the initialization of the controller:

Python

self.api_key = "sk-..." # Replace this with your actual API Key
self.base_url = "https://dashscope.aliyuncs.com/compatible-mode/v1"
Ensure your robot (simulation or real) is publishing to the following topics defined in PerceptionModule and NavigationModule:

/camera/rgb/image_raw

/camera/depth/image_raw

/camera/depth/camera_info

/scan (Lidar)

/odom

/cmd_vel

## Experimental


