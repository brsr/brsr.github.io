---
layout: post
title: "Life-Like Hexagonal Cellular Automata"
tags: python jupyter
---
There are two "neighborhoods" often used in cellular automata, the Von Neumann neighborhood of 4 cells and the Moore neighborhood of 8 cells. Conway's Game of Life uses the Moore neighborhood. One can also used a neighborhood of 6 cells on a skewed grid to implement a hexagonal cellular automata. Hexagonal cellular automata don't seem to be quite as rich as the Game of Life, but still produce some interesting patterns. One produces a maze of twisty little passages, all alike:

<p align="center">
<img src="/assets/images/hex_cell.png" alt="Image of a cellular automata on a hex grid" />
</p>

[Here's a Jupyter Notebook implementing Life-like cellular automata on a hex grid.](https://github.com/brsr/math/blob/master/Life-Like%20Hex%20Cellular%20Automata.ipynb) To use it interactively, download it to your local machine.
