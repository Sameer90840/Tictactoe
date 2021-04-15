#include<bits/stdc++.h>
using namespace std;

char** createBoard(int boardSize=3)
{ // ** is array of pointers(2d arrays)
  // here new is returning dynamically allocated array
   char** board = new char*[boardSize];
   for(int i=0; i<boardSize; i++)
   {
       board[i] = new char[boardSize]; //2d array ....every index of board stored a array(2D)
   }
     return board;
}
void labelBoard(char **board, int boardSize)
{
    char counter = '0';
    for(int i=0; i<boardSize; i++)
    {
        for(int j=0; j<boardSize; j++)
        {
            board[i][j] = counter++;
        }
    }
}
void displayBoard(char **board, int boardSize)
{
    system("cls");
    printf("\n\n");

    int unitSize = 3 * boardSize; // 4 times bcoz of one " " , board[i][j] and two " |"
    //Logic starts here
    for(int i=0; i<boardSize; i++)
    {
        for(int j=0; j<boardSize; j++)
        {
            if(j < boardSize-1)
            
                cout << " " << board[i][j] << " |";
            else
                 cout << " " << board[i][j] << " ";
            
        }
        cout << endl;
        if(i < boardSize - 1)
            for(int k=0; k<=unitSize; k++)
            cout << "-";
            
        cout<<endl;
        
    }
    cout<<endl;
}
void choiceToCords(int *x, int *y, int boardSize)
{
    int ch;
    cin >> ch;

    *x = ch / boardSize; // it gives the row number
    *y = ch % boardSize; // it gives the column number;
}
int checkWinCondition(char **board, int boardSize, int playerTurn)
{
    for(int i=0; i<boardSize; i++)
    {
        int rowVal = board[i][0]; int rowMatch=1; //to check row 
        int colVal = board[0][i]; int colMatch=1; //to check column
        for(int j=0; j < boardSize; j++)
        {
            if(board[i][j]==rowVal)
            rowMatch++;
            if(board[j][i]==colVal)
            colMatch++;
        }
        if(rowMatch == boardSize + 1 || colMatch == boardSize + 1)
          return playerTurn;
    }
      char leftD = board[0][0]; // to check the left diagonal
      int lD_match = 1; 
      char rightD = board[boardSize-1][0];  //to check the right diagonal
      int rD_match = 1;

      for(int i=0; i<boardSize; i++)
      {
          if(board[i][i]==leftD)
             lD_match++;
         if(board[boardSize-1][i])
            rD_match++;
      }
       if(leftD == boardSize || rightD == boardSize)
        return playerTurn;

       return -1; 
}
void runGame(char **board, int boardSize, char **playerIds)
{
   char Weapon[] = {'X', 'O'};
   int playerTurn = 0;   // bcoz har bar change hoga 0 hai toh next bar 1 hoga ...isse next ki chance aati rhegi
   int rounds = 0;
   displayBoard(board, boardSize);

   while(rounds < 9)
   {
       playerTurn = (playerTurn) ? 0:1; // ternary operators...
        
       int X, Y;
       cout << playerIds[playerTurn] << ", your turn >> \n";
       choiceToCords(&X, &Y, boardSize);

       board[X][Y] = Weapon[playerTurn];
       displayBoard(board, boardSize);

       int winner = checkWinCondition(board, boardSize, playerTurn);
       if(winner != -1){
           cout << playerIds[winner] << " has won, bow down yall" << endl;
           exit(0); // terminates the program....
       }
        rounds++;
   }
     cout << "It's a Draw" << endl;
}
char **fetchPlayers()
{
    char **playerNames = new char* [2];
    for(int i=0; i<2; i++)
    {
       playerNames[i] = new char[32]; // each player has name only upto 32 ;
       cout <<" Player " << (i+1) << ">>";
        cin >> playerNames[i];
    }
     return playerNames;
}
int main()
{
   int boardSize;
   cout << "Board Size?! ";
   cin >> boardSize;
   char **board = createBoard(boardSize);
   labelBoard(board, boardSize);
   displayBoard(board, boardSize);

   char** playerIds = fetchPlayers();
   runGame(board, boardSize, playerIds);
   return 0;
}
