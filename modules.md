
```text
backyard
├── Cargo.lock
├── Cargo.toml
└── src
    ├── garden
    │   └── vegetables.rs
    ├── garden.rs
    └── main.rs
```

The crate root file in this case is _src/main.rs_, and it contains:

Filename: src/main.rs

```rust
use crate::garden::vegetables::Asparagus;

pub mod garden;

fn main() {
    let plant = Asparagus {};
    println!("I'm growing {plant:?}!");
}
```

The `pub mod garden;` line tells the compiler to include the code it finds in _src/garden.rs_, which is:

Filename: src/garden.rs

```rust
pub mod vegetables;
```

Here, `pub mod vegetables;` means the code in _src/garden/vegetables.rs_ is included too. That code is:

```rust
#[derive(Debug)]
pub struct Asparagus {}
```

A quick mental image: you’re standing in your Rust project folder. The folder is the package. Inside it sits the crate, the thing Cargo will compile into a library or executable. Inside the crate grows a tree of modules that correspond to your `.rs` files.


space_mission/          ← package
│
├── Cargo.toml
└── src/
    ├── main.rs         ← binary crate
    ├── lib.rs          ← library crate
    ├── rocket.rs       ← module inside lib.rs
    └── systems/
         ├── mod.rs     ← systems module folder
         └── navigation.rs   ← submodule


You can use the `use` keyword to bring a module into scope:

```rust
mod front_of_house {
    pub mod hosting {
        pub fn add_to_waitlist() {}
    }
}

use crate::front_of_house::hosting;

pub fn eat_at_restaurant() {
    hosting::add_to_waitlist();
}
```

Note that `use` only creates the shortcut for the particular scope in which the `use` occurs.

This throws an error:

```rust
mod front_of_house {
    pub mod hosting {
        pub fn add_to_waitlist() {}
    }
}

use crate::front_of_house::hosting;

mod customer {
    pub fn eat_at_restaurant() {
        hosting::add_to_waitlist();
    }
}
```



```rust
use std::fmt;
use std::io;

fn function1() -> fmt::Result {
    // --snip--
}

fn function2() -> io::Result<()> {
    // --snip--
}
```

 Bringing two types with the same name into the same scope requires using their parent modules.


```rust
use std::fmt;
use std::io;

fn function1() -> fmt::Result {
    // --snip--
}

fn function2() -> io::Result<()> {
    // --snip--
}
```

As you can see, using the parent modules distinguishes the two `Result` types. If instead we specified `use std::fmt::Result` and `use std::io::Result`, we’d have two `Result` types in the same scope, and Rust wouldn’t know which one we meant when we used `Result`.

There’s another solution to the problem of bringing two types of the same name into the same scope with `use`: after the path, we can specify `as` and a new local name, or _alias_, for the type.

```rust
use std::fmt::Result;
use std::io::Result as IoResult;

fn function1() -> Result {
    // --snip--
}

fn function2() -> IoResult<()> {
    // --snip--
}
```

Instead of 

```rust
// --snip--
use std::cmp::Ordering;
use std::io;
// --snip--
```

We can 

```rust
// --snip--
use std::{cmp::Ordering, io};
// --snip--
```


`pub use` makes that _re-exported name_ available to **any code that imports your crate**.

```rust
mod front_of_house {
    pub mod hosting {
        pub fn add_to_waitlist() {}
    }
}

pub use crate::front_of_house::hosting;

pub fn eat_at_restaurant() {
    hosting::add_to_waitlist();
}
```
