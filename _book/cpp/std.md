# Standard Library

## Standard stream input and output&#x20;

```cpp
#include <iostream>

using namespace std;

int main()
{
    cout << "How many hours did you work?";
    double hoursWorked;
    cin >> hoursWorked;
    
    cout << "How much did you get paid per hour";
    double payPerHour;
    cin >> payPerHour;
    
    double amtPaid = hoursWorked * payPerHour;
    cout.setf(ios::fixed);
    cout.precision(2);
    cout << "Charged " << 0.1*amtPaid << endl;
    
}
```

&#x20;`std::cin >> v` discards any whitespace in the standard input stream, then reads from the standard input into variable `v` It returns `std::cin` , which has type `istream` , in order to allow chained input operations.\
`std::cout << v` returns `std::cout` with type `ostream` .

&#x20;&#x20;

## Pass the variable as reference

```cpp
#include <iostream>
#include <string>


using namespace std;


void polarToCartesian(double rho, double theta, double& x, double& y);
int main()
{

	double x;
	double y;

	double radius = 10;
	double angle = 3.1415926/4;

	polarToCartesian(radius, angle, x, y);

	cout << x << endl;
	cout << y << endl;
}

void polarToCartesian(double rho, double theta, double& x, double& y)
{
	// double x: here the variable will be passed by 
	//      value, so it wont change the x in the main routine
	// double& x: here the & means: pass the variable 
	//      by reference
	x = rho * cos(theta);
	y = rho * sin(theta);
}

```

Often used when you need to return more than 1 variables in the function, but C++ doesn't allow you to do that. So you can pass the references of the variable you want to edit.&#x20;

Define the parameter in the function as `double& x` and x will be a new name for any external variable you pass in.&#x20;

&#x20;**Array** is passed to function as reference intrinsically.  **Pointer** is not([https://www.geeksforgeeks.org/passing-reference-to-a-pointer-in-c/](https://www.geeksforgeeks.org/passing-reference-to-a-pointer-in-c/)). But using the pointer you can edit the value in that memory position.&#x20;

## Array 1D

```cpp
#include <iostream>
#include <string>


using namespace std;

string colors[4] = {"Red", "Orange", "Yellow", "Blue"};
// Perfect initialization.

string colors[4] = {"Red", "Orange", "Yellow"};
// It compiles. One element undefined.
cout << colors[3] << endl; // You can do that, but undefined behavior 
cout << colors[4] << endl; // You can even do that, but undefined behavior


string colors[4] = {"Red", "Orange", "Yellow", "Orange", "Yellow"}; 
// Compile error, too many initializer



double scores[2] = {2, 9};



```

Note that number of elements in the array has to be known in compilation time, which means you cannot let the user input that number.

```cpp
//Something about static_cast< >

int nScores = 0;
int total = 0;

// code here to change the nScores and total

double mean = static_cast<double>(total) / nScores;
// here staric_cast<double> create a temporary variable
// that is double

double mean = (double)total / nScores; // You cam do that but not recommended in C++
```

```cpp
// Now we add array to the program

#include <iostream>
#include <string>

using namespace std;

const int MAX_NUMBER_OF_SCORES = 10000;
int scores[MAX_NUMBER_OF_SCORES]; // create a array that's large enough to hold everything
// but you don't necessarily use all of them
int nScores = 0;
int total = 0;
cout << "Enter the scores (negative to quit):" << endl;
for (;;)
{
    int s;
    cin >> s;
    if (s < 0) break;
    if (nScores == MAX_NUMBER_OF_SCORES) 
    {
        cout << "I can only handle " << MAX_NUMBER_OF_SCORES << " scores!" << endl;
        cout << "Continuing with just the first " << MAX_NUMBER_OF_SCORES << " scores." << endl;
        break;
    }
    total += s;
    scores[nScores] = s;
    nScores++;
}
if (nScores == 0)
    cout << "There were no scores, so no statistics." << endl;
else
{
    double mean = static_cast<double>(total) / nScores;
    cout << "The mean is " << mean << endl;
    double sumOfSquare = 0;
    for (int k = 0; k < nScores; k++)
    {
        double diff = scores[k] - mean;
        sumOfSquares += diff * diff;
    }
    cout << "The standard deviation is " << sqrt(sumOfSquares/nScores) << endl;
}
```



```cpp
//define a function that takes array
//In C++, when you pass the array to a function, you don't copy the array, but
//in a way similar to reference
//In order not to change array, you can use const
double computeMean(const int a[], int n)
{
    // There is no way to figure out how big the array is from the a
    // You have to pass in number n and trust the user 
    
    //a[5] = 12;  // it wont compile. You cannot modify 
    
    
    if (n <= 0)
        return 0;
    int t = 0;
    for (int k = 0; k < n; k++ )
    {
        t += a[k]
    }
    return static_cast<double>(t) / n;
    
}

void setAll(int a[], int n, int value)
{
    for (int k = 1; k < n; k++)
    {
        a[k] = value;
    }
}
```

