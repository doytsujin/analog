analog Documentation
====================

For a comprehensive specification, consult the [reference](./reference.md).

Symbology
---------

`analog` is composed primarily of non-alphanumeric symbols.

Here is a general overview of which concepts relate to which symbols...

```rust
=   ~ declaration
:   ~ function
@   ~ type system

?   ~ conditional
|   ~ looping

^   ~ catch-all
~   ~ comment
$   ~ mutablity

[]  ~ collection
{}  ~ custom collection
```

Statements
----------

All statements are initialized with these operators or literals.

```rust
~ struct declaration
@= Planet {}
    name   &str
    number i32

~ variable
= world Planet
    name   "Earth"
    number "3"

~ function
:= hello x &str
    :: "Hello {}" x

~ function call
:hello world
```

One special combination of symbols used throughout this book is `::`, which is shorthand for the `println!` macro.

Indentation
-----------

Blocks of code are defined through indentation and have their own scope.
Valid indentation is 4 space characters.

```rust
:= block
    :: "this is a block of code,"
    :: "which is also a function."
    :: "it has 3 println calls."
```

*Tab characters and any other number of spaces are considered invalid syntax and will not compile.*

Comments
--------

Comments are denoted by `~`.

```rust
~   line                //
~!  inner line doc      //!
~~  outer line doc      ///

~*  block  *~           /* */
~*! inner block doc *~  /*! */
~** outer block doc *~  /** */

~ this is a comment
= pi 3.14 ~ will be a variable
```

Variables
---------

Variables are declared with `=`.

```rust
= pi 3.1415927
= pie &str "rhubarb"

= ints []
    12 229 42

= strs [&str]
    "list"
    "items"
    "as needed"
```

Functions
---------

`fn` is declared with `:=` and called with `:name`.

```rust
~ declaration
:= f          ~ no args
:= f a &str   ~ with args
:= f >> &str  ~ returns a value
:== f         ~ const fn

:f            ~ no args
:f a          ~ with args
```

To `return` a value, we use `>`.

```rust
:= one_more x i8 >> {i8 i8}
    = y x + 1
    > {x y}

= both :onemore 1
```

Input parameters can be indented with `<`.

```rust
:= group_params >> ![&str]
    < param1 &str
    < param2 &str
    < param3 &str
    :: "function body begins here"
    :: "lets group..."
    > ![] param1 param2 param3
```

Return signature can also be indented after the parameters.

```rust
:= group_params
    < param1 &str
    < param2 &str
    < param3 &str
    >> ![&str]
    :: "function body begins here"
    :: "lets group..."
    > ![]
        param1 param2 param3
```

### Methods

Methods are chained with `.` notation.
When chained methods require parameters, we separate them by whitespace.

```rust
= build :Building.new.name "greenhouse" .build

= build :Building.new
    .name "greenhouse"
    .build
```

### Closures

We can define closures with `\x\`.

```rust
~ inline
:= double \x\ x * 2

~ indented
:= double \x\
    x * 2
```

Using a `Fn` parameter...

```rust
:= ten_times f F
    <<< F @ Fn{i32}
    |i| in 0..10
        f{i}

:ten_times \x i32\
    :: "hello {}" x
```

We can `move` with `/x\`.

```rust
= word "konnichiwa".to_owned

:ten_times /x\
    :: "{} {}" word x
```

### Generics / Trait Bounds

We can define a generic with `T` and apply trait bounds with the `@` prefix.

```rust
:= new T @Default >> T
    :T.default
```

However, we sometimes have long signatures.

```rust
:= what T @Display @Clone U @Clone @Debug t &T u &U >> i32
    :: "whatever"
```

So let's declare a `where` clause with `<<<`.

```rust
:= what t &T u &U
    <<< T @ Display Clone
        U @ Clone Debug
    :: "{}" t
```

We can specify return types which `impl` traits with `@`.

```rust
:= ever >> @Display
    > """
        everything
        more, like
        this
```

Reference/Dereference
---------------------

References `&` and dereferences `*` work the same as in Rust except no spaces are allowed.

```rust
= x 5
= y &x

!assert_eq(5, x)
!assert_eq(5, *y)
```

Mutability
----------

`mut` is defined with the `$` prefix.

We can mutate a variable by starting a statement with `$var`.
Compound assignment expressions can be used here.

```rust
~ declaration
= $x 4
:: "is {}" x

~ mutation
$x += 1
$x += 2
:: "then {}" x

~ shadowing
= x "shadow"
:: "I am a {}" x
```

Lifetimes
---------

Lifetimes are declared with the `` ` `` postfix.

```rust
== TRACK &str`static "thoughtograph"

:= noise`static
    < i &str`static
    < o &str`static
    ||
        :. i :. o
```

Macros
------

Due to the nature of macros, they are called exactly as they would be in Rust, but with `!` as a prefix instead of a suffix.

```rust
!println("these are")

:: "equivalent"
```

We can forego beginning and closings braces with the `!!!` prefix.

```rust
= value !!!json
    {
        "num": 200,
        "is": true,
        "features": [
            "serde",
            "json"
        ]
    }

:: "{:?}" value
```

Consts
------

To declare a `const`, we use `==`

```rust
== FOLDER &str`static "/places/found/"
```

Literals
--------

### Boolean

```rust
true
false
```

### Strings

We declare string literals with `"`.

```rust
= single "I am a str"

= multi """
    multiple lines are easy
    those 4 spaces to the left aren't read,
       but those three spaces are...
    dont forget \"escape\" \nsequences"
