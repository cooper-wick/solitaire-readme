# Solitaire

## Overview
**Solitaire** is a command-line implementation of classic solitaire card games written in **Java**, supporting multiple rule variants including **Klondike** and **Whitehead**. The game allows players to select the desired solitaire variant via command-line arguments at runtime.

The project follows a **Model–View–Controller (MVC)** architecture, separating game logic, user interaction, and visual presentation. Gameplay is supported through textual input and output, allowing for effective input validation and rule enforcement.

This project was developed as part of **Northeastern University’s CS3100 (Object-Oriented Design)** course. As a result, the code is private to maintain the academic integrity of the class, but can be made available upon request for a limited time.

## Key Features
- Command-line playable Solitaire game
- Multiple rule variants (Klondike and Whitehead)
- MVC architecture
- Text-based view rendered to standard output
- String-based user input with argument parsing
- Full JUnit test suite
- Input validation and error handling

## How to Play

### Quickstart
This project does **not** include a prebuilt executable JAR.
The game is run directly using `java` and the project’s source code.

From the project root, run:

```bash
java klondike.Klondike <gameType> [numPiles] [numDraw]
```

### Controls

Gameplay is controlled through **text-based commands** entered via standard input. Each command consists of a short string followed by one or more integers.

All pile indices provided by the user are **1-based**.

#### Available Commands

- `mpp <sourcePile> <numCards> <destPile>`  
  Move cards from one cascade pile to another.

- `md <destPile>`  
  Move the top card from the draw pile to a cascade pile.

- `mpf <sourcePile> <foundation>`  
  Move the top card from a cascade pile to a foundation pile.

- `mdf <foundation>`  
  Move the top card from the draw pile to a foundation pile.

- `dd`  
  Discard the top card from the draw pile.

- `q` or `Q`  
  Quit the game immediately.

#### Notes
- Commands are **case-sensitive**, except for quitting (`q` or `Q`).
- Invalid commands or illegal moves will display an error message and prompt for input again.
- The game can be quit at **any time**, including while entering command arguments.

### Rules Overview

The game supports multiple solitaire rule sets.

#### Klondike
[TODO]


#### Whitehead
[TODO]

## Screenshots

## Architecture Highlights

### Model
[TODO]

### View
[TODO]

### Controller
[TODO]

### Testing
[TODO]

### Status
Archived — maintained for portfolio and academic documentation purposes.
