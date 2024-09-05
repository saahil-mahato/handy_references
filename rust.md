# **Rust Programming Language Reference**

## **1. Basics**

### **1.1 Hello, World!**
The simplest Rust program, which prints "Hello, world!" to the console.

```rust
fn main() {
// This line prints "Hello, world!" to the console
    println!("Hello, world!");
}
```

### **1.2 Variables**
Variables in Rust are immutable by default. You can use `mut` to make them mutable.

```rust
fn main() {
    let x = 5;           // Immutable variable, cannot be changed
    let mut y = 10;      // Mutable variable, can be changed later
    y += 5;              // y is now 15
    println!("x: {}, y: {}", x, y);  // Output: x: 5, y: 15
}
```

### **1.3 Data Types**
Rust is statically typed. Types can be integers, floats, booleans, characters, and strings.

```rust
let a: i32 = 10;      // 32-bit signed integer
let b: f64 = 3.14;    // 64-bit floating-point number
let c: bool = true;   // Boolean type (true/false)
let d: char = 'R';    // Character type, represented by single quotes
let e: &str = "Rust"; // String slice, which is a reference to a string
```

### **1.4 Functions**
Functions are blocks of code that perform specific tasks. They can accept parameters and return a value.

```rust
fn add(x: i32, y: i32) -> i32 {  // Function that adds two numbers and returns the result
    x + y  // Return the sum of x and y
}

fn main() {
    let result = add(5, 10);  // Call the add function with 5 and 10
    println!("Result: {}", result);  // Output: Result: 15
}
```

### **1.5 Conditionals**
Conditionals let you execute different code based on certain conditions using `if`, `else if`, and `else`.

```rust
fn main() {
    let number = 10;
    if number > 5 {  // If the number is greater than 5
        println!("Greater than 5");
    } else {  // Otherwise
        println!("Not greater than 5");
    }
}
```

### **1.6 Loops**
Rust provides different types of loops: `while`, `for`, and `loop`. Loops allow you to run a block of code multiple times.

```rust
// While loop runs as long as the condition is true
let mut n = 0;
while n < 5 {
    println!("n: {}", n);
    n += 1;
}

// For loop iterates over a range or a collection
for i in 0..5 {  // 0..5 is a range from 0 to 4
    println!("i: {}", i);
}

// Infinite loop using loop
loop {
    println!("Looping...");
    break;  // Break the loop to stop it
}
```

### **1.7 Match (Pattern Matching)**
The `match` statement allows you to compare a value against multiple patterns and execute code based on which pattern matches.

```rust
fn main() {
    let number = 7;
    match number {
        1 => println!("One"),   // If number is 1, print "One"
        2 | 3 | 5 | 7 | 11 => println!("Prime"),  // If number is 2, 3, 5, 7, or 11, print "Prime"
        _ => println!("Other"),  // The underscore (_) is a catch-all for any other value
    }
}
```

## **2. Ownership and Borrowing**

### **2.1 Ownership**
Rust has a unique feature called ownership, which ensures memory safety. Each value in Rust has a single owner. When ownership is transferred (or "moved"), the previous owner cannot use the value anymore.

```rust
fn main() {
    let s1 = String::from("Rust");  // s1 owns the string "Rust"
    let s2 = s1;  // Ownership is moved to s2, s1 is no longer valid
    // println!("{}", s1); // This would cause an error because s1 is no longer valid
    println!("{}", s2);  // Output: Rust
}
```

### **2.2 Borrowing**
Borrowing allows you to use a value without taking ownership of it. You can borrow a value by passing a reference to it with `&`.

```rust
fn main() {
    let s1 = String::from("Rust");  // s1 owns the string "Rust"
    let len = calculate_length(&s1);  // s1 is borrowed, not moved
    println!("Length of '{}': {}", s1, len);  // Output: Length of 'Rust': 4
}

fn calculate_length(s: &String) -> usize {  // s is a reference to a String
    s.len()  // Return the length of the string
}
```

### **2.3 Mutable Borrowing**
Mutable borrowing allows you to modify a value while borrowing it. However, Rust ensures that you can only have one mutable reference at a time to prevent data races.

