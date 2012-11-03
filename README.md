# Enova Conquest

A board game, played similar to Risk, but wth the following goals:

* More tactical, less random
* Supports an arbitrary number of players on the same board
* Players can play simultaneously by queuing actions which are executed all at once

Considerations:

* Scalability
* Concurrency
* Simplicity for players

## Rules

### Game board
The board is divided into square tiles, the size of the board being proportional to the number of players. Each tile can be either:

* Open
* Impassable
* Castle

<table>
  <tr>
    <td>C</td> <td></td> <td></td> <td></td> <td>C</td>
  </tr>
  <tr>
    <td></td> <td>X</td> <td></td> <td>X</td> <td></td>
  </tr>
  <tr>
    <td></td> <td></td> <td>&nbsp;</td> <td></td> <td></td>
  </tr>
  <tr>
    <td></td> <td>X</td> <td></td> <td>X</td> <td></td>
  </tr>
  <tr>
    <td>C</td> <td></td> <td></td> <td></td> <td>C</td>
  </tr>
</table>

### Gameplay
* Each tile, except for impassable tiles, is owned by whichever player either currently has units on the tile or who last did. 
* Players start out with 2 units on a single Castle tile.
* Castle tiles produce one unit per turn, at the beginning of the turn.
* Players can set one action per each tile that they own.
* Players can either **move** units, which will initiate an attack if into another player's territory, order units to **defend**, or order units to **support** surrounding territories.

### Battle
In an attack, each player's combat strength is: (number of units) + (number of **support**ing units in adjacent territories) + (number of **defend**ing units in territory, if being attacked)

To decide the outcome of the battle, we add some chance into the mix. Let the **battle variance** be 25% of the total combat strength of both players. Then, compute each player's battle score as `(combat strength) + rand(1..battle variance)`. The player with the higher battle score wins the battle.

Half (rounded down) of the loser's retreating units are killed, and half retreat evenly to adjacent friendly or unowned tiles. If no safe place is available to retreat to, the units are routed and lost.

#### Example
Player 1 has 10 units in tile X and attacks tile Y, which has 4 units and is owned by Player 2. Those 8 units were defending, and an adjacent tile Z has 2 units supporting Player 2.

Player 1's combat strength is: `10 + 0 + 0 = 10`

Player 2's combat strength is: `4 + 2 + 4 = 10`

The battle variance is: `.25 * (10 + 10) = 5`

So, both player's battle scores are `10 + rand(5)`. Each player has an equal chance of winning, but Player 1 ends up with 13 versus Player 2's 12.

Player 2 loses 2 units. The remaining 2 units retreat to tile Z.
