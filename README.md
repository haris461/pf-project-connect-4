#include <iostream>
#include <stdlib.h> 
#include <windows.h>

#define HEIGHT 6
#define WIDTH 7
using namespace std;
void draw_board();
void player_movement(int player);
bool check_for_winner(int x, int y, int player);
bool check_diagonal_combo_SW_NE(int x, int y, int player);
bool check_diagonal_combo_NW_SE(int x, int y, int player);
bool check_vertical_combo(int x, int y, int player);
bool check_horizontal_combo(int x, int y, int player);

The using namespace std; statement is used for convenience, allowing the use of cout and cin without the std:: prefix.
Function declarations for various game functions such as drawing the board, player movement, and checking for winning combinations.

int board_info[HEIGHT][WIDTH] = { {0,0,0,0,0,0,0},
                                 {0,0,0,0,0,0,0},
                                 {0,0,0,0,0,0,0},
                                 {0,0,0,0,0,0,0},
                                 {0,0,0,0,0,0,0},
                                 {0,0,0,0,0,0,0} };

              A 2D array board_info representing the game board is initialized with zeros. Each element corresponds to a cell on the board.



                                 

                                 void draw_board()
{
    cout << endl;

    for (int y = 0; y < HEIGHT; y++)
    {
        for (int x = 0; x < WIDTH; x++)
        {
            cout << "| ";
            if (board_info[y][x] == 0) cout << " ";
            else if (board_info[y][x] == 1) cout << "X";
            else if (board_info[y][x] == 2) cout << "O";
        }
        cout << "\n---------------------" << endl;
    }
}
//This function prints the current state of the game board.

void player_movement(int player)
{

    int choice;
    cout << "\nPlayer" << player << ", please select a number from 1 - 7: ";
    cin >> choice;


    if (cin.fail())
    {
        cout << "Error!";
        exit(1);
    }

    while (choice > WIDTH || choice <= 0)
    {
        cout << "\nPlease select again: ";
        cin >> choice;
    }

    int number = 0;
    while (board_info[(HEIGHT - 1) - number][(choice - 1)] != 0)
    {
        number++;
        if (number > (HEIGHT - 1))
        {
            cout << "\nPlease select again: ";
            cin >> choice;
            number = 0;
        }
    };

    board_info[(HEIGHT - 1) - number][choice - 1] = player;
    LastMoveY = (HEIGHT - 1) - number;
    LastMoveX = choice - 1;
}

//This function handles the movement of a player, taking input from the user, validating it,
and updating the game board accordingly.
These functions are responsible for checking if a player has won the game based on the latest move. They check for winning combinations horizontally, vertically, and diagonally.
Main Function:
(Not included in the provided code snippet)

Overall, the program sets up the game board, allows players to make moves, and checks for a winner after each move. The game is represented in the console window, and players take turns making moves until a winner is determined or the board is filled.






bool check_for_winner(int x, int y, int player)
{
    bool winner;

    if (check_diagonal_combo_SW_NE(x, y, player)) return true;
    else if (check_diagonal_combo_NW_SE(x, y, player)) return true;
    else if (check_vertical_combo(x, y, player)) return true;
    else if (check_horizontal_combo(x, y, player)) return true;
    else return false;
}

The check_for_winner function is designed to determine if the current move made by a player at position (x, y) has resulted in a winning combination. It calls four other functions to check for a win in various directions: diagonally from southwest to northeast (check_diagonal_combo_SW_NE), diagonally from northwest to southeast (check_diagonal_combo_NW_SE), vertically (check_vertical_combo), and horizontally (check_horizontal_combo). The function returns true if any of these checks indicates a winning combination, and false otherwise.






bool check_diagonal_combo_SW_NE(int x, int y, int player)
{
    int score = 1;
    int count = 1;

    while ((y - count >= 0) && (x + count < WIDTH))
    {
        if (board_info[y - count][x + count] == player)
        {
            score++;
            count++;
        }
        else break;
    }

    count = 1;
    while ((y + count < HEIGHT) && (x - count >= 0))
    {
        if (board_info[y + count][x - count] == player)
        {
            score++;
            count++;
        }
        else break;
    }

    if (score == 4) return true;
    else return false;
}



The check_diagonal_combo_SW_NE function checks for a winning combination diagonally from southwest to northeast starting from the given coordinates (x, y). It counts the consecutive occurrences of the player's symbol in both directions and returns true if there are at least four in a row.



bool check_diagonal_combo_NW_SE(int x, int y, int player)
{
    int score = 1;
    int count = 1;

    while ((y + count >= 0) && (x + count < WIDTH))
    {
        if (board_info[y + count][x + count] == player)
        {
            score++;
            count++;
        }
        else break;
    }

    count = 1;
    while ((y - count < HEIGHT) && (x - count >= 0))
    {
        if (board_info[y - count][x - count] == player)
        {
            score++;
            count++;
        }
        else break;
    }

    if (score == 4) return true;
    else return false;
}


The check_diagonal_combo_NW_SE function checks for a winning combination diagonally from northwest to southeast starting from the given coordinates (x, y). Similar to the previous function (check_diagonal_combo_SW_NE), it counts the consecutive occurrences of the player's symbol in both diagonal directions and returns true if there are at least four in a row.






bool check_vertical_combo(int x, int y, int player)
{
    int score = 1;
    int count = 1;

    while (y + count >= 0 && y + count < HEIGHT)
    {
        if (board_info[y + count][x] == player)
        {
            score++;
            count++;
        }
        else break;
    }

    if (score == 4) return true;
    else return false;
}




The check_horizontal_combo function checks for a winning combination horizontally starting from the given coordinates (x, y). It counts the consecutive occurrences of the player's symbol in both left and right directions and returns true if there are at least four in a row.



bool check_horizontal_combo(int x, int y, int player)
{
    int score = 1;
    int count = 1;

    while ((x + count >= 0) && (x + count < WIDTH))
    {
        if (board_info[y][x + count] == player)
        {
            score++;
            count++;
        }
        else break;
    }

    count = 1;
    while ((x - count < WIDTH) && (x - count >= 0))
    {
        if (board_info[y][x - count] == player)
        {
            score++;
            count++;
        }
        else break;
    }

    if (score == 4) return true;
    else return false;
}





                                 

int LastMoveX, LastMoveY;

int main()
{
    int counter = 0;
    bool winner = false;

    srand(GetTickCount());
    cout << "Please select a number from 1-7" << endl;
    cout << "| 1| 2| 3| 4| 5| 6| 7" << endl;
    cout << "---------------------";
    draw_board();

    for (int i = 0; i < 21; i++)
    {
        player_movement(1);
        draw_board();
        winner = check_for_winner(LastMoveX, LastMoveY, 1);
        if (winner)
        {
            cout << "\nYou Win" << endl;
            break;
        }

        player_movement(2);
        draw_board();
        winner = check_for_winner(LastMoveX, LastMoveY, 2);
        if (winner)
        {
            cout << "\nYou Win" << endl;
            break;
        }
    }

    system("PAUSE");
    return 0;
}

