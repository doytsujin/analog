analog Reference
================

Statements
----------

```rust
~     ~ comment
=     ~ variable
==    ~ const

:=    ~ fn
:==   ~ const fn
:     ~ fn call

@=    ~ struct/enum
@:    ~ impl
@@    ~ type
@@@   ~ trait

+     ~ use
++    ~ pub use
+=    ~ mod

?     ~ if
??    ~ match
?=    ~ if let

||    ~ loop
|n|   ~ for
|?|   ~ while
|?= | ~ while let

::  ~ println!
:.  ~ print!
:;  ~ eprintln!
:,  ~ eprint!
```

Comments
--------

```rust
~ line comment
= pi 3.1416 ~ inline comment

~* block comment *~

~! inner line doc
~*! inner block doc *~

~~ outer line doc
~** outer block doc *~
```

Functions
---------

```rust
~ declaration
:= say
    :: "Hi"

~ with args
:= say_it a &str b &str
    :: "{} {} are!" a b

~ call
:say
:say_it "there" "you"
:say_it
    "indented"
    "we"

~ return a value
:= get_number >> u64
    > 32367482

~ indented parameters
:= see
    < a &str
    < b &str
    :: "{} {}" a b

~ where clause
:= see T E >> @Display
    < t T
    < u U
    <<< T @ Display
        U @ Display Clone
    :: "all {}" all
    all

```

### Methods/Closures

```rust
~ inline
:var.iter.map \a\ :fn a :fn b .and.unwrap

~ indented
:var.iter
    .map \a\
        :fn a
        :fn b
    .and
    .unwrap
```

Literals
--------

```rust
true      ~ bool
"str"     ~ char
"string"  ~ String
42        ~ i32
3.14      ~ f64
```

`[]`
----

```rust
[]    ~ array
&[]   ~ slice
![]   ~ Vec
```

### Array / Slice / Vec
```rust

= array [i32 4]
    1 2 3 4

= slice &[]
    1 2 3 4

= vec ![]
    1 2 3 4

:: vec[0]
```

`{}`
----

### Unit

```rust
= unit {}
```

### Tuple

```rust
= tuple {"an" 8}

= tuple {&str u8}
    "an" 8

:: tuple.0
```

### Tuple Struct

```rust
@= Planet {&str u8}

= third Planet
    "Earth" 3

:: third.0
```

### Struct

```rust
@= Door {}
    key &str
    num u8

= access Door
    key "value"
    num 82

:: access.key
```

`<>`
----

```rust
<>    ~ Option<>
?<>   ~ Result<T E>
0<>   ~ Ok<>
!<>   ~ Err<>
```

### Enum

```rust
@= Action <>
    Refresh
    Flip
    KeyPress {char}
    Click {}
        x i64
        y i64

= flips Action<Flip>
= press Action<KeyPress> "v"
= click Action<Click> x 8.3 y 1.3
```

### Option

```rust
= some <53>
= none <>
```

### Result

```rust
= ok     %<>
= err    !<>

:= result >> ?<&str {}>
    ? 22
        22 > ?<"catch">
        ^  > !{}
```

`?`
---

### If Else

```rust
= day "day"
= suns 2

? day == "night"
    :: "uh oh..."
    ^? suns > 1
        :: "whew!"
    ^: "logical"
```

### If Let

```rust
= num <7>

?= <n> num
    :: "matched " n
```

### Match

```rust
~ true/false
= motion true

? motion
    :: "we are moving!"
    ^: "we are still"

~ shell command
? (uname)
    :: "command succeeded!"
    ^: "command failed"
```

```rust
~ number
= num 9

? num
    <= 5 :: "less than or is 5"
    >  6 :: "more than 6"
    == 6 :: num
```

```rust
~ enum
@= Arrow <>
    Left
    Up
    Down
    Right

= input Arrow<Up>

? input
    Up   :: "only one way"
    Down :: "nothing here"
    ^    :: "imaginary spectrum"
```

```rust
~ iterative string
= hardwoods [] "oak" "maple" "cherry" "walnut"
= softwoods [] "spruce" "pine" "cedar" "yew"

? "pine"
    [ hardwoods ] :: "is a hardwood"
    [ softwoods ] :: "is a softwood"
    "bamboo" :: "good stuff"
    ^: "plant more trees!"
```

`|`
---

### Loop

```rust
||
    = $count 1
    ? $count == 3
        :: "three"
        >>>
    :: "{}" count
    ? count == 5
        :: "no more needed"
        <<<
```

### While

```rust
= $num 1

|?| $n << 3
    :: $n
    $n + 1
    ^: "reached 3"
```

### For

```rust
= nums []
    1 2 3

|n| nums
    :: n
    ? n == 3 :: "number 3 is in the list"
```

### Printing

```rust
:: "println"
:. "print"
:; "eprintln"
:, "eprint"

:: "Newlines can be added with indentation,
    and they will be nicely aligned"

:: """ var1 var2
    You can always use ...\n
    \t\"escape characters\"

    {var2} {var2}

    Indentation of nesting will be ignored...
        but extra whitespace is acknowledged.
```

Symbology
--------

### Keywords

From Rust:

```rust
as       ~ @
async    ~
await    ~
break    ~ <<<
const    ~ ==
continue ~ >>>
crate    ~
dyn      ~
else     ~ ^
enum     ~ @= Enum <>
extern   ~
false    ~ false
fn       ~ :=
for      ~ |n|
if       ~ ?
impl     ~ @:
in       ~ implied
let      ~ =
loop     ~ ||
match    ~ ??
mod      ~ +=
move     ~ /x\
mut      ~ $
pub      ~ +
ref      ~ &!
return   ~ >
self     ~ self
Self     ~ Self
static   ~
struct   ~ @= Struct {}
super    ~ super
trait    ~ @@@
true     ~ true
type     ~ @@
union    ~
unsafe   ~
use      ~ +
where    ~ <<<
while    ~ |?|
```

From symbol:

```rust
~ comment
` lifetime/label
! macro
@ struct/enum/impl/type/trait
#
$ mut
%
^ else/catch-all
& reference
* dereference
<
> return
+ pub/use/mod
= variable/const
, namespace chaining
. method chaining
/
? if/match
\
| loop/while/for
;
: function
'
" string

() expression grouping
[] growable collection
{} custom collection
<> monad
```

### Items

```rust
+=  ~ mod
    ~ extern crate
+   ~ use
:=  ~ fn
@@  ~ type
@=  ~ struct
@=  ~ enum
    ~ union
==  ~ const
    ~ static
@@@ ~ trait
@:  ~ impl
    ~ extern
```