```rust
fn main() {
    let mut s = String::from("Hello");  // s is a mutable String
    change(&mut s);  // Borrow s mutably
    println!("{}", s);  // Output: Hello, world
}

fn change(s: &mut String) {  // s is a mutable reference to a String
    s.push_str(", world");  // Modify the string
}
```

### **2.4 Slices**
Slices are references to a portion of a collection, like a string or an array. They allow you to work with parts of data without copying them.

```rust
fn main() {
    let s = String::from("Rust programming");
    let slice = &s[0..4]; // Slice of the first 4 characters of s, which is "Rust"
    println!("{}", slice);  // Output: Rust
}
```

## **3. Structs and Enums**

### **3.1 Structs**
Structs are custom data types that let you group related data together.

```rust
struct User {
    username: String,
    email: String,
    active: bool,
}

fn main() {
    // Creating an instance of the User struct
    let user1 = User {
        username: String::from("alice"),
        email: String::from("alice@example.com"),
        active: true,
    };
    println!("Username: {}", user1.username);  // Accessing a field of the struct
}
```

### **3.2 Enums**
Enums are types that can represent multiple possible variants. Each variant can hold different types and amounts of associated data.

```rust
enum Message {
    Quit,  // No data associated
    Move { x: i32, y: i32 },  // Struct-like data
    Write(String),  // Single String data
}

fn main() {
    let msg = Message::Write(String::from("Hello"));  // Create an instance of the Write variant
    match msg {
        Message::Quit => println!("Quit"),  // Handle the Quit variant
        Message::Move { x, y } => println!("Move to ({}, {})", x, y),  // Handle the Move variant
        Message::Write(text) => println!("Message: {}", text),  // Handle the Write variant
    }
}
```

## **4. Error Handling**

### **4.1 Result and Option**
Rust uses the `Result` and `Option` types for error handling. `Result` is used when an operation can either succeed (`Ok`) or fail (`Err`). `Option` is used when a value could be `Some` or `None`.

```rust
fn divide(x: i32, y: i32) -> Result<i32, String> {  // Function that returns a Result
    if y != 0 {
        Ok(x / y)  // If y is not zero, return the result as Ok
    } else {
        Err(String::from("Division by zero"))  // If y is zero, return an error
    }
}

fn main() {
    match divide(10, 2) {
        Ok(result) => println!("Result: {}", result),  // Handle the Ok case
        Err(e) => println!("Error: {}", e),  // Handle the Err case
    }
}
```

### **4.2 Unwrapping**
`unwrap()` is a method that extracts the value inside `Some` or `Ok`, but it will cause the program to panic (crash) if used on `None` or `Err`.

```rust
fn main() {
    let some_number = Some(10);  // Option with a value
    let unwrapped = some_number.unwrap();  // Extract the value from the Option
    println!("Unwrapped: {}", unwrapped);  // Output: Unwrapped: 10
}
```

## **5. Advanced Concepts**

### **5.1 Generics**
Generics allow you to write functions, structs, enums, or traits that can operate on many different types without sacrificing type safety.

```rust
fn largest<T: PartialOrd>(list: &[T]) -> &T {  // A generic function that finds the largest value in a slice of any type T
    let mut largest = &list[0];
    for item in list {
        if item > largest {
            largest = item;
        }
    }
    largest
}

fn main() {
    let numbers = vec![1, 2, 3, 4, 5];
    println!("Largest number: {}", largest(&numbers));  // Output: Largest number: 5
}
```

### **5.2 Traits**
Traits define shared behavior that types can implement. If a type implements a trait, it gains the behavior described by that trait.

```rust
trait Summary {  // Define a trait named Summary
    fn summarize(&self) -> String;  // A method signature that types implementing this trait must define
}

struct Article {  // A struct representing an article
    title: String,
    author: String,
    content: String,
}

impl Summary for Article {  // Implement the Summary trait for the Article struct
    fn summarize(&self) -> String {  // Define how to summarize an article
        format!("{} by {}", self.title, self.author)
    }
}

fn main() {
    let article = Article {
        title: String::from("Rust Programming"),
        author: String::from("Alice"),
        content: String::from("Content of the article..."),
    };
    println!("Article Summary: {}", article.summarize());  // Output: Article Summary: Rust Programming by Alice
}
```

