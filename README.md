# Tic-Tac-Toe-iOS-Tutorial

&copy; 2020 Charles Pisciotta

An iOS (Swift) tutorial on how to create Tic-Tac-Toe using SwiftUI and MVVM.

<center>
    <img src="tic-tac-toe.png" width=50%>
</center>

In this tuturiol, we'll be going over how to program tic-tac-toe. This tutorial assumes that you have a basic understanding of Swift and programming and, as a result, will focus more on the "why?" behind the code rather than the "what?". 

## Model Layer

### Creating the Player

### Creating the Game

``` swift
// Game.swift

import Foundation

/// A struct to represent universal 2-player game logic.
struct Game {

    /// The first player in the 2-player game.
    private(set) var player1: Player

    /// The second player in the 2-player game.
    private(set) var player2: Player

    /// Indicates if it is player's 1 turn or player's 2 turn.
    ///
    /// By default, this variable indicates that `player1` is first.
    private(set) var turn: PlayerPick = .player1

    /// This creates a new 2-player game.
    ///
    /// By default, this init creates `player1` with `Player 1` as its name. 
    /// It also utilizes the default initializer for player which indicates that `player1` starts with 0 points. 
    /// The same is true for `player2`, however, with its name as `Player 2`.
    init() {
        self.player1 = Player(name: "Player 1")
        self.player2 = Player(name: "Player 2")
    }

    /// This method adds points for a player in a 2-player game.
    ///
    /// By default, the number of points, `numPoints`, added is 1.
    /// - Parameter numPoints: The number of points to add to a player's score. By default, this value is 1.
    /// - Parameter player: This enum case indicates which player gained the points.
    mutating func addPoints(_ numPoints: Int = 1, for player: PlayerPick) {
        switch player {
        case .player1: self.player1.addPoints(numPoints)
        case .player2: self.player2.addPoints(numPoints)
        }
    }

    /// This method switches the turn for the current 2-player game.
    mutating func switchTurn() {
        switch turn {
        case .player2: self.turn = .player1
        case .player1: self.turn = .player2
        }
    }

    /// This enum will indicate if logic applies to Player 1 or Player 2.
    enum PlayerPick {
        case player1
        case player2
    }

}

```

## View Model Layer

### Creating the Tic Tac Toe Logic

## View Layer

### Creating the Tic Tac Toe Board

### Creating the Tic Tac Toe Cell

### Creating the Points View
