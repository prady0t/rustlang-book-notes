
In case of Strings, if we are counting the index and storing it in a variable. If the String goes out of scope, the variable stores a useless value. Having to worry about the index in the string getting out of sync with the data in the variable is tedious and error-prone! Hence, we use String Slice

```rust
    let s = String::from("hello world");

    let hello = &s[0..5];
    let world = &s[6..11];
```


![[trpl04-07.svg]]

```rust
fn main() {
    let mut s = String::from("hello world");

    let word = first_word(&s);

    s.clear(); // error!

    println!("the first word is: {word}");
}
```

Here’s the compiler error:

```console
$ cargo run
   Compiling ownership v0.1.0 (file:///projects/ownership)
error[E0502]: cannot borrow `s` as mutable because it is also borrowed as immutable
  --> src/main.rs:18:5
   |
16 |     let word = first_word(&s);
   |                           -- immutable borrow occurs here
17 |
18 |     s.clear(); // error!
   |     ^^^^^^^^^ mutable borrow occurs here
19 |
20 |     println!("the first word is: {word}");
   |                                  ------ immutable borrow later used here

For more information about this error, try `rustc --explain E0502`.
error: could not compile `ownership` (bin "ownership") due to 1 previous error
```


This is better

```rust
fn first_word(s: &str) -> &str {
```

than

```rust
fn first_word(s: &String) -> &str {
```

If we have a string slice, we can pass that directly. If we have a `String`, we can pass a slice of the `String` or a reference to the `String` (String[..] -> str).