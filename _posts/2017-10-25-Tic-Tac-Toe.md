---
layout: default
---

## Tic Tac Toe
### A Simple Python Experiment
This Tic-Tac-Toe application is a simple bit of Python code that I decided to work on in order to improve my Python exposure as well as play around with graphical
programming techniques with Tkinter.  The code itself was not terribly difficult to implement, as there are no truly taxing algorithms in such a simple game.
The entirity of the code is available on github [here](https://github.com/jkd28/Tic-Tac-Toe). 
This is a side project that a group of my friends and I started in the early days of Summer, 2017.  Our goal was to get more programming practice and implement
something that was a bit "bigger" than anything we had done for class.  After a riveting game of RISK in our kitchen, we came up with the idea to create the game
in code.  

### Project Layout
We began to design the project and came up with three divisions of the game:  
  * `Display`  
    This section contains all information and implementations required to produce a simple GUI for user interaction with our game.
     There are several files included in this seciont of our code, including `DisplayDriver.java`...
  * `Map`  
    This section implements the Graph data structure we used as the implementation for the actual board.  This also includes several
    graph-related algorithms. Most notably, we use a Breadth-First Search to implement the path detection between territories.  This
    section includes `Map.java`, `Territory.java` ...
  * `Driver`  
    This section acts as the intermediary between the `Display` and `Map` sections, processing the information compiled in both and
    passing it along the proper channels.  Most gameplay rules/aspects are implemented and enforced here.  This section also keeps
    track of the Player object, which is used to determine win condition and order of play.  

### Progress
As of June 2, 2017, our Risk Board Game Project is still in development.
