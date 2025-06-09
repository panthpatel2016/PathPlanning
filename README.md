# PathPlanning
A Robotics Path Planning Simulator - Made by Panth, Nauman, Abheek and Rajat

# I. Core Functional Requirements (What the path planner MUST do):

Goal Specification:

Start Point: The path planner must accept a defined starting position (e.g., coordinates, node in a graph).
End Point: The path planner must accept a defined target position or region.

Static Obstacles: Identify and avoid fixed obstacles in the environment.
Dynamic Obstacles: Handle moving obstacles (e.g., other robots, people, vehicles). This introduces requirements for prediction and reactive planning.

Feasibility: Generate a path that is kinematically and dynamically feasible for the robot/agent. This means considering its turning radius, maximum velocity, acceleration limits, etc.
Collision-Free: The generated path must not intersect with any obstacles.
Completeness: The planner should find a path if one exists.
Smoothness: Generate a path that is continuous and differentiable to allow for smooth robot motion.
Connectivity: The path must be a continuous sequence of traversable points/states from start to goal.
Environment Representation:

Map Input: The planner must be able to ingest and interpret information about the environment (e.g., occupancy grid, point cloud, graph, CAD model).
State Space Definition: Clearly define the possible states of the robot/agent (e.g., (x, y), (x, y, theta), (x, y, theta, velocity)).

Path Representation: Output the planned path in a usable format (e.g., sequence of waypoints, control commands, trajectory).
Path Metadata: Provide information about the path (e.g., length, estimated time, cost).
Failure Notification: Clearly indicate if no path can be found.


# II. Performance Requirements (How well the path planner performs):

Computational Efficiency (Time Complexity):

Planning Time: The time taken to compute a path should be within acceptable limits for the application (e.g., real-time for dynamic environments, offline for static environments).
Scalability: Performance should degrade gracefully as the environment size or complexity increases.

Path Length: Minimize the total distance traveled.
Time to Traverse: Minimize the estimated time to reach the goal.
Energy Consumption: Minimize energy expended.
Smoothness/Jerk: Minimize changes in acceleration to reduce wear and tear or improve comfort.
Safety Margin: Maximize distance from obstacles.
Handling Sensor Noise/Uncertainty: Ability to function effectively despite imperfect sensor data.
Environmental Changes: Adapt to minor changes in the environment (e.g., new small obstacles).
Failure Recovery: How the system behaves if it deviates from the planned path.
Memory Usage: The amount of memory required to store the map, search tree, and path should be manageable for the target hardware.

# III. Non-Functional Requirements (Constraints and qualities of the system):

Maintainability:

Code Structure: Well-organized, modular, and easy to understand code.

API Design: Intuitive and well-defined interfaces for integration with other systems.
Configuration: Easy to configure parameters (e.g., cost weights, resolution).
Scalability (in terms of environment/robot complexity): Can the planner handle larger maps, more dimensions (e.g., 3D), or more complex robot kinematics?
Portability:Ability to run on different operating systems or hardware platforms.
Real-Time Capabilities:Guaranteed response times and execution deadlines.