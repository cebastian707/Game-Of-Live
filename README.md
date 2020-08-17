# Game-Of-Live
#pragma once
/*Cebastian Santiago
the program use a text file to readand set values to a 2D array.all the values set to true
is as "living cell" and it will print as a character type '*' in the table when the function
print() is call.
The program ask for the user how many generation wants that the program do, then the program
can print the table in the generation writed by the user.


boolMatrix();
pre: make the 2 int values into a score for 2D array table.
post : make a 2D bool array with all tha values "false"

bool get(const int row, const intand col)const;
pre: use rowand col integers to find the location on the 2D array.
post : returns the value of the location -> 'true' or 'false'

void set( int row, int col, bool temp);
pre: use rowand col integers to find the location on the 2D array and then use temp to
set the value.
post : change the value of the location depending if temp is 'true' or 'false'.

void print()const;
pre: it has to be call after the program reads all the values.
post : print the table with all the with char '*' for all the values that were set.

int rowCount(const int row) const;
pre: the value row is use to chose one row from the table.
post : count all the true values in the row.

int colCount(const int col)const;
pre:the value col is use to chose one column.
post : count all the true values in the select column.

int totalCount()const;
pre: use the 2D array.
post : sum and print the number of all the "true" values of the bool array.

int neighborCount(const int& row, const int& col)const;
pre:use rowand col to read all the values of the 2D array.
post : count all the neighbors(bool = true) that have one location of the array.
*/

class boolMatrix
{
public:
	static const int NUM_ROWS = 21;
	static const int NUM_COLS = 22;
	void set(int, int, bool);
	boolMatrix();
	void print()const;
	int colcount(const int)const;
	int rowcount(const int)const;
	int totalcount()const;
	int neighborCount(const int, const int)const;
	bool get(int row, int col)const;
	
private:
	bool Matrix[NUM_ROWS][NUM_COLS];
};


//CEBASTIAN SANTIAGO
#include<iostream>
#include<cassert>
#include<iomanip>
#include"boolMatrix.h"
#include<assert.h>

using namespace std;

int boolMatrix::neighborCount(const int Row,const int Col)const
{
	int trueCount = 0;

	for (int row = Row - 1; row <= Row + 1; row++)
	{
		for (int col = Col - 1; col <= Col + 1; col++)
		{
			if (row >= 0 && row < NUM_ROWS && col >= 0 && col < NUM_COLS && get(row, col))
			{
				trueCount++;
			}
		}
	}

	if (get(Row, Col))
	{
		return trueCount-1;
	}
	return(trueCount);
}



bool boolMatrix::get(int row, int col) const
{
	assert(row >= 0 && row < NUM_ROWS);
	assert(col >= 0 && col < NUM_COLS);
	return Matrix[row][col];
}



int boolMatrix::totalcount()const
{
	int total = 0;

	for (int row = 0; row < NUM_ROWS; row++)
	{
		for (int col = 0; col < NUM_COLS; col++)
		{
			if (Matrix[row][col] == true)
				total++;
		}
	}
	return total;
}

void boolMatrix::set(int row , int col , bool val)
{
	assert(row >= 0 && row < NUM_ROWS);
	assert(col >= 0 && col < NUM_COLS);
	Matrix[row][col] = val;

}


int boolMatrix::rowcount(const int rowindex)const
{
	int colums = 0;
	assert(rowindex >= 0 && rowindex < NUM_ROWS);
	for (int col = 0; col < NUM_COLS; col++)
	{
		if (Matrix[rowindex][col] == true)
			colums++;



	}

	return colums;
}

int boolMatrix::colcount(const int colindex)const
{
	int count = 0;
	assert(colindex >= 0 && colindex < NUM_COLS);
	for (int row = 0; row < NUM_ROWS; row++)
	{
		if (Matrix[row][colindex] == true)
			count++;

	}
	return count;

}

boolMatrix::boolMatrix()
{
	for (int row = 0; row < NUM_ROWS; row++)
	{
		for (int col = 0; col < NUM_COLS; col++)
		{
			Matrix[row][col] = false;
		}
	}
}
void boolMatrix::print()const
{
	cout << "  ";
	for (int col = 0; col < NUM_COLS; col++)
	{
		cout << col % 10;
	}
	cout << endl;

	for (int row = 0; row < NUM_ROWS; row++)
	{
		cout << setw(2) << row % 20;
		for (int col = 0; col < NUM_COLS; col++)
		{


			if (Matrix[row][col] == true)
			{
				cout << "*";
			}
			else
			{
				cout << " ";
			}

		}
		cout << endl;
	}
	cout << endl;
}


