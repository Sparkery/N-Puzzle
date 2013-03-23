N-Puzzle
========

Place all the numbers in ascending order making restricted moves.

The code will need to be run using a JVM. The code compiles and can be run, but it is preferrable to make it a .jar file or .exe file for convenience.

The board of NxN (2 <= N <= 24) size is randomly generated. The validity of the position is then considered based on its solvability. The solvability of the board is done by finding the permutation inversion of said board. If it is solvable, the game can be played using the mouse or arrow keys. Otherwise, another board is generated. Because there is a 50% chance the board is solvable, loading a board does not take very long.
