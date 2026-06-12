# Asteroids (Boot.dev OOP Python Project)

This repository contains an Asteroids-style game built as part of the
Boot.dev Object-Oriented Programming curriculum for backend development with
Python.

The project uses `pygame` and emphasizes OOP fundamentals such as inheritance,
composition, and object lifecycle management through sprite groups.

## What This Project Demonstrates

- OOP design with shared behavior via a base class (`CircleShape`)
- Sprite-based architecture using `pygame.sprite.Sprite` and `Group`
- A fixed-step game loop pattern (`dt` from `clock.tick(60)`)
- Collision detection between player, asteroids, and projectiles
- Object splitting behavior (large asteroid -> two smaller asteroids)
- Lightweight telemetry via JSONL logs (`game_state.jsonl`, `game_events.jsonl`)

## Tech Stack

- Python `>=3.13`
- `pygame==2.6.1`

Dependency metadata lives in `pyproject.toml`.

## Project Structure

- `main.py`: Program entry point and game loop
- `constants.py`: Tunable gameplay constants
- `circleshape.py`: Base class for circular game entities
- `player.py`: Player movement, rotation, drawing, and shooting
- `shot.py`: Projectile behavior
- `asteroid.py`: Asteroid behavior and splitting logic
- `asteroidfield.py`: Timed asteroid spawning from screen edges
- `logger.py`: State/event logging in JSONL format
- `game_state.jsonl`: Runtime state snapshots (generated)
- `game_events.jsonl`: Runtime event log (generated)

## Core OOP Model

- `CircleShape` is the shared base class for all circular entities.
	- Owns common fields: `position`, `velocity`, `radius`
	- Exposes `collides_with(other)` for distance-based collision checks
- `Player`, `Asteroid`, and `Shot` inherit from `CircleShape`.
- `AsteroidField` is a timed spawner (sprite-like manager) that creates new
	`Asteroid` instances.

Entities are attached to one or more sprite groups through the `containers`
class attribute, set in `main.py`. This keeps update/draw/collision concerns
separated by group.

## Gameplay Behavior

- Player starts centered on screen.
- Asteroids spawn periodically from random screen edges.
- Shots destroy asteroids.
- Large asteroids split into two smaller asteroids with rotated velocities.
- Collision between any asteroid and player ends the game.

## Controls

- `W`: Move forward
- `S`: Move backward
- `A`: Rotate left
- `D`: Rotate right
- `Space`: Shoot (with cooldown)
- Close window: Quit

## Logging

The game logs useful runtime data for debugging/inspection:

- `game_state.jsonl`
	- Snapshot about once per second
	- Includes sprite group counts and sampled sprite data
	- Stops logging after ~16 seconds
- `game_events.jsonl`
	- Records discrete events such as:
		- `asteroid_shot`
		- `asteroid_split`
		- `player_hit`

Both files are rewritten when a new run starts.

## Run Locally

### 1) Create and activate a virtual environment

```bash
python3 -m venv .venv
source .venv/bin/activate
```

### 2) Install dependencies

```bash
pip install pygame==2.6.1
```

### 3) Start the game

```bash
python main.py
```

## Notes for Boot.dev Learners

- Tweak values in `constants.py` to feel how movement and difficulty change.
- Inspect `logger.py` outputs to understand game-state evolution over time.
- A natural extension is adding score, lives, or screen-wrapping behavior.
