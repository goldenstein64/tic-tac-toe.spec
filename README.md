# tic-tac-toe-spec

This is a specification of a game of tic-tac-toe. It tests the important parts of a programming language and their ecosystem:

- array manipulation (`Board.data`)
- the call stack (`App.{playGame, choosePlayers}`)
- nullability/nilability (`App.{playGame, displayWinner}`)
- error handling (`App.choosePlayer`, `Human.getMove`)
- OOP stuff
  - how to implement an interface (`Player`, `Connection`)
  - how to extend an abstract class (`Computer`)
  - accessibility of fields and methods
- random number generation (`Computer`)
- unit tests
- integration tests
- parametric tests
- mocking
- documentation, both editor and publishing support

There are several implementations of the application in several languages.

## File Structure

There is a general file structure for the game's source code. Here is a tree without any of the extensions.

```plain
.
├── Application
├── player/
│   ├── Player
│   ├── Human
│   ├── Computer
│   ├── EasyComputer
│   ├── MediumComputer
│   └── HardComputer
└── data/
    ├── Board
    ├── Mark
    ├── Message
    └── Connection
```

There is also a file structure for a console application using this as a library.

```plain
.
├── ConsoleConnection
└── Main
```

And one for the tests.

```plain
.
├── Application
├── player/
│   ├── Human
│   ├── CommonTactics
│   └── HardComputer
├── data/
│   ├── Board
│   └── Mark
└── test/
    └── MockConnection
```

If there is a language or runtime constraint that doesn't allow this file structure, exceptions can be made. The game source and tests can be in the same folder or separate ones, as long as it is easy to tell what is a test and what is source code.

Here is a UML diagram created from my Lua implementation of the codebase.

![tic-tac-toe UML](assets/images/tic_tac_toe_uml.png)

## Gameplay

A program running the tic-tac-toe game should have a workflow like below:

1. The user chooses _Player X_ and _Player O_'s player type, either a _Human_ or a _Computer_.
   - If the user chooses _Computer_, they also choose its difficulty.
2. An empty board is shown and Player X goes first.
3. Turn alternates between X and O until a win condition is reached or the board is full.
4. A winner is announced. If there is no winner, a tie is announced instead.

### Difficulties

- Easy computers simply choose a random unoccupied position on the board.
- Medium computers are programmed to iterate through an ordered list of "move getters." One position is chosen at random from the first non-empty set returned by a "move getter." There are six "move getters," listed in descending priority:
  1. positions that guarantee a win in 1 turn
  2. positions that stop the other player from winning next turn
  3. positions that guarantee a win in 2 turns
  4. the center of the board
  5. the corners of the board
  6. the sides of the board
- Hard computers are programmed to use the Minimax algorithm. Every possible outcome a tic-tac-toe board can have is judged and explored recursively, and the best set of choices is computed from this judgment. One position is then chosen from this set at random.
  - This tends to be an expensive operation, so it can be partially or completely implemented in a pre-computed fashion instead.

## I/O

The inputs and outputs of the program are sent through an abstraction layer so that messages can be tracked clearly. This is very useful for unit testing purposes; it can be mocked so that inputs can be set by tests and outputs can be compared.

If the program receives an invalid input, it sends an error to output and asks for another input. This is specified in the tests.

In theory, this also allows completely abstracting away how the program is interacted with. A desktop application could be built around the program instead of a console application, for example.

## Example Implementations

- a console application, the most common and simplest
- a desktop application with UI components
- a static website
- any of these implementations built with multiplayer in mind