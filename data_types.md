Other than the usual data types, Rust has 2 compound ones:

- Tuples: Different data types, fixed length.

To access elements in the tuple:

```rust
fn main() {
    let x: (i32, f64, u8) = (500, 6.4, 1);

    let five_hundred = x.0;

    let six_point_four = x.1;

    let one = x.2;
}
```

```rust
fn main() {
    let tup = (500, 6.4, 1);

    let (x, y, z) = tup;

    println!("The value of y is: {y}");
}
```

- Arrays: Same data type, fixed length. Arrays are useful when you want your data allocated on the stack.

```rust
fn main() {
    let a = [1, 2, 3, 4, 5];
}
```

Arrays are more useful when you know the number of elements will not need to change.

```rust
let a: [i32; 5] = [1, 2, 3, 4, 5];
```

To access elements in the array:

```rust
fn main() {
    let a = [1, 2, 3, 4, 5];

    let first = a[0];
    let second = a[1];
}
```


- A _vector_ is a similar collection type provided by the standard library that _is_ allowed to grow or shrink in size because its contents live on the heap. If you’re unsure whether to use an array or a vector, chances are you should use a vector.
