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
The project was tested with Ubuntu 20.04 and Jetson Orin/Xavier NX. We assume that you have already installed the necessary dependencies such as CUDA, ROS Noetic, and Conda.

**1. Clone the Code**
```bash
git clone --depth 1 git@github.com:Your-Repo/Mapless-Nav-VLM.git
```

**2. Create Virtual Environment**

I specified the libraries used in the provided scripts to ensure VLM and visual processing compatibility.
```bash
conda create --name mapless_nav python=3.8
conda activate mapless_nav
cd Mapless-Nav-VLM
# Install dependencies required by PerceptionModule and NavigationModule
pip install numpy opencv-python openai scikit-learn matplotlib rospkg
```

**3. Build Workspace**

Build the ROS workspace to ensure message types and packages are recognized.
```bash
conda deactivate
# Assuming the cloned repo is inside a catkin workspace src folder
cd ../.. 
catkin_make
source devel/setup.bash
```

## Test the System

You can test the navigation logic using the provided Python scripts. Note that the system relies on the Qwen-VL API for semantic understanding.

**1. Start the Simulator**

The controller supports `turtlebot3_waffle` and standard Gazebo environments. Ensure you have the simulation running before starting the controller.
```bash
# Terminal 1
source devel/setup.bash
export TURTLEBOT3_MODEL=waffle
roslaunch turtlebot3_gazebo turtlebot3_world.launch
```

**2. Configure API Key**

Before running the main controller, you must set your DashScope/OpenAI API key in `test_ctrl.py`.
```python
# Open test_ctrl.py and modify the following line:
self.api_key = "sk-your-actual-api-key-here" 
```

**3. Start the Main Controller**

This script initializes the `PerceptionModule` and `NavigationModule` and handles the VLM interaction loop.
```bash
# Terminal 2
cd Mapless-Nav-VLM/src
conda activate mapless_nav
# Grant execution permissions
chmod +x *.py
python test_ctrl.py
```

**4. Visualization**

Start RVIZ to visualize the global path planning, debug markers, and navigation goals.
```bash
# Terminal 3
# You may need to add the topics /local_global_debug_path and /local_global_debug_markers manually in RVIZ
rviz
```

The system will visualize the A* path and debug markers on the topics `/local_global_debug_path` and `/local_global_debug_markers`.

You can interact with the system via the terminal menu to trigger scanning or navigation tasks:
```text
************************************************************
Please select an operation:
  1. Execute navigation task
  2. Continue recording new targets
  3. View recorded targets
  4. Clear invalid targets (No 3D position)
  5. View statistics
  6. Exit
************************************************************
```
## Experimental