### **5.3 Lifetimes**
Lifetimes are Rust's way of ensuring that references are valid as long as they need to be. They prevent dangling references, which could lead to memory safety issues.

```rust
fn longest<'a>(x: &'a str, y: &'a str) -> &'a str {  // 'a is a lifetime parameter
    if x.len() > y.len() {
        x
    } else {
        y
    }
}

fn main() {
    let string1 = String::from("long string");
    let string2 = "short";
    let result = longest(&string1, &string2);  // Both strings must live at least as long as the result
    println!("Longest: {}", result);  // Output: Longest: long string
}
```

### **5.4 Closures**
Closures are anonymous functions you can save in a variable or pass as arguments to other functions. Unlike regular functions, closures can capture values from their environment.

```rust
fn main() {
    let add = |a, b| a + b;  // A closure that adds two numbers
    println!("Sum: {}", add(2, 3));  // Output: Sum: 5

    let multiply_by_two = |x| x * 2;  // A closure that multiplies a number by 2
    println!("Multiply by two: {}", multiply_by_two(5));  // Output: Multiply by two: 10
}
```

### **5.5 Concurrency**
Concurrency in Rust is achieved using threads. Rust ensures thread safety by enforcing the ownership and borrowing rules across threads.

```rust
use std::thread;
use std::time::Duration;

fn main() {
    let handle = thread::spawn(|| {  // Spawn a new thread
        for i in 1..10 {
            println!("Spawned thread: {}", i);
            thread::sleep(Duration::from_millis(1));  // Sleep for 1 millisecond
        }
    });

    for i in 1..5 {
        println!("Main thread: {}", i);  // Main thread runs simultaneously with the spawned thread
        thread::sleep(Duration::from_millis(1));
    }

    handle.join().unwrap();  // Wait for the spawned thread to finish
}
```

### **5.6 Asynchronous Programming**
Asynchronous programming in Rust allows you to write code that runs concurrently without needing multiple threads. The `async` and `await` keywords enable you to work with asynchronous functions.

```rust
use tokio::time::{sleep, Duration};  // Tokio is an async runtime for Rust

async fn async_func() {  // Define an asynchronous function
    println!("Start async function");
    sleep(Duration::from_secs(2)).await;  // Sleep asynchronously for 2 seconds
    println!("End async function");
}

#[tokio::main]  // This macro creates a Tokio runtime and starts the async function
async fn main() {
    println!("Before async call");
    async_func().await;  // Call the async function and wait for it to complete
    println!("After async call");
}
```

### **5.7 Macros**
Macros are a way of writing code that writes other code (metaprogramming). Macros can generate code at compile time based on the input you provide.

```rust
macro_rules! say_hello {  // Define a simple macro
    () => {
        println!("Hello, world!");  // The macro expands to this code
    };
}

fn main() {
    say_hello!();  // Call the macro, which prints "Hello, world!"
}
```

### **5.8 Unsafe Code**
Unsafe code allows you to perform operations that the Rust compiler cannot guarantee are safe, such as dereferencing raw pointers. You should only use unsafe code when absolutely necessary.

```rust
fn main() {
    let mut num = 5;

    let r1 = &num as *const i32;  // Create a raw pointer to an immutable reference
    let r2 = &mut num as *mut i32;  // Create a raw pointer to a mutable reference

    unsafe {  // Enter an unsafe block to dereference raw pointers
        println!("r1 is: {}", *r1);  // Dereference r1
        println!("r2 is: {}", *r2);  // Dereference r2
    }
}
```

---

This manual covers the essential concepts of Rust, from basic syntax to advanced features like generics, traits, lifetimes, and concurrency. By understanding these concepts, you'll be well-equipped to write safe, efficient, and concurrent Rust programs.
