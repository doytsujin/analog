analog Documentation
====================


Symbology
---------

`analog` replaces Rust's keywords with non-alphanumeric symbols.

Here is a general overview of which concepts relate to which symbols...

```rust
=   ~ assignment/declaration
#   ~ struct/enum
+   ~ pub

?   ~ conditional
|   ~ looping

~   ~ comment
^   ~ mutablity

[]  ~ collection
{}  ~ custom collection
```

Here is an example...

```rust
~ this is a comment

~ struct declaration
#= Planet
    name: &str
    number: i32
    star: Star

~ pub enum declaration
@+ Star
    MainSequence
    RedGiant
    WhiteDwarf
    RedDwarf
    Supergiant

~ variable
= world Planet
    name: "Earth"
    number: 3
    star: Star::MainSequence

~ function
:= hello(x: &str)
    println!("Hello {}", x)

hello(world.name)
```

For a comprehensive list of keywords, take a look at the [symbology](./symbology.md).

Indentation
-----------

Blocks of code are defined through indentation rather than braces.

Valid indentation is 4 space characters.

*Tabs and any other number of spaces are considered invalid syntax.*

```rust
:= block()
    println!("this is a block of code,")
    println!("it is also a function.")
    println!("")
```

We can forego closing braces on function calls when indenting.

```rust
function(
    "no end brace required"

println!
    "macros only require `!`"
```
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
~ literals
= pi = 3.1415927
= sym = 'π'
= pie: String "rhubarb"

= ^array: [i32; 3]
    [0; 3]

= strs: Vec<&str> vec!
    "list"
    "items"
    "as needed"
```

Functions
---------

`fn` is declared with `:=`.

```rust
:= f()          ~ no args
:= f(a: &str)  ~ with args
:= f() -> &str  ~ returns a value
:== f()         ~ const fn
```

To `return` a value, we use `>>`.
This is equivalent to removing the trailing `;`.

```rust
:= one_more(x: i8) -> (i8 i8)
    = y x + 1
    >> (x y)

= both one_more(2)
```

Input parameters can be indented with `-`. When indenting, we must denote this with `function(-)`.

```rust
:= group_params(-) -> Vec<&str>
    - param1: &str
    - param2: &str
    - param3: &str
    println!("function body begins here")
    println!("lets group...")
    >> vec![param1, param2, param3]
```

Return signature can also be indented after the parameters.

```rust
:= art_is_hard(-)
    - param1: &str
    - param2: &str
    - param3: &str
    -> Vec<&str>
    >> vec![param3, param2, param1]
```

### Chaining

```rust
= build Building::new().name("greenhouse").build()

= build Building::new()
    .name("greenhouse")
    .build()
```

### Closures

Closures can be called with `/x/`.

```rust
~ inline
= double /x/ x * 2

~ indented
= double /x/
    x * 2
```

Using a `Fn` parameter...

```rust
:= ten_times(f: F)
    <<< F: Fn(i32)
    |i| 0..10
        f(i)

