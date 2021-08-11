analog documentation
--------------------

All keywords are non-alphanumeric symbols.
These keywords are often combined into sets of 2 in order to provide specific functionality.

Here is a general overview of which concepts relate to which symbols...

```bash
#   | comment
?   | conditional evaluation
%   | catch-all
|   | looping
:   | printing to stdout
!   | interpolation
()  | shell command
&   | command status code
..  | linebreak
```

Some early examples utilize syntax declared later in the documentation.
Everything should be clear after reading the documentation in full.

If it isn't, please file an issue.


### Indentation

`analog` interprets code left to right, line by line.

Blocks of code are denoted and evaluated through indentation.

Valid indentation is 4 space characters.

```bash
block =
    :: "this is a block of code... "
    :: "it prints text to stdout."
```

*Tab characters and any other number of spaces are considered invalid syntax and will not compile.*

### Inline syntax

Reserved keywords such as the `%` or `:` operators can be used inline rather than indenting.

```bash
x = 0

# indented
|?| x << 3
    :: x
    ++ x

# inline
|?| x << 3 :: x ++ x
```

Lines which call user made functions can be ambiguous, so the `..` operator must be used.

```bash
x = 0
increment n = ++ n

# indented
|?| x << 3
    :: x
    increment x

|?| x << 3 :: x .. increment x
```


### Comments

Any character following a `#` will not be read by the compiler / interpreter.

```bash
# this is a comment...

x = 3 # the code to the left will be read
```


### Types

`analog` is a typed language.
The following are the available types and their syntax...

```bash
word        # function call
@macro      # builtin type/macro

(cmd)       # shell command
&0          # status code

"str"       # string
'a'         # char
42          # int
3.14        # float
[ 1 2 3 ]   # array
#{ k:v h:p } # hashmap
```


#### Functions
All barewords are function calls.

Function names must begin with a letter and can contain `a-Z`,`0-9`, and the `_` character.

Since all keywords are non-alphanumeric, the programmer has a blank slate for every function name on every script.
If you call a function that does not exist then your script will not compile.

```bash
f =          # function
f a b =      # function which takes arguments
#f * = @args  # function which takes any number of arguments
```

Here is an example of a custom made 'increment' function which prints a supplied number and then increments it.

```bash
increment x =
    :: x
    ++ x
```


#### Aliases
Provides type aliasing capabilities.
Unsure of full potential here...

```bash
@true   # truthy variant
@false  # falsy variant

@&      # shell command status code
@cmd    # [ @& "stdout" "stderr" ]
@args   # "[arguments] when file called as a script
```


#### Variables
Variables are declared through function definitions.

```bash
f = "str"       # string
f = 'c'         # char
f = 42          # int
f = 3.14        # float
f = [ 1 2 3 ]   # array
#f = { k:v h:p } # hashmap
```


#### Strings
All strings are UTF-8, immutable and consist of all chars between `" "`s.

Equivalent to a Rust `str`.

```bash
str = "I am immutable"
multi = "I can be a multi-line string
    simply by indenting, or by using
    \"escape\" \nsequences."
```

##### Escaping
The following are valid escape sequences within strings and are equivalent to chars:

```bash
\n  # Newline
\r  # Carriage return
\t  # Tab
\"  # Double quote
\\  # Backslash
\!  # Exclamation
\0  # Null
```


#### Chars
Equivalent to a Rust `char`.

```bash
char = 'c'
```


#### Integers
Equivalent to a Rust `i32`.

Integers have 2 builtin operators.

```bash
++   | increment by 1
--   | decrement by 1
```

As shown in the first example...

```bash
x = 0

|?| x << 3 :: x ++ x
```


#### Arrays
Equivalent to a Rust `Vec`.
All items in an array must be of the same type.

```bash
array_ints = [ 1 2 3 ]
array_floa = [ 1.5 2.8 3.1 ]
array_char = [ 'a' 'b' 'c' ]
array_strs = [ "one" "two" "e and f" ]
```

You can specify the type for an array in order to prevent needing to encapsulate each value.

```bash
array_floa = .[ 1 2 4.3 ]
array_char = '[ a b c ]
array_strs = "[ quotes can be used "to include whitespace" ]

easy = "[ you can see how easy this is ... ]
hard = [ "over" "having" "to" "do" "this" "..."]
```

