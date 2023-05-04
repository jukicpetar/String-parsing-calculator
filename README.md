Simple c++ string parsing calculator

(for some reason github text editor ignores multiplication sign next to digit or bracket)

This simple calculator supports 4 basic operations (+,-,/,*) and brackets.
It accepts input as string, for example: 2.5*((2-1)+3), and it returns solution.
In order to solve equation, it needs to find numbers and operators.
It uses 2 stacks, one where it keeps numbers it finds in equation, and another one where it keeps indexes of numbers it needs to multiply.
My solution iterates through string from last character to first. This way it doesn't need more than 2 stacks because when it encounters minus operator it is able to push negative value to stack immediately because we do not depend on precedence.
Also, we  treat division as multiplication, for example, 2/3, it pushes 2 and 1/3 to stack of numbers, and their indexes in that stack to stack of numbers we need to multiply.
When it comes to end of equation, it first multiply numbers and then add all remaining numbers and returns solution.
In case of brackets, it calls recursive function which pushes solution of brackets to stack of numbers.

Let's review most complex functions needed to solve equation.


Function that solve equation is solveEqui(char* equi, int& it, bool tc=0)
"it" is iterator, we keep track at what character we are currently. tc (short for "to close") is true if calculator encountered bracket and it needs to be closed. This way we eliminated need to check if there is any unclosed bracket in equation.
In case there is unclosed bracket, it returns nan. For example "2+3)" or "2+(5+2" - invalid inputs
Function first checks if current character (on place of it - short for iterator) is bracket, if it is, it calls itself again. After that, next step is to find numbers.

Function that finds number is findNumber(char* equi, int& it)
In our case, number is everything between operators or brackets. Function pushes all characters between them to new string that is converted to number.
This way, we eliminate need of checking if there is any operator that stands on end of equation that is not allowed to stay there. "2+3+" or "5*2/" are wrong inputs and it returns nan. Repeated operators are eliminated too. For "2++3" or "3+*5" nan is returned.
Also it eliminates need of checking if there is any other unallowed character - letter or something else. "2+3abc5"
Function also moves iterator to next character after number - digit or bracket.


If current character is closing bracket, number is pushed to stack and solution of those brackets is pushed to stack of parent function. In case of + we push found number to stack, in case of minus, we push negative value of that number to stack, in case of * we push that number to stack, and index of that and next number to another stack. In case of /, we push reciprocal value to stack, and index of that and next number to another stack. In case of * and /, we eliminate need to check if any of them are on start of string, for example: "*2-3" or "/10+2". Those are invalid inputs and nan is returned. If character after number is not operator of bracket, nan is returned.
Before pushing number to stack, we need to reverse it because we are going from end of string to start of it.
Also, we need to check if number is valid.

Function ValidateNumber checks if all characters are digits or commas. If there is any character unallowed character, it returns nan. Also it checks if there is more than 1 decimal comma and if it is on right place.

Function multiplyNumbers multiplies checks all numbers that must be multiplied. Don't forget it keeps indexes of numbers. For example, "2*3*4+5*6", in numbers stack, there are 2,3,4,5,6, and on stack for multiplying there are pairs of indexes, (0,1),(1,2),(3,4). This functions multiply numbers and on left indexes it leaves zeros. After multiplication, stack of numbers looks like this: 0,0,24,0,30.

Function addNumbers adds all remaining numbers and returns result.

It is also worth of mention, when calculator encounters bracket, it recursivly calls solveEqui(equi,it,1) and moves iterator to next character after brackets. Before it goes to search for another number, it is needed to apply what operators do to solution of brackets.
In case of +, nothing is needed to change, because solution is already on stack, in case of -, we need to make solution negative, in case of /, we need to make solution reciprocal and push it and index of next number to stack of indexes of numbers that are multiplied. In case of *, we push index of solution and next number to another stack.
