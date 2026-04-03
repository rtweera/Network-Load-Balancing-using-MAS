# Network Load Balancing using MAS

This project demonstrates **network load balancing with a Multi-Agent System (MAS)** using:

- **Mesa** for agent-based simulation
- **Pygame** for interactive visualization

The system models users and servers as autonomous agents that communicate, negotiate, and rebalance load in a decentralized way.

## Table of Contents

- [Overview](#overview)
- [Key Features](#key-features)
- [How the Simulation Works](#how-the-simulation-works)
- [Project Structure](#project-structure)
- [Requirements](#requirements)
- [Installation](#installation)
- [Running the Simulation](#running-the-simulation)
- [UI Controls](#ui-controls)
- [Configuration Parameters](#configuration-parameters)
- [Runtime Output](#runtime-output)
- [Validation](#validation)
- [Documentation](#documentation)
- [Contributing](#contributing)
- [License](#license)

## Overview

Traditional load balancing is often centralized. This project explores a decentralized alternative where:

- users request service from server agents,
- server agents accept, reject, or redirect load,
- underutilized servers request users from peers,
- severely underutilized servers can redistribute users and terminate.

This creates adaptive behavior in dynamic network conditions (changing traffic and available servers).

## Key Features

- Agent-based load balancing simulation
- Dynamic user lifecycle (spawn, connect, disconnect, die, respawn)
- Dynamic server lifecycle (spawn, negotiate, redistribute, terminate)
- Server-to-server load transfer logic
- Interactive visualization with network topology and message log
- Butterfly-effect trigger to introduce small disturbances and observe cascading effects

## How the Simulation Works

### Agent Types

1. **UserAgent**
   - Requests a server connection
   - Maintains connection state
   - Disconnects/retries when needed
   - Dies after a random lifespan and is removed

2. **ServerAgent**
   - Accepts user connections up to capacity
   - Delegates to other servers when full
   - Requests users from other servers when underutilized
   - Redistributes users and terminates when severely underutilized

3. **LoadBalancerModel**
   - Maintains global state
   - Spawns initial and dynamic users/servers
   - Uses a custom scheduler to step users first, then servers
   - Collects per-step summary metrics

### Step Flow (High Level)

For each step, the model:

1. Cleans user references
2. Maintains user population limits
3. Steps all user agents
4. Steps all server agents
5. Collects summary statistics
6. Prints step results

## Project Structure

```text
Network-Load-Balancing-using-MAS/
├── README.md
├── pyproject.toml
├── poetry.lock
├── docs/
│   └── Network-load-balancing-using-MAS.pdf
└── src/
    ├── model.py          # User, Server, and Model logic
    ├── visualization.py  # Pygame visualization layer
    └── run.py            # Simulation entry point and UI controls
```

## Requirements

- Python **3.12+**
- [Poetry](https://python-poetry.org/)

Core dependencies (managed in `pyproject.toml`):

- `mesa==1.2.0`
- `pygame>=2.6.1,<3.0.0`

## Installation

From the repository root:

```bash
poetry install
```

## Running the Simulation

From the repository root:

```bash
poetry run python src/run.py
```

Alternative (inside Poetry shell):

```bash
poetry shell
python src/run.py
```

## UI Controls

The simulation window includes these buttons:

- **Start**: run continuous simulation
- **Pause**: pause simulation
- **Step**: execute one step (when paused)
- **Restart**: recreate model with initial parameters
- **Butterfly**: trigger a small random perturbation (disconnect one user from a random server)

## Configuration Parameters

Current model defaults used by `src/run.py`:

- `min_users=2`
- `max_users=15`
- `initial_users=12`
- `initial_servers=3`
- `max_server_capacity=4`
- `user_spawn_chance=0.5`

Additional model parameters available in `LoadBalancerModel`:

- `server_failure_chance`
- `server_up_chance`

You can modify parameter values in `src/run.py` where `LoadBalancerModel(...)` is created.

## Runtime Output

The simulation provides:

- Visual network state (users, servers, connections)
- On-screen event log (communication/collaboration/transfer/butterfly messages)
- Console step summaries:
  - total users
  - per-server allocation
  - users spawned/died
  - servers spawned/died

## Validation

A basic source validation command:

```bash
python -m compileall src
```

## Documentation

Detailed supporting documentation is available in:

- `docs/Network-load-balancing-using-MAS.pdf`

## Contributing

Contributions are welcome.

1. Open an issue to discuss significant changes.
2. Fork the repository and create a feature branch.
3. Make focused changes and test locally.
4. Open a pull request with a clear description.

## License

[MIT](https://mit-license.org/)
