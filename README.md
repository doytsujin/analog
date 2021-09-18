analog
======

`analog` is an analogous, indented syntax for the Rust programming language.

```rust
= ^door_open [false; 100]

|pass| 1..101
    = ^door pass
    |?| ^door <= 100
        ^door_open[door - 1] !^door_open[door - 1]
        ^door += pass;

|i &is_open| ^door_open.iter().enumerate(
    println!
        "Door {} is {}.",
        (i + 1),
        ?? is_open "open" !! "closed"
```

Documentation
-------------

To learn about the syntax, consult the [documentation](./docs/docs.md).

The [symbology](./docs/symbology.md) provides a comprehensive overview of symbolic keywords.

Contributing
------------

This is free and unencumbered software released into the public domain, as are all contributions.
