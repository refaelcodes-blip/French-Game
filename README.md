# Corner the Blue

*A small French abstract strategy game implemented in C++ and Windows Forms.*

**Corner the Blue** is a minimalist two-player abstract strategy game inspired by traditional French “hare games” (also known as the **French Military Game / Hare and Hounds**).  

One player controls **three red pieces** that advance together like a small squad.  
The other player controls a single **blue piece** that can move more freely but is outnumbered.  

There is **no randomness** and **no hidden information** – every game is decided purely by planning and tactics.

---

## Overview

- **Players:** 2  
  - Red: three pieces  
  - Blue: one piece  
- **Genre:** Asymmetrical abstract strategy (“hunt game”)  
- **Platform:** Desktop  
- **Tech stack:** C# (.NET) + Windows Forms  
- **Goal:**  
  - Red tries to trap the blue piece so it cannot move.  
  - Blue tries to slip past the red line and reach the area “below” all red pieces.

This project is designed as a **small, readable example** of a turn-based board game implemented in classic WinForms. It can also be used as a testbed for simple AI or search algorithms.

---

## Rules

### Board and pieces

- The game is played on a fixed small board (see screenshots in this repository).
- The red player has **three red pieces** starting near the **top** of the board.
- The blue player has **one blue piece** starting closer to the **bottom** of the board.

### Turn order

- **Red moves first.**
- Players then **alternate turns**.
- On each turn, a player moves **exactly one** of their own pieces.

### Movement

#### Red (three red pieces)

On Red’s turn:

- A red piece may move **one step forward** (towards the blue side of the board), or  
- **One step sideways** (left or right), if there is a connection / square there.  
- **Red pieces may never move backward.**

Additional details:

- There are **no captures** and **no jumps**.
- A red piece can only move to an **adjacent, empty** position that is connected by a board line.

#### Blue (single blue piece)

On Blue’s turn:

- The blue piece may move **one step in any direction**:
  - forward, backward, sideways, or diagonally (following the board lines).
- The blue piece **cannot jump** and **cannot land on an occupied** position.

---

## How to Win

The game can end in two ways:

### Red wins: cornering the blue piece

Red wins if:

- It is Blue’s turn, and  
- The blue piece has **no legal moves**:
  - every adjacent position is either off the board, occupied by a red piece, or not connected by a valid line.

In this case, the blue piece is completely **trapped**, and Red wins the game.

### Blue wins: slipping past the reds

Blue wins immediately if, after a move:

- The blue piece reaches a position **below all red pieces**, i.e.:
  - it is closer to the bottom edge of the board than every red piece.

Because red pieces are not allowed to move backward, they can no longer rebuild a blocking line behind the blue piece – the blue piece has successfully **broken through** the defense.

---

## How to Play (controls & basics)

1. **Start the application**

   - Build and run the project in Visual Studio (or your preferred C# IDE).
   - A classic Windows Forms window with the game board will appear.

2. **Selecting and moving pieces**

   - On your turn, **click** a piece of your color (red or blue).
   - Then click a valid adjacent position on the board to move there.
   - The application checks whether the move is legal according to the rules above.

3. **Game flow**

   - Red moves first, then Blue, and so on.
   - After each move, the game automatically checks:
     - whether the blue piece is completely blocked (**Red wins**), or  
     - whether the blue piece has moved to a position below all reds (**Blue wins**).
   - When a winner is detected, a message is shown and the game can be restarted.

4. **Recommended use**

   - Play a few quick games to get used to the asymmetric movement rules.
   - Try deliberate “bad” moves to see how fast one side can lose with a single mistake.

---

## Strategy Tips

### For Red (three pieces)

- Keep your red pieces **close together** to form a solid wall.
- Avoid rushing forward with a single piece – this can create a **gap** for Blue.
- Try to **push** the blue piece towards a **corner** or side edge with fewer escape routes.

### For Blue (one piece)

- Use your freedom of movement to keep **multiple options** available.
- Sometimes a **short retreat** or sideways move is the best way to tempt Red into breaking formation.
- Look for moments when one red piece drifts away from the others – that may be your chance to **slip through**.

---

## Implementation (C# / Windows Forms)

This digital version of **Corner the Blue** is implemented as a classic desktop application using **C#** and **.NET Windows Forms**.

Key technical points:

- The board is drawn using standard **Windows Forms painting (GDI+)**.
- **Mouse events** are used to select and move pieces on the board.
- Game state is stored in simple C# classes, e.g.:
  - board representation (connections / coordinates),
  - piece positions and current player,
  - legal move generation and win-condition checks.
- After each move, the game checks:
  - whether Red has trapped Blue (no moves remain), or  
  - whether Blue has slipped past the red line (blue is below all red pieces).

Because the logic is compact and self-contained, this project can be:

- a **learning example** for beginners in C# and Windows Forms,
- a **starting point** for AI experiments (search trees, evaluation functions), or
- the basis for a port to other frameworks (WPF, Unity, web front-end, etc.).

---
### AI / Move selection algorithm

For another board game project (Renzu) I used a classic minimax search with an evaluation function in the range 0–1.  
For **Corner the Blue** the AI uses a different approach that is better suited to this small, highly tactical game.

Here the evaluation is essentially binary: every position is treated as either a win (1) or a loss (0) for the side to move.

Instead of trying to brute-force all possible moves on the game graph in real time, the algorithm focuses on a carefully selected subset of positions. It treats each reachable board position as a node in a directed graph and performs a graph walk to identify positions whose outcome can be proven exactly:

- any position that immediately leads to a forced loss for the opponent is marked as winning;
- any position where all legal moves lead only to positions that are winning for the opponent is marked as losing.

By iterating this process over the relevant part of the state graph, the engine builds a set of “anchor” positions whose outcome (win or loss) is known with certainty.

During actual play, the move-selection routine does not explore the full game tree. It expands only a subset of candidate moves until it reaches one of these pre-classified positions; at that moment the algorithm already knows the final outcome of that line and can stop searching deeper.

In practice this allows the engine to find a strong move almost instantly, without having to enumerate every possible move on the graph in real time, while still relying on exact win/loss information instead of a vague heuristic score.


## Screenshots

Screenshots are included in this repository:

1. **Starting position**  
   Three red pieces at the top, one blue piece near the bottom.

2. **Red victory – blue is completely blocked**  
   The blue piece cannot move anywhere; all adjacent positions are blocked.

3. **Blue victory – blue has slipped past the red line**  
   The blue piece has reached a position below all red pieces and wins immediately.

> For a more detailed description (including formatted rules and captions),
> see the accompanying **Word / PDF description** in this repository.
