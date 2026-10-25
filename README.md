
# A* Robot Pathfinding

A C++17 implementation of the A* search algorithm for pathfinding in weighted, two-dimensional grid environments.

The project simulates a robot navigating from a starting position to a destination while avoiding obstacles and minimizing the accumulated traversal cost.

It was developed to explore heuristic search, graph traversal, path reconstruction, and object-oriented software design.

## Features

- Weighted 2D grid environments loaded from text files.
- Four-directional movement (up, down, left, right).
- Obstacle and boundary detection.
- A* search using a Manhattan-distance heuristic.
- Open and closed node management.
- Cost-based updates and reopening of previously explored nodes.
- Path reconstruction using parent references.
- Search trace and solution export to text files.

## Getting Started

### Requirements

- C++17-compatible compiler (GCC recommended)
- GNU Make

### Installation

Clone the repository:

```bash
git clone https://github.com/PabloMoredaCE/A-star-search.git
cd A-star-search/src
make
```

### Usage

Run the executable with an input environment and two output filenames:

```bash
./astar ../tests/entorno1-eval.txt search_output.txt path_output.txt
```

The program:

1. Loads the environment from the input file.
2. Identifies the starting position and destination.
3. Executes the A* search.
4. Prints the search process to the terminal.
5. Saves the search trace and final path to separate files.

## Environment Representation

The environment is defined as a rectangular grid of space-separated integers.

| Value | Meaning |
|---|---|
| `0` | Starting position |
| `10` | Destination |
| `-1` | Obstacle |
| Other values | Traversal cost |

The cost of moving to a neighboring cell is determined by the value of that cell. Entering the destination has a fixed cost of `2`.

The robot can move horizontally or vertically, provided that the target position is inside the grid and is not an obstacle.

## A* Search Algorithm

A* is an informed search algorithm that combines the accumulated path cost with an estimate of the remaining cost.

Each node is evaluated using:

\[
f(n) = g(n) + h(n)
\]

Where:

- `g(n)` is the accumulated traversal cost from the starting position.
- `h(n)` estimates the remaining cost to the destination.
- `f(n)` estimates the total cost of a path through node `n`.

### Heuristic Function

This implementation uses a scaled Manhattan-distance heuristic:

\[
h(n) = 2(|r_g-r_n| + |c_g-c_n|)
\]

The multiplier reflects the minimum movement cost of the provided test environments.

For optimality guarantees, each movement must cost at least `2`. Using lower-cost cells could make this heuristic overestimate the remaining cost.

### Search Process

1. Insert the starting node into the open list.
2. Select the open node with the lowest evaluation value.
3. Move the selected node to the closed list.
4. If the node represents the destination, reconstruct the path.
5. Otherwise, generate its valid neighboring positions.
6. Update the corresponding node when a lower accumulated cost is discovered.
7. Repeat until the destination is reached or no open nodes remain.

Closed nodes can be reopened when a lower-cost path to the same position is found.

### Path Reconstruction

Each generated node stores its predecessor's position.

Once the destination is reached, the algorithm follows these parent references back to the starting position and reverses the resulting sequence.

The final output includes the path coordinates and accumulated traversal cost.

## Project Architecture

```text
A-star-search/
├── README.md
├── src/
│   ├── Makefile
│   ├── main.cpp
│   ├── environment.h / .cpp
│   ├── robot.h / .cpp
│   ├── node.h
│   └── search.h / .cpp
└── tests/
    ├── entorno1-eval.txt
    └── entorno2-eval.txt
```

### Main Components

| Component | Responsibility |
|---|---|
| `Environment` | Stores the grid, provides cell access, and renders the resulting path. |
| `Robot` | Manages the robot's position and validates movement. |
| `Node` | Stores a search state, traversal cost, accumulated cost, and parent position. |
| `Search` | Implements node expansion, cost evaluation, open/closed lists, and path reconstruction. |
| `main` | Handles input files, initializes the search, and manages output. |

## Implementation Details

The open and closed lists are implemented using `std::vector<Node>`.

The algorithm selects the next node by performing a linear scan of the open list to find the minimum evaluation value.

It also performs linear searches when checking whether a generated position is already present in either list.

This approach prioritizes simplicity and visibility of the algorithm's internal behavior over performance on large environments.

Potential optimizations include priority queues and hash-based position lookup.

## Test Environments

Two example environments are available in the `tests/` directory:

- `entorno1-eval.txt`
- `entorno2-eval.txt`

These provide weighted grids with obstacles and designated start and goal positions.

They can be used to inspect the selected path, accumulated cost, and node exploration process.

The current repository includes example input environments rather than an automated unit-testing framework.

## Limitations and Future Improvements

- Validate malformed and incomplete input environments.
- Add automated correctness tests.
- Introduce configurable heuristic functions.
- Improve open-list selection using a priority queue.
- Optimize repeated node lookup.
- Benchmark execution time and explored-node counts across different environments.

## Technologies

- C++17
- Standard Template Library (STL)
- GNU Make

## Author

Pablo Moreda Vega

Computer Engineering student at Universidad de La Laguna.
