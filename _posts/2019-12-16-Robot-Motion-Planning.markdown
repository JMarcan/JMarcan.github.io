---
layout: post
title:  "Robot Motion Planning - Data Science Nanodegree Capstone Project"
date:   2019-12-16 17:03:22 +0100
categories: jekyll update
---
#The article is in the progress with the project and will be finished by December 2019

![Maze](/assets/robot-motion-planning/maze_1.png)

This post is about my graduation project for Udacity Data Science Nanodegree.
I've choosen Robot Motion Planning following my interrest in robotics and autonomous systems.

# Project description
The project solves [Micromouse](https://en.wikipedia.org/wiki/Micromouse) competition.

The project simulates a virtual robot navigating a virtual maze (in contracts with the physical robot in a maze used in the competition).

## Problem Statement

## Metrics

## 1. Data Exploration
Robot gathers data about the World around him in form of three obstacle sensors. Mounted on the front of the robot, its right side, and its left side.
Obstacle sensors detect the number of open squares in the direction of the sensor; for example, in its starting position, the robot’s left and right sensors.will state that there are no open squares in those directions and at least one square towards its front.



[Maze](/assets/robot-motion-planning/maze_1.png)
In the picture above you can example maze in which is robot placed in.
You can see the maze is multiply connected, which means that simple algorithms as Wall Follower will be not able to handle it.

## 2. Exploratory Visualisation

## 3. Benchmark

## 4. Data Preprocessing

## 5. Free-Form Visualisation

## 6. Improvement

## Reflection


You can find the implemented algorithm in my [GitHub Repository](https://github.com/JMarcan/robot_motion_planning_maze)