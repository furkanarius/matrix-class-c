
# Matrix Classes in C++
C++ code applying power iteration with deflation in order to find eigenvalues and eigenvectors of any vectors with non-complex eigenvalues. The code uses Matrix class.
You can use the code in Matrix.cpp and Matrix.h in your projects asking you to do Matrix operations. The use of classes will make the operations much easier.

**To learn how to use and understand what's going on here:**
[How to Create a Matrix Class Using C++ on Medium](https://medium.com/@furkanicus/how-to-create-a-matrix-class-using-c-3641f37809c7)

Or you can always read it here below:
# How to Create a Matrix Class Using C++

In this tutorial, we’ll be creating a simple Matrix class in C++. First off, why do we create a class instead of just doing operations?

* It helps us use matrices as if they’re like floats or doubles that we can do operations on the fly.
* We can change one function(method) and it will apply to all operations in the main function

These two main reasons make it advantageous to create our own Matrix class, regardless of the price we have to pay: it takes a lot of effort if you’re doing the first time.

Let me start with a crash course on matrices. A matrix can have a number of rows and columns and each cell of a matrix can be represented by their combination.


![](https://miro.medium.com/max/760/1*ZgE0aCTEZhqk_OltJuTnsw.gif)

For instance, the above matrix is a 4x5 matrix. It means it has 4 rows and 5 columns. If you want to represent 11, you can write it in this form: V(1,5). Row number always comes first.

There are some simple and some complex things you can do with a matrix, but we won’t delve into those complex ones. You can add two matrices for instance, but they have to have the same row and columns. The same rules apply to the subtraction.

For multiplication, however, we’ve got a fairly difficult rule. [I’ll just post the video describing that process](https://www.youtube.com/watch?v=sYlOjyPyX3g&feature=emb_title), since PatrickJMT did a pretty good job explaining that concept.

We also have Transpose operation which is basically changing rows with columns and columns with rows. For instance, Row 1 will be your new Column 1 and Row 2 will be your new Column 2 and so forth.

The drill is over, now we get started.

It’s possible to create a class inside **main.cpp, **but I’d rather have them in separate files. The first file we need is **Matrix.h. .h **is the abbreviation of header and we’ll store only the definitions of our methods and our Matrix. We start this file with library declarations which we’ll need for this project.

```c++
//  matrix.h
//
//  Created by Furkanicus on 12/04/15.
//  Copyright (c) 2015 Furkan. All rights reserved.
// Learnrope.com

#ifndef __EE_242_Project_2__matrix__
#define __EE_242_Project_2__matrix__

#include <stdio.h>
#include <fstream> // for file access
#include <iostream>
#include <stdlib.h>
#include <sstream>
#include <string>
#include <vector>
#include <tuple>
#include <cmath>

using std::vector;
using std::tuple;

class Matrix {
```
We actually don’t need file stream libraries, but in the project, we had to read from a text file and insert into a Matrix class instance. Note that, while Matrix class represents the whole thing, an instance of that class would be only one Matrix class. We define the instances only in the main.cpp and not in the class file. Vector library is to create a dynamically allocated matrix model that automatically destroys itself when not needed. The code in line 22 and line 23 are used instead of **using std namespace**. Since this was a header file, we don’t need to use the whole namespace. The last line in the code above indicates the beginning of Matrix class. When you’re defining a class, first write the keyword **class** then continue with a class name of your choice. In our case, it was obviously **Matrix.**

```c++
private:
unsigned m_rowSize;
unsigned m_colSize;
vector<vector<double> > m_matrix;
```

Just after the left curly brace, we need to define our class members. The first member is **m_rowsize**. Its data type is chosen to be **unsigned. **The reason we do that is to save some memory space and it’s a safe choice since the Row Size and Column Size of any matrix can’t be negative. The last member of the Matrix class is **m_matrix.**

## Our data members
 -  m_rowsize
 -  m_colsize
 - m_matrix
 
**m_matrix** has an interesting data type which I suspect some of you haven’t heard of before. In C++, vector data type stores a sequence of characters for any given data type. That’s why when we define a vector, we need to specify its members data type in brackets. An example would be a vector with integers. The way you define it is the following:

## vector<int> myVector;

Look at the code again. You see that we defined the vector’s members data type as another vector with members having double data type. We have to leave an extra space between two right angle brackets since it might cause some problems.

The keyword private is a safety net for class type. It ensures that these members can be used only inside the class methods. They will be inaccessible from main.cpp. Since, we’ll need the row and column number, we’ll have to use get methods for that.

That’s all there’s to it for Matrix class members. The difficult part is about to begin.
```c++
public:
Matrix(unsigned, unsigned, double);
Matrix(const char *);
Matrix(const Matrix &);
~Matrix();
```

Public keyword draws the border between things we can use in main cpp and can’t. While we prefer to keep the data members within the land of private, most methods belong to the public. Otherwise, this whole class thing would have been a black box which no one could access. The second line is something called a constructor. That code initiates your code inside the main.cpp. You can have multiple constructors and they’ll apparently construct different Matrix classes. In this example, we have only 3 constructors. The first constructor will be the default one. It’ll take the row size, column size and an initial value for each cell. Note that, here we only declare the constructor, thus there’s no need to write any variable names for the time being. The second constructor is the special one. It’ll take in only the file name and fill in the matrix with the values from that text file. The last constructor is a copy constructor. The only reason of its existence is to copy the members of one class instance to another. The last line contains a destructor which we only write for the sake of convention.

```c++
// Matrix Operations
Matrix operator+(Matrix &);
Matrix operator-(Matrix &);
Matrix operator*(Matrix &);
Matrix transpose();
```

Now, it’s time for some matrix operations. Let me explain the first line part by part:

* **Matrix** keyword indicates that its output will be of Matrix type. You get another Matrix when you add two matrices
* **operator+** is overloading **+ **operator.

What’s that mean, overloading? I guess it’s **DIGRESSION TIME:**

Now, when you add 3 and 4, you write it in this form: 3+4. You basically have added two integers, right? But, when you write 3.5 + 4.5, that’s a whole another operation, an operation occurring two floats. As you see, we used the same + sign for two different operation, although essentially doing the same thing, they’re still different in the eyes of C++ compiler.

By overloading an operator in a matrix method, we allow ourself to add two matrices in the following form:

### C = A + B;

You see if we used a standard function, we had to write it in this form, which I find boring:

### C = A.add(B);

* Inside the parentheses, we have **Matrix &. **It takes that matrix’s address and use the Matrix itself, not its copy. This is done for efficiency.
* Finally the transpose function. It doesn’t take any arguments. It only takes the transpose of the matrix of interest.
```c++
// Scalar Operations
Matrix operator+(double);
Matrix operator-(double);
Matrix operator*(double);
Matrix operator/(double);
```
If you understood the operator overloading logic from the previous method declarations, you can easily understand what these 4 lines of code there for. They will take double as arguments and do scalar operations. For instance, if you multiply a matrix by 4, it means you’ll multiply the whole thing by 4.

```c++
// Aesthetic Methods
double& operator()(const unsigned &, const unsigned &);
void print() const;
unsigned getRows() const;
unsigned getCols() const;
```
We’re near the end of this header file. The same approach, line by line:

* The first line of code is written to make our life easier when we refer to a specific cell in a matrix. Suppose you have a 4x4 matrix and you only need the number in 3rd row, 4th column. This line makes it possible to write it in this form: **Matrix(3,4)**. We again use the operator overloading. The first argument refers to the row number and the second argument refers to the column number.
* The second line of code is to print the whole matrix in Matlab form. Takes no arguments and outputs nothing.
* The third and fourth lines are the get method I mentioned before. You only need them because you can’t access the data members from the main function.
```c++
// Power Iteration
tuple<Matrix, double, int> powerIter(unsigned, double);
// Deflation
Matrix deflation(Matrix &, double&);
};
#endif
```
This final code snippet is the whole reason we’re making this Matrix class. As you see, tuple lets us get multiple outputs from one function(method), which is neat.

You have to use }; to cap off a class definition and don’t forget to write #endif at the very end.

## PART 2 — Matrix.cpp

In this sequence, I’ll be explaining the **matrix.cpp **file. This file contains the implementations of all class functions. Let’s keep the intro short and start.

```c++
//
//  matrix.cpp
//
//  Created by Furkanicus on 12/04/15.
//  Copyright (c) 2015 Furkan. All rights reserved.
//

#include "matrix.h"
using namespace std;

// Constructor for Any Matrix
Matrix::Matrix(unsigned rowSize, unsigned colSize, double initial){
    m_rowSize = rowSize;
    m_colSize = colSize;
    m_matrix.resize(rowSize);
    for (unsigned i = 0; i < m_matrix.size(); i++)
    {
        m_matrix[i].resize(colSize, initial);
    }
}
```

Line 8–9 is important for the whole thing to function properly. If not included, you’ll get an error. From line 12–20, we witness to the first constructor. The first argument this function takes is the row number. Column number follows along and the last argument, initial, lets you decide on the initial value of each cell — you can change the value of every single cell later on. By the way, in case you’ve found some other implementations, let me admit this. I was inspired by [Mike’s](http://www.quantstart.com/articles/Matrix-Classes-in-C-The-Source-File) code and dumbed some of its parts down for my project. Back to our code, line 13 and 14 assign the class members to the given values. Line 15 uses of the methods(functions) of vectors. It resizes the vector’s row size with the given row size. Finally, the for loop iterates over the vector and resizes it as to the column number. This was the first constructor. The way to use in the main.cpp is the following:

### A_matrix = Matrix(3, 3, 0.5);

You see how simple it is to create a matrix. But, we’re not done yet. We’re not even done with constructors. The second constructor is made to pull values from a txt file and create a matrix class out of it.

```c++
// Constructor for Given Matrix
Matrix::Matrix(const char * fileName){
    ifstream file_A(fileName); // input file stream to open the file A.txt

    // Task 1
    // Keeps track of the Column and row sizes
    int colSize = 0;
    int rowSize = 0;
    
    // read it as a vector
    string line_A;
    int idx = 0;
    double element_A;
    double *vector_A = nullptr;
    
    if (file_A.is_open() && file_A.good())
    {
        // cout << "File A.txt is open. \n";
        while (getline(file_A, line_A))
        {
            rowSize += 1;
            stringstream stream_A(line_A);
            colSize = 0;
            while (1)
            {
                stream_A >> element_A;
                if (!stream_A)
                    break;
                colSize += 1;
                double *tempArr = new double[idx + 1];
                copy(vector_A, vector_A + idx, tempArr);
                tempArr[idx] = element_A;
                vector_A = tempArr;
                idx += 1;
            }
        }
    }
    else
    {
        cout << " WTF! failed to open. \n";
    }
    
    int j;
    idx = 0;
    m_matrix.resize(rowSize);
    for (unsigned i = 0; i < m_matrix.size(); i++) {
        m_matrix[i].resize(colSize);
    }
    for (int i = 0; i < rowSize; i++)
    {
        for (j = 0; j < colSize; j++)
        {
            this->m_matrix[i][j] = vector_A[idx];
            idx++;
        }
    }
    m_colSize = colSize;
    m_rowSize = rowSize;
    delete [] vector_A; // Tying up loose ends
    

}
```

This one is pretty long, so I advise you going through it to understand what the code is doing. But, even if you don’t understand what’s happening, just know that if you have the file in the same directory as the main.cpp, the code will work just as expected and desired.

The exciting part is about to begin.

## Matrix Addition & Subtraction

```c++
// Addition of Two Matrices
Matrix Matrix::operator+(Matrix &B){
    Matrix sum(m_colSize, m_rowSize, 0.0);
    unsigned i,j;
    for (i = 0; i < m_rowSize; i++)
    {
        for (j = 0; j < m_colSize; j++)
        {
            sum(i,j) = this->m_matrix[i][j] + B(i,j);
        }
    }
    return sum;
}

// Subtraction of Two Matrices
Matrix Matrix::operator-(Matrix & B){
    Matrix diff(m_colSize, m_rowSize, 0.0);
    unsigned i,j;
    for (i = 0; i < m_rowSize; i++)
    {
        for (j = 0; j < m_colSize; j++)
        {
            diff(i,j) = this->m_matrix[i][j] - B(i,j);
        }
    }
    
    return diff;
}
```

This is where operator overloading comes to play. I’ll only explain addition and that should suffice. As usual, we start with **Matrix Matrix::operator+(Matrix &B). **Well, that’s actually not so usual, but let me explain it. The first word **Matrix** tells us what the output will be. Since when two matrices added together, we usually get another matrix, the output should be of Matrix type. Then comes the **usual **operator overload syntax. Finally, we add the reference of the matrix argument to prevent the use of copying procedure of another matrix. In line 3, we generate a new Matrix instance. This instance will be the one we will return as the result of this method. It’s row size and column size will be equal to the two other matrices. Initially, you can give any value to **sum** matrix’s cells, but it really doesn’t matter. After that we need to declare two variables as iterators. We need two for loops as we will cover rows and columns. Had it been a three dimensional matrix, we would have needed three for loops one inside another. In line 9, **sum **matrix’s ith row and jth column will be equal to the sum of m_matrix’s ith row and jth column and **B** matrix’s ith row and jth column. I admit this was a bad sentence, but I hope the point is clear. Outside the loop, we return the **sum **class, which we have declared in line 3. Subtraction of two matrices is almost the same with a difference of **minus** sign.
```c++
// Multiplication of Two Matrices
Matrix Matrix::operator*(Matrix & B){
    Matrix multip(m_rowSize,B.getCols(),0.0);
    if(m_colSize == B.getRows())
    {
        unsigned i,j,k;
        double temp = 0.0;
        for (i = 0; i < m_rowSize; i++)
        {
            for (j = 0; j < B.getCols(); j++)
            {
                temp = 0.0;
                for (k = 0; k < m_colSize; k++)
                {
                    temp += m_matrix[i][k] * B(k,j);
                }
                multip(i,j) = temp;
                //cout << multip(i,j) << " ";
            }
            //cout << endl;
        }
        return multip;
    }
    else
    {
        return "Error";
    }
}
```
Multiplication of two matrices is more complex than summation and subtraction. This time we have a sanity check beginning at line 4. We check whether column size of the first matrix is equal to the row number of the second matrix. If this requirement is not met, we simply return a string warning the user there’s something warn in a curt way.

This time we need three for loops. Let me explain the logic behind that. Normally, when we’re doing matrix multiplication, we multiply the first row with the first column of the second matrix and add them all up to get the first cell of the resultant matrix. But, compiler will go over all this one by one. Thus, we have to tell the compiler that it has to first multiply first row’s first element with the second matrix’s first column’s first element. Once it’s finished with the first cell of the resultant matrix, it should go on multiply first row, again, with the second column of the second matrix. The compiler should continue its operation until it reaches the last column of the second matrix. Then, it should jump to the second row of the first matrix and do the same thing until all rows are done.

We have to iterate each column and row in each scalar multiplication which constitutes the basis of the innermost for loop. This loop will iterate over both the first matrix’s each row and the second matrix’s each column. Since the rows and columns should have the same number of elements, we can simply use one iterator to go over. **k variable** is doing this very operation.

As we go upper in the for loop hierarchy, we get j which iterates over the columns of the second matrix. Remember, without changing the row of the first matrix, we multiply with the every column of the second matrix one by one. Thus, we need this second for loop. Lastly, we need another variable that will iterate over the rows of the first matrix when all columns are completed for the first row.

Now, the limits for each operator. **k iterator** is limited to the column size of the first matrix or the row size of the second matrix. Recall that they’re the same and that was our condition at the beginning. **j iterator** is limited to the second matrix’s column number as it should not go over what is not existent. Lastly, **i iterator **is limited to the first matrix’s row number.

In line 22, we return the result. Also, we’ve used **temp **variable, since we have to add all the numbers we’ve gotten as we multiplied each row member with its consecutive column member in the next matrix. Then, we equalize temp to zero to prevent and miscalculation in the second loop.

I have not created matrix division as it requires taking matrix inverse. However, by using [the general formula](https://people.richland.edu/james/lecture/m116/matrices/inverses.html) for taking the inverse of a matrix, you can implement it this matrix class. Just fork it in Github and start working on it.
```c++
// Scalar Addition
Matrix Matrix::operator+(double scalar){
    Matrix result(m_rowSize,m_colSize,0.0);
    unsigned i,j;
    for (i = 0; i < m_rowSize; i++)
    {
        for (j = 0; j < m_colSize; j++)
        {
            result(i,j) = this->m_matrix[i][j] + scalar;
        }
    }
    return result;
}

// Scalar Subraction
Matrix Matrix::operator-(double scalar){
    Matrix result(m_rowSize,m_colSize,0.0);
    unsigned i,j;
    for (i = 0; i < m_rowSize; i++)
    {
        for (j = 0; j < m_colSize; j++)
        {
            result(i,j) = this->m_matrix[i][j] - scalar;
        }
    }
    return result;
}

// Scalar Multiplication
Matrix Matrix::operator*(double scalar){
    Matrix result(m_rowSize,m_colSize,0.0);
    unsigned i,j;
    for (i = 0; i < m_rowSize; i++)
    {
        for (j = 0; j < m_colSize; j++)
        {
            result(i,j) = this->m_matrix[i][j] * scalar;
        }
    }
    return result;
}

// Scalar Division
Matrix Matrix::operator/(double scalar){
    Matrix result(m_rowSize,m_colSize,0.0);
    unsigned i,j;
    for (i = 0; i < m_rowSize; i++)
    {
        for (j = 0; j < m_colSize; j++)
        {
            result(i,j) = this->m_matrix[i][j] / scalar;
        }
    }
    return result;
}
```
There’s also scalar operations we should be worried about. They’re pretty simple and once you understand how one of them works, you’ll see that the rest is exactly the same. Let me start with addition. This time our argument will not of Matrix type, but double. The argument is presciently named **scalar. **In line 3, we see our temporary matrix to return something. We iterate over rows and columns and add the scalar to each member of our matrix. The order of iteration is not of importance. Just make sure you are not adding scalar to a member multiple times. The same thing goes for all the rest as I said before.

```c++
// Returns value of given location when asked in the form A(x,y)
double& Matrix::operator()(const unsigned &rowNo, const unsigned & colNo)
{
    return this->m_matrix[rowNo][colNo];
}

// No brainer - returns row #
unsigned Matrix::getRows() const
{
    return this->m_rowSize;
}

// returns col #
unsigned Matrix::getCols() const
{
    return this->m_colSize;
}

// Take any given matrices transpose and returns another matrix
Matrix Matrix::transpose()
{
    Matrix Transpose(m_colSize,m_rowSize,0.0);
    for (unsigned i = 0; i < m_colSize; i++)
    {
        for (unsigned j = 0; j < m_rowSize; j++) {
            Transpose(i,j) = this->m_matrix[j][i];
        }
    }
    return Transpose;
}

// Prints the matrix beautifully
void Matrix::print() const
{
    cout << "Matrix: " << endl;
    for (unsigned i = 0; i < m_rowSize; i++) {
        for (unsigned j = 0; j < m_colSize; j++) {
            cout << "[" << m_matrix[i][j] << "] ";
        }
        cout << endl;
    }
}
```
The method in line 2 overloads parentheses letting us use this beautiful form: A(1,3). It takes the row number and column number and returns what’s held at that point.

Methods in line 8 and line 14 return row and column number, respectively.

The method in line 20 takes no arguments since its sole task is to take the transpose of a given matrix.

The method in line 33 is a constant one, printing the matrix in a way we’re used to. Credits go to Matlab.
```c++
// Returns 3 values
//First: Eigen Vector
//Second: Eigen Value
//Third: Flag
tuple<Matrix, double, int> Matrix::powerIter(unsigned rowNum, double tolerance){
    // Picks a classic X vector
    Matrix X(rowNum,1,1.0);
    // Initiates X vector with values 1,2,3,4
    for (unsigned i = 1; i <= rowNum; i++) {
        X(i-1,0) = i;
    }
    int errorCode = 0;
    double difference = 1.0; // Initiall value greater than tolerance
    unsigned j = 0;
    unsigned location;
    // Defined to find the value between last two eigen values
    vector<double> eigen;
    double eigenvalue = 0.0;
    eigen.push_back(0.0);
    
    while(abs(difference) > tolerance) // breaks out when reached tolerance
    {
        j++;
        // Normalize X vector with infinite norm
        for (int i = 0; i < rowNum; ++i)
        {
            eigenvalue = X(0,0);
            if (abs(X(i,0)) >= abs(eigenvalue))
            {
                // Take the value of the infinite norm as your eigenvalue
                eigenvalue = X(i,0);
                location = i;
            }
        }
        if (j >= 5e5) {
            cout << "Oops, that was a nasty complex number wasn't it?" << endl;
            cout << "ERROR! Returning code black, code black!";
            errorCode = -1;
            return make_tuple(X,0.0,errorCode);
        }
        eigen.push_back(eigenvalue);
        difference = eigen[j] - eigen[j-1];
        // Normalize X vector with its infinite norm
        X = X / eigenvalue;
        
        // Multiply The matrix with X vector
        X = (*this) * X;
    }
    
    // Take the X vector and what you've found is an eigenvector!
    X = X / eigenvalue;
    return make_tuple(X,eigenvalue,errorCode);
}

Matrix Matrix::deflation(Matrix &X, double &eigenvalue)
{
    // Deflation formula exactly applied
    double denominator = eigenvalue / (X.transpose() * X)(0,0);
    Matrix Xtrans = X.transpose();
    Matrix RHS = (X * Xtrans);
    Matrix RHS2 = RHS * denominator;
    Matrix A2 = *this - RHS2;
return A2;
}
```
Finally, we have [power iteration](http://www.wikiwand.com/en/Power_iteration) and [deflation](http://www.heikohoffmann.de/htmlthesis/node136.html). This might be the most important part of this tutorial, but I choose to explain only how to use tuples and why. As you’ll realize later, we need to return more than one variable. One can say it’s always better to keep a function short, but for the sake of cohesion, I kept it in one function. I know, my mistake. Change it if you will.

In order to use tuples, we need to include **tuple **library.  We do not need to make up a variable name for what we’ll return in the method declaration and when returning them, we simply use make_tuple function and put all the variables in the same order we have defined in the declaration.

The code for deflation starts from line 55. It’s the same algorithm taken from college textbooks.

That’s the end of Matrix.cpp!

## Part 3 — Main.cpp

This is the final part and putting all the code together. Since I have added comments explaining each step, you don’t need my assistance as much.

In this file, we take three arguments in the command line. The first one is the file name which we’ll take our matrix values, the second argument is the tolerance value and the last one is the name of the file we’ll output.

Our main aim is to use power iteration with deflation to get the eigenvalues we want.
```c++
//
//  main.cpp
//
//  Created by Furkanicus on 10/04/15.
//  Copyright (c) 2015 M.Furkan Akyurek. All rights reserved.
//  Learnrope.com
// First Argument for file name
// Second argument for the tolerance
// Third argument for the name of the output file


#include <fstream> // for file access
#include <iostream>
#include <stdlib.h>
#include <sstream>
#include <string>
#include <cmath> // For Absolute function
#include "matrix.h" // Matrix Class


using namespace std;

int main(int argc, char * argv[]) {
    
    //                        //
    /* Start of Opening Files */
    //                        //
    {
    if (argc > 1)
    {
        cout << "argv[1] = " << argv[1] << endl;
        cout << "argv[2] = " << argv[2] << endl;
        cout << "argv[3] = " << argv[3] << endl;
    }
    else
    {
        cout << "No file name entered. Abort! Abort!";
        return -1;
    }
    
    //----------------------------------//
    
    // String to Double Conversion
    string::size_type indexer;
    double tolerance = std::stod(argv[2],&indexer);

    // Initialization of A matrix
    Matrix A(argv[1]);
    int row = A.getRows();
    
    // Pick an X vector
    // 1,2,3,4........
    Matrix X(row,1,1.0);
    for (unsigned i = 1; i <= row; i++)
    {
        X(i-1,0) = i;
    }
    
    // Error code defined to quit program when complex eigenvalues
    int errorCode;
    double eigenvalue = 0.0;
    
    // Power Iteration to find an eigenvalue
    tie(X, eigenvalue, errorCode) = A.powerIter(row, tolerance);
    
    // Quit if flag raised
    if (errorCode == -1)
    {
        return -1;
    }
    cout << "First Eigenvalue: " << eigenvalue << endl;
        // Outputting X Vector
        ofstream myfile_x (argv[3]);
        myfile_x << "Eigenvalue#1:" << eigenvalue << endl;
        myfile_x << endl;
        for (unsigned i = 0; i < row; i++)
        {
            myfile_x << "X" << i << " = " << X(i,0) << endl;
            myfile_x << endl;
        }
        X.print();
    
    // Beginning to execute deflation
    double eigenvalue2;
    Matrix X1(row,1,1.0); // Another X Vector for Power Iteration
    Matrix A2 = A.deflation(X, eigenvalue); // Deflation Applied
    // Power Iteration for Second one
    tie(X1, eigenvalue2, errorCode) = A2.powerIter(row, tolerance);
    if (errorCode == -1)
    {
        return -1;
    }
    //                           //
    /* Start of Output Functions */
    //                           //
    

    myfile_x << "Eigenvalue#2:" << eigenvalue2 << endl;
    cout << "Second Eigenvalue: " << eigenvalue2 << endl;
    myfile_x.close();
    
    }
    return 0;
}
```
