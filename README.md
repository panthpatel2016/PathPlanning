# What?

A ROS 2 path-planning and control demo for turtlesim that builds a Probabilistic Roadmap, finds a shortest path with Dijkstra’s algorithm, and follows it using a Pure Pursuit controller. Obstacles are axis-aligned boxes, while collisions are checked with a simple line-segment test. Runs fully in `rclpy` with turtles penning the obstacle boxes and flags. [ROS Documentation](https://docs.ros.org/en/foxy/Tutorials/Beginner-CLI-Tools/Introducing-Turtlesim/Introducing-Turtlesim.html)

# How?

- **Sampling & Graph Creation**: Randomly sample free points (nodes) in the grid. Connect each node to its $k$ nearest neighbors when the straight line is collision-free. Edge weight is Euclidean distance given below. This is a classic [Probabilistic Roadmap](https://www.cs.cmu.edu/~motionplanning/papers/sbp_papers/PRM/prmbasic_01.pdf) idea to build a roadmap in free space and query it for a path.

$$
 \space d(a,b)=\sqrt{(ax−bx)^2+(ay−by)^2}
$$

- **Shortest Path Finding:** Run [Dijkstra’s algorithm](https://en.wikipedia.org/wiki/Dijkstra%27s_algorithm) on the roadmap from start to goal using a **min-heap priority queue**, maintaining `dist[v]` and `prev[v]` until the goal is popped.

- **Collision Checks**: A path segment $p \rightarrow q$ intersects a rectangle if an endpoint is inside or it intersects any of the 4 edges. Segment-segment intersection uses the orientation test:
    
    Two segments intersect if the orientations of each w.r.t the other straddle $(o_1o_2 < 0 \space and \space o_3o_4 < 0)$. 
    
- **Pure Pursuit - Path Following:** Choose a lookahead distance $L_d$ along the line; shrink $L_d$ if the line from the robot to that point would cross any obstacle.
    
    Express the lookahead point in the robot frame by rotating by $-\theta$:
    
    $$
    x_ld = \cos(-\theta)\Delta x - \sin(-\theta)\Delta y,
    
    \newline
    y_ld = \sin(-\theta)\Delta x + \cos(-\theta)\Delta y.
    $$
    
    Curvature command (curvature of the circular arc that would pass through the turtle and the lookahead point) from the geometry is found to be the following: 
    
    $$
    \kappa = \frac{2 y_ld}{L_d^2}
    $$
    
    These commands get translated into steering commands for the turtle
    
- **ROS2/ Turtlesim:** Subscribes to `/turtle1/pose`, publishes to `/turtle1/cmd_vel`, and uses `Spawn`, `SetPen`, `TeleportAbsolute` services to draw obstacles/flags and position the turtle.
