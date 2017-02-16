---
layout: post
title:  "JavaScript: The Good Parts Notes I: Analyzing JS and Grammar"
date:   2017-1-17 12:00:01-0400
---

I've been neck deep in code for a while and recently had a chance to dive into Node and Javascript. Among searching for resources, a common title reappeared: Douglas Crockford's "Javascript: the Good Parts". So far I see why. Crockford blends precise language without getting overly complex in his explanations. Great railroad diagrams visually allow you to see the code. I highly recommend you pick up a copy! So without further ado, here are my notes on the first two chapters of "Javascript: the Good Parts" by Douglass Crockford.

 ==== CHAPTER 1: Analyzing Javascript: ===

Good: functions, loose typing, dynamic objects, and an expressive object literal notation

Bad: programming model based on global variables. JS depends on global variables for linkage. All of the top level variables of all compilation units are tossed together in a common namespace called the global object. This is a bad thing because global variables are evil and in JS they are fundamental. JS gives us tools to negate this problem.

Functions: first class objects with mostly lexical scoping. More in common with Lisp and Scheme than with Java. It is Lisp in C’s clothing.

Javascript is a loosely typed language, so Javascript compilers are unable to detect type errors. 

JS has a very powerful object literal notation. Objects can be created simply by listing their components. This notation was the inspiration for JSON.

JS has a class free object system in which objects inherit properties directly from other objects.

=== CHAPTER 2: GRAMMAR =====

Whitespace: usually insignificant, occasionally necessary to use to separate sequences of characters that would otherwise be combined into a single token. 

Two forms of comments:
	1. Block comments: /* */
		=> try not to use these because these pairs can occur in regex literals (and cause a syntax error )

	2. Line ending comments: // 

Names: a letter optionally followed by one or more letters, digits, or underbars. A name cannot be one of the reserved words. Used for statements, variables, property names, operators and labels.

Numbers: has a single number type. There is no separate integer type, so 1 and 1.0 are the same value. This is great cuz problems of overflow in short integers are avoided all you need to know about a number is that it is a number.
	-Exponents: value of the literal is computed by multiplying the part before the e by 10 raised to the power of the part after the e. So 100 and 1e2 are the same number.
	-Negative numbers can be formed using the - prefix operator
	-The value of NaN is a number value that is the result of an operation that cannot produce a normal result. NaN is not equal to any value including itself. You can detect NaN with the isNaN(number) function
	-The value of Infinity represents all values greater than 1.79769313486231570e+308. 
	-Numbers have methods. Javascript has a Math object that contains a set of methods that act on numbers. E.g.: the Math.floor(number) method can be used to convert a number into an integer.

Strings: can be wrapped in single or double quotes. The \backslash is the escape character. Javascript was built at a time when Unicode was 16-bit so all characters in Javascript are 16 bits wide. 

Escaped characters: double quote, single quote, backslash, slash, backspace, formed, new line, carriage return, tab, 4 hexadecimal digits. The \u convention allows for specifying character code points numerically. Example: “A” === “\u0041”

Strings have length property. Example: “Seven”.length === 5
Strings are immutable. Once it is made, a string can never be changed. But it is easy to make a new string by concatenating other strings together with the + operator.
	- Two strings containing the exact same characters in the same order are considered to be the same string.
	- Strings have methods ‘cat’.toUppercase() === ‘CAT’

Statements:
	A compilation unit contains a set of executable statements. In web browsers, each <script> tag delivers a compilation unit that is compiled and immediately executed. Lacking a linker, Javascript throws them all together in a common global namespace.

When used inside a function, the var statement defines the functions private variables.

The switch, while, for, and do statements are allowed to have an optional label prefix that interacts with the break statement.

Statements can be an expression statement, disruptive statement, or try statement.

Statements are executed from top to bottom. The sequence of execution can be altered by the conditional statements (if and switch), by the looping statements (while, for and do), by the disruptive statements (break, return, and throw), and by function invocation.

A block is a set of statements wrapped in curly braces. Blocks don’t create a new scope so variables should be defined at the top of the function not in blocks.

Falsey values: false, null, undefined, the empty string ‘’, the number 0, the number NaN
Truthey: all other values (including the string ‘false’)


