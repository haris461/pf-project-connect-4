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

int board_info[HEIGHT][WIDTH] = { {0,0,0,0,0,0,0},
                                 {0,0,0,0,0,0,0},
                                 {0,0,0,0,0,0,0},
                                 {0,0,0,0,0,0,0},
                                 {0,0,0,0,0,0,0},
                                 {0,0,0,0,0,0,0} };

                                 

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

