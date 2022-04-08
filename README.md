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

// Fruit Position
int fruitx, fruity;

int score, gameover, flag;
int height, width;

// setup the snake game
void setup()
{ //Intialise the Snake
	height = 20, width = 40;
	x = width / 2, y = height / 2;
	score = 0;
	srand(time(0));
// Initialising fruitx position using rand() function
label1:
	fruitx = rand() % 40;
	if (fruitx == 0)
	{
		goto label1;
	}
// Initialising fruity position using rand() function
label2:
	fruity = rand() % 20;
	if (fruity == 0)
	{
		goto label2;
	}
}

//Loading function for printing loading Screen
void loading()
{
	int i, j, width, height;
	height = 20, width = 40;

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
	//Nested Loop for printing Loading...
	for (int loop = 0; loop < 3; ++loop)
	{
		for (int x = 0; x < 4; ++x)
		{gotoxy(16,10);
			printf("Loading%.*s   \b\b\b", x, "...");
			fflush(stdout); //force printing as no newline in output
			usleep(700000);
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
					printf("o");
				}
				else if (i == fruitx && j == fruity)
				{
					printf("*");
				}
				else
				{
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
		case 'a':
			flag = 1;
			break;
		case 's':
			flag = 2;
			break;
		case 'd':
			flag = 3;
			break;
		case 'w':
			flag = 4;
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
	usleep(90000);
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

		//generate a new fruit
	label3:
		fruitx = rand() % 40;
		if (fruitx == 0)
		{
			goto label3;
		}
	label4:
		fruity = rand() % 20;
		if (fruity == 0)
		{
			goto label4;
		}
	}
	else if (x < 1 || x > width - 1 || y < 1 || y > height)
	{
		gameover = 1;
		gotoxy(16,10);
		if (gameover == 1)
		{
			gotoxy(40, 20);
			printf("\nHighscore %d", score);
		}
	}
}

//function declaration
int main()
{         setup(); //Generate Boundary
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
