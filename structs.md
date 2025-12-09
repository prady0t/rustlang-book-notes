```rust
struct User {
    active: bool,
    username: String,
    email: String,
    sign_in_count: u64,
}
```

Creating an instance

```rust
fn main() {
    let user1 = User {
        active: true,
        username: String::from("someusername123"),
        email: String::from("someone@example.com"),
        sign_in_count: 1,
    };
}
```

Note that the entire instance must be mutable; Rust doesn’t allow us to mark only certain fields as mutable


If the function param name is the same as the struct fields we can use a shorthand:


```rust
fn build_user(email: String, username: String) -> User {
    User {
        active: true,
        username,
        email,
        sign_in_count: 1,
    }
}
```

For this operation 

```rust
fn main() {
    // --snip--

    let user2 = User {
        active: user1.active,
        username: user1.username,
        email: String::from("another@example.com"),
        sign_in_count: user1.sign_in_count,
    };
}
```

We can use shorthand:

```rust
fn main() {
    // --snip--

    let user2 = User {
        email: String::from("another@example.com"),
        ..user1
    };
}
```


Note that the struct update syntax uses `=` like an assignment; this is because it moves the data.

In this example, we can no longer use `user1` after creating `user2` because the `String` in the `username` field of `user1` was moved into `user2`. If we had given `user2` new `String` values for both `email` and `username`, and thus only used the `active` and `sign_in_count` values from `user1`, then `user1` would still be valid after creating `user2`. Both `active` and `sign_in_count` are types that implement the `Copy` trait, so the behaviour we discussed in the [“Stack-Only Data: Copy”](https://doc.rust-lang.org/stable/book/ch04-01-what-is-ownership.html#stack-only-data-copy) section would apply. We can also still use `user1.email` in this example, because its value was not moved out of `user1`.



These are tuple structs

```rust
struct Color(i32, i32, i32);
struct Point(i32, i32, i32);

fn main() {
    let black = Color(0, 0, 0);
    let origin = Point(0, 0, 0);
}
```


Unit-Like Structs Without Any Fields

You can also define structs that don’t have any fields! These are called _unit-like structs_ because they behave similarly to `()`


In the `User` struct definition in Listing 5-1, we used the owned `String` type rather than the `&str` string slice type. This is a deliberate choice because we want each instance of this struct to own all of its data and for that data to be valid for as long as the entire struct is valid.

&str has copy trait hence it's value can exist (after assignment to a dif value)even after struct goes out of scope


Defining methods for structs


```rust
#[derive(Debug)]
struct Rectangle {
    width: u32,
    height: u32,
}

impl Rectangle {
    fn area(&self) -> u32 {
        self.width * self.height
    }
}

fn main() {
    let rect1 = Rectangle {
        width: 30,
        height: 50,
    };

    println!(
        "The area of the rectangle is {} square pixels.",
        rect1.area()
    );
}
```


If we wanted to change the instance that we’ve called the method on as part of what the method does, we’d use `&mut self` as the first parameter.


When you call a free function:

distance(p1, &p2)


Rust **cannot** guess what you meant.

Maybe the function expects:

- a reference?
    
- a mutable reference?
    
- ownership?
    

There’s no special rule for functions.

But methods have a clear, predictable receiver: `self`.

That gives Rust enough information to safely auto-borrow.

Hence:

in context of methods, these are same

```rust
p1.distance(&p2);
(&p1).distance(&p2);
```

Associated functions that aren’t methods are often used for constructors that will return a new instance of the struct. These are often called `new`, but `new` isn’t a special name and isn’t built into the language. For example, we could choose to provide an associated function named `square` that would have one dimension parameter and use that as both width and height, thus making it easier to create a square `Rectangle` rather than having to specify the same value twice:

```rust
impl Rectangle {
    fn square(size: u32) -> Self {
        Self {
            width: size,
            height: size,
        }
    }
}
```


The `Self` keywords in the return type and in the body of the function are aliases for the type that appears after the `impl` keyword, which in this case is `Rectangle`.

To call this associated function, we use the `::` syntax with the struct name; `let sq = Rectangle::square(3);` is an example. This function is namespaced by the struct: the `::` syntax is used for both associated functions and namespaces created by modules