# xoxgo

Welcome to **xoxgo**! yes, i know, another tic-tac-toe game, but this one is special because i wrote it. in Go. in just 3 days. and coded it in 2 hours. so, prepare to be moderately impressed. or not. idc actually.

## What's the Deal?

This is a classic 3x3 Tic-Tac-Toe (or XoX game, call with what do you want) game where you can challenge an ai that was created with a touch of randomness, a sprinkle of frustration, and a dash of "why did I do this?" to make it just a bit more interesting. perfect for those moments when you just need to outsmart a few lines of code.

## Features (if you can call them that)

- **Play Against AI**: Because playing against yourself is no fun.
- **Simple and Fast**: i made it in 2 hours. dont expect too much. (in fact, i spent more time writing this documentation than i did writing code.)
- **Humorous and real.**: You willll feel my pain when you read the goddamm comments.
- **Basic Input Handling**: The game actually understands what you're typing. just don't go it with too harsh, please?
- **No Restart Option**: Because who have second chances in life?

## How to Run (as if you didn't know at all)

1. make sure you have Go installed. If not, [get it here](https://go.dev/dl/).
2. clone this repository (like every other project) .
	 `git clone https://github.com/mvarr/xoxgo.git 
	 `cd xoxgo
3. type`go run xoxgo.go` in your terminal.
4. Follow the instructions on the screen. make your move by entering the row and column numbers (1-3). dont mess it up.

## How to Play

1. **Start the game**: start up your terminal and watch the magic happen.
2. **Make your move**: Enter your choice of row and column when prompted.
3. **Let the AI have a go**: sit back and watch as the ai makes its (probably not-so-strategic) move.
4. **Win or lose**: The game will tell you if you're won or if the ai has managed to snatch victory from the jaws of randomness. or, yk, it could be a draw.

## Code Walkthrough

#### Game Board Printing Function

```
func gameboard(board [][]string) {
	for i := 0; i < len(board); i++ {
	fmt.Println(board[i])
	}
}
```
this function is literally just printing the board. i know, dull stuff.

#### Player Move

```
func playerchoice(board [][]string, freenumbers [][]bool) {
	for {
		var row, columm int
		fmt.Println("Please make a choice:\nrow number (1-3):")
		fmt.Scanln(&row)
		row = row - 1
		fmt.Println("columm number (1-3):")
		fmt.Scanln(&columm)
		columm = columm - 1
		if row <= 3 && columm <= 3 {
			if freenumbers[row][columm] == true {
				board[row][columm] = "X" // replaced "_" with "X"
				freenumbers[row][columm] = false
				break
			} else {
				fmt.Println("You tried to fill a box that already exists. Please try again.")
			}
		} else {
			fmt.Println("You have entered an incorrect entry, please try again")
		}
	}
}
```

here, the player makes a move. if you try to make an invalid move, the game will politely tell you to try again. because, well, rules.

#### AI Move

```
func ai(board [][]string, freenumbers [][]bool) {
	airow := rand.Intn(3)
	aicolumm := rand.Intn(3)
	if freenumbers[airow][aicolumm] == false {
		for freenumbers[airow][aicolumm] != true {
			airow = rand.Intn(3)
			aicolumm = rand.Intn(3)
		}
	}
	board[airow][aicolumm] = "O"
	freenumbers[airow][aicolumm] = false
}
```

the ai makes its move randomly because sophisticated algorithms are overrated. if it picks a taken spot, it keeps trying until it finds a free one. consistance is key.

#### Game Status Check

```
func gamestatus(board [][]string) {
	if board[0][0] == "X" && board[0][1] == "X" && board[0][2] == "X" || board[1][0] == "X" && board[1][1] == "X" && board[1][2] == "X" || board[2][0] == "X" && board[2][1] == "X" && board[2][2] == "X" || board[0][0] == "X" && board[1][1] == "X" && board[2][2] == "X" || board[0][2] == "X" && board[1][1] == "X" && board[2][0] == "X" || board[0][0] == "X" && board[1][0] == "X" && board[2][0] == "X" || board[0][2] == "X" && board[1][2] == "X" && board[2][2] == "X" || board[0][1] == "X" && board[1][1] == "X" && board[2][1] == "X" {
		fmt.Println("Player won!") write
		os.Exit(0)
	}
	if board[0][0] == "O" && board[0][1] == "O" && board[0][2] == "O" || board[1][0] == "O" && board[1][1] == "O" && board[1][2] == "O" || board[2][0] == "O" && board[2][1] == "O" && board[2][2] == "O" || board[0][0] == "O" && board[1][1] == "O" && board[2][2] == "O" || board[0][2] == "O" && board[1][1] == "O" && board[2][0] == "O" || board[0][0] == "O" && board[1][0] == "O" && board[2][0] == "O" || board[0][2] == "O" && board[1][2] == "O" && board[2][2] == "O" || board[0][1] == "O" && board[1][1] == "O" && board[2][1] == "O" {
		fmt.Println("Computer Won!")
		os.Exit(0)
	}
}
```

Checking who won is basically a giant mess of "or" statements. im not proud of it, but hey, it works. if sees there’s a winner, it graciously announces the winner, refuses to elaborate further and leaves. if not, the game goes on.

#### Game Engine

```
func engine(board [][]string, freenumbers [][]bool) { 
	for i := 0; i < 4; i++ {
		fmt.Println("Compuer is making his move...")
		ai(board, freenumbers)
		gameboard(board)
		gamestatus(board)
		playerchoice(board, freenumbers)
		fmt.Println("Updated board:")
		gameboard(board)
		gamestatus(board)
	}
}
```

the engine runs the game in turns. ai goes first (cause I said so), then you. this loops for 4 rounds (or if someone decides to end my pain and wins), so 8 moves in total.

#### Main Function

```
func main() {
	fmt.Println("Welcome to XoX game!")
	fmt.Println("Board initializing...")
	freenumbers := [][]bool{
		{true, true, true},
		{true, true, true},
		{true, true, true},
	}
	_ = freenumbers
	board := [][]string{
		{"_", "_", "_"},
		{"_", "_", "_"},
		{"_", "_", "_"},
	}
	fmt.Println("Game board:")
	gameboard(board)
	if rand.Intn(2) == 1 {
		playerchoice(board, freenumbers)
		fmt.Println("Updated board:")
		gameboard(board)
		engine(board, freenumbers)
		fmt.Println("Game ended in a draw.")
	} else {
		engine(board, freenumbers)
		ai(board, freenumbers)
		fmt.Println("Updated board:")
		gameboard(board)
		gamestatus(board)
		fmt.Println("Game ended in a draw.")
	}
}
```

This is where the fun begins. game initializes, and then we randomly decide if the player or the ai goes first.

## Conclusion

so there you have it. [I've Said It Before and I'll Say It Again](https://www.youtube.com/watch?v=91lJhEzMaH4)  thats my first Go project, done in a couple of hours after learning the language in a few days. it's not perfect, but it's mine. and remember, this game is a testament to what can be achieved in a few hours with a touch of sarcasm and a lot of trial and error. its also a reminder that even when things go chaotic, it’s okay to laugh it off and move on. enjoy the chaos (or at least pretend you do.) , and may the force with X's win!
