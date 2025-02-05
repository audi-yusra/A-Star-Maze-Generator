# Pathfinding and Maze Generation using A* Algorithm

This project demonstrates the implementation of the A* pathfinding algorithm along with a maze generation algorithm using recursive backtracking. The visualization is done using the `pygame` library in Python.

## Table of Contents
1. [Introduction](#introduction)
2. [Features](#features)
3. [Requirements](#requirements)
4. [Installation](#installation)
5. [Usage](#usage)
6. [Code Explanation](#code-explanation)
7. [References](References)
8. [Future Enhancements](#FutureEnhancements)

## Introduction

The A* algorithm is a widely used pathfinding algorithm that finds the shortest path between two points in a grid. This project combines the A* algorithm with a maze generation algorithm to create a visual representation of how the algorithm works.

## Features

- **Maze Generation**: Generates a random maze using the recursive backtracking algorithm.
- **Pathfinding**: Uses the A* algorithm to find the shortest path from the start to the endpoint.
- **Interactive Interface**: Allows users to set start and end points, generate new mazes, and visualize the pathfinding process.

## Requirements

- Python 3.x
- Pygame library

## Installation

1. Clone the repository:
   ```bash
   git clone https://github.com/your-username/pathfinding-maze-generation.git
   cd pathfinding-maze-generation
   ```

2. Install the required dependencies:
   ```bash
   pip install pygame
   ```

## Usage

1. Run the script:
   ```bash
   python pathfinding_maze.py
   ```

2. Use the mouse to set the start and end points:
   - **Left Click**: Set start (yellow) and end (green) points.
   - **Right Click**: Remove start or endpoints.

3. Use the keyboard to control the simulation:
   - **Space**: Start the A* algorithm to find the path.
   - **C**: Clear the grid and generate a new maze.

## Code Explanation

### Cell Class

The `Cell` class represents each cell in the grid. It contains methods to manage the state of the cell, such as whether it is a barrier, start, end, or part of the path.

```python
class Cell:
    def __init__(self, row, col, SIZE, total_rows):
        self.row = row
        self.col = col
        self.total_rows = total_rows
        self.SIZE = SIZE
        self.color = WHITE
        self.f_score = float("inf")
        self.g_score = float("inf")
        self.camefrom = None
        self.neighbors = []
        self.visited = False
```

### Maze Generation

The maze is generated using the recursive backtracking algorithm. The `make_maze` function initializes the grid and uses a stack to keep track of visited cells.

```python
def make_maze(WIDTH, gap, WIN):
    rows = WIDTH // gap
    grid = [[Cell(i, j, gap, rows) for j in range(rows)] for i in range(rows)]
    stack = []
    current = grid[1][1]
    stack.append(current)
    while stack:
        neighbors = current.check_neighbors(grid)
        if neighbors:
            choosen_one = random.choice(neighbors)
            choosen_one.make_visited()
            grid = remove_wall(current, choosen_one, grid)
            current.make_visited()
            current = choosen_one
            if choosen_one not in stack:
                stack.append(choosen_one)
        else:
            current.make_visited()
            current = stack[-1]
            stack.remove(stack[-1])
    return grid
```

### A* Algorithm

The A* algorithm is implemented in the `A_star` function. It uses a heuristic function to estimate the cost from the current cell to the end cell and finds the shortest path.

```python
def A_star(draw, grid, start, end):
    open_set = [start]
    while open_set:
        current = lowest_node(open_set)
        if current == end:
            node = current.camefrom
            while node != start and node:
                node.make_path()
                node = node.camefrom
                draw()
            return
        open_set.remove(current)
        current.make_closed()
        for neighbor in current.neighbors:
            temp_g_score = current.g_score + 1
            if temp_g_score < neighbor.g_score:
                neighbor.camefrom = current
                neighbor.g_score = temp_g_score
                neighbor.f_score = temp_g_score + h(end.get_pos(), neighbor.get_pos())
                if not neighbor in open_set:
                    open_set.append(neighbor)
                    neighbor.make_open()
        start.make_start()
        end.make_end()
        draw()
    print("no solution")
```

##References

#**Wikipedia**: A* Search Algorithm

#Maze Generation Algorithms

#Pygame Documentation

##FutureEnhancements

- Adding diagonal movement.

- Implementing other pathfinding algorithms (Dijkstra, BFS, DFS).

- Improving UI with additional settings.