ten_times(/x i32/
    println!("hello {}" x)
```

We can `move` with `\\`.

```rust
= data vec![1, 2, 3]
= closure \\ //
    println!("captured {:?} by value", data)

= word "konnichiwa".to_owned()
```

### Generics / Trait Bounds

```rust
:= new<T: Default>() -> T
    T::default()
```

We can declare a `where` clause with `<<<`.

```rust
:= what(t: &T, u: &U)
    <<< T: Display + Clone
        U: Clone + Debug
    println!("{}", t)
```

We can specify return types which `impl` traits with the `#` prefix.

```rust
:= ever >> #Display
    > "like this"
```

Reference/Dereference
---------------------

References and Dereferences must be prefixes (no trailing whitespace).
```rust
= x 5
= y &x

assert_eq!(5, x)
assert_eq!(5, *y)

~ valid
= z &&&x

~ invalid
= z &&& x
```

We can declare `ref` with `&&`.

```rust
= maybe_name Some(String::from("Alice"))

??? maybe_name
    Some(&& n)
        println!("Hello, {}", n)
    _
        println!("Hello, world")

println!("Hello again, {}", maybe_name.unwrap_or("world".into()))
```

Mutability
----------

`mut` is defined with the `^` prefix. We must call mutable variables with this prefix.

```rust
~ declaration
= ^x 4
println!("is {}", x)

~ mutation
^x += 1
^x += 2
println!("then {}", x)

~ shadowing
= x "shadow"
println!("I am a {}", x)
```

Lifetimes
---------

Lifetimes are declared with the `` ` `` postfix.

```rust
== TRACK: &str`static "thoughtograph"

:= noise<`static>()
    - i &str`static
    - o &str`static
    ||
        print!("{} {}", i, o)
```

Macros
------

```rust
= value json!
    {
        "num": 200,
        "is": true,
        "features": [
            "serde",
            "json"
        ]
    }

println!("{:?}", value)
```

Consts
------

To declare a `const`, we use `==`.

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

```rust
= a "I am a str"
= b: String "I am a String"
```

### Integer

```rust
~ implicit
= int 3

~ explicit
= int: i8 -3

~ explicit (suffix)
= int -3i8
```

### Float

```rust
~ implicit
= float 3.0

~ explicit
= float: f32 1.8

~ explicit (suffix)
= int -3f32
```

#### Array

```rust
~ implicit
= array [1, 2, 3]

~ explicit
= array: [u8; 3]
    [1, 2, 3]

~ single value initialization
= array [u8; 3]
    [0; 3]

~ access by index
println!("{}", array[0])
```

### Unit

```rust
= nothing ()
```

### Tuple

```rust
~ implicit
= tuple (88, true)

~ explicit
= tuple (u8, bool) (88, true)

~ access by index
println!("{}", tuple.0)
```

### Tuple Struct

```rust
~ declaration
#= TupleStruct (&str, &str)

~ assignment
= named TupleStruct
    ("easy", "pie")

~ access by index
println!("{}", named.0)
```

### Struct

```rust
~ declaration
#= Recipe
    onions: i32
    peppers: i32
    rice: bool

~ initialization
= tasty Recipe
    onions: 2
    peppers: 4
    rice: true

~ access field
println!("{}", recipe.peppers)
```

#### Enums

```rust
~ declaration
@= Action
    Refresh
    Flip
    KeyPress(char)
    Click
        x: i64
        y: i64


~ access variant
= flip  Action::Flip
= press Action::KeyPress("p")
= click Action::Click
    x: 7
    y: 0
```

`#:` Impl
---------

We can define `impl` blocks with `#:`.

```rust
#= When
    x: i32
    y: i32
    z: i32

#: When
    := origin() -> Self
        >> Self x: 0 y: 0 z: 0

    := up(^self) -> Self
        ^self.z += 1
```

To implement a trait on a type we use `<<`.

```rust
+ std::fmt

#= Point
    x: f64
    y: f64

#: Point << fmt::Display
    := fmt(&self, f: &^fmt::Formatter<`_>) -> fmt::Result
        write!(f, "({self.x}, {self.y})", self.x, self.y)

= origin: Point
    x: 0
    y: 0

println!("{}", origin)
```

Lets add some generics...

```rust
~ inline
#: SpecialType << CustomTrait<A: TraitA>
    := some_fn

~ indented
#: SpecialType << CustomTrait<A B>
    <<< A: TraitA
        B: TraitC TraitD
    := some_fn
```

`##` Alias
----------

We can define a `type` alias with `##`.

```rust
## Point (u8, u8)

= here Point(48, 62)

println!("{} {}", here.1, here.0)
```

We can typecast with `@type` `as` a postfix.

```rust
= int 28

println!("{}", int@f64)
```

`###` Trait
-----------

We can define a `trait` with `###`.

```rust
### Top
    := twist() -> bool
    := spin(speed: i32) -> i32
    := color(self) -> Self
```

`+` Pub / Use / Mod
-------------------

### Visibility

