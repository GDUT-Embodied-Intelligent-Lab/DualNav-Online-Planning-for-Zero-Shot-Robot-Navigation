# DualNav-Online-Planning-for-Zero-Shot-Robot-Navigation

Introduction:
We propose DualNav, a zero-shot mapless navigation framework for autonomous robot navigation in unknown and unstructured environments, which integrates (i) asynchronous foundation-model reasoning, (ii) active visual-semantic memory, and (iii) orientation-constrained local planning into a parallel dual-stream architecture.

Asynchronous Dual-Stream Architecture: To address the frequency mismatch between slow semantic reasoning and fast motion control, our approach decouples high-level perception and planning from low-level reactive control, employing a timestamp-based state synchronization mechanism to compensate for foundation-model inference latency, thereby eliminating stop-and-go behavior and ensuring continuous, smooth motion execution.

Active Visual-Semantic Memory & Goal-Aware Planning: Instead of relying on pre-built geometric maps, we maintain a lightweight sparse semantic database through multi-strategy DBSCAN clustering, enabling robust target localization and zero-shot semantic querying. Furthermore, we incorporate an explicit angle-maintenance cost into the local planner to preserve continuous visual alignment with the target during obstacle avoidance, bridging the gap between semantic intent and geometric execution.
