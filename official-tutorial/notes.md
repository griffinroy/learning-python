# SCOPE

This section tracks notes from [The Official Python Tutorial](https://docs.python.org/3.8/tutorial/index.html).

# 2. Using the Python Interpreter

## 2.1 Invoking the Interpreter
Issue a command to the Python interpreter via `python3 -c command [arg] ...`. It is best practice to enclose the command in single quotes due to Python's usage of special characters. Remember:
* For single quotes, the literal value of each character is preserved.
* For double quotes, the literal value of each character except for `$`, `` ` ``, `!`, and `\` is preserved. The backslash is only special when followed by `$`, `` ` ``, `"`, `\`, or newline. When followed by one of those characters, the backslash is removed; otherwise, they are unmodified.

Invoke a module as a script via `python3 -m module [arg] ...`. Enter interactive mode afterwards with an additional `-i` before the script.

### 2.1.1 Argument Passing
The script name and any additional arguments are stored in the `argv` variable in the `sys` module. Based on the script and arguments given, `sys.argv[0]` will be one of the following:
* `''`  when `python3 -i` is used;
* `'-'` when the `python3 -i -` is used (standard input);
* `'-c'` when `python3 -ic command [arg] ...` is used; or
* `'module'` when `python3 -im module [arg] ...` is used.

Any other arguments passed in `[arg] ...` will be stored in the subsequent indices of `sys.argv`.

### 2.1.2 Interactive Mode
In interactive mode, the primary prompt is displayed as `>>>`. When multi-line constructs are invoked, a secondary prompt will be displayed on any continuation lines as `...`.

## 2.2 The Interpreter and Its Environment

### 2.2.1 Source Code Encoding
By default, Python source files are treated as having the UTF-8 encoding. The standard library (and anything you write) should only use ASCII characters for identifiers. To display those characters correctly, the following must be true:
* The editor must recognize that encoding as UTF-8; and
* The font must support all of the characters in the file.

The default encoding can be overwritten by adding a special comment line at the top of the file: `# -*- coding: encoding -*-`, where _encoding_ is one of the valid codecs supported by Python. For files with a UNIX "shebang" line at the top, the encoding line shall be the second line of the file.

# 3. An Informal Introduction to Python
Comments: Start with the hash character `#` and extend to the end of the line. A hash character within a string literal is not interpreted as a comment, but rather as a literal hash character.

## 3.1 Using Python as a Calculator

### 3.1.1 Numbers
Unless otherwise specified, the following operators support mixed-type operands; integers will be converted into floating-point type if _any_ of the operator's operands are floating-point type. Otherwise, they will return integers.
* `+`, `-`, `*`, and `/` are the basic addition, subtraction, multiplication, and division operators.
    * `/` will always return a floating-point result, even if both of its operands are integers.
* `//` is floor division (rounds down, even for negative numbers).
* `**` is the power operator (`5 ** 2` returns `25`).
* `()` groups operations to override default order of operations.
* `=` is the assignment operator (there is no result printed from this operation)

Any printed results are stored in `_` (like `ans` in MATLAB).

### 3.1.2 Strings
String values can be specified using either `'...'` or `"..."`. With either style, enclosed occurrences of that quote type must be escaped (e.g. `'"doesn\'t"'` and `"\"doesn't\""` are the "same"). Single- vs. double-quoted strings are not different types.
* Observation: the interpreter seems to favor simplicity, so it will cast strings to be whichever allows it to have the fewest escaped characters.

Python supports special characters in strings (e.g. newline is `'\n'`), but they will not be treated specially unless printed via the `print()` function.
```
>>> foo = 'first line\nsecond line'
>>> foo
'first line\nsecond line'
>>> print(foo)
first line
second line
```

To prevent this behavior in `print()` (for things like paths, etc. which often use the backslash), declare those strings as _raw_ strings:
```
>>> foo = r'C:\some\name'
>>> foo
'C:\\some\\name'
>>> print(foo)
C:\some\name
```

Multi-line strings are supported via `'''...'''` and `"""..."""`. You can escape newline characters with `\`.

Operators:
* `+` concatenate string literals, variables, and expressions.
* `*` repeat string literals, variables, and expressions
* `s[n]` indexes the string, returning a substring (no char type)
    * Out-of-bounds indexing results in an error.
    * Positive indexing begins with `0`, ends with `len(s)-1`.
    * Negative indexing begins with `-1` (corresponding to the last character in the string), and ends with `-len(s)` (corresponding to the first character in the string).
    * _Slicing_ returns a substring, instead of a single character (e.g. `'python'[0:2]` returns `'py'`, `'python'[2:5]` returns `'thon'`):
        * Out of bounds slicing does not result in an error.
        * The starting index is always included, and the ending index is always excluded. Therefore, `s[:i] + s[i:]` always yields `s`.
        * Slicing can be a ternary operation: `s[start:end:stepsize]`. For example, to reverse a string of unknown length `s`, use `s[-1::-1]` (read: "begin at the `-1`st index, and proceed to the end via a step size of `-1`").

Python strings are immutable (you cannot change them); in order to "change" a string, you must create a new one.

### 3.1.3 Lists
Lists are mutable (can be changed) compound data types (they can contain multiple different types). The syntax for defining a list is as follows:
```
>>> myList = [1, 4, 9, 16, 25]
```

Indexing and slicing a list returns _references_ to those elements. Therefore, unlike strings, list elements can be changed after instantiation.

## 3.2 First Steps Toward Programming
Multiple assignment: Can assign multiple values at once using the syntax `foo, bar = 'baz', 'qux'`. The right hand side of a multiple assignment expression is evaluated from the left to the right, with all evaluation happening before any assignment.

While loops: Loop over expressions while the condition is true. Zero and non-zero integers map to `false` and `true` respectively, like in C++. An empty sequence type (e.g. string, list) is `false`, a non-empty sequence type is `true`. Comparison operators are the same as in C (`<`, `>`, `<=`, `>=`, `!=`, `==`).

Expression scope is determined by indentation (unlike C/C++, which define scope using `{}`):
<pre>
>>> while <i>condition</i>:
...     <i>expressions</i>
...
</pre>

# 4. More Control Flow Tools

## 4.1 `if` Statements
Python syntax for if-elseif-else statements:
<pre>
>>> if <i>condition</i>:
...     <i>expressions</i>
... elif <i>other_condition</i>:
...     <i>expressions</i>
... else:
...     <i>expressions</i>
...
</pre>

## 4.2 `for` Statements
Python's `for` loop syntax differs from that of C/C++; instead of specifying the iteration step and halting condition (something like `for (int i = 0; i < 10; ++i) ...`), a `for` loop in Python iterates over the items in a sequence (like a list, etc.) in the order that they appear:
```
>>> words = ['cat', 'window', 'defenestrate']
>>> for w in words:
...     print(w, len(w))
...
cat 3
window 6
defenestrate 12
```

It is recommended to not modify a sequence that you are iterating over; instead, loop over a copy and modify the original.

## 4.3 The `range()` Function
The `range()` function generates arithmetic sequences (great for `for` loops). Similarly to slicing, it takes up to three arguments: `range(start, end, stepsize)`. These arguments can only be integers. Range objects are considered to be an _iterable_ type, which is compatible with `for` loops and some functions (like `sum()`). 

`range` objects can be indexed, but they do not behave like `list`s when printed. You can create a `list` object from a `range` object, however:
```
>>> my_range = range(0, 5)
>>> print(my_range)
range(0, 5)
>>> my_list = list(my_range)
>>> print(my_list)
[0, 1, 2, 3, 4]
```

## 4.4 `break` and `continue` Statements, and `else` Clauses in Loops
The `break` statement behaves similarly in Python to C/C++: it breaks out of the innermost enclosing `for` or `while` loop.

The `continue` statement behaves similarly in Python as in C/C++: it skips execution to the next iteration of the innermost enclosing `for` or `while` loop.

`for` and `while` loops can have `else` clauses. In a `for` loop, the `else` clause executes after the loop has exhausted its iterable. If a `while` loop, the `else` clause executes after the loop condition becomes false. The `else` clause will _not_ execute if either loop was exited via a `break` expression. That is, a loop's `else` clause will execute if the loop has a nominal exit condition.

## 4.5 `pass` Statements
The `pass` statement does nothing. It is a filler statement for cases where a statement is required syntactically, but no action is required. It is silently ignored.

## 4.6 Defining Functions
The following is an example of a function in Python:
<pre>
>>> def fib(n):
...     <i>"""Print the Fibonacci series up to n."""</i>
...     a, b = 0, 1
...     while a < n:
...         print(a, end=' ')
...         a, b = b, a + b
...     print()
...
>>> fib(500)
0 1 1 2 3 5 8 13 21 34 55 89 144 233 377
</pre>

The `def` keyword begins a function's definition. What follows it is the name of the function, and a parenthesized list of parameters. In the following lines, the function's statements are indented

The string immediately following the definition statement is the docstring, which is a string that documents the function.

Since everything in Python is an object (see the [aside](#aside-pass-by-value-vs-reference)), there is nothing special about a function's name; one could write `f = fib`, call `f(100)`, and get the same result as calling `fib(100)`.

By default, the value returned by all functions is `None` (built-in name). This result is usually suppressed by the interpreter (but can be seen by calling `print(f())`):
```
>>> def f():
...     pass
...
>>> f()
>>> print(f())
None
```

The return value can be overwritten however, with the addition of a `return` statement. A function can return multiple values.

### Aside: Pass by Value vs. Reference
One key paradigm to understand argument passing behavior is that every expression in Python evaluates to a reference. For example, the expression `'foo'` evaluates to a reference to a string literal that contains the letters `'foo'`. Assigning that reference to a variable (e.g. `s1 = 'foo'`) results in that variable referencing the same object. But if one were to then call `s1 = 'bar'`, the reference to the immutable string `'foo'` is broken, and `s1` instead contains a reference to the immutable string `'bar'`.

These _immutable_ objects can be thought of as referencees to const objects (in a C++ sense) insofar as the referenced object cannot be changed, but unlike C++ the reference can be reseated to refer to a different object.

Thus, in keepint with the paradigm, all arguments are passed as references. If that object is mutable, mutations of that object within the function's execution will be reflected in the symbol table of the caller (i.e. C++ pass-by-reference behavior). If an operation within the function overwrites the reference (e.g. re-seating it to reference another object), then any further operations on that object are disassociated from the original reference and will thus be lost outside the scope of the function (i.e. C++ pass-by-value behavior). Any changes to immutable objects break the object reference by definition, and therefore mimic pass-by-value behavior.

## 4.7 More on Defining Functions
In Python, functions can be called with a variable number of arguments. This function can be called with fewer arguments than the maximum it is designed to allow:
```
def guessPassword(password = "password", max_attempts = 3):
    """Gives the user max_attempts attempts to guess the password"""
    attempts = 0
    while attempts < max_attempts:
        guess = input("Guess the password: ")
        attempts += 1

        if guess == password:
            print("Access granted.")
            return True

        else:
            print("Incorrect.", end=' ')

    print("Access Denied.")
    return False
```

Note: default values are evaluated only once (at the point of function definition). Any changes to the default value will be reflected in subsequent calls to the function.

### 4.7.1 Positional Arguments
All of the below are valid calls to the function `guessPassword()`. Note that optional arguments given in this way are _positional_; they must correspond to the ordering in the function definition statement:
1. `guessPassword()`: Give only required arguments
2. `guessPassword("foo")`: Give required arguments and the first optional argument
3. `guessPassword("foo", 1)`: Give required arguments and multiple optional arguments

### 4.7.2 Keyword Arguments
Optional arguments can be given out of order via the use of keyword arguments (of the form `kwarg=value`). This allows calls of the function the modify any ordering of any subset of the optional arguments:
* `guessPassword(max_attempts=1)`: overwrites value of `max_attempts` without changing default value of `password`

Keyword arguments must follow positional arguments in a function call. All parameters can be passed as keyword arguments, except when the function has the special `/` or `*` parameters (see [Section 4.7.3](#473-special-parameters)).

### 4.7.3 Special Parameters
A developer can force users to pass arguments to the function in a specific way via the `/` and `*` parameters. 
* All parameters preceding the `/` parameter must be passed positionally
* All parameters preceding the `*` parameter can be passed positionally or via keyword
* All parameters following the `*` parameter must be passed via keyword

### 4.7.4 Arbitrary Argument Lists
Arbitrary arguments can be passed to a function following zero or more formal arguments. Any parameters given after an arbitrary parameter are necessarily passed by keyword argument only. These _variadic_ arguments will be stored in a tuple:
```
>>> def generateRelativePath(*folders, filesep='/'):
...     return filesep.join(folders)
```

An analogous option exists for arbitrary keyword arguments.
```
def f(formal_args, **other_kwargs):
    ...
```

FIXME: find a use case for this functionality.

### 4.7.5 Unpacking Argument Lists
A tuple storing data that are desired to be passed into a function separately as arbitrary positional arguments may be unpacked via the `*` operator:
```
>>> range_args = [5, 0, -1]
>>> list(range(*range_args))
[5, 4, 3, 2, 1]
```

Similarly, a dictionary storing data that are desired to be passed into a function separately as keyword arguments may be unpacked via the `**` operator:
```
>>> guessPassword_args = {"max_attempts" : 2, "password" : "foo"}
>>> test.guessPassword(**guessPassword_args)
Guess the password: password
Incorrect. Guess the password: foo
Access granted.
```

### 4.7.6 Lambda Expressions
Lambdas provide shorthand syntax for the definition of anonymous, single-expression functions. The following defines a function `f` that adds together two numbers: `f = lambda a, b: a + b`. Lambdas can be used like other function objects and, like nested functions, can reference variables from the containing scope.

FIXME: find a good use case for this functionality.

### 4.7.7 Documentation Strings (Docstrings)
Follow these conventions for docstrings:
```
>>> def f():
...     """Short, concise one-line description of function.
...
...     Detailed description, following one blank line. Indentation
...     determined by that of the first line of the detailed
...     description. Closing quotes on their own line.
...     """
...     pass
...
```

A function's docstring can be accessed via `print(f.__doc__)`.

### 4.7.8 Function Annotations
Function annotations are optional metadata that document the input and return type(s) of the function. A function's annotations can be accessed via `print(f.__annotations__)`, and set via the following syntax:
```
>>> def printMeal(food: str = "bread", drink: str = "water") -> str:
...     return food + " and " + drink
...
>>> print(printMeal.__annotations__)
{'food': <class 'str'>, 'drink': <class 'str'>, 'return': <class 'str'>}
```

## 4.8 Intermezzo: Coding Style
Most Python projects adhere to the [PEP8](https://peps.python.org/pep-0008/) style guide. Here are the basics:
* Use 4-space indentation (no tabs)
* Wrap lines starting at the 80th character/column
* Separate functions, classes, and larger code blocks within functions with blank lines
* When possible, put comments on their own line
* Use docstrings
* Use spaces around operators and after commas, but not directly inside of bracketing constructs (e.g. `a = f(1, 2) + g(3, 4)`)
* Use `UpperCamelCase` naming for classes, and `lowercase_with_underscores` naming for functions and methods. Always use `self` as the name for the first method argument (RE: classes)
* Don't use non-default encodings
* Don't use non-ASCII characters in identifiers