//CEBASTIAN SANTIAGO
#include<iostream>
#include<fstream>
#include"boolMatrix.h"
#include<assert.h>

using namespace std;
//pre::this function gets the dead total of the program
int total(boolMatrix&, int, int);
//pre::gets the number of dead cells
int deadcol(boolMatrix&,int,int);
//pre::gets the number of generations 
void getGenerations(boolMatrix&);
//pre::this function prints the result
void printResults( boolMatrix& life , const int , const int);
//pre::reads the file thats gen zero
void genzero(boolMatrix& life);
//pre::this determines who lives and who dies
bool Playing_God(const boolMatrix, const int, const int);
//pre::this determines the next gen
void nextgen(boolMatrix&);
//pre::determines the dead people in a row
int rowdead(boolMatrix&, int,int);

int main()
{
   		
	boolMatrix cells = boolMatrix();
	int row = 10;//this is the row for the row.count member function
	int col = 10;//this is the colum for the col.count member function
	//function calls
	genzero(cells);
	getGenerations(cells);
	printResults(cells,row,col);
	
	return 0;
}
//post::function gets the total form the grid and subtract to see the dead total
int total(boolMatrix& gonerip, int row, int col)
{
	int minus = 0;
	int total = 0;
	for (int row = 0; row < boolMatrix::NUM_ROWS; row++)
	{
		for (col = 0; col < boolMatrix::NUM_COLS; col++)
		{
			if (gonerip.get(10,10) == false)
				total++;
			
		}
	}
	minus = total - gonerip.totalcount();
	return minus;
}
//post::function determines the next generation using two boolMatrix members and calls another function playing god  
void nextgen(boolMatrix& dios){
	boolMatrix copy = dios;
	for(int row = 0; row < boolMatrix::NUM_ROWS; row++)
	{
		for(int col = 0; col < boolMatrix::NUM_COLS; col++)
		{
			copy.set(row, col, Playing_God(dios, row, col));
		}

	}
	dios = copy;
}
//post::gets the number of dead cell in a colum using a class memember
int deadcol(boolMatrix& live,int col , int row){
	col = live.colcount(1+6) + col;
		return col;
}
//post::gets the number of dead cells in a row 
int rowdead( boolMatrix&dead, int cell,int row) {
	cell = dead.rowcount(16-2) + cell;
		return cell;
}
//post::bool function that determines how many values a nieghbor has and see if he lives
bool Playing_God(const boolMatrix cell, const int row, const int col) {

	if (cell.neighborCount(row,col) == 3)
	{
		return (true);
	}
	else if(cell.neighborCount(row,col) <= 1 || cell.neighborCount(row,col) >= 4)
	{
		return(false);
	}
	else
	{
		return(cell.get(row, col));
	}
}
//post::ask the user how many genration does he want 
void getGenerations(/*out*/ boolMatrix& dead)	{
	int numGenerations = 0;
	cout << "How many generations in total?:";
	cin >> numGenerations;
	for (int count  = 0; count < numGenerations; count++){
		nextgen(dead);
	}
}
//post::function prints the results only reason why boolMatrix is becuase I'm writing to the dead cells,row and dead alive 
void printResults(/*in*/  boolMatrix& life , const int row , const int col)	{
	
	life.print();
	cout << endl;
	cout << "The grid after 5 generations have passed." << endl;
	cout<<"(GENERATION #5) GAME OF currentgen STATISTICS"<<endl;
	cout << endl;
	cout << "Total alive in row 10  = " << life.rowcount(row) << endl;
	cout << "Total alive in col 10 = " << life.colcount(col) << endl;
	cout << "Total dead in row 16 = " << rowdead(life, row, col) << endl;
	cout << "Total dead in col 1 = " << deadcol(life, row, col) << endl;
	cout << "Total alive = " << life.totalcount() << endl;
	cout << "Total dead = " << total(life,row,col) << endl;
}
//post::reads gen zero and opens it up
void genzero(boolMatrix& life)	{

	int row = 0, col = 0;
	ifstream games("lifedata.txt");
	games >> row >> col;
	while (games)
	{
		life.set(row, col, true);
		games >> row >> col;
	}

	games.close();
}


















