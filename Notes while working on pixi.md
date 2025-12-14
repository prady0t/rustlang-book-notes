- **What is a map()**

For an `Option<T>`,  
`map(f)` produces an `Option<U>` by doing one of two things:

• If it’s `Some(t)`, return `Some(f(t))`.  
• If it’s `None`, return `None`.

Example:

```rust

let x: Option<i32> = Some(10);
let y = x.map(|n| n * 2);
// y = Some(20)

```

- What is `as_ref()`

`as_ref()` on an `Option<T>` is a polite way of saying:  
“Keep the outer `Option`, but turn the _inside_ into a reference instead of moving it.

If you have: `Option<String>`and you call: `as_ref()` You get:

`Option<&String>`

Notice the shape stays the same

example

```rust
let name: Option<String> = Some("Ada".into());

// Without as_ref(), using map would move the String:
let len = name.as_ref().map(|s| s.len());

```

- **`as_mut()`**: There’s also a sibling called `as_mut()`, which gives you:

```rust
Option<&mut T>

```


- What is `and_then()` : 

While `map` is great for transforming a value inside an `Option`, sometimes you need to perform an operation that itself returns another `Option`. This is where `and_then` shines.

The key difference between `map` and `and_then` is that the closure passed to `and_then` must return an `Option`. This allows you to chain multiple computations that may fail (returning `None`).

Ex:

```rust
fn double_then_to_string(opt: Option<i32>) -> Option<String> {
    opt.and_then(|x| {
        if x > 0 {
            Some(x * 2) // First computation: doubling the value
        } else {
            None // Fail if the value is negative or zero
        }
    })
    .and_then(|x| Some(x.to_string())) // Second computation: converting to string
}

fn main() {
    let some_value = Some(5);
    let no_value: Option<i32> = None;
    let negative_value = Some(-3);

    println!("{:?}", double_then_to_string(some_value)); // Output: Some("10")
    println!("{:?}", double_then_to_string(no_value));   // Output: None
    println!("{:?}", double_then_to_string(negative_value)); // Output: None
}
```
