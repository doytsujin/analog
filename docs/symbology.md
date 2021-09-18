analog Symbology
================

```rust
~      ~ comment
=      ~ variable
==     ~ const

:=     ~ fn
:==    ~ const fn

#=     ~ struct/enum
#:     ~ impl
##     ~ type
###    ~ trait
#T     ~ as/impl on return signature

+      ~ use
++     ~ pub use
+=     ~ mod
++=    ~ pub mod

??     ~ if
???    ~ match
??=    ~ if let

||     ~ loop
|n|    ~ for
|??|   ~ while
|??= | ~ while let

/x/    ~ closure
\\     ~ move
```

Keywords
--------

From Rust:

```rust
as       ~ #
async    ~
await    ~
break    ~ <<<
const    ~ ==
continue ~ >>>
crate    ~
dyn      ~
else     ~ %
enum     ~ #=
extern   ~
false    ~ false
fn       ~ :=
for      ~ |n|
if       ~ ??
impl     ~ #: and #Trait
in       ~ implied
let      ~ =
loop     ~ ||
match    ~ ???
mod      ~ +=
move     ~ //x/
mut      ~ ^
pub      ~ +
ref      ~ &&
return   ~ >>
self     ~ self
Self     ~ Self
static   ~
struct   ~ #=
super    ~ super
trait    ~ ###
true     ~ true
type     ~ ##
union    ~
unsafe   ~
use      ~ +
where    ~ <<<
while    ~ |?|
```

From symbol:

```rust
~   ~ comment
`   ~ lifetime/label
!   ~
#   ~ struct/enum/impl/type/trait
#   ~
$   ~ mut
%   ~ else
^   ~
&   ~ reference
*   ~ dereference
<   ~
>   ~
+   ~ pub/use/mod
=   ~ variable/const
,   ~
.   ~
/   ~ closure/move
?   ~ if/match
\   ~
|   ~ loop/while/for
;   ~
:   ~ fn
'   ~ char
"   ~ string
```
