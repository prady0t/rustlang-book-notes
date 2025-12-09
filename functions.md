How to define and call a function:

```rust
fn main() {
    another_function(5);
}

fn another_function(x: i32) {
    println!("The value of x is: {x}");
}
```

The `let y = 6` statement does not return a value, so there isn’t anything for `x` to bind to.
This is different from what happens in other languages, such as C and Ruby, where the assignment returns the value of the assignment.
In those languages, you can write `x = y = 6` and have both `x` and `y` have the value `6`; **that is not the case in Rust.**

Expressions do not have a semicolon.
Statements have a semicolon.

Ex:

```rust
fn main() {
    let y = {
        let x = 3;
        x + 1
    };

    println!("The value of y is: {y}");
}
```

(**A new scope block created with curly brackets is an expression**)

In a function with a return value, if the return keyword is not used, it usually returns the last expression.

Ex:

```rust
fn five() -> i32 {
    5
}

fn main() {
    let x = five();

    println!("The value of x is: {x}");
}
```

So this throws an error:

```rust
fn plus_one(x: i32) -> i32 {
    x + 1;
}
```


In rust you can also return a value after breaking the loop:

```rust
fn main() {
    let mut counter = 0;

    let result = loop {
        counter += 1;

        if counter == 10 {
            break counter * 2;
        }
    };

    println!("The result is {result}");
}
```

You can also name the loops in the nested case and reference them.

```rust
fn main() {
    let mut count = 0;
    'counting_up: loop {
        println!("count = {count}");
        let mut remaining = 10;

        loop {
            println!("remaining = {remaining}");
            if remaining == 9 {
                break;
            }
            if count == 2 {
                break 'counting_up;
            }
            remaining -= 1;
        }

        count += 1;
    }
    println!("End count = {count}");
}
```

The `'counting_up` here has nothing to do with memory lifetimes, even though it uses a leading apostrophe. It's just a _loop label_.

To print elements in an array:

```rust
fn main() {
    let a = [10, 20, 30, 40, 50];

    for element in a {
        println!("the value is: {element}");
    }
}
```
