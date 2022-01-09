---
title: "Tic Tac Toe Challenge – Version 1"
date: 2022-01-06T16:39:10-06:00
draft: false
omit_header_text: true
tags:
- Programming
- Python
featured_image: "/images/posts/toy-ge3f3268ac_1280.jpg"
---

{{< figure src="/images/posts/toy-ge3f3268ac_1280.jpg" >}}

In order to get better at programming, I decided to find programming challenges online and track how I solve them here. After a quick search on [Duck Duck Go](https://www.duckduckgo.com/), I found a set of challenges at Ryan’s Tutorials. The first on the list is a Tic Tac Toe game which states:

> Build a game of Tic Tac Toe. I would suggest you start off building a two player version then if you want a little extra challenge see if you can build a version where you get to challenge the computer.

I decided to do this challenge in Python since that is the language I’ve been using recently, but I do plan on attempting some challenges in different languages. I may also  do some challenges in multiple languages. So, if you are looking for the complete python code for this challenge, it is located in the [github repository](https://github.com/ProgrammingAway/CodingChalenges/blob/main/TicTacToe/tictactoe.py). However, I suggest you try it by yourself first, and then come back to look at my solution.

I didn’t put too much thought into this challenge when I chose to do it, but I wanted these challenges to be fairly quick and therefore, text based. When I went to start typing code, I realized I would have to put some thought into how to play Tic Tac Toe in text form.

I first started thinking about the data structure to hold the game board. Originally, I was thinking of creating a two-dimensional array since that would be most similar to an actual Tic Tac Toe game board. However, after a few lines of code I decided it would probably be easier to represent this in just a one-dimensional array. So I created an array for the game board that would originally hold the position number, but eventually will hold the player character when a play is made. I also created a list of arrays that represent winning sequences based on that board.

Here are the global variables I set up:

{{< highlight python >}}
# Game Board:
#
# 0 | 1 | 2
# ---------
# 3 | 4 | 5
# ---------
# 6 | 7 | 8

board = [0, 1, 2, 3, 4, 5, 6, 7, 8]
winningSequences = ([0, 3, 6], [1, 4, 7], [2, 5, 8], [0, 1, 2],
                    [3, 4, 5], [6, 7, 8], [0, 4, 8], [2, 4, 6])
{{< / highlight >}}

So if you think of the board variable as defining the places on the board represented in the comments, then you can define the limited set of winning sequences.

I then created a printBoard() function that does just what you think it does, prints out the game board:

{{< highlight python >}}
def printBoard():
    # Prints the current state of the board
    print()
    print(str(board[0]) + " | " + str(board[1]) + " | " + str(board[2]))
    print("---------")
    print(str(board[3]) + " | " + str(board[4]) + " | " + str(board[5]))
    print("---------")
    print(str(board[6]) + " | " + str(board[7]) + " | " + str(board[8]))
    print()
{{< / highlight >}}

I also wanted to create a function to check if the current move wins the game since this function would be called after every move. Since I plan on replacing the initial number in the board array with the player character, I need to test if all the indexes in the winning sequence equal the same character. Then if that character equals X, player X wins, otherwise player O wins. Here’s the code I came up with:

{{< highlight python >}}
def checkForWin(count):
    # Checks the board for a winning sequence
    for sequence in winningSequences:
        if board[sequence[0]] == board[sequence[1]] == board[sequence[2]]:
            printBoard()
            if board[sequence[0]] == "X":
                print("Player 'X' won!!")
            if board[sequence[0]] == "O":
                print("Player 'O' won!!")
            return 1
    return 0
{{< / highlight >}}

This code will return 1 if a player won, and 0 if the game is to continue.

Finally I started creating code to play the game. Here is what I started with:

{{< highlight python >}}
win = 0
count = 0
while win == 0:  # =1 for win, =0 to continue play
    printBoard()
    if count % 2 == 0:  # Even count is player X
        player = "X"
    else:  # Odd count is player O
        player = "O"
    move = int(input("Player " + player + ": Enter number of space you want to place your piece: "))
    while move not in range(9) or board[move] != move:
        move = int(input("Please pick an unused space between 0 and 8: "))
    board[move] = player
    win = checkForWin(count)
    count = count + 1
{{< / highlight >}}

So while no one had won, we do the following:

print the board
- choose the current player based on which play count it is (X for even count, O for odd count)
- ask the player what position he wants to place his character
- next we check to make sure that position is a valid position and one that is not already chosen
- if it is not valid, ask the question again
- after a valid position is chosen, place the player character in that position in the array
- check if that play was a winning play
- increment the count and repeat the while command
This code worked, unless if no one won. We need to handle the possibility of a draw. The way I handled this problem is to use the count variable that tracks the number of plays. The maximum number of plays for a Tic Tac Toe game is 9. If no one has won after 9 turns (or 8 if counting from 0), then the game is a draw. So this requires us to send the count variable to the checkForWin() function and make the following changes:

{{< highlight python >}}
def checkForWin(count):
    # Checks the board for a winning sequence
    # Also checks if there are no more moves and calls a draw
    if count == 8:
        printBoard()
        print("The game is a draw!")
        return 2
    for sequence in winningSequences:
        if board[sequence[0]] == board[sequence[1]] == board[sequence[2]]:
            printBoard()
            if board[sequence[0]] == "X":
                print("Player 'X' won!!")
            if board[sequence[0]] == "O":
                print("Player 'O' won!!")
            return 1
    return 0
{{< / highlight >}}

Be sure to change the call to checkForWin() in the while function to send the count variable.

Now the game worked, but there was no way to continue to play the game without running the program again. In order to play the game multiple times, we would need to put that while loop into its own function. So I created the playGame() funciton:

{{< highlight python >}}
def playGame():
    # while there is not a win or draw, then ask the player for the next move
    win = 0
    count = 0
    while win == 0:  # =1 for win, =2 for draw, =0 to continue play
        printBoard()
        if count % 2 == 0:  # Even count is player X
            player = "X"
        else:  # Odd count is player O
            player = "O"
        move = int(input("Player " + player +
                   ": Enter number of space you want to place your piece: "))
        while move not in range(9) or board[move] != move:
            move = int(input("Please pick an unused space between 0 and 8: "))
        board[move] = player
        win = checkForWin(count)
        count = count + 1
{{< / highlight >}}

After creating the playGame() function, the complete code would be as follows:

{{< highlight python >}}
# Tic Tac Toe
# Build a game of Tic Tac Toe. I would suggest you start off building a two player version then if
# you want a little extra challenge see if you can build a version where you get to challenge the
# computer. (from https://ryanstutorials.net/programming-challenges/)

play = input("Would you like to play Tic Tac Toe? (Y/N) ")

if "y" not in play.lower():
    print("Ok, bye!")
    exit(1)

# Game Board:
#
# 0 | 1 | 2
# ---------
# 3 | 4 | 5
# ---------
# 6 | 7 | 8

board = [0, 1, 2, 3, 4, 5, 6, 7, 8]
winningSequences = ([0, 3, 6], [1, 4, 7], [2, 5, 8], [0, 1, 2],
                    [3, 4, 5], [6, 7, 8], [0, 4, 8], [2, 4, 6])


def printBoard():
    # Prints the current state of the board
    print()
    print(str(board[0]) + " | " + str(board[1]) + " | " + str(board[2]))
    print("---------")
    print(str(board[3]) + " | " + str(board[4]) + " | " + str(board[5]))
    print("---------")
    print(str(board[6]) + " | " + str(board[7]) + " | " + str(board[8]))
    print()


def checkForWin(count):
    # Checks the board for a winning sequence
    # Also checks if there are no more moves and calls a draw
    if count == 8:
        printBoard()
        print("The game is a draw!")
        return 2
    for sequence in winningSequences:
        if board[sequence[0]] == board[sequence[1]] == board[sequence[2]]:
            printBoard()
            if board[sequence[0]] == "X":
                print("Player 'X' won!!")
            if board[sequence[0]] == "O":
                print("Player 'O' won!!")
            return 1
    return 0


def playGame():
    # while there is not a win or draw, then ask the player for the next move
    win = 0
    count = 0
    while win == 0:  # =1 for win, =2 for draw, =0 to continue play
        printBoard()
        if count % 2 == 0:  # Even count is player X
            player = "X"
        else:  # Odd count is player O
            player = "O"
        move = int(input("Player " + player +
                   ": Enter number of space you want to place your piece: "))
        while move not in range(9) or board[move] != move:
            move = int(input("Please pick an unused space between 0 and 8: "))
        board[move] = player
        win = checkForWin(count)
        count = count + 1


while "y" in play.lower():
    playGame()
    play = input("Would you like to play again? (Y/N) ")
    board = [0, 1, 2, 3, 4, 5, 6, 7, 8]

print("Thanks for playing")
{{< / highlight >}}

So that is how I solved the Tic Tac Toe programming challenge in Python. Let me know if you solved it in a different way in the comments below.

 

As you leave, please enjoy this picture of one of the beautiful beaches of Panama, Red Frog Beach.

-Brian