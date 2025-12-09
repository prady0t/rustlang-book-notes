
This code is not valid:

```rust
fn longest(x: &str, y: &str) -> &str { 
if x.len() > y.len() { x } else { y } 
}

```


This is a situation where this could be problematic:

```rust
fn main() {
    let s1 = String::from("this lives long");
    let result;

    {
        let s2 = String::from("short");
        result = longest(s1.as_str(), s2.as_str());  // <-- danger zone
    } // s2 goes *poof* here

    println!("{}", result); // result might point to s2
}

```

`'a` is a generic lifetime, the generic lifetime `'a` will get the concrete lifetime that is equal to the smaller of the lifetimes of `x` and `y`.

```rust
fn longest<'a>(x: &'a str, y: &'a str) -> &'a str {
    if x.len() > y.len() { x } else { y }
}
```


This also compiles even though no lifetime is specified:


```rust
fn first_word(s: &str) -> &str {
    let bytes = s.as_bytes();

    for (i, &item) in bytes.iter().enumerate() {
        if item == b' ' {
            return &s[0..i];
        }
    }

    &s[..]
}
```


The patterns programmed into Rust’s analysis of references are called the _lifetime elision rules_. These aren’t rules for programmers to follow; they’re a set of particular cases that the compiler will consider, and if your code fits these cases, you don’t need to write the lifetimes explicitly.

These are set of compiler rules for lifetimes that are automatically applied to functions and impl block:

- The first rule is that the compiler assigns a lifetime parameter to each parameter that’s a reference. In other words, a function with one parameter gets one lifetime parameter: `fn foo<'a>(x: &'a i32)`; a function with two parameters gets two separate lifetime parameters: `fn foo<'a, 'b>(x: &'a i32, y: &'b i32)`; and so on.

- The second rule is that, if there is exactly one input lifetime parameter, that lifetime is assigned to all output lifetime parameters: `fn foo<'a>(x: &'a i32) -> &'a i32`.

- The third rule is that, if there are multiple input lifetime parameters, but one of them is `&self` or `&mut self` because this is a method, the lifetime of `self` is assigned to all output lifetime parameters. 

Let’s apply the first rule: each parameter gets its own lifetime. This time we have two parameters instead of one, so we have two lifetimes:

```rust
fn longest<'a, 'b>(x: &'a str, y: &'b str) -> &str {
```

You can see that the second rule doesn’t apply because there is more than one input lifetime. The third rule doesn’t apply either, because `longest` is a function rather than a method, so none of the parameters are `self`. After working through all three rules, we still haven’t figured out what the return type’s lifetime is. This is why we got an error trying to compile the code in Listing 10-20: the compiler worked through the lifetime elision rules but still couldn’t figure out all the lifetimes of the references in the signature.


Here is an example where the third lifetime elision rule applies:

```rust
impl<'a> ImportantExcerpt<'a> {
    fn announce_and_return_part(&self, announcement: &str) -> &str {
        println!("Attention please: {announcement}");
        self.part
    }
}
```

There are two input lifetimes, so Rust applies the first lifetime elision rule and gives both `&self` and `announcement` their own lifetimes. Then, because one of the parameters is `&self`, the return type gets the lifetime of `&self`, and all lifetimes have been accounted for.


All string literals have the `'static` lifetime, which we can annotate as follows:

```rust
let s: &'static str = "I have a static lifetime.";
```

The text of this string is stored directly in the program’s binary, which is always available. Therefore, the lifetime of all string literals is `'static`.