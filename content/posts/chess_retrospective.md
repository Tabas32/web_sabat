+++ 
draft = false
date = 2023-05-25T22:22:26+02:00
title = "My chess program post mortem"
description = "Retrospecitve on my first C++ program"
slug = ""
authors = [ "Marian Sabat" ]
tags = [ "cpp" ]
categories = []
externalLink = ""
series = []
+++

Ok, I would be lying if I said that this is my first-ever program
written in C++. But I will be counting this one as first.

So what is this program about? I wanted to make a simple chess game
so I can learn C++. Of course, at the beginning, I overestimated
my knowledge. But let's start from the start.

## What I wanted to do
Initially, I wanted to create a fully working chess game in the
terminal. The user would be using official chess notation to make
moves. The game would be playable against the computer or as 
multiplayer.

## What I've actually done
Frankly, the game is not done, but I am not planning to finish it.
At the current state, the chessboard is displayed, and the user can
provide input in the form of specifying from which position to move
where. There is no game loop, nor winning condition. Forget playing
against the computer. Actually, not all moves can be performed, like
castling. But if you would want to play against someone, you could,
even if only in a smaller ruleset.

Look, if I showed it to someone, they would recognize it as a chess.
I made it with very limited knowledge of C++, so I call it an absolute
win.

## Code
The code for this program can be found on my GitHub:
https://github.com/Tabas32/chess_cpp

![Chess game](/web_sabat/images/chess_retrospective/cpp_chess.png)

To build the project, I've used CMake. Of course, I wanted to do some
testing, so for unit tests, I've used GTest.

At the beginning, I had some ideas about how I wanted to create this
game. The code is separated into two modules: one for the game
implementation without any output, and the user interface module.
In the end, I ended up with the following structure:

```
src
├── gameCore/
│   ├── enums/
│   │   ├── Color.hpp
│   │   └── PieceType.hpp
│   ├── pieces/
│   │   ├── Bishop.cpp
│   │   ├── Bishop.hpp
│   │   ├── King.cpp
│   │   ├── King.hpp
│   │   ├── Knight.cpp
│   │   ├── Knight.hpp
│   │   ├── Pawn.cpp
│   │   ├── Pawn.hpp
│   │   ├── Piece.cpp
│   │   ├── Piece.hpp
│   │   ├── PieceMovement.cpp
│   │   ├── PieceMovement.hpp
│   │   ├── Queen.cpp
│   │   ├── Queen.hpp
│   │   ├── Rook.cpp
│   │   └── Rook.hpp
│   ├── Board.cpp
│   ├── Board.hpp
│   ├── Chess_game.cpp
│   └── Chess_game.hpp
├── tui/
│   ├── BoardBuilder.cpp
│   └── BoardBuilder.hpp
└── main.cpp

```

From a logical standpoint, I made a Piece class, and every type of
piece then inherited from it. Each piece overrode the move method of
Piece with its custom movement pattern. All pieces were instantiated
inside the play method of the Chess_game class. Then the pieces were
passed as input to the Board. The Board was actually driving all
movements, as it had a reference to all pieces. From this, we got
this weird chain where to make a move, you ask the Board to do it,
and the Board would ask the Piece at the specified position for
moves that it can perform. You probably can guess that the piece
required a reference to the board, as by itself, it is clueless
about other pieces.

For the terminal interface, I have used the ftxui library because I
was lazy to come up with something myself.

## So, what can I take from all this
I have realized that I know much less than I thought initially.
First of all, in the beginning, I made plans, and during
implementation, I quickly abandoned those plans and took lots of
shortcuts.

I have no idea how CMake works. I was fighting with it the whole
time. But in the end, I must say it would have been even harder
without it. I tried to use a simple Makefile at the beginning, but
after the files started piling up, I was lost.

Working with GTest was fine. I was able to create unit tests for the
movements of all pieces. Thanks for that. At least I could be
somewhat sure that it was working. However, after that, my testing
practice had ended. Mostly, I must say that I don't like adding new
files because then I have to touch CMake again.

The next thing is my environment. I'm using Neovim to code, with LSP.
I love the Vim editor. However, I miss that no one is holding my
hand. Clangd was always broken, and the errors that it showed were
a pain to decipher. But I think I can make it work; I just need to
get used to it more and actually learn the C++ language. Another
thing is a debugger. As of right now, I don't have anything set up
that I would be comfortable working with. I used GDB a little bit,
but again, for me to be able to use it, I need to play with CMake
to enable it. It would be great to have some more graphical tool
for debugging.

The last thing I need to mention is that my C++ knowledge is abysmal.
I had a huge trouble creating header files and including them
throughout all the files. I strayed away from using references and
pointers. I remember that when I was using C, pointers were a normal
thing to use for me. But here, somehow, I struggle a lot and confuse
pointers and references. I keep using pointers over references as
they make more sense to me.

At one point, I totally forgot that I need to keep track of memory.
I instantiated my objects in one method and passed them back in a
vector. Then I tried to access those objects, and guess what?
Segfault. As I went out of scope, my stack got cleared, and all my
objects were gone. Such a dumb mistake.

I think that a lot of my issues would be gone if I didn't look at it
as Java or Kotlin code. My first instinct is to create a class for
everything. That is probably not true for C++. More so at the
beginning, but very often, I found myself frustrated that there is
no function that would do something that I needed. I'm very used to
functional style of programming, and getting used to the fact that I
need to write everything myself was harder than I expected. I tried
multiple coding styles that I got recommended from somewhere. Really
bad idea. I should have used what I'm comfortable with from the
beginning. It would prevent a lot of rewriting.

So this is what I need to focus on:
* Learn CMake.
* Learn to use a debugger.
* Study some real C++ code and look for how they are using header
files and class structures.
* Learn more about pointers and references.
* Learn more about const.