Switch Statement: performs a multiway branch. It compares the expression for equality with all of the specified cases. The expression can produce a number or string. When an exact match is found, the statements of the matching case clause are executed. If there is no match, the optional default statements are executed.
	- A case clause contains one or more case expressions. The case expressions need not be constants. The statements following a clause should be a disruptive statement to prevent fall through into the next case. The break statement can be used to exit from a switch.

var text;
var fruits = document.getElementById("myInput").value;

switch(fruits) {
    case "Banana":
        text = "Banana is good!";
        break;
    case "Orange":
        text = "I am not a fan of orange.";
        break;
    case "Apple":
        text = "How you like them apples?";
        break;
    default:
        text = "I have never heard of that fruit...";
}

A while statement performs a simple loop. If the expression is falsey, then the loop will break. While the expression is truthey, the block will be executed.

The for statement has 3 optional clauses:
	1. initialization: initializes the loop variable
	2. condition: tests loop the variable against a completion criterion. if condition is omitted, the condition of true is assumed. if condition is false, the loop breaks.
	3. increment: loop increments if the block is executed and loop repeats with the condition

	for (var i = 0; i < array.length; i++) {
		array.push(“hi”) // “hi” is added for every item of in the array
	}


The other “for in” loop enumerates the property names (or keys) of an object. On each iteration, another property name string from the object is assigned to the variable. 
	- usually necessary to test object.hasOwnProperty(variable) to determine whether the property name is truly a member of the object or was found instead of the prototype chain:
	for (myVar in obj){
		if (obj.hasOwnProperty(myvar)){
			…
		}
	}


The do statement: similar to while statement except the expression is tested after the block is executed instead of before . That means the block will always be executed at least once.
	=> do {block} while (expression);

The try statement: executes a block and catches any exceptions that were thrown by the block. The catch clause defines a new variable that will receive the exception.
	=> try {block} catch (variable name) {block}

The throw statement raises an exception. If the throw statement is in a try block, then control goes to the catch clause. Otherwise, the function invocation is abandoned and control goes to the catch clause of the try in the calling function.
	
The expression is usually an object literal containing a name property and a message property. The catcher of the exception can use that information to determine what to do.
	 => throw (expression);

The return statement causes the early return from a function. It can also specify the value to be returned. If a return is not specified, then the return value will be undefined. JS does not allow a line end between the return and the expression.
	=> return (expression);

The break statement causes the exit from the loop statement or a switch statement. It can optionally have a label that will cause an exit from the labeled statement. JS does not allow a line end between the break and the label.

An expression statement can either assign values to one or more variables or members, invoke a method, delete a property from an object. The = operation is used for assignment

The simplest expressions are:
	- a literal values (string or number), 
	- invocation expressions preceded by new
	- a refinement expression preceded by delete, 
	- expression wrapped in parentheses
	- an expression preceded by a prefix operator
	- an expression followed by: an infix operator
	- another expression, the ? ternary operator followed by another expression, then by :, and then by yet another expression, 
	-an invocation
	- a refinement.

Operators: Here is the order of precedence from highest to lowest. (higher means they bind the tightest… parentheses can be used to alter the normal precedence):

Highest Precedence:
. [ ] ( ) … refinement and invocation
delete new typeof _ - ! … unary operators
* / % … multiplication, division, modulo
+ -  … add/concat, subtract
 >= <= > < … inequality
=== !=== … equality
&& … logical and
|| … logical or
? : … ternary
Lowest Precedence

Typeof: produces values like ’number’, ‘string’, boolean, undefined, function, object. If the operand is an array or null, the the result is object

The / operator can produce a noninteger even if both operands are integers.

A refinement is used to specify a property or element of an object or array. 

Object literals: notations for specifying new objects. The names of the properties can be specified as names or as strings. The names are treated as literal names, not variable names (so the names of the properties of the object must be known at compile time). the values of the properties are expressions.
	=> { objectName : expression }

Array literals: specify new arrays
	=> [ element, element2 ]

Function literals: defines a function value. It can have an optional name that it can use to call itself recursively. It can specify a list of parameters that will act as variables initialized by the invocation arguments. The body of the function includes variable definitions and statements.



