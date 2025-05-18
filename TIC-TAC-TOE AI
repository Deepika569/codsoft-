from tkinter import *
from tkinter import messagebox

root = Tk()
root.geometry("330x550")
root.title("Tic Tac Toe")
root.resizable(0, 0)

frame1 = Frame(root)
frame1.pack()
titleLabel = Label(frame1, text="Tic Tac Toe", font=("arial", 26), bg="MidnightBlue", fg="white", width=16)
titleLabel.grid(row=0, column=0)

optionFrame = Frame(root, bg="beige")
optionFrame.pack()

frame2 = Frame(root, bg="DarkRed")
frame2.pack()

statusFrame = Frame(root, bg="beige")
statusFrame.pack()

statusLabel = Label(statusFrame, text="Turn: X", font=("arial", 20), bg="beige")
statusLabel.grid(row=0, column=0)

board = {1: " ", 2: " ", 3: " ",
         4: " ", 5: " ", 6: " ",
         7: " ", 8: " ", 9: " "}

turn = "x"
game_end = False
mode = "singlePlayer"

def changeModeToSinglePlayer():
    global mode
    mode = "singlePlayer"
    singlePlayerButton["bg"] = "SteelBlue"
    multiPlayerButton["bg"] = "beige"

def changeModeToMultiplayer():
    global mode
    mode = "multiPlayer"
    multiPlayerButton["bg"] = "SteelBlue"
    singlePlayerButton["bg"] = "beige"

def updateBoard():
    for key in board.keys():
        buttons[key-1]["text"] = board[key]

def checkForWin(player):
    # rows
    if (board[1] == board[2] == board[3] == player) or \
       (board[4] == board[5] == board[6] == player) or \
       (board[7] == board[8] == board[9] == player):
        return True
    # columns
    elif (board[1] == board[4] == board[7] == player) or \
         (board[2] == board[5] == board[8] == player) or \
         (board[3] == board[6] == board[9] == player):
        return True
    # diagonals
    elif (board[1] == board[5] == board[9] == player) or \
         (board[3] == board[5] == board[7] == player):
        return True
    return False

def checkForDraw():
    return all(board[key] != " " for key in board.keys())

def restartGame():
    if messagebox.askyesno("Restart Game", "Are you sure you want to restart the game?"):
        global game_end
        game_end = False
        for button in buttons:
            button["text"] = " "

        for i in board.keys():
            board[i] = " "

        titleLabel.config(text="Tic Tac Toe")
        statusLabel.config(text="Turn: X")
        updateBoard()

def minimax(board, isMaximizing):
    if checkForWin("o"):
        return 1
    if checkForWin("x"):
        return -1
    if checkForDraw():
        return 0

    if isMaximizing:
        bestScore = -100
        for key in board.keys():
            if board[key] == " ":
                board[key] = "o"
                score = minimax(board, False)
                board[key] = " "
                bestScore = max(score, bestScore)
        return bestScore
    else:
        bestScore = 100
        for key in board.keys():
            if board[key] == " ":
                board[key] = "x"
                score = minimax(board, True)
                board[key] = " "
                bestScore = min(score, bestScore)
        return bestScore

def playComputer():
    bestScore = -100
    bestMove = 0
    for key in board.keys():
        if board[key] == " ":
            board[key] = "o"
            score = minimax(board, False)
            board[key] = " "
            if score > bestScore:
                bestScore = score
                bestMove = key
    board[bestMove] = "o"
    updateBoard()

def play(event):
    global turn, game_end
    if game_end:
        return

    button = event.widget
    buttonText = str(button)
    clicked = buttonText[-1]
    if clicked == "n":
        clicked = 1
    else:
        clicked = int(clicked)

    if button["text"] == " ":
        if turn == "x":
            board[clicked] = turn
            if checkForWin(turn):
                winningLabel = Label(frame1, text=f"{turn.upper()} wins the game", bg="MidnightBlue", font=("Arial", 26), width=16)
                winningLabel.grid(row=0, column=0, columnspan=3)
                game_end = True
            turn = "o"
            statusLabel.config(text="Turn: O")
            updateBoard()
            if mode == "singlePlayer" and not game_end:
                playComputer()
                if checkForWin("o"):
                    winningLabel = Label(frame1, text=f"O wins the game", bg="MidnightBlue", font=("Arial", 26), width=16)
                    winningLabel.grid(row=0, column=0, columnspan=3)
                    game_end = True
                turn = "x"
                statusLabel.config(text="Turn: X")
                updateBoard()
        else:
            board[clicked] = turn
            updateBoard()
            if checkForWin(turn):
                winningLabel = Label(frame1, text=f"{turn.upper()} wins the game", bg="MidnightBlue", font=("Arial", 26), width=16)
                winningLabel.grid(row=0, column=0, columnspan=3)
                game_end = True
            turn = "x"
            statusLabel.config(text="Turn: X")

        if checkForDraw() and not game_end:
            drawLabel = Label(frame1, text=f"Game Draw", bg="MidnightBlue", font=("Arial", 26), width=16)
            drawLabel.grid(row=0, column=0, columnspan=3)
            game_end = True

# Change Mode options
singlePlayerButton = Button(optionFrame, text="SinglePlayer", width=13, height=1, font=("Arial", 15), bg="beige", relief=RAISED, borderwidth=5, command=changeModeToSinglePlayer)
singlePlayerButton.grid(row=0, column=0, columnspan=1, sticky=NW)

multiPlayerButton = Button(optionFrame, text="Multiplayer", width=13, height=1, font=("Arial", 15), bg="beige", relief=RAISED, borderwidth=5, command=changeModeToMultiplayer)
multiPlayerButton.grid(row=0, column=1, columnspan=1, sticky=NW)

# Tic Tac Toe Board
buttons = []
for i in range(3):
    for j in range(3):
        button = Button(frame2, text=" ", width=4, height=2, font=("Arial", 30), bg="DarkRed", fg="white", relief=RAISED, borderwidth=5)
        button.grid(row=i, column=j)
        button.bind("<Button-1>", play)
        buttons.append(button)

restartButton = Button(frame2, text="Restart Game", width=19, height=1, font=("Arial", 20), bg="beige", relief=RAISED, borderwidth=5, command=restartGame)
restartButton.grid(row=3, column=0, columnspan=3)

root.mainloop() # type: ignore
