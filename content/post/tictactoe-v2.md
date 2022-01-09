---
title: "Tic Tac Toe Challenge – Version 2"
date: 2022-01-08T16:39:10-06:00
draft: false
omit_header_text: true
tags:
- Programming
- Python
featured_image: "/images/posts/tic-tac-toe-gcdabc1c51_1920.jpg"
disable_share: true
---

{{< figure src="/images/posts/tic-tac-toe-gcdabc1c51_1920.jpg" >}}

Shortly after writing a [Tic Tac Toe program](https://programmingaway.com/post/tictactoe_v1) in python for one of my programming challenges, I ran across a new Tic Tac Toe programming challenge from [Robert Heaton](https://robertheaton.com/2018/10/09/programming-projects-for-advanced-beginners-3-a/). While looking through his challenge, I decided my program was too simple and needed to be updated with better written functions. The functions in my original Tic Tac Toe program were hardcoded in some ways and needed to be more abstracted. Functions need to do just one thing and need to be able to accomodate potential changes in the future without re-writing the function.

So the first thing I did was make the game board size a variable that can be controlled by the user. Now we aren’t stuck with a 3×3 board, however, this would require us to re-write most of the functions since they were hardcoded to use a 3×3 board. The first thing I needed to do is to create a initializeBoard() function that created a 2 dimensional array based off the boardSize variable.

{{< highlight python >}}
def initializeBoard(boardSize):
    # Creates a 2 dimentional martix of boardSize
    board = []
    for x in range(boardSize):
        column = []
        for y in range(boardSize):
            column.append(None)
        board.append(column)
    return board
{{< / highlight >}}

The next thing I did was update the printBoard() function to use this new board:

{{< highlight python >}}
def printBoard(board):
    # Prints the current state of the board
    rowIndex = 0
    positionIndex = 0
    print()
    for row in board:
        boardRow1 = " | "
        boardRow2 = "---"
        for col in row:
            if col is None:
                if positionIndex < 10:
                    boardRow1 += " "
                if positionIndex < 100:
                    boardRow1 += " "
                boardRow1 += str(positionIndex)
                boardRow2 += "---"
            else:
                boardRow1 = boardRow1 + " " + col + " "
                boardRow2 += "---"
            boardRow1 += " | "
            boardRow2 += "---"
            positionIndex += 1
        if rowIndex == 0:
            print(boardRow2)
        print(boardRow1)
        print(boardRow2)
        rowIndex += 1
    print()
{{< / highlight >}}

I will have to put some practical limit on the board size to keep the game managable, but I wanted the printBoard() function to be able to handle a large size board. I added conditionals that tested if there are more than 10 or 100 positions on the board so I can add proper spacing and everything would line up. But I did think testing anything bigger than that would make the board too big and make the game too hard to play. However, I may be able to accomplish this by using some print formatting. I will look into that.

I could have used the same method of checking for a draw as my old program, but I decided to give it it’s dedicated function and actually test for any empty spaces on the board. Hopefully this is more robust.

{{< highlight python >}}
def checkForDraw(board):
    # Checks to see if there are any empty spaces on the board
    # If not, it is a draw
    for row in board:
        for col in row:
            if col is None:
                return False
    return True
{{< / highlight >}}

Next I will update the checkForWin() function. Since the size of the board is parameterized, I cannot store a list of all winning combinations before hand. I will need to create a list dynamically based off the size chosen by the user. In order to not calculate this every time I check for a win, I will call a function after the board size is chosen and send the resulting list to the checkForWin() function. So first, this is the function to create all possible winning combinations:

{{< highlight python >}}
def winningSequences(boardSize):
    # Return all the sequences that can win the game
    colSequences = []
    rowSequences = []
    for a in range(boardSize):
        col = []
        row = []
        for b in range(boardSize):
            col.append((a, b))
            row.append((b, a))
        colSequences.append(col)
        rowSequences.append(row)

    diagSequences = []
    diag1 = []
    diag2 = []
    for x in range(boardSize):
        diag1.append((x, x))
        diag2.append((x, boardSize-1-x))
    diagSequences.append(diag1)
    diagSequences.append(diag2)

    return colSequences + rowSequences + diagSequences
{{< / highlight >}}

Now this is the resulting checkForWin() function:

{{< highlight python >}}
def checkForWin(board, winningSequencesList):
    # Check if either player has won the game
    for seq in winningSequencesList:
        boardValues = []
        for (x, y) in seq:
            boardValues.append(board[x][y])
        if len(set(boardValues)) == 1 and boardValues[0] is not None:
            return True
    return False
{{< / highlight >}}

Next I remove the section of the playGame() function that gets the move from the user and add it into it’s own function. I do this so I can swap out this function for an AI function at some point in the future. This function is as follows:

{{< highlight python >}}
def get_move(board, player):
    boardSize = len(board)
    boardRange = boardSize * boardSize
    while True:
        answer = input("Please pick an unused space between 0 and " + str(boardRange-1) + ": ")
        if answer.isdigit():
            move = int(answer)
            row = int(move / boardSize)
            col = int(move % boardSize)
            if move in range(boardRange) and board[row][col] == None:
                break
            elif move not in range(boardRange):
                print("Choose a valid space.")
            else:
                print("Choose an unused space.")
        else:
            print("Please enter a valid number.")
    return (row, col)
{{< / highlight >}}

After creating the getMove() function, that leaves the playGame() function. Here is what is needed for that function:

{{< highlight python >}}
def playGame(board, winningSequencesList):
    # while there is not a win or draw, then ask the player for the next move
    win = 0
    draw = 0
    count = 0
    while win == 0 and draw == 0:  # =1 for win, =2 for draw, =0 to continue play
        printBoard(board)
        if count % 2 == 0:  # Even count is player X
            player = "X"
        else:  # Odd count is player O
            player = "O"
        move = get_move(board, player)
        board[move[0]][move[1]] = player
        draw = checkForDraw(board)
        win = checkForWin(board, winningSequencesList)
        count = count + 1
    printBoard(board)
    if draw:
        print("The game is a draw!")
    if win:
        print("Player " + player + " won!")
{{< / highlight >}}

So finally, here is the final version of the code:

{{< highlight python >}}
# Tic Tac Toe V2
# Another attempt at a Tic Tac Toe program challenge
# (from https://robertheaton.com/2018/10/09/programming-projects-for-advanced-beginners-3-a/)


def initializeBoard(boardSize):
    # Creates a 2 dimentional martix of boardSize
    board = []
    for x in range(boardSize):
        column = []
        for y in range(boardSize):
            column.append(None)
        board.append(column)
    return board


def printBoard(board):
    # Prints the current state of the board
    rowIndex = 0
    positionIndex = 0
    print()
    for row in board:
        boardRow1 = " | "
        boardRow2 = "---"
        for col in row:
            if col is None:
                if positionIndex < 10:
                    boardRow1 += " "
                if positionIndex < 100:
                    boardRow1 += " "
                boardRow1 += str(positionIndex)
                boardRow2 += "---"
            else:
                boardRow1 = boardRow1 + " " + col + " "
                boardRow2 += "---"
            boardRow1 += " | "
            boardRow2 += "---"
            positionIndex += 1
        if rowIndex == 0:
            print(boardRow2)
        print(boardRow1)
        print(boardRow2)
        rowIndex += 1
    print()


def checkForWin(board, winningSequencesList):
    # Check if either player has won the game
    for seq in winningSequencesList:
        boardValues = []
        for (x, y) in seq:
            boardValues.append(board[x][y])
        if len(set(boardValues)) == 1 and boardValues[0] is not None:
            return True
    return False


def winningSequences(boardSize):
    # Return all the sequences that can win the game
    colSequences = []
    rowSequences = []
    for a in range(boardSize):
        col = []
        row = []
        for b in range(boardSize):
            col.append((a, b))
            row.append((b, a))
        colSequences.append(col)
        rowSequences.append(row)

    diagSequences = []
    diag1 = []
    diag2 = []
    for x in range(boardSize):
        diag1.append((x, x))
        diag2.append((x, boardSize-1-x))
    diagSequences.append(diag1)
    diagSequences.append(diag2)

    return colSequences + rowSequences + diagSequences


def checkForDraw(board):
    # Checks to see if there are any empty spaces on the board
    # If not, it is a draw
    for row in board:
        for col in row:
            if col is None:
                return False
    return True


def get_move(board, player):
    boardSize = len(board)
    boardRange = boardSize * boardSize
    while True:
        answer = input("Please pick an unused space between 0 and " + str(boardRange-1) + ": ")
        if answer.isdigit():
            move = int(answer)
            row = int(move / boardSize)
            col = int(move % boardSize)
            if move in range(boardRange) and board[row][col] == None:
                break
            elif move not in range(boardRange):
                print("Choose a valid space.")
            else:
                print("Choose an unused space.")
        else:
            print("Please enter a valid number.")
    return (row, col)


def playGame(board, winningSequencesList):
    # while there is not a win or draw, then ask the player for the next move
    win = 0
    draw = 0
    count = 0
    while win == 0 and draw == 0:  # =1 for win, =2 for draw, =0 to continue play
        printBoard(board)
        if count % 2 == 0:  # Even count is player X
            player = "X"
        else:  # Odd count is player O
            player = "O"
        move = get_move(board, player)
        board[move[0]][move[1]] = player
        draw = checkForDraw(board)
        win = checkForWin(board, winningSequencesList)
        count = count + 1
    printBoard(board)
    if draw:
        print("The game is a draw!")
    if win:
        print("Player " + player + " won!")


play = input("Would you like to play Tic Tac Toe? (Y/N) ")

if "y" not in play.lower():
    print("Ok, bye!")
    exit(1)

while "y" in play.lower():
    boardSize = int(input("What size board do you want to play with? "))
    board = initializeBoard(boardSize)
    winningSequencesList = winningSequences(boardSize)
    playGame(board, winningSequencesList)
    play = input("Would you like to play again? (Y/N) ")

print("Thanks for playing")
{{< / highlight >}}

So that is how I improved on my solution to the Tic Tac Toe programming challenge in Python. Let me know if you solved it in a different way in the comments below.

Brian