## Array 2D

```cpp
const int NWEEKS = 5;
const int NDAYS = 7;

double meanForADay(const int a[][NDAYS], int nRows, int dayNumber);
// When you pass a 2D array into the function, you leave the 
// first dimension but have to put in the 2nd dimension
int main()
{
    int attendance[NWEEKS][NDAYS];
    //...
    double meanWed = meanForADay(attendance, NWEEKS, 2);
}

double meanForADay(const int a[][NDAYS], int nRows, int dayNumber)
{
    if (nRows <= 0)
        return 0;
    int total = 0;
    for (int w = 0; w < nRows; w++)
    {
        total += a[w][dayNumber];
    }
    return static_cast<double>(total) / nRows;
}
```

## Array Multi-D

```cpp
int multiplexAttendance[16][5][7];
void f(int b[][5][7],...);
```

## Char and String (revisit for C-string, char array)

```cpp
int k = 97;
char c = 'a';  //Represented by 97 if ASCII it the encoding

int k2 = 'a'; //If ASCII is the encoding, k2 is 97
char c2 = 97; //IF ASCII is the encoding, c2 is 'a'

k++;  // k is now 98
c++;  // c = c + 1. If ASCII, c had 97 ('a'), c+1 is 98 ('b')

double d = 3.5;
cout << d; // calls the function for doubles; it writes '3' '.' '5'
// if ASCII, this is 51 46 53
cout << k; // calls the function for ints; in writes '9' '8'
// if ASCII, this is 57 56
cout << c; // calls the function for chars; it writes 'b'
// if ASCII, this is 98

c == 'b'; // we are comparing the encoding
c > 'a';

string s1 = "hello";
string s2 = "help";
string s3 = "helping";
string s4 = "hElp";

s1 < s2; //true because in l's encoding is smaller than p
s2 < s3; // true 
s2 < s4; //If ASCII, false because 'E' < 'e'

s1 < "help"; //ture
'hello' < s2; // true
"hello" < "help"; // BAD IDEA!!! implementation-dependent behavior, because 
// it won't necessary use the operator defined in string library





```

C++ is compatible with C. But C uses a different library for the string. How does C deal with string?&#x20;

```c
/*
C string
use array and '\0' to mark the ending of the string

    0   1   2   3   4    5   6   7   8   9   
t: 'H' 'e' 'l' 'l' 'o' '\0'  .   .   .   .
*/

#include <cstring>  // defines strlen, strcpy, etc
using namespace std;


char t[10] = "Hello";  //it will add '\0' at the end
//make sure 10 is larger than your string length, while '\0' will occupy one position
char s[100] = ""; //it will have a '\0'

for (int k = 0; t[k] != '\0'; k++)
    cout << t[k] << endl;

cout << t; //if t is a c-string, it will call a function to write the characters until '\0'
cin.getline(s, 100) // read string (for c string) for maximum 100 character 

cout << strlen(t); // writes 5

strcpy(s, t); // strcpy(destination, source); 
// s = t; // Error! Won't compile, can't assign arrays

strcat(s, "!!!");  
// s += "!!!"; // Error! won't compile -- no += for arrays

if (strcmp(t,s) < 0 )
    ...
            strcmp(a, b)
             negative if a comes before b
                 0    if a equals b
             positive if a comes after b
             
             
Is a equal to b?
    Wrong:  if (strcmp(a, b))
                ...
    Right:  if (strcmp(a, b) == 0)
                ...
                
Does a come before b?
    Wrong:  if (a < b)
                ...
    Right:  if (strcmp(a, b) < 0)
                ...

void makeUppercase(string& s)
{
    // for string
    for(int i = 0; i != s.size(); i++)
    {
        s[i] = toupper(s[i]);
    }
}            
             
void makeUppercase(char s[])
{
    // for c string
    for(int i = 0; s[i] != '/0'; i++)
    {
        s[i] = toupper(s[i]);
    }    
}             







```

## Pointer

