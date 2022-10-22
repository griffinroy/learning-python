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
```
>>> while condition:
...     expression1
...     expression2
...
```