We can declare `pub` on struct and enums with `+` in place of `=`.

We can declare `pub` on fields with `+` as a prefix.

```rust
#+ Star
    id i32
    +name &str
    +date i32
```

### Use

We can `use` any Rust module with the `+` operator.

```rust
~ use
+ serde_json::Result

~ grouping
+ serde_json::{ json, Value }

~ indented
+ serde_json::{
    json, Value

~ pub use
++ versions::Versioning
```

### Mod

We can declare a `mod` with `+=`

```rust
~ reads from file `fungi.an/fungi.rs`
+= fungi

~ anything nested will be part of that mod
+= grow
    := now_thats
        println!("progress")

~ pub mod
++= aquatic
```

Flow
----

We can alter the flow of code execution in two ways...

1. Conditionals `?`
2. Loops        `|`

`?` Conditionals
----------------

Any statement that begins with a `?` will perform some form of comparison.

There are `if` statements and `match` statements.

```rust
?? x < 5   ~ if x is less than 5
??= x y    ~ if let x = y
??? x      ~ match x
```

### If

When a line begins with a `??`:

- `??` can be read as "if"
- `!!` can be read as "else"

```rust
= num 8

?? num == 12
    println!("looks like you")
!!
    println!("differentation")
```

```rust
= day "day"
= suns 2

?? "night" == day
    println!("uh oh...")
!! ?? suns > 1
    println!("whew!")
!!
    println!("logical.")
```

### If Let

An `if let` statement is written as `??=`.

```rust
= num Some<7>

??= Some<n> num
    println!("there is some {}", n)
```

### Matching

We can declare a `match` statement with `???`.

We define LHS and RHS with `->`.

```rust
= num 9

??? num
    1 -> println!("one")
    2 -> println!("two")
    3 -> println!("three")
    4 -> println!("four")
    5 -> println!("five")
    _ -> println!("something else")
```

We can imply `->` with indentation.

```rust
= message ??? num
    0 | 1
        "not many",
    2 ..= 9
        "a few",
    _
        "lots"
```

We can still use subpattern binding.

```rust
#= Tup(i32, i32)

??? Tup(1, 2) {
    Tup(z @ 1, _) | Tup(_, z @ 2)
        assert_eq!(z, 1)
    _ -> panic!()
}
```

```rust
= message ??? maybe_digit {
    Some(x) ?? x < 10 -> process_digit(x),
    Some(x) -> process_other(x),
    None -> panic!(),
```

`|` Loops
---------

Any statement that begins with `|` will perform some form of loop.

```rust
||          ~ loop
|??| x < 3  ~ while x is less than 3
|??= x| y   ~ while let x = y
|x| y       ~ for x in y
|x y| z     ~ for x y in z
```

### Loop

`||` is an infinite loop.

- `<<<` will `break` the loop
- `>>>` will `continue`

```rust
||
    = $count 1
    ? $count == 3
        println!("three")
        >>>
    println!("{}" count)
    ? count == 5
        println!("no more needed")
        <<<
```

Labels can be declared with the `` ` `` postfix.

```rust
||`label
    println!("label")
    <<<
```

### While

We can write a `while` statement with `|??|` followed by a comparison operator.

`|??| x < 3` can be read as "while x is less than 3".

```rust
= ^n 1

|??| ^n < 3
    ^n += 1
```

### While Let

A `while let` statement is written as `|??= |`.

```rust
= ^x vec![1, 2, 3]

|??= Some<y>| ^x.pop
    println!("y {}", y)

|??= _| 5
    println!("Irrefutable patterns are always true")
    <<<
```

### For

`|n| i` can be read as "for `n` in i".

```rust
= nums [1, 2, 3]

|n| nums
    println!("{}", n)
    ?? n == 3 println!("number 3 is in the list")
```

Attributes
----------

```rust
#![allow(warnings)]
+ std::fmt::Display

#[derive(Display, Clone, Copy)]
#= Point
    x i64
    y i64
```

Fin
---

If you have found something unclear, please file an issue.
