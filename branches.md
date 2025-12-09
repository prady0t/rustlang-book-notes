If expressions are just like any other language:

```rust
fn main() {
    let number = 3;

    if number < 5 {
        println!("condition was true");
    } else {
        println!("condition was false");
    }
}
```

It’s also worth noting that the condition in this code _must_ be a `bool`. If the condition isn’t a `bool`, we’ll get an error. For example, try running the following code:

```rust
fn main() {
    let number = 3;

    if number {
        println!("number was three");
    }
}
```

```console
$ cargo run
   Compiling branches v0.1.0 (file:///projects/branches)
error[E0308]: mismatched types
 --> src/main.rs:4:8
  |
4 |     if number {
  |        ^^^^^^ expected `bool`, found integer

For more information about this error, try `rustc --explain E0308`.
error: could not compile `branches` (bin "branches") due to 1 previous error
```


Because `if` is an expression, we can use it on the right side of a `let` statement to assign the outcome to a variable

```rust
fn main() {
    let condition = true;
    let number = if condition { 5 } else { 6 };

    println!("The value of number is: {number}");
}
```
