analog reference
----------------

For detailed information please consult the [documentation](./docs.md).


### Functions

```bash
# declaration
print_var x = :: x

# call
print_var "pancakes"
```


### Variables

```bash
f = "str"       # string
f = 'c'         # char
f = 42          # int
f = 3.14        # float
f = [ 1 2 3 ]   # array
#f = { k:v h:p } # hash map
```


### Aliases

```bash
@true       # truthy type variant
@false      # falsy type variant

@&          # shell command status code
@cmd        # [ @& "stdout" "stderr" ]
```


### Printing

```bash
:: "print to stdout"
:; "print without newline "
:: 3

:: "Newlines can be added with indentation,
    and they will be nicely aligned.

    You may also use ...\n
    \t\"escape characters\"

    Indentation of nesting will be ignored...
        but extra whitespace is acknowledged."
```


### Array Printing

```bash
letters = [ 'a' 'b' 'c' ]

print_abc =
    :: ":: " [ letters ] # newline separated
    :: [ ":: " letters ] # anything within brackets will iterate
    ;: ";;" [ letters ]  # prevent newlines during iteration
```


### Escaping

These will match standard Rust escape codes with the addition of `\!`.

```bash
\n  # Newline
\r  # Carriage return
\t  # Tab
\"  # Double quote
\!  # Exclamation
\\  # Backslash
\0  # Null
```


### Integer Operators

```bash
num = 3

++ num  # increment by 1
-- num  # decrement by 1
```


### Shell Commands

```bash
# runs command
(cmd)   # returns @cmd
&(cmd)  # returns @&
!(cmd)  # returns stdout as string
!!(cmd) # returns stderr as string

# returns @true if status code is 0
?(cmd)

# silence stdout
(cmd);

# redirection and piping
(cmd < stdin > stdout &> stderr)
(cmd1 | cmd2)

# grouping commands, returns [@cmd]
(cmd (1) (2) ) # short circuiting
[cmd (1) (2) ] # all commands are run asynchronously

# interpolation
cache = "~/.cache/analog"
(rm -rf !cache!)
```


### If / Else

```bash
day = "day"
suns = 2

? day == "night"
    :: "uh oh..."
    %? suns >> 1
        :: "whew!"
    %: "logical."
```


### Match

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

# status code check`
```bash
?(uname)
    :: "command succeeded"
    %: "command failed"
```


### For

```bash
nums = [ 1 2 3 ]

|n| nums
    :: n
    ? n == 3 :: "number 3 is in the list"
```


### While

```bash
num = 1

|?| n << 3
    :: n
    ++ 1
    %: "reached 3"
```


