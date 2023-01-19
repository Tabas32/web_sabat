---
title: "ColorMaster"
date: 2019-10-06T22:09:53+02:00
draft: false
---
Codes: [Github](https://github.com/Tabas32/colors) \\
Download: [UlozTo](https://uloz.to/file/yY4PDuh4pjuW/colors-exe)

**ColorMaster** is two player 2D game.
Your objective is to reach control over game board.
In every turn you choose color, in which you want to expand, and
neighbour blocks of same color will be absorbed to your influence of power.
Player that controls 50% of all blocks is winner.

<img src="/static/cmaster1.png" alt="CopyMaster menu" width="600"/>

Game is build in GameMaker game engine.
Although GameMaker allows you to create games without writing code, this is not the case.
In the background, it uses algorithm for finding shortest route.
Simplest way to implement this game would be to create object for every block.
However this approach wastes lot of resources.
For that reason was chosen better option, of tiling board, using graph as data template.

<img src="/static/cmaster2.png" alt="Game" width="600"/>