```

### Integer

```rust
~ implicit
= int 3

~ explicit
= int i8 -3

~ explicit (suffix)
= int -3i8
```

### Float

```rust
~ implicit
= float 3.0

~ explicit
= float @f32 1.8

~ explicit (suffix)
= int -3f32
```

`[]`
----

Collections are denoted by `[]`.

```rust
= v []      ~ array
= v &[]     ~ slice
= v ![]     ~ Vec
```

#### Array

```rust
~ implicit
= array [] 1 2 3

~ explicit
= array [u8; 3]
    1 2 3

~ single value initialization
= array [u8; 3]
    0; 3

~ access by index
:: "{}" array[0]
```

#### Slice

```rust
~ implicit
= slice &[] 1 2 3

~ explicit
= slice &[u8]
    1 2 3

~ access by index
:: "{}" slice[1]
```

#### Vec

```rust
~ implicit
= vec ![] 1 2 3

~ explicit
= vec ![u8]
    1 2 3

~ access by index
:: "{}" vec[2]
```

`{}`
----

The `unit` type, tuples and structs are all denoted by `{}`.

### Unit

```rust
= nothing {}
```

### Tuple

All Tuple values must appear within `{}`.

```rust
~ implicit
= tuple {88 true}

~ explicit
= tuple {u8 bool}
    {88 true}

~ access by index
:: "{}" tuple{0}
```

### Tuple Struct

We define named collections with `@=`.

```rust
~ declaration
@= TupleStruct {&str &str}

~ assignment
= named TupleStruct
    "easy" "pie"

~ access by index
:: "{}" named{0}
```

### Struct

Structs require defining a field for each value.
Fields and values are delimited by spaces.

```rust
~ declaration
@= Recipe {}
    onions  i32
    peppers i32
    rice    bool

~ initialization
= tasty Recipe
    onions  2
    peppers 4
    rice    true

~ access field
:: "{}" recipe{peppers}
```

`<>`
----

Enums and Rust's monadic patterns are implemented with `<>`.

#### Enums

```rust
~ declaration
@= Action <>
    Refresh
    Flip
    KeyPress{char}
    Click{}
        x i64
        y i64

~ access variant
= flip  Action<Flip>
= press Action<KeyPress{"p"}>
= click Action<Click>
    x 7 y 0
```

#### Option `<>`

`Option<T>` has the shorthand `<T>`

```rust
= option <3>

~ match
?? option
    <3> :: "Some(3)"
    <>  :: "None"

~ if let
?= <3> option
    :: "Some(3)"
    ^: "None"
```

#### Result `?<>`

`Result<T, E>` has the shorthand `<T E>?`.

- `Ok<T>` has the shorthand `<T>%`
- `Err<E>` has the shorthand `<E>!`

*Empty braces imply the `unit` type.*

```rust
= ok <&str>% "works"
= err <&str>! "errdout"

:= check status <bool> >> <i32, {}>?
    ?= b <status>
        :: "{}" b
        >> <123>%
        ^> <>!
```

However, quite often we do not need detailed error handling.
We can automate usage with `<T>??`. This will automate `Result` with the `anyhow` crate.

```rust
:= get_config >> <HashMap>??
    = config std,fs,read_to_string? "config.json"
    = map HashMap serde_json,from_str? &config
    -> map
```

The `?` postfix operator works like it does in Rust.

`@:` Impl
---------

We can define `impl` blocks with `@:`.

Using our previous `Recipe` struct...

```rust
@= When {}
    x i32
    y i32
    z i32

@: When
    := origin >> Self
        > Self x 0 y 0 z 0

    := up $self >> Self
        $self{z} self{z} + 1
```

To implement a trait on a type we use `<`.

```rust
+ std,fmt

@= Point {}
    x f64
    y f64

@: Point < std,fmt,Display
    := fmt self f &$fmt,Formatter`_ >> fmt,Result
        !write(f, "({self.x}, {self.y})")

= origin Point x 0 y 0
:: "{}" origin
```

Lets add some generics...

```rust
~ inline
@: SpecialType < CustomTrait A @TraitA
    := traitfn

~ indented
@: SpecialType < CustomTrait A B
    <<< A @ TraitA
        B @ TraitC TraitD
    := traitfn
```

`@@` Alias
----------

We can define a `type` alias with `@@`.

```rust
@@ Point {u8 u8}

= here Point(48 62)

:: "{} {}" here{1} here{0}
```

We can typecast with `@type` `as` a postfix.

```rust
= int 28

:: "{}" int@f64
```

