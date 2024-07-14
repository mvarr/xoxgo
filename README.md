package main

import (
	"fmt"
	"math/rand"
	"os"
)

func gameboard(board [][]string) { //this is for printing board to screen easily
	for i := 0; i < len(board); i++ {
		fmt.Println(board[i])
	}
}

func playerchoice(board [][]string, freenumbers [][]bool) { // this is where player make moves
	for {
		var row, columm int
		fmt.Println("Please make a choice:\nrow number (1-3):")
		fmt.Scanln(&row)
		row = row - 1                       //			>---------------\
		fmt.Println("columm number (1-3):") //		 				 	|
		fmt.Scanln(&columm)                 //						 	|
		columm = columm - 1                 //			>----------------\_____> matched user row-column count with
		if row <= 3 && columm <= 3 {        //								 computer row-column count
			if freenumbers[row][columm] == true {
				board[row][columm] = "X" // 		replaced "_" with "X"
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

func gamestatus(board [][]string) {
	if board[0][0] == "X" && board[0][1] == "X" && board[0][2] == "X" || board[1][0] == "X" && board[1][1] == "X" && board[1][2] == "X" || board[2][0] == "X" && board[2][1] == "X" && board[2][2] == "X" || board[0][0] == "X" && board[1][1] == "X" && board[2][2] == "X" || board[0][2] == "X" && board[1][1] == "X" && board[2][0] == "X" || board[0][0] == "X" && board[1][0] == "X" && board[2][0] == "X" || board[0][2] == "X" && board[1][2] == "X" && board[2][2] == "X" || board[0][1] == "X" && board[1][1] == "X" && board[2][1] == "X" {
		fmt.Println("Player won!") //to be honest, i have no better idea than write
		os.Exit(0)                 //this astronomic "or and" statement
	}
	if board[0][0] == "O" && board[0][1] == "O" && board[0][2] == "O" || board[1][0] == "O" && board[1][1] == "O" && board[1][2] == "O" || board[2][0] == "O" && board[2][1] == "O" && board[2][2] == "O" || board[0][0] == "O" && board[1][1] == "O" && board[2][2] == "O" || board[0][2] == "O" && board[1][1] == "O" && board[2][0] == "O" || board[0][0] == "O" && board[1][0] == "O" && board[2][0] == "O" || board[0][2] == "O" && board[1][2] == "O" && board[2][2] == "O" || board[0][1] == "O" && board[1][1] == "O" && board[2][1] == "O" {
		fmt.Println("Computer Won!")
		os.Exit(0) //i tried to implement "do you want to restart?" function, but ended up with frustration.
	}
}

func engine(board [][]string, freenumbers [][]bool) { //this is where is game systemized, first ai then player
	for i := 0; i < 4; i++ { //and repeat this 4 times. this means 8 moves made in this function. first -or last-
		fmt.Println("Compuer is making his move...") //is in the main function.
		ai(board, freenumbers)
		gameboard(board)
		gamestatus(board)
		playerchoice(board, freenumbers)
		fmt.Println("Updated board:")
		gameboard(board)
		gamestatus(board)
	}
}

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
	if rand.Intn(2) == 1 { //deciding who will be starting first.
		playerchoice(board, freenumbers) //bright side.
		fmt.Println("Updated board:")
		gameboard(board)
		engine(board, freenumbers)
		fmt.Println("Game ended in a draw.")
	} else { //dark side.
		engine(board, freenumbers)
		ai(board, freenumbers)
		fmt.Println("Updated board:")
		gameboard(board)
		gamestatus(board)
		fmt.Println("Game ended in a draw.")
	}
}
