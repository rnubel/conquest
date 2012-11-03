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

The board is divided into square tiles, the size of the board being proportional to the number of players. Each tile can be either:

* Open
* Impassable
* Castle

Each tile, except for impassable tiles, is owned by whichever player either currently has units on the tile or who last did. 
Castle tiles produce one unit per turn.

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