`@@@` Trait
-----------

We can define a `trait` with `@@@`.

```rust
@@@ Top
    := twist >> bool
    := spin speed i32 >> i32
    := color self >> Self
```

`+` Pub / Use / Mod
-------------------

### Visibility

We can declare `pub` items with `+` as a prefix.

```rust
@= +Star {}
    id    i32
    +name &str
    +date i32
```

There is a shorthand to make a struct and all it's fields public.

```rust
@= Star {+}
    id   i32
    name &str
    date i32
```

We can do the same with enums.
```rust
@= Star <+>
    RedGiant
    WhiteDwarf
```

### Use

We can `use` any Rust module with the `+` operator. `,` is used to chain modules.

Non-standard modules such as `std,collections,HashMap` will automatically import when used.

```rust
+ serde_json,Result

+ serde_json,
    json Value

~ pub use
++ versions.Versioning
```

### Mod

We can declare a `mod` with `+=`

```rust
~ reads from file `fungi.an/fungi.rs`
+= fungi

~ pub mod
+= +helpers

~ anything nested will be part of that mod
+= grow
    := now_thats
        :: "progress"
```

Flow
----

We can alter the flow of code execution in two ways...

1. Conditionals `?`
2. Loops        `|`

### `?` Conditionals

Any statement that begins with a `?` will perform some form of comparison.
Code is then conditionally executed based on the condition.

There are `if` statements and `match` statements.

```rust
? x < 5   ~ if x is less than 5
? x       ~ match x
?= <x> y  ~ if let Some(x) = y
```

#### If

When a line begins with a `?` and contains a comparison operator:

- `?` can be read as "if"
- `^` can be read as "else"
- `^?` can be read as "else if"


- `^:` is a shorthand for "else println!"
- `^.` is a shorthand for "else print!"
- `^;` is a shorthand for "else eprintln!"
- `^,` is a shorthand for "else eprint!"

Unlike most other indented languages, the else statement is part of the indentation.

```rust
= num 8

? num == 12
    :: "looks like you"
    ^: "differentation"
```

```rust
= day "day"
= suns 2

? "night" == day
    :: "uh oh..."
    ^? suns > 1
        :: "whew!"
    ^: "logical."
```

#### If Let

An `if let` statement is written as `?=`.

```rust
= num <7>

?= <n> num
    :: "there is some {}" n
```

#### Matching

We can declare a `match` statement with `??`.

`^` is used for the `_` catch-all.

A match statement will stop evaluation after its first match.

```rust
~ strings
= uhm "uhm"

?? uhm
   "oh" :: "ohhh"
   "uh" :: "uhhh"
    ^ :: "{}" uhm
```

```rust
~ numbers
= num 9

?? num
    <= 5 :: "less than or is 5"
    >  6 :: "more than 6"
    == 6 :: num
```

```rust
~ enums
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

We can match by iterating over collections like so...

```rust
= hard [] "oak" "maple" "cherry" "walnut"
= soft [] "spruce" "pine" "cedar" "yew"

?? "pine"
    [ hard ] :: "is a hardwood"
    [ soft ] :: "is a softwood"
    "bamboo" :: "good stuff"
    ^        :: "plant more trees!"
```

### `|` Loops

Any statement that begins with `|` will perform some form of loop.

```rust
||          ~ loop
|?| x < 3   ~ while x is less than 3
|?= <x>| y  ~ while let Some(x) = y
|x| y       ~ for x in y
|x y| z     ~ for x y in z
```

#### Loop

`||` is an infinite loop.

- `<<<` will `break` the loop
- `>>>` will `continue`

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

Labels can be declared with the `` ` `` postfix.

```rust
||`label
    :: "label"
    <<<
```

#### While

We can write a `while` statement with `|?|` followed by a comparison operator.

`|?| x < 3` can be read as "while x is less than 3".

Anything after an optional `^` operator will execute immediately once the condition is false

```rust
= $n 1

|?| n < 3
    $n += 1
    ^: "reached 3"
```

#### While Let

A `while let` statement is written as `|?= |`.

```rust
= $x ![] 1 2 3

|?= <y>| $x.pop
    :: "y {}" y

|?= _| 5
    :: "Irrefutable patterns are always true"
    <<<
```

#### For

`|n| vec` can be read as "for `n` in vec".

```rust
= nums [] 1 2 3

|n| nums
    :: n
    ? n == 3 :: "number 3 is in the list"
```

You can get creative with nesting and inline syntax...

```rust
= nums [[i32; 3]]
    [] 1 2 3
    [] 4 5 6

|n| nums :: "n: {}" n
    |i| n :: "i: {}" i
        ? i == 2 :: "number 2 is in the list"
        ? i == 4 :: "number 4 is in the list"
```

Attributes
----------

Attributes have multiple values separated solely by whitespace.

```rust
#![allow(warnings)]
+ std,fmt,Display

#[derive(Display Clone Copy)]
@= Point {}
    x i64
    y i64
```

Fin
---

If you have found something unclear, please file an issue.
