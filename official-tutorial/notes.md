## SCOPE

This section tracks notes from [The Official Python Tutorial](https://docs.python.org/3.8/tutorial/index.html).

## 2. Using the Python Interpreter

### 2.1 Invoking the Interpreter
Issue a command to the Python interpreter via `python3 -c command [arg] ...`. It is best practice to enclose the command in single quotes due to Python's usage of special characters. Remember:
* For single quotes, the literal value of each character is preserved.
* For double quotes, the literal value of each character except for `$`, `` ` ``, `!`, and `\` is preserved. The backslash is only special when followed by `$`, `` ` ``, `"`, `\`, or newline. When followed by one of those characters, the backslash is removed; otherwise, they are unmodified.

Invoke a module as a script via `python3 -m module [arg] ...`. Enter interactive mode afterwards with an additional `-i` before the script.

#### 2.1.1 Argument Passing
The script name and any additional arguments are stored in the `argv` variable in the `sys` module. Based on the script and arguments given, `sys.argv[0]` will be one of the following:
* `''`  when `python3 -i` is used;
* `'-'` when the `python3 -i -` is used (standard input);
* `'-c'` when `python3 -ic command [arg] ...` is used; or
* `'module'` when `python3 -im module [arg] ...` is used.

Any other arguments passed in `[arg] ...` will be stored in the subsequent indices of `sys.argv`.

#### 2.1.2 Interactive Mode
In interactive mode, the primary prompt is displayed as `>>>`. When multi-line constructs are invoked, a secondary prompt will be displayed on any continuation lines as `...`.

### 2.2 The Interpreter and Its Environment

#### 2.2.1 Source Code Encoding
By default, Python source files are treated as having the UTF-8 encoding. The standard library (and anything you write) should only use ASCII characters for identifiers. To display those characters correctly, the following must be true:
* The editor must recognize that encoding as UTF-8; and
* The font must support all of the characters in the file.

The default encoding can be overwritten by adding a special comment line at the top of the file: `# -*- coding: encoding -*-`, where _encoding_ is one of the valid codecs supported by Python. For files with a UNIX "shebang" line at the top, the encoding line shall be the second line of the file.