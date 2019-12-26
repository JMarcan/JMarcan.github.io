---
layout: post
title:  "Robot Motion Planning - Data Science Nanodegree Capstone Project"
date:   2019-12-16 17:03:22 +0100
categories: jekyll update
---
![Maze Headline Picture. Source: gettyimages.de](/assets/robot-motion-planning/maze_headline_picture.jpg)

This post is about my graduation project for Udacity Data Science Nanodegree.
I've choosen Robot Motion Planning following my interrest in robotics and autonomous systems.

## Project description
The project solves [Micromouse](https://en.wikipedia.org/wiki/Micromouse) competition.

The project simulates a virtual robot navigating a virtual maze. (In contrast with the competition where is used physical robot in a maze).

## Problem Statement
The robot is tasked with finding a path from the lower left corner of the maze to its center.
The maze has size n x n where n = {12, 14, 16}. In the first run the robot tries to map a maze to figure out the shortest possible path there. In the next run, the robot attempts to reach the center in the fastest time possible using what he previously learned about the maze.

## 1. Data Exploration
# 1.1. Robot interaction with the World
Robot gathers data about the World around him in form of three obstacle sensors. Mounted on the front of the robot, its right side, and its left side.
Obstacle sensors detect the number of open squares in the direction of the sensor; for example, in its starting position, the robot’s left and right sensors will state that there are no open squares in those directions and at least one square towards its front.

# 1.2. Maze structure
The robot is placed into multiply connected maze which means that simple algorithms as Wall Follower will be not able to handle it and we need to use something more sophisticated as Tremaux's algorithm.
The maze has size n x n where n = {12, 14, 16}. 
The maze is configured in a text file where each cell has 4-bit value in range <0-15> representing wall configuration of a given cell.
 - Each number represents a four-bit number that has a bit value of 0 if an edge is closed (walled) and 1 if an edge is open (no wall)
- The 1s register corresponds with the upwards-facing side, the 2s register the right side, the 4s register the bottom side, and the 8s register the left side
- For example, the number 10 means that a square is open on the left and right, with walls on top and bottom (0x1 + 1x2 + 0x4 + 1x8 = 10)

In the next chapter you can see example of 12x12 maze in which the robot is placed. The solution is there marked by the green line and takes 31 steps.

## 2. Exploratory Visualisation
![Maze](/assets/robot-motion-planning/maze_1_solution.png)

## 3. Benchmark
The rules provide us with budget of 1000 time steps for exploring the maze and 1000 time steps to find a path. Exceeding this is considered as failed run.

The jury in this competition calculating the final score is the script `run.py` which calculates score in the following way:
- Score = Number of steps to complete Run 2 + (Number of steps to complete Run 1)/30

In this project I use this as the benchmark. 
To make bechmarking more user friendly and self-explanatory, we could calculate what is our algorithm deviation in % in comparison with the best possible solution for a given maze.


## 4. Data Preprocessing
There is no data-preprocessing needed as maze specification is provided to us. As robot moves, `run.py` takes care about reading the maze definition and returning sensor values to the robot.

## 5. Free-Form Visualisation
Based on the rules for maze creation I've created my own maze that I used for quick verification that my algorithm works properly. The maze contains loops that could confuse algorithm without beeing too much branched.
The dimensions are 12x12, the starting point is in the left bottom corner and the destination is located in the center, marked in the picture below by yellow color.
![My Prototyping Maze](/assets/robot-motion-planning/my_prototyping_maze.png)

## Results
Algorithm solved mazes with the following scores:

| Maze | Score | Run Time 1 - Exploration | Run Time 2 - Racing |
| test_maze_01 | 29.17 | 334 | 17 |
| test_maze_02 | 42.93 | 448 | 27 |
| test_maze_03 | 49.4 | 582 | 29 |

## 6. Improvement
For robot moving the same speed forward as backward, we could extend robot's view to sense in all 4 directions, so he can directly go backward without spending time by turning around.

The model was developed for solving virtual mazes,
however, to implement it for real physical robot we would need to do several adjustments:
- Core logic would stay the same, Treumax's Algorithm does not bring heavy load to embedded CPU or its memory
- Personally, I'd integrate Bluetooth module to transmit us robot steps into computer for further analysis and visualisation
- On sensor level we would need to filter noise from real signal when a sensor sees the obstacle
- For robot's movement we would need to take into consideration its circle of diameter

You can find the implemented algorithm in my [GitHub Repository](https://github.com/JMarcan/robot_motion_planning_maze)