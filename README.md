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
This project does **not** include a prebuilt executable JAR. The game is run directly using `java` and the project’s source code.

From the project root, run:

```bash
java klondike.Klondike <gameType> [numPiles] [numDraw]
```

#### Arguments
- **`gameType`** (required)  
  Specifies which Solitaire variant to play.  
  - `basic` — Standard Klondike Solitaire
  - `whitehead` — Whitehead Klondike

- **`numPiles`** (optional)  
  Number of cascade piles.  
  Defaults to `7` if not provided.

- **`numDraw`** (optional)  
  Number of cards drawn at a time from the draw pile.  
  Defaults to `3` if not provided.


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

The game implements classic **Klondike Solitaire**, along with the **Whitehead Klondike** variant. Both versions share the same overall objective and core structure, with specific rule differences outlined below.

---

#### Klondike

Klondike is played using a deck composed of one or more **equal-length, single-suit runs** of consecutive cards starting from Ace.

**Game Areas**
- **Foundation Piles**:  
  Initially empty. Each foundation pile builds upward from **Ace to the highest card**, using a single suit.  
  The number of foundation piles equals the number of Aces in the deck.
- **Cascade Piles**:  
  Cards are dealt into a configurable number of piles. All cards start face-down except the bottom card of each pile.
- **Draw Pile**:  
  Contains all remaining cards. A fixed number of cards are revealed, but only the first revealed card may be used.

**Build Rules**
- Face-up cards in cascade piles must form **descending sequences** of consecutive ranks.
- Builds must **alternate colors** (red on black or black on red).

**Legal Moves**
- Move the bottom face-up card of a cascade pile to a foundation pile.
- Move a build of face-up cards from one cascade pile to another, preserving valid build rules.
- Move the first draw card to a cascade pile or foundation pile, following the same placement rules.
- Discard the first draw card to the bottom of the draw pile (the draw pile is recycled indefinitely).
- Quit the game at any time.

**Scoring and End Condition**
- The score is calculated by **summing the values of the top card in each foundation pile**.
- The game ends when **no legal moves remain**.

---

#### Whitehead

Whitehead Klondike follows the same structure and objective as Klondike, with the following rule changes:

- All cascade pile cards are dealt **face-up**.
- Builds must be **single-colored** (red on red or black on black).
- When moving multiple cards between cascade piles, the moved cards must form a **single-suit run**.
- Any card value may be moved into an empty cascade pile (not just Kings).

All other rules remain unchanged from standard Klondike.

## Screenshots
<table style="background-color:#0d1117; padding:10px; border-radius:8px;">
  <tr>
    <td align="center" style="padding:10px;">
      <img src="https://github.com/user-attachments/assets/REPLACE_WITH_NEW_IMAGE_ASSET_URL"
           width="350" />
    </td>
    <td align="center" style="padding:10px;">
      <img src="https://github.com/user-attachments/assets/5e27f965-3aea-42c1-ac6e-1bee4fd1cce6"
           width="350" />
    </td>
  </tr>
  <tr>
    <td align="center" style="padding:10px;">
      <img src="https://github.com/user-attachments/assets/5d0b8c6a-0bc4-4ae4-9ee5-966b6468d93c"
           width="350" />
    </td>
    <td align="center" style="padding:10px;">
      <img src="https://github.com/user-attachments/assets/831beb40-3db4-491a-a452-a15c28dbb548"
           width="350" />
    </td>
  </tr>
</table>

## Architecture Highlights

### Model
The model implements the ruleset and state management for Solitaire variants.

- Core game behavior is defined through the `KlondikeModel` interface, enabling controller and view decoupling.
- `BasicKlondike` and `WhiteheadKlondike` provide variant-specific rules, while commonalities are shared in `AbstractKlondike`.
- The model is decomposed into smaller components including `Card` and `Pile`.
- Card representations use explicit `Rank` and `Suit` enums, supporting comparison and consistent rendering.

This structure allows new Solitaire variants to be added with minimal impact on existing code.

### View

The view is a **textual renderer** that translates the current game state into a human-readable command-line display.

- The view depends only on the `KlondikeModel` interface and never mutates game state.
- All rule-specific behavior is determined by the model and communicated via queries to the model.
- Card rendering uses fixed-width formatting to ensure consistent alignment across ranks (including two-character ranks like `10`), preserving a readable cascade layout.
- Face-down cards are displayed as `?`, while empty foundation piles are rendered as `<none>`, providing clear visual cues to the player.
- The view supports output to any `Appendable`, allowing for redirection to standard output, logs, or test classes.

This design keeps presentation logic fully isolated from game rules and input handling, resulting in a testable, variant-agnostic user interface.

### Controller

The controller translates user input into game actions while maintaining a strict separation between user interaction and game logic. It serves as the connection between the model and the textual view.

The controller processes **string-based commands with numeric arguments**, mapping them to operations on the model. Input is validated at the model level, ensuring that invalid commands or illegal moves do not alter the game state.

Key design decisions include:
- **Model Decoupling**: The controller contains no Solitaire-specific logic, allowing it to operate across different game variants (Klondike and Whitehead).
- **Input parsing**: Invalid input and unexpected end-of-input are handled by asking the user to retry input.
- **Safe termination**: Players may quit at any point, causing a final game state render before exiting.

This design enables new Solitaire variants or alternative views to be introduced without modifying controller logic.

### Testing
The project includes a **comprehensive JUnit test suite** covering all architectural layers.

- Model behavior is validated through unit tests for cards, piles, scoring, and variant-specific rules.
- Controller behavior is tested using mock models and appendables to verify error handling and game state.
- View output is validated through string comparisons.
- Mocks are used to isolate components for comprehnsible tests.

### Status
Archived — maintained for portfolio and academic documentation purposes.
