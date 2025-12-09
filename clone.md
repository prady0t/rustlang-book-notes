- **`Copy`**: cheap, implicit bitwise copy; no destructor; both bindings stay usable.
    
- **`Clone`**: explicit, potentially expensive deep copy; you call `.clone()`.
    
- **`Drop`**: destructor logic that runs exactly once when a value is going out of scope (or you call `std::mem::drop`).


## `Copy`

- Marker trait: _“this type can be duplicated by just copying its bits.”_
    
- Happens **implicitly** on move (so using the value after a “move” is still allowed).
    
- **Cannot** implement `Drop`, and any type that **has** a `Drop` cannot be `Copy`.
    
- A type is `Copy` only if **all its fields are `Copy`**.
    

Common `Copy` types: integers, floats, `char`, `bool`, `&T` (shared refs), arrays/tuples of `Copy` types.


## `Clone`

- Trait for **explicit** duplication that may do work (allocate, refcount bump, deep copy, etc.).
    
- You call it: `let y = x.clone();`
    
- You often `#[derive(Clone)]`; you can also implement it manually.
    

Examples:

- `String::clone()` allocates and copies bytes.
    
- `Arc<T>::clone()` bumps a reference count (cheap but not zero-cost).


## `Drop`

- Trait that lets you run cleanup code when a value is destroyed.
    
- Runs **exactly once** when the value leaves scope or when you call `std::mem::drop(value)`.
    
- Drop order is reverse of creation (LIFO) within a scope.
    
- Types with `Drop` are **not** `Copy`.