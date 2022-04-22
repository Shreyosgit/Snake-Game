# Snake-Game
Hello! everyone this is my first project on building a Snake Game in command prompt using C

```c
// Making the Snake Game
#include <stdio.h>
#include <stdlib.h>
#include <conio.h>
#include <unistd.h>
#include <time.h>

int i, j;

// Snake Position
int x, y;

// Snake Node Coordinates
int snakenodeX[1000], snakenodeY[1000];

// Snake Length
int snakelength;
int k, L, P;

// Fruit Position
int fruitx, fruity;

int score, gameover, flag;
int height, width;

// setup the snake game
void setup()
{ //Intialise the Snake
	height = 20, width = 56;
	x = width / 2, y = height / 2;
	snakelength = 0;
	score = 0;
	srand(time(0));
// Initialising fruitx position using rand() function
label1:
	fruitx = rand() % ((width - 3) + 1); //rand() % (upperlimit-(lowerlimit+1)+lowerlimit)
	if (fruitx == 0 || fruitx == snakenodeX[L])
	{
		goto label1;
	}
// Initialising fruity position using rand() function
label2:
	fruity = rand() % ((height - 3) + 1); //rand() % (upperlimit-(lowerlimit+1)+lowerlimit)
	if (fruity == 0 || fruity == snakenodeY[L])
	{
		goto label2;
	}
}

//Loading function for printing ASCII art and loading Screen
void loading()
{
	char play;

	//printing the boundary
	for (i = 0; i < height; i++)
	{
		for (j = 0; j < width; j++)
		{
			if (i == 0 || i == height - 1)
			{
				printf("_");
			}
			else if (j == 0 || j == width - 1)
			{
				printf("|");
			}
			else
			{
				printf(" ");
			}
		}
		printf("\n");
	}

	//Printing ASCII art
	gotoxy(4, 8);
	printf(" _____            _          _____                \n");
	gotoxy(4, 9);
	printf("|   __| ___  ___ | |_  ___  |   __| ___  ___  ___ \n");
	gotoxy(4, 10);
	printf("|__   ||   || .’|| ‘_|| -_| |__   || .’|| . || .’|\n");
	gotoxy(4, 11);
	printf("|_____||_|_||__,||_,_||___| |_____||__,||_  ||__,|\n");
	gotoxy(4, 12);
	printf("                                        |___|     \n");

	gotoxy(56, 20);
	printf("\nPress P to Continue!");
	scanf(" %c", &play);

	//Nested Loop for printing the Boundary and Loading...
	if (play == 'p' || play == 'P')
	{
		system("cls");
		for (i = 0; i < height; i++)
		{
			for (j = 0; j < width; j++)
			{
				if (i == 0 || i == height - 1)
				{
					printf("_");
				}
				else if (j == 0 || j == width - 1)
				{
					printf("|");
				}
				else
				{
					printf(" ");
				}
			}
			printf("\n");
		}
		for (int loop = 0; loop < 2; ++loop)
		{
			for (int x = 0; x < 4; ++x)
			{
				gotoxy(24, 10);
				printf("Loading%.*s   \b\b\b", x, "...");
				fflush(stdout); //force printing as no newline in output
				usleep(700000);
			}
		}
	}
}

// Draw() funtion definiton
void draw()
{
	system("cls");

	//Nested loop for printing the Boundary
	for (j = 0; j < height; j++)
	{
		for (i = 0; i < width; i++)
		{
			if (j == 0 || j == height - 1)
			{
				printf("_");
			}
			else if (i == 0 || i == width - 1)
			{
				printf("|");
			}
			// Printing snake
			else
			{
				if (i == x && j == y)
				{
					printf("O");
				}
				else if (i == fruitx && j == fruity)
				{
					printf("*");
				}
				else
				{
					int ch = 0;
					for (int P = 0; P < snakelength; P++)
					{
						if (i == snakenodeX[P] && j == snakenodeY[P])
						{
							printf("+");
							ch = 1;
						}
					}
					if (ch == 0)
						printf(" ");
				}
			}
		}
		printf("\n");
	}
}

// input() function definition
void input()
{ //Using Kbhit() to identify the input and getch() to get the input
	if (kbhit())
	{
		switch (getch())
		{
		case 'A':
			flag = 1;
			break;
		case 'a':
			flag = 1;
			break;
		case 'S':
			flag = 2;
			break;
		case 's':
			flag = 2;
			break;
		case 'D':
			flag = 3;
			break;
		case 'd':
			flag = 3;
			break;
		case 'W':
			flag = 4;
			break;
		case 'w':
			flag = 4;
			break;
		case 'X':
			flag = 5;
			break;
		case 'x':
			flag = 5;
			break;
		}
	}
}

// logic() function definition
void logic()
{
	int delay;
	//when score is 0 delay will be 90000 microseconds
	if (score == 0)
	{
		delay = 90000;
	} //when score is >0 delay will be decreased
	else if (score != 0)
	{
		delay = 90000 - score * 10;
	}

	usleep(delay);

	int prevX = snakenodeX[0]; // storing the first node coordinate of snakelengthX & Y array to prevX & Y
	int prevY = snakenodeY[0];
	snakenodeX[0] = x;
	snakenodeY[0] = y;

	for (L = 1; L < snakelength; L++)
	{
		int prev2X = snakenodeX[L];
		int prev2Y = snakenodeY[L];
		snakenodeX[L] = prevX;
		snakenodeY[L] = prevY;
		prevX = prev2X;
		prevY = prev2Y;
	}
	switch (flag)
	{
	case 1:
		x--;
		break;
	case 2:
		y++;
		break;
	case 3:
		x++;
		break;
	case 4:
		y--;
		break;
	case 5:
		break;
	}
	// After Eating a fruit
	if (x == fruitx && y == fruity)
	{
		score += 10;
		printf("Score %d", score);
		snakelength++;

		//generate a new fruit
	label3:
		fruitx = rand() % ((width - 3) + 1); //rand() % (upperlimit-(lowerlimit+1)+lowerlimit)
		if (fruitx == 0 || fruitx == snakenodeX[L])
		{
			goto label3;
		}
	label4:
		fruity = rand() % ((height - 3) + 1); //rand() % (upperlimit-(lowerlimit+1)+lowerlimit)
		if (fruity == 0 || fruity == snakenodeY[L])
		{
			goto label4;
		}
	}
	for (L = 0; L < snakelength; L++)
	{
		if (x == snakenodeX[L] && y == snakenodeY[L])
		{
			gameover = 1;
		}
	}

	if (gameover == 1)
	{ //Game Over ASCII art
		gotoxy(8, 8);
		printf(" _____                  _____\n");
		gotoxy(8, 9);
		printf("|   __|___ _____ ___   |     |_ _ ___ ___\n");
		gotoxy(8, 10);
		printf("|  |  | .'|     | -_|  |  |  | | | -_|  _|\n");
		gotoxy(8, 11);
		printf("|_____|__,|_|_|_|___|  |_____|\\_/|___|_|");
	}
	if (gameover == 1)
	{
		gotoxy(56, 20);
		printf("\nScore %d", score);
	}
	//wall teleportation Syntax
	if (x < 1)
	{
		x = width - 1;
	}
	if (x > width - 1)
	{
		x = 1;
	}
	else if (y < 1)
	{
		y = height - 1;
	}
	else if (y > height - 1)
	{
		y = 1;
	}
}

//function declaration
int main()
{
	setup();   //Generate Boundary
	loading(); //Prints loading screen
	// Untill the game is over
	while (!gameover)
	{
		draw();
		input();
		logic();
	}
}
```
