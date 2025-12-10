know that `Box<dyn Error> means the function will return a type that implements the `Error` trait, but we don’t have to specify what particular type the return value will be. This gives us flexibility to return error values that may be of different types in different error cases. The `dyn` keyword is short for _dynamic_.


To read from env var


```rust
        let ignore_case = env::var("IGNORE_CASE").is_ok();
```


TO print to stderr:


```rust
    let config = Config::build(&args).unwrap_or_else(|err| {
        eprintln!("Problem parsing arguments: {err}");
        process::exit(1);
    });
```

