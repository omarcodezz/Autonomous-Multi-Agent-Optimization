# Autonomous-Multi-Agent-Optimization
# EcoCAR C++ Development Challenge

## Project Overview

This project was completed as part of the **EcoCAR Connected and Automated Vehicle (CAV) Development Challenge**. The objective was to develop an efficient C++ algorithm to coordinate a colony of autonomous agents (ants) navigating a grid-based environment to locate, collect, and return food to a central home location.

Each ant operates with limited energy, while movement costs vary depending on the terrain. The challenge required developing a strategy that balances exploration, resource allocation, and energy-efficient path planning to maximize the amount of food successfully retrieved.

My solution uses a multi-stage approach combining **energy-based task allocation, structured grid exploration, shortest-path calculations, greedy assignment optimization, and a relay-based retrieval strategy** for food that cannot be returned directly.

The project demonstrates practical applications of algorithm design, resource management, and autonomous multi-agent coordination using C++.
## Algorithm Design

The solution follows a multi-stage strategy designed to maximize food retrieval while accounting for limited agent energy and terrain-dependent movement costs.

### 1. Energy-Based Task Allocation

- Rank all ants by their available energy in descending order.
- Assign the higher-energy half to exploration and the remaining ants to collection roles.
- Prioritize agents with greater energy reserves for exploration across the map.

### 2. Structured Grid Exploration

- Divide the environment into scanning regions based on each ant's food detection radius.
- Calculate scanning coordinates to distribute exploration across the grid.
- Estimate the shortest-path energy cost from the home location to each scanning target.
- Prioritize lower-cost scanning locations and record discovered food coordinates while avoiding duplicates.

### 3. Energy-Aware Path Planning

Movement costs depend on the elevation difference between adjacent terrain cells:

**Movement Cost = 1 + |Elevation Difference|**

The algorithm uses the simulation's shortest-path utility to estimate the energy required for travel.

For each potential food retrieval, it calculates:

**Total Cost = Cost to Food + Cost to Home**

Only assignments where an ant has sufficient energy to complete the round trip are considered feasible.

### 4. Global Greedy Assignment

- Generate all feasible ant–food assignments across the colony.
- Sort assignments by total estimated energy cost in ascending order.
- Greedily select the lowest-cost assignments while ensuring each ant and food location is assigned at most once per cycle.
- Execute selected retrievals and return collected food to the home location.

This approach prioritizes energy-efficient retrievals without the computational complexity of finding a globally optimal assignment.

### 5. Relay-Based Food Retrieval

When no direct retrieval is feasible, the algorithm attempts a relay strategy:

- Identify ants capable of reaching food but lacking sufficient energy to return home.
- Evaluate relay candidates that can exhaust their remaining energy while transporting food toward home.
- Prioritize candidates capable of carrying food farther before running out of energy.
- Record the food's new location after it is dropped, allowing another ant to retrieve it in a subsequent cycle.

This extends the retrieval strategy beyond food that can be collected through a single round trip.

## Technologies Used

| Technology | Purpose |
|---|---|
| C++20 | Core algorithm implementation and simulation logic |
| CMake | Build configuration and project compilation |
| GCC / G++ | C++ compilation |
| Git & GitHub | Version control and project documentation |
| Visual Studio Code | Development and debugging |

## Building & Running

### Prerequisites

Ensure the following tools are installed:

- A C++20-compatible compiler (GCC or equivalent)
- CMake
- A compatible build tool (such as MinGW Make)

### 1. Clone the Repository

```bash
git clone https://github.com/YOUR_USERNAME/YOUR_REPOSITORY.git
cd YOUR_REPOSITORY
```

Replace the URL with your repository's actual GitHub URL.

### 2. Configure the Project

Create a build directory and configure the project using CMake:

```bash
mkdir build
cd build
cmake .. -G "MinGW Makefiles"
```

The example above uses MinGW Makefiles on Windows. Other platforms may require a different CMake generator.

### 3. Compile

```bash
cmake --build .
```

### 4. Run the Simulation

On Windows, execute the compiled simulation:

```bash
./dev_challenge.exe
```

The program initializes the simulation, executes the colony's foraging strategy, and reports the final score when the game ends.

### 5. Run Unit Tests

```bash
./antworld_tests.exe
```

The unit tests validate the functionality of the simulation framework.

## Reproducibility

The simulation uses a configurable random seed. By default, `main.cpp` initializes the environment using:

```cpp
const uint32_t SEED = 12345;
```

Keeping the same seed allows algorithm changes to be evaluated under consistent simulation conditions. Different seeds can be used to evaluate performance across different environments.

## Project Structure

The project is organized around a C++ simulation framework and a custom foraging algorithm.

```text
CAV-EIC-Y1-Dev-Challenge/
├── include/
│   └── antworld.h
├── src/
│   ├── antworld.cpp
│   ├── applicant_solution.cpp
│   ├── main.cpp
│   └── useable_functions.cpp
├── CMakeLists.txt
└── README.md
```

### Key Components

| File | Description |
|---|---|
| `applicant_solution.cpp` | Custom multi-agent coordination and optimization algorithm |
| `antworld.cpp` | Simulation initialization, scoring, and agent lifecycle management |
| `useable_functions.cpp` | Agent movement, food detection, and pheromone operations |
| `main.cpp` | Simulation entry point and execution loop |
| `antworld.h` | Core simulation classes, data structures, and function declarations |
| `CMakeLists.txt` | Build configuration |

*The structure above shows the principal project files; additional framework utilities and tests may be present in the complete repository.*

## My Contributions

My primary contribution was designing and implementing the foraging strategy in `applicant_solution.cpp`, using the provided EcoCAR simulation framework.

Key contributions include:

- **Multi-Agent Coordination:** Developed an energy-based task allocation strategy to coordinate exploration and food retrieval across multiple autonomous agents.
- **Exploration Optimization:** Implemented structured grid scanning with energy-cost prioritization to identify food locations efficiently.
- **Resource-Constrained Planning:** Integrated terrain-dependent shortest-path costs into feasibility checks for agent assignments.
- **Greedy Task Assignment:** Developed a colony-wide greedy selection algorithm to prioritize low-cost food retrievals while preventing duplicate assignments.
- **Relay-Based Retrieval:** Implemented a fallback strategy that moves food closer to home when a direct return trip is infeasible.
- **Iterative Optimization:** Refined the algorithm through repeated simulation testing, debugging, and performance evaluation.

### Framework Attribution

The underlying simulation environment, including agent movement, terrain generation, food detection, scoring, and pathfinding utilities, was provided as part of the EcoCAR Development Challenge.

My implementation focuses on the decision-making logic that coordinates these existing components to improve food collection efficiency.
