# DualNav-Online-Planning-for-Zero-Shot-Robot-Navigation

## Introduction
We present DualNav, a zero-shot mapless navigation framework that enables mobile robots to follow natural language instructions in unknown and unstructured environments, without relying on pre-existing maps. The system is built upon an asynchronous parallel architecture that integrates (i) vision-language perception and active memory, (ii) dual-frequency closed-loop control, (iii) orientation-constrained local planning, and (iv) timestamp-based state synchronization into a unified framework.

Asynchronous Dual-Stream Architecture
To resolve the frequency mismatch between slow semantic reasoning and fast reactive control, our approach decouples high-level perception and planning from low-level motion execution. We employ a timestamp-based state synchronization mechanism to compensate for foundation-model inference latency, eliminating stop-and-go behavior and ensuring smooth, continuous motion.

Active Visual-Semantic Memory & Goal-Aware Planning
Instead of relying on dense geometric maps, we construct a lightweight sparse semantic database through multi-strategy DBSCAN clustering and open-vocabulary detection, enabling robust target localization and zero-shot semantic queries. Furthermore, an explicit angle-maintenance cost is incorporated into the local planner to preserve continuous visual alignment with the target during obstacle avoidance.

Performance & Generalization
Extensive experiments in Gazebo and large-scale factory simulations demonstrate that DualNav achieves a navigation success rate of 86.7%, surpassing serial baselines by 33.4%, with a terminal positioning error of 0.091 m. The system also exhibits strong generalization in perceptually aliased and repetitive environments, validating its applicability in real-world unstructured settings.


