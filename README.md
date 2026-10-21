# Autonomous Multi-Agent Optimization

**EcoCAR C++ Development Challenge**

An energy-aware multi-agent coordination algorithm for autonomous exploration and resource retrieval in a terrain-constrained simulation environment.

## Project Overview

This project was developed as part of the **EcoCAR Connected and Automated Vehicle (CAV) Development Challenge**. The objective was to implement an efficient C++ algorithm to coordinate a colony of autonomous agents (ants) navigating a grid-based environment to locate, collect, and return food to a central home location.

Each agent operates with limited energy, and movement costs vary with terrain elevation. The challenge required balancing exploration, resource allocation, and energy-efficient path planning to maximize successful food retrieval.

My solution combines **energy-based task allocation, structured grid exploration, shortest-path cost estimation, global greedy assignment, and relay-based retrieval**.

## Performance & Validation

| Metric | Result |
|---|---|
| Best recorded simulation score | 38 |
| Framework unit tests | 40/40 passed |
| Programming language | C++ |
| Default simulation seed | 12345 |
| Maximum simulation steps | 1,000 |

The solution was refined through iterative simulation testing, debugging, and algorithm optimization.

The results above reflect previously recorded development runs and have not been independently reproduced for this publication.

## Algorithm Design

### 1. Energy-Based Task Allocation

- Rank agents by available energy in descending order.
- Assign the higher-energy half of the colony to initial exploration, with the remaining agents designated as collectors.
- Prioritize higher-energy agents for exploration to improve their ability to reach scanning locations.

Following initial exploration, the retrieval algorithm evaluates available agents based on their energy and ability to complete food collection tasks.

### 2. Structured Grid Exploration

- Divide the terrain into scanning regions based on the agents' food detection radius.
- Generate scanning coordinates distributed across the environment.
- Estimate shortest-path travel costs from home to each scanning location.
- Prioritize lower-cost scanning targets to reduce exploration energy expenditure.
- Record discovered food coordinates while preventing duplicate entries.

### 3. Energy-Aware Path Planning

Movement energy depends on the elevation difference between adjacent terrain cells:

**Movement Cost = 1 + |Elevation Difference|**

The algorithm uses the simulation framework's shortest-path and path-cost utilities to estimate energy requirements.

For each candidate retrieval:

**Total Cost = Cost to Food + Cost to Home**

Only assignments where an agent has sufficient energy to complete the estimated round trip are considered feasible.

### 4. Global Greedy Assignment

- Generate feasible agent–food assignments across the colony.
- Sort candidate assignments by estimated total energy cost.
- Select the lowest-cost available assignments while preventing duplicate agent or food assignments within a cycle.
- Execute selected retrievals and return collected food to the home location.

This greedy strategy favors low-cost retrievals without requiring the computational complexity of globally optimal matching.

### 5. Relay-Based Food Retrieval

When no direct retrieval assignment is feasible, the algorithm evaluates a relay-based fallback strategy.

- Identify agents with sufficient energy to reach a food location but insufficient energy for a complete return trip.
- Verify that an agent can exhaust its remaining energy exactly along the return path.
- Prioritize eligible relay candidates with greater remaining energy after reaching the food.
- Move the food toward home until the relay agent exhausts its energy.
- Record the food's new location after the simulation drops it, allowing subsequent retrieval attempts.

This strategy extends the range of food that can potentially be recovered while avoiding relay assignments that would leave an agent stranded with unused energy.

## Technologies Used

| Technology | Purpose |
|---|---|
| C++ | Algorithm implementation and simulation integration |
| CMake | Build configuration and compilation |
| GCC / G++ | C++ compiler toolchain |
| Git & GitHub | Version control and documentation |
| Visual Studio Code | Development and debugging |

## Building & Running

### Prerequisites

- CMake 3.20 or newer
- A C++20-compatible compiler
- A compatible build system

### 1. Clone the Repository

```bash
git clone https://github.com/YOUR_USERNAME/YOUR_REPOSITORY.git
cd YOUR_REPOSITORY
```

Replace the URL with the actual repository URL.

### 2. Configure the Project

```bash
cmake -S . -B build
```

CMake selects an available generator by default. Windows users working with MinGW can explicitly select the `MinGW Makefiles` generator if desired.

### 3. Compile

```bash
cmake --build build
```

### 4. Run the Simulation

On Windows using a single-configuration generator such as MinGW Makefiles:

```bash
./build/dev_challenge.exe
```

On Linux or macOS:

```bash
./build/dev_challenge
```

For multi-configuration generators, the executable may be located in a configuration-specific directory such as `build/Debug/`.

The simulation executes the foraging algorithm and reports the final score when the game terminates.

### 5. Run Tests

```bash
ctest --test-dir build --output-on-failure
```

The CMake configuration registers the framework test executable with CTest when testing is enabled.

## Reproducibility

The simulation uses a configurable random seed. By default, `main.cpp` initializes the environment with:

```cpp
const uint32_t SEED = 12345;
```

Using a consistent seed allows algorithm changes to be compared under the same generated environment.

The simulation also enforces a maximum of 1,000 execution steps.

To evaluate different environments, the seed can be modified or randomized.

## Project Structure

```text
CAV-EIC-Y1-Dev-Challenge/
├── include/
│   └── antworld.h
├── src/
│   ├── antworld.cpp
│   ├── applicant_solution.cpp
│   ├── main.cpp
│   └── useable_functions.cpp
├── test/
│   └── antworld_tests.cpp
├── CMakeLists.txt
└── README.md
```

Additional challenge documentation and configuration files may be included in the repository.

### Key Components

| File | Description |
|---|---|
| `applicant_solution.cpp` | Custom multi-agent coordination and optimization algorithm |
| `antworld.cpp` | Simulation framework and world management |
| `useable_functions.cpp` | Agent operations and simulation utilities |
| `main.cpp` | Simulation entry point |
| `antworld.h` | Core classes, data structures, and declarations |
| `antworld_tests.cpp` | Framework tests |
| `CMakeLists.txt` | Build and testing configuration |

## My Contributions

My primary contribution was designing and implementing the colony's decision-making and resource-retrieval strategy in `applicant_solution.cpp`.

Key contributions include:

- **Multi-Agent Coordination:** Implemented energy-based role allocation for initial exploration and colony-wide retrieval planning.
- **Exploration Optimization:** Developed structured scanning targets prioritized by estimated energy cost.
- **Resource-Constrained Planning:** Incorporated terrain-aware shortest-path costs into assignment feasibility checks.
- **Greedy Task Assignment:** Implemented cost-based agent–food matching while preventing duplicate assignments.
- **Relay-Based Retrieval:** Designed a fallback strategy for moving otherwise unreachable food closer to home.
- **Iterative Optimization:** Refined the solution through simulation testing, debugging, and performance evaluation.

### Framework Attribution

The underlying simulation framework, including terrain generation, agent movement, food detection, scoring, and pathfinding utilities, was provided as part of the EcoCAR Development Challenge.

My implementation focuses on the autonomous decision-making algorithm that coordinates these components to improve resource collection efficiency.

## Key Takeaways

This project strengthened my understanding of multi-agent coordination, resource-constrained optimization, and practical algorithm design.

It also demonstrated the importance of balancing computational efficiency against solution quality, designing fallback strategies for constrained environments, and validating algorithms through reproducible simulation testing.