Function calls can be used within these array declarations with the `!` operator.

This can be thought of as 'quoting in lisp.

```bash
elderberries = "Elderberries"

result = "[ !elderberries are delicious ]

escapes = "[ this means we must escape \! and \" "when needed" ]
```


### Printing

Printing a line to stdout is accomplished with the `::` operator.
It will take any number of arguments which implement display.

```bash
a_newline = "a newline "

:: "I am immutable"
:: "I am " a_newline 4 ' ' 'y' 'o' 'u'

:: "Newlines can be created with indentation.
    Look how nicely aligned these lines are...

    You may also use ...\n
    \t\"escape characters\"

    Indentation of nesting will be ignored...
        but extra whitespace is acknowledged."
```

To print without adding a newline, use the `:;` operator.

```bash
:; "Hello"
:: " Earth!"
```


#### Array printing
Every argument passed to `:` operators will implement it's display trait.
However, arrays do not implement display.

We can use `[ ]`s to iterate over functions which return arrays.

```bash
letters = [ 'a' 'b' 'c' ]

print_abc =
    # prints newline separated
    :: ":: " [ letters ]

    # anything within brackets will iterate
    :: [ ":: " letters ]

    # we can prevent newlines after the iteration
    :; [ ":;" letters ] " here come letters! "

    # or during...
    ;; ";;" [ letters ]

# test it for yourself
print_abc
```


### Shell command execution

`analog` can call itself a 'shell' scripting language due to the ease of calling external binaries.
Every call to execute a shell command is placed in `( )`s.

- `@&` is an alias for the command's status code.
- `@cmd` is a builtin type for [ @& "stdout" "stderr" ].


```bash
# runs shell command
# you can denote the return value with a prefix

(cmd)   # returns @cmd
&(cmd)  # returns @&
!(cmd)  # returns stdout as string
!!(cmd) # returns stderr as string

# function which returns @true if status code is 0
?(cmd)

# printing of stdout and stderr is implicit
# however, it can be silenced with the `;` postfix
(cmd);

# redirection and piping can be accomplished within `( )`
(cmd < stdin > stdout &> stderr)
(cmd1 | cmd2)

# you can chain short circuiting commands with `&&`
(cmd1 && cmd2)

# or by having multiple commands on the same line
(cmd1) (cmd2)

# logical "or" operations can be accomplished with `||`
(cmd1 || cmd2)

# "or" through a catch-all
?(cmd1) % (cmd2)
```

#### Command grouping
You can group commands with additional `( )`s.

```bash
# grouping evaluates the same as being inline but provides us access as an array.

( (cmd 1) (cmd 2) )     # returns [@cmd]
&( (cmd 1) (cmd 2) )    # returns [@&]
!( (cmd 1) (cmd 2) )    # returns ["stdout"]
!!( (cmd 1) (cmd 2) )   # returns ["stderr"]

# function which returns @true if status code for ALL commands is 0
?( (cmd 1) (cmd 2) )

# groups allow you to expand commands
(cmd -p (1) (2) )

# groups also provides access to an indented syntax
(cmd -p
    (1)
    (2)
)
```

#### Asynchronous commands
You can run groups of commands asynchronously with `[ ]`s.

```bash
[ (cmd1) (cmd2) ]   # returns [@cmd]
&[ (cmd1) (cmd2) ]  # returns [@&]
![ (cmd1) (cmd2) ]  # returns ["stdout"]
!![ (cmd1) (cmd2) ] # returns ["stderr"]

# runs, prints, returns @true if status code of ALL commands is 0
?[ (cmd1) (cmd2) ]

# commands can still be expanded...
$[cmd -p (1) (2) ]
```


#### Interpolation
Interpolation in shell commands is accomplished by placing a function call within `! !`s.

You cannot nest interpolations.

```bash
bin = "~/.cargo/bin/analog"
cache = "~/.cache/analog/"
config = "~/.config/analog/"

uninstall_analog = (rm -rf !bin! !cache! !config!)
```


### `?` Conditionals

Any statement that begins with a `?` will perform some form of comparison.

There are `if` statements and `match` statements.

```bash
? x < 5     # if x is less than 5
? x         # match on type
```

#### Comparison operators

