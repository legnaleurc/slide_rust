# Rust

# Outline

* Structs
* Enums
* Match

# Structs

```rust
struct Point {
    x: i32,
    y: i32,
}

fn main() {
    let origin = Point { x: 0, y: 0 }; // origin: Point

    println!("The origin is at ({}, {})", origin.x, origin.y);
}
```

# Structs

```rust
fn main() {
    let origin = Point { x: 0, y: 0 };

    // error: cannot assign to immutable field `origin.x`
    origin.x = 42;
}
```

# Structs

`mut` is used to describe binding, not fields.

```rust
struct Point {
    // error: expected identifier, found keyword `mut`
    mut x: i32,
    y: i32,
}
```

# Structs

You can use `&mut T` just like primitive types.

```rust
{
    let tmp = &mut origin;
    tmp.y = 42;
}

println!("The origin is at ({}, {})", origin.x, origin.y);
```

# Structs

You can use `&mut` pointer as field type.

```rust
struct PointRef<'a> {
    x: &'a mut i32,
    y: &'a mut i32,
}

fn main() {
    let mut point = Point { x: 0, y: 0 };

    {
        let r = PointRef { x: &mut point.x, y: &mut point.y };

        *r.x = 5;
        *r.y = 6;
    }

    assert_eq!(5, point.x);
    assert_eq!(6, point.y);
}
```

# Update Syntax

A `struct` can include `..` to indicate that you want to use a copy of some
other struct for some of the values.

```rust
struct Point3d {
    x: i32,
    y: i32,
    z: i32,
}

let mut point = Point3d { x: 0, y: 0, z: 0 };
point = Point3d { y: 1, .. point };
```

# Tuple Structs

These structs do not have field names.
However, having field names will be better in most of times.

```rust
struct Color(i32, i32, i32);
struct Point(i32, i32, i32);

let black = Color(0, 0, 0);
let origin = Point(0, 0, 0);
```

# Tuple Structs

Special case: make a simple type to make it distinct to original type.

```rust
struct Inches(i32);

let length = Inches(10);

// destructuring
let Inches(integer_length) = length;
println!("length is {} inches", integer_length);
```

# Unit-Like Structs

A struct can be empty, which will works like a special value.

```rust
struct Electron;

let x = Electron;
```

# Enums

An `enum` represents data is one of several possible variants.

```rust
enum Message {
    Quit, // like unit-like struct
    ChangeColor(i32, i32, i32), // like tuple struct
    Move { x: i32, y: i32 }, // like normal struct
    Write(String), // like tuple struct
}
```

# Enums

Use `::` to access the variant.

```rust
let x: Message = Message::Move { x: 3, y: 4 };
```

# Enums

Destructuring for enums are forbidden.

```rust
fn process_color_change(msg: Message) {
    let Message::ChangeColor(r, g, b) = msg; // compile-time error
}
```

# Enums

Constructors as functions.

```rust
let v = vec!["Hello".to_string(), "World".to_string()];

let v1: Vec<Message> = v.into_iter().map(Message::Write).collect();
```

# Match

Just like switch-case.

```rust
let x = 5;

match x {
    1 => println!("one"),
    2 => println!("two"),
    3 => println!("three"),
    4 => println!("four"),
    5 => println!("five"),
    _ => println!("something else"),
}
```

# Match

`match` is also an expression.

```rust
let x = 5;

let number = match x {
    1 => "one",
    2 => "two",
    3 => "three",
    4 => "four",
    5 => "five",
    _ => "something else",
};
```

# Matching On Enums

You can use `match` to extract `enum`, or use `if let`.

```rust
enum Message {
    Quit,
    ChangeColor(i32, i32, i32),
    Move { x: i32, y: i32 },
    Write(String),
}

fn quit() { /* ... */ }
fn change_color(r: i32, g: i32, b: i32) { /* ... */ }
fn move_cursor(x: i32, y: i32) { /* ... */ }

fn process_message(msg: Message) {
    match msg {
        Message::Quit => quit(),
        Message::ChangeColor(r, g, b) => change_color(r, g, b),
        Message::Move { x: x, y: y } => move_cursor(x, y),
        Message::Write(s) => println!("{}", s),
    };
}
```

# Summary

...
