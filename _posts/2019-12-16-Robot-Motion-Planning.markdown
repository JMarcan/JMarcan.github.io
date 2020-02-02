---
layout: post
title:  "Robot Motion Planning - Data Science Nanodegree Capstone Project"
date:   2019-12-16 17:03:22 +0100
categories: jekyll update
---
![Maze Headline Picture. Source: gettyimages.de](/assets/robot-motion-planning/maze_headline_picture.jpg)

This post is about my graduation project for Udacity Data Science Nanodegree.
I've chosen Robot Motion Planning following my interest in robotics and autonomous systems.

## Project description
The project solves [Micromouse](https://en.wikipedia.org/wiki/Micromouse) competition.

The project simulates a virtual robot navigating a virtual maze. (In contrast with the competition where is used physical robot in a maze).

## Problem Statement
The robot is tasked with finding a path from the lower-left corner of the maze to its center.
The maze has size n x n where n = {12, 14, 16}. In the first run the robot tries to map a maze to figure out the shortest possible path there. In the next run, the robot attempts to reach the center in the fastest time possible using what he previously learned about the maze.

To solve this problem I intend to map the whole maze with the robot in the first run. Tremaux Algorithm will be used for this.
Then when we have stored a maze map, we will calculate the shortest path to the destination. Dijkstra's algorithm will be used for this.
Lastly, in the last run robot will follow the shortest path.

## 1. Data Exploration
# 1.1. Robot interaction with the World
Robot gathers data about the World around him from three obstacle sensors. Mounted on the front of the robot, its right side, and its left side.
Obstacle sensors detect the number of open squares in the direction of the sensor; for example, in its starting position, the robot’s left and right sensors will state that there are no open squares in those directions and at least one square towards its front.

# 1.2. Maze structure
The robot is placed into a multiply-connected maze which means that simple algorithms as Wall Follower will be not able to handle it and we need to use a more sophisticated algorithm as Tremaux's algorithm.
The maze has size n x n where n = {12, 14, 16}. 
The maze is configured in a text file where each cell has a 4-bit value in range <0-15> representing the wall configuration of a given cell.
 - Each number represents a four-bit number that has a bit value of 0 if an edge is closed (walled) and 1 if an edge is open (no wall)
- The 1s register corresponds with the upwards-facing side, the 2s register the right side, the 4s register the bottom side, and the 8s register the left side
- For example, the number 10 means that a square is open on the left and right, with walls on top and bottom (0x1 + 1x2 + 0x4 + 1x8 = 10)

In the next chapter, you can see an example of a 12x12 maze in which the robot is placed. The solution is there marked by the green line and takes 31 steps.

## 2. Exploratory Visualisation
![Maze](/assets/robot-motion-planning/maze_1_solution.png)

## 3. Benchmark
The rules provide us with a budget of 1000 time steps for exploring the maze and 1000 time steps to find a path. Exceeding this is considered as failed run.

The jury in this competition calculating the final score is the script `run.py` which calculates score in the following way:
- Score = Number of steps to complete Run 2 + (Number of steps to complete Run 1)/30
- 30 is here a predefined constant to penalize 30x more final racing steps than exploratory steps. Thus if 30 next exploratory steps will save at least 1 final racing step, it's worth to invest them into further exploration.

In this project I use this as the benchmark.
To make benchmarking more user-friendly and self-explanatory, we could calculate what is our algorithm deviation in % in comparison with the best possible solution for a given maze.


## 4. Data Preprocessing
There is no data-preprocessing needed as maze specification is provided to us. As robot moves, `run.py` takes care of reading the maze definition and returning sensor values to the robot.

## 5. Free-Form Visualisation
Based on the rules for maze creation I've created my maze that I used for quick verification that my algorithm works properly. The maze contains loops that could confuse the algorithm without being too much branched.
The dimensions are 12x12, the starting point is in the left bottom corner and the destination is located in the center, marked in the picture below by yellow color.
![My Prototyping Maze](/assets/robot-motion-planning/my_prototyping_maze.png)

## 6. Implementation
# 6.1 Implementation
The main robot handling function is the function `next_move` receiving in each step input from sensors and deciding on the next move.

The robot location with its heading is updated with each step and logged into the file, so the robot movement in the maze can be later visualized.

I've implemented there 3 modes of operations. 1. `explore`, 2. `search`, 3. `race`.

# 6.1.1. The mode 1. explore
The robot starts in the mode 1. `explore` where the goal is to drive through the maze and create the robot's internal map.

For exploration and decision about the next step is used `Tremaux's algorithm`. I've chosen Tremaux's algorithm for the reason that it's guaranteed to find a solution in each maze and humans could use this algorithm in the maze too.
The algorithm works in the following way:
 1. If you reach a dead-end, turn around, start backtracking and mark the path as visited twice (=sign for the robot to don't go there again)
 2. Till you reach a junction, continue and mark the path as visited (as visited twice if backtracking = sign for robot to don't go there again)
 3. If you arrive at a junction that you have not visited yet, 
  if it has still unvisited paths choose an arbitrary unvisited path and follow it. If it was backtracking, set it to normal mode again. Otherwise, if there are no unvisited paths, threat it as a dead-end and execute step 1
 4. If you arrive at a junction that you have already visited, 
 if not backtracking threat it as a dead-end and start backtracking. Otherwise, if backtracking and if it has still unvisited paths choose an arbitrary unvisited path and follow it. Set it from backtracking to normal mode again.
 if there are no unvisited paths anymore, continue backtracking following the way the robot originally entered the junction.

The main difficulty during implementation was for me to properly handle backtracking. Originally I've used a simple method of returning to the previous node, but as the maze is not simple-connected, in the case when it required return through multiple nodes the algorithm got stuck. 
So I implemented, in addition, tracking the path by which the robot came, so it can follow during the backtracking the opposite direction to its origin.

Before I've tried the Wall follower algorithm, but this was not able to find its ways to the maze center as this maze is not simply connected.

# 6.1.2. The mode 2. search
When the robot finishes the exploration mode and maps the whole maze (which is in Tremaux's algorithm indicated by returning to initial position),
he enters into 2. `search` mode and calculates the shortest path by `Djiskra's algorithm` commonly used for this. 

List of actions the robot shall take to get to the destination by shortest path is stored in a variable.

# 6.1.3. The mode 3. race
After the shortest path is calculated, the robot can enter into 3. `race mode` where he follows the actions previously calculated to get into the destination by the shortest path.


# 6.2. Refinement
To optimize the final score the robot will receive, after having a working algorithm, I've enhanced racing mode so it fully utilizes the robot's ability to move by up to 3 fields in a given direction during one step, thus reducing total time it takes to robot to race into the destination.

## 7. Results
Using Tremaux's algorithm to create an internal map of the maze in the first run (Exploration) and Djiskra's algorithm to find the shortest path in the second run (Racing)
solved mazes with the following scores:

| Maze | Score | Run Time 1 - Exploration | Run Time 2 - Racing |
| test_maze_01 | 29.17 | 334 | 17 |
| test_maze_02 | 42.93 | 448 | 27 |
| test_maze_03 | 49.4 | 582 | 29 |

You can see that with increasing maze size it takes robot more steps to map the maze and to race to its center. Results are satisfying for me as the shortest path is followed in the racing mode.

## 8. Improvement
For robot moving the same speed forward as backward, we could extend the robot's view to sense in all 4 directions, so he can directly go backward without spending time by turning around.

The model was developed for solving virtual mazes,
however, to implement it for real physical robot we would need to do several adjustments:
- Core logic would stay the same, Treumax's Algorithm does not bring heavy load to embedded CPU or its memory
- Personally, I'd integrate Bluetooth module to transmit us robot steps into computer for further analysis and visualisation
- On sensor level we would need to filter noise from real signal when a sensor sees the obstacle
- For robot's movement we would need to take into consideration its circle of diameter

## 9. Reflection
The project was an interresting extension of the project I did in the past where I've build robotic car for simple connected mazes where I've implemented Wall follower algorithm without any further sophistication.
Here (although only in simulated environment) I had to deal in addition with multiple connected maze, implement more sophisticated algorithm as Tremaux's, create internal map of the maze and use Djiskra's algorithm to find the shortest path that can be then followed in the final race.

I'm interrested in next projects in this area. 

You can find the implemented solution in my [GitHub Repository](https://github.com/JMarcan/robot_motion_planning_maze)