```bash
<<      # less than
<=      # less than or equal to
>>      # greater than
>=      # greater than or equal to
==      # equal
=/      # not equal
=%      # remainder
=&      # remainder
```

#### If / Else

When a line begins with a `?` and contains a comparison operator:

- `?` can be read as "if"
- `%` can be read as "else"
- `%?` can be read as "else if"

- `%:` is a shorthand for "else print line"
- `%;` is a shorthand for "else print (without newline)"

Unlike most other indented languages, the else statement is part of the indentation.

```bash
# if / else
? "pretzels" =/ "pancakes"
    :: "logical."
    %: "sounds delicious!"

# if / else if / else
day = "day"
suns = 2

? day == "night"
    :: "uh oh..."
    %? suns >> 1
        :: "whew!"
    %: "logical."
```

#### Matching

When a line begins with a `?` and contains a returned type:

- `?` can be read as "match"
- `%` can be interpreted as a "catch-all"

A match statement will stop evaluation after its first match.

```bash
# string

hardwoods = [ "oak" "maple" "cherry" "walnut" ]
softwoods = [ "spruce" "pine" "cedar" "yew" ]

? "pine"
    == [ hardwoods ] :: "is a hardwood"
    == [ softwoods ] :: "is a softwood"
    == "bamboo" :: "good stuff..."
    %: "plant more trees!"

# integer and float
num = 9

? num
    <= 5 :: "less than or is 5"
    >> 6 :: "more than 6"
    == 6 :: 6

# true/false
?(uname)
    :: "command succeeded!"
    %: "command failed"

# note the distinct syntax above.
# the following is invalid syntax
? ?(uname)
```

The `?` keyword makes the following "if" and "match" statement synonymous;
the only difference is where the indentation is placed.

```bash
? "q" == "q"
    :: "continue"
    %: "um..."

? "q"
    == "q" :: "continue"
    %: "um..."

# inline
? "q" == "q" :: "continue" %: "um..."
```

Match arms can also be arrays which will be iterated through.

```bash
bsd = [ "FreeBsd" "NetBsd" ]

? !(uname)
    == "Linux" :: "penguins"
    == "Redox" :: "ions"
    == [ bsd ] :: "angels"
    %: "change"
```


### `||` Loops

Any statement that begins with `| |`s will perform some form of loop.

There are `for` statements and `while` statements.

```bash
|x| y       # for x in y
|x y| z     # for [x y] in z
|?| x << 3  # while x is less than 3
```

#### For

`|n| array` can be read as "for `n` in array".

```bash
nums = [1 2 3]

|n| nums
    :: n
    ? n == 3 :: "number 3 is in the list"
```

You can get creative with nesting and inline syntax...

```bash
nums = [ [1 2 3] [4 5 6] ]
|n| nums :: "n " n
    |i| n :: "i " i
        ? i == 2 :: "number 2 is in the list"
        ? i == 4 :: "number 4 is in the list"
```

#### While

When a statement begins with `|?|` followed by a comparison operator, the nested code will loop until the predicate is satisfied.

`|?| x << 3` can be read as "while x is less than 3"

Anything after an optional `%` operator will execute immediately once the predicate is satisfied.

The following is equivalent to the first `for` example

```bash
nums = [1 2 3]

|?| n << 3
    :: n
    ++ 1
    %: "reached 3"
```


### Example

`analog`'s syntax is meant to provide enhanced reliabilty in the area shell scripting through a syntax that is simple to read and understand.

Let's make a backup script...

We start with separate commands.

```bash
(rsync -avhP ~/.config/ /media/usb/backup/.config/)
(rsync -avhP ~/.local/ /media/usb/backup/.local/)
```

And then expand them...

```bash
(rsync -avhP
    (~/.config/ /media/usb/backup/.config/)
    (~/.local/ /media/usb/backup/.local/)
)
```

Now lets use some interpolation...

```bash
backup = "/media/usb/backup/"

(rsync -avhP
    (~/.config/ !backup!.config/)
    (~/.local/ !backup!.local/)
)
```

Or whatever works for you...

```bash
backup = "/media/usb/backup/"
folders = "[ .config .local ]

|f| folders
    (rsync -avhP ~/!f!/ !backup!!f!/)
```

That's it!