Const and pointer ([https://www.learncpp.com/cpp-tutorial/610-pointers-and-const/](https://www.learncpp.com/cpp-tutorial/610-pointers-and-const/))\
`const char *p` implies that the variable it points to is constant, but the pointer itself is not. So `*p = new value` is illegal while `p = p + 1` is okay.

## Class

```cpp
#include <iostream>
#include <string>

using namespace std;

class Pet
{
public:
    Pet(string nm, int initialHealth); // declaration of the constructor function
    void eat(int amt);
    void play();
    string name() const; // 'const' after () means this function will not change the input variables
    int health() const;
    bool alive() const;    
private: // private members cannot be accessed by external user
    string n_name; 
    int m_health;
    
};

// constructor for Pet to initialize
Pet::Pet(string nm, int initialHealth)
{
	n_name = nm;
	m_health = initialHealth;
}

void Pet::eat(int amt)
{
	m_health += amt;
}

void Pet::play()
{
	m_health--;
}

string Pet::name() const
{
	return n_name;
}

int Pet::health() const
{
	return m_health;
}

bool Pet::alive() const
{
	return m_health > 0;
}

void reportStatus(const Pet* p)
{
	cout << p->name() << " has health level " << p->health();
	if (!p->alive())
		cout << ", so has died";
	cout << endl;
}

void careFor(Pet* p, int d)
{
	if (!p->alive())
	{
		cout << p->name() << " is still dead" << endl;
		return;
	}

	if (d % 3 == 0)
		cout << "You forgot to feed " << p->name() << endl;
	else
	{
		p->eat(1); 
		cout << "You fed " << p->name() << endl;
	}

	p->play();
	reportStatus(p);
}

int main()
{
	Pet* myPets[2];
	myPets[0] = new Pet("Fluffy", 2);
	myPets[1] = new Pet("Frisky", 4);
	for (int day = 1; day <= 9; day++)
	{
		cout << "====== Day " << day << endl;
		for (int k = 0; k < 2; k++)
		{
			careFor(myPets[k], day);
		}

		cout << "======" << endl;
	}
	for (int k = 0; k < 2; k++)
	{
		if (myPets[k]->alive())
			cout << "Animal Control has come to rescue " << myPets[k]->name() << endl;
		delete myPets[k];
	}
	
}


```

## Iterator

what is an iterator? What does this concept refer to?[http://www.cplusplus.com/reference/iterator/#:\~:text=Iterator%20definitions,dereference%20(%20\*%20)%20operators).](http://www.cplusplus.com/reference/iterator/#:\~:text=Iterator%20definitions,dereference%20\(%20\*%20\)%20operators\).)

An iterator is any object that, pointing to some element in a range of elements (such as an array or a container), has the ability to iterate through the elements of that range using a set of operators (with at least the increment (`++`) and dereference (`*`) operators).\
\
The most obvious form of iterator is a pointer: A pointer can point to elements in an array, and can iterate through them using the increment operator (`++`). But other kinds of iterators are possible. For example, each container type (such as a list) has a specific iterator type designed to iterate through its elements.

Similar to pointer, a 'constant iterator' means the object this iterator points to cannot be modified.

## Vector (C++ STL)

[https://www.geeksforgeeks.org/vector-in-cpp-stl/](https://www.geeksforgeeks.org/vector-in-cpp-stl/)

In c++ standard libirary there's \<vector> which defines vector. It's same as dynamic arrays with the ability to resize itself automatically when an element is inserted or deleted, with their storage being handled automatically by the container.&#x20;

## Enum

Enumeration type. [https://en.cppreference.com/w/cpp/language/enum](https://en.cppreference.com/w/cpp/language/enum)

#### Unscoped enumeration

```cpp
// syntax
enum name { enumerator = constexpr , enumerator = constexpr , ... }
// underlying type is an implementation-defined integral type
// int or unsigned int


enum name : type { enumerator = constexpr , enumerator = constexpr , ... }	


//e.g.

enum Buffer { Color = 1, Depth = 2};
```

#### Scoped enumeration

```cpp
// syntax
enum struct|class name { enumerator = constexpr , enumerator = constexpr , ... }

enum struct|class name : type { enumerator = constexpr , enumerator = constexpr , ... }	


//e.g.

enum class Buffer
{
    Color = 1,
    Depth = 2
};
```

## argc and argv\[]

To run program in command line and to give arguments.\
[https://www.geeksforgeeks.org/command-line-arguments-in-c-cpp/](https://www.geeksforgeeks.org/command-line-arguments-in-c-cpp/)

```cpp
int main(int argc, char *argv[])
{

}

// or 

int main(int argc, char **argv)
{
    cout << "You have entered " << argc 
         << " arguments:" << "\n"; 
  
    for (int i = 0; i < argc; ++i) 
        cout << argv[i] << "\n"; 
    
    
    float arg1;
    int arg2;
    std::string arg3; 
    if (argc >= 4)
    {
        arg1 = std::stof(argv[1]);
        arg2 = std::stoi(argv[2]);
        arg3 = std::string(argv[3]);
    }
    else
    {
        // return 0;
    }
      
        
            
    return 0; 
}

// in cmd
// ./xxx var1 var2 var3 
```

`argc` : int, number of arguments input in the cmd. Note that the first argument will always be the executable filename.\
`argv` : double pointer to char, a array of cstrings. Note thet `argv[0]` will be a string containing the executable filename.&#x20;

In cstring, a char array is used for a single string. So for multiple variables (multiple strings), we need a double pointer.&#x20;
