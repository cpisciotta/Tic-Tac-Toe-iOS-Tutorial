# Tic-Tac-Toe-iOS-Tutorial

&copy; 2020 Charles Pisciotta

An iOS (Swift) tutorial on how to create Tic-Tac-Toe using SwiftUI and MVVM.

<p align="center">
    <img src="example.mov" width=30%>
</p>

In this tuturiol, we'll be going over how to program tic-tac-toe. This tutorial assumes that you have a basic understanding of Swift and programming and, as a result, will focus more on the "why?" behind the code rather than the "what?". 

## Model Layer

### Creating the Player

``` swift
// Player.swift

import Foundation

/// A struct representing universal player logic in a game.
struct Player {

    /// The player's first or full name.
    let name: String

    /// The total points that a player has scored.
    private(set) var points: Int

    /// Creates a new player for a game.
    /// - Parameters:
    ///   - name: The player's first or full name.
    ///   - points: The points that a player has scored in a game.
    init(name: String, points: Int = 0) {
        self.name = name
        self.points = points
    }

    /// Adds `numPoints` to the player's total score.
    /// - Parameter numPoints: The number of points that a player achieves. Default is 1.
    mutating func addPoints(_ numPoints: Int = 1) {
        self.points += numPoints
    }
}
```

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

### Extending Game to Specialize for Tic Tac Toe

``` swift
import Foundation

/// This extension will provide specific Tic Tac Toe logic which must be determined by a specific player.
extension Game.PlayerPick {

    /// This variable will indicate the preset marks a player will make for Tic Tac Toe.
    var mark: String {
        switch self {
        case .player2: return "⭕️"
        case .player1: return "❌"
        }
    }
}
```

## View Model Layer

### Creating the Tic Tac Toe Logic

## View Layer

### Creating the Tic Tac Toe Board

``` swift
// TicTacToeBoard.swift

import SwiftUI

// This `Shape` draws a traditional tic-tac-toe board to fill the given CGRect.
struct TicTacToeBoard: Shape {

    // This function draws a tic-tac-toe board to fill the given CGRect.
    func path(in rect: CGRect) -> Path {

        // Get the CGPoint values necessary to draw a tic-tac-toe board.
        // This computes the end-points of the horizontal and vertical lines.
        let values = BoardValues(rect: rect)

        // Initialize a path to draw a tic-tac-toe board.
        var path = Path()

        // Draw the upper horizontal line of a tic-tac-toe board.
        path.move(to: values.leftThirdTop)
        path.addLine(to: values.rightThirdTop)

        // Draw the lower horizontal line of a tic-tac-toe board.
        path.move(to: values.leftThirdBottom)
        path.addLine(to: values.rightThirdBottom)

        // Draw the left vertical line of a tic-tac-toe board.
        path.move(to: values.topThirdLeft)
        path.addLine(to: values.bottomThirdLeft)

        // Draw the right vertical line of a tic-tac-toe board.
        path.move(to: values.topThirdRight)
        path.addLine(to: values.bottomThirdRight)

        // Return the tic-tac-toe board.
        return path
    }
}
```

``` swift
extension TicTacToeBoard {

    /// This struct computes the end points of the vertical and horizontal lines of a traditional tic-tac-toe board.
    private struct BoardValues {

        /// This CGRect represents the available space to fill a tic-tac-toe board.
        private let rect: CGRect

        /// This initializer determines CGRect where a tic-tac-toe board will be drawn.
        init(rect: CGRect) { self.rect = rect }

        /// This factor indicates the spacing of the vertical and horizontal lines for a traditional tic-tac-toe board.
        private static let factor: CGFloat = 1.0 / 3.0

        /// The top endpoint of the left vertical line.
        var topThirdLeft: CGPoint {
            CGPoint(x: rect.maxX * Self.factor,
                    y: 0.0)
        }

        /// The top endpoint of the right vertical line.
        var topThirdRight: CGPoint {
            CGPoint(x: rect.maxX * 2 * Self.factor,
                    y: 0.0)
        }

        /// The bottom endpoint of the left vertical line.
        var bottomThirdLeft: CGPoint {
            CGPoint(x: rect.maxX * Self.factor,
                    y: rect.maxY)
        }

        /// The bottom endpoint of the right vertical line.
        var bottomThirdRight: CGPoint {
            CGPoint(x: rect.maxX * 2 * Self.factor,
                    y: rect.maxY)
        }

        /// The left endpoint of the upper horizontal line.
        var leftThirdTop: CGPoint {
            CGPoint(x: 0.0,
                    y: rect.maxY * Self.factor)
        }

        /// The right endpoint of the upper horizontal line.
        var rightThirdTop: CGPoint {
            CGPoint(x: rect.maxX,
                    y: rect.maxY * Self.factor)
        }

        /// The left endpoint of the lower horizontal line.
        var leftThirdBottom: CGPoint {
            CGPoint(x: 0.0,
                    y: rect.maxY * 2 * Self.factor)
        }

        /// The right endpoint of the lower horizontal line.
        var rightThirdBottom: CGPoint {
            CGPoint(x: rect.maxX,
                    y: rect.maxY * 2 * Self.factor)
        }

    }

}
```

### Creating the Tic Tac Toe Cell

### Creating the Points View
