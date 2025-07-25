# Brief Phrases

## In Rust, if your program compiles, it will probably run.

## You can't segfault if you don't have null.

## Rust does not hide complexity from developers, it offers them the right tools to manage all the complexity.

# Why Rust? Isn't Node.js good enough?

## 1. Type Safety

Rust's strong type system enforces many errors at compile time, reducing runtime bugs and improving code reliability.

## 2. Systems Language Features

Rust provides low-level control over system resources such as RAM and CPU, which is essential for performance-critical applications.
Meaning Rust is a low-level language that runs very close to the hardware, allowing for fine-grained control over system resources.
Compared to JavaScript, Rust is incredibly fast and efficient.
The code is statically analyzed, which means that many errors can be caught at compile time before building and optimising the binary for performance. This binary then runs directly on the CPU, making it much faster than interpreted languages like JavaScript.

## 3. Concurrency

Running multiple tasks on multiple cores is called concurrency. For example, if we run a code in Node.js, it will run on a single thread from let's say 32 cores.
Whereas in Rust, we can run multiple tasks on multiple cores, which is called parallelism.
Rust's ownership model and concurrency features allow for safe and efficient parallel execution, making it easier to

## 4. Memory Safety

Here's where C and Rust differ. C does not have a memory safety model, which means that it is possible to write code that can cause memory leaks or segmentation faults.

Rust has a concept of owners,borrowing and lifetimes, which ensures that memory is managed safely and efficiently.

# Initializing a Rust Project

- run `cargo init` to create a new Rust project in pwd

It created 2 files:

- `Cargo.toml`: This is the manifest file for Rust's package manager, Cargo. It contains metadata about the project, such as its name, version, dependencies, and other configuration options.(similar to package.json in Node.js)

- `src/main.rs`: This is the main source file for the Rust project. It contains the entry point of the program, which is the `main` function. This is where the execution of the program starts.

## `cargo init`

1. We can create an end user application with `cargo init --bin` or a library with `cargo init --lib`.

Simply running `cargo init` will create a binary project by default, kinda like a end user application backend.

`cargo build` will compile the project and create an executable in the `target/debug` directory. (For development builds)

`cargo build --release` will compile the project and create an optimized executable in the `target/release` directory. (For production builds)

if we run `target/debug/hello` it will run the executable.

# Simple Variables

### Numbers

```rust
i8 // 8-bit signed integer
i16 // 16-bit signed integer
i32 // 32-bit signed integer
i64 // 64-bit signed integer
i128 // 128-bit signed integer

u8 // 8-bit unsigned integer
u16 // 16-bit unsigned integer
u32 // 32-bit unsigned integer
u64 // 64-bit unsigned integer
u128 // 128-bit unsigned integer

f8 // 8-bit floating point (not supported in Rust)
f16 // 16-bit floating point
f32 // 32-bit floating point
f64 // 64-bit floating point
f128 // 128-bit floating point (not supported in Rust)


```

For instance, i8 can store 2^7 values, which is from -128 to 127. why? because first bit is used for the sign (positive or negative), leaving 7 bits for the value. Whereas u8 can store 2^8 values, which is from 0 to 255, because it does not need to store the sign, leaving all 8 bits for the value.

By default, Rust uses `i32` for signed integers and `u32` for unsigned integers.
By default, Rust uses `f64` for floating point numbers.

---

### Signed vs Unsigned

- Signed integers can represent both positive and negative numbers.
- Unsigned integers can only represent positive numbers, but they can represent a larger range of positive numbers than signed integers of the same size.

---

In Rust, variables are immutable by default, meaning that once a variable is assigned a value, it cannot be changed. To make a variable mutable, we need to use the `mut` keyword.

```rust
let mut x = 5;
x = 10;
```

---

## Booleans

```rust
let is_active: bool = true;
let is_logged_in: bool = false;
```

---

## Strings

We can't just do

```rust
let name = "John";
```

Because strings are not fixed data type and can cause RAM issues, they keep on changing in size, so we need to use the `String` type, which is a growable, heap-allocated data structure, it is the most common way to represent strings in Rust.

```rust
let name: String = String::from("John");
```

# Memory Management in Rust

### Core Idea

Whenever you run a program (C++, Rust, JS), it allocates and deallocates memory on the RAM.
For example, for the following JS code:

```javascript
function main() {
  runLoop();
}

function runLoop() {
  let x = [];
  for (let i = 0; i < 100000; i++) {
    x.push(1);
  }
  console.log(x);
}

main();
```

as the `runLoop` function runs, a new array is pushed to RAM, and eventually `garbage collected` after the function execution is complete.

![alt text](https://www.notion.so/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F085e8ad8-528e-47d7-8922-a23dc4016453%2F3ef22272-00bb-4dad-9073-eb3c0c784cc0%2FScreenshot_2024-04-23_at_5.22.38_AM.png?table=block&id=014e5ce2-366d-441b-bccd-d5cb07f1516c&cache=v2)

Garbage collection is a process where the runtime environment automatically frees up memory that is no longer in use, but it can lead to performance issues and unpredictable pauses in execution. Basically it can make the language slower and less efficient.

In Rust, memory management is done through a system of ownership with a set of rules that the compiler checks at compile time. This means that Rust does not have a garbage collector like JavaScript or C++. But these rules ensure that memory is managed safely and efficiently without the need for a garbage collector.
Rust forces you to write code in a way that ensures memory safety and prevents memory leaks, which is a common problem in languages like C and C++'s manual memory management.

# Jargon 1 - Mutability

Immutable variables represent variables whose values cannot be changed after they are assigned. In Rust, variables are immutable by default, meaning that once a variable is assigned a value, it cannot be changed.

Immutability in Rust assures that there's no any probability of a race condition, which is a situation where two or more threads access shared data and try to change it at the same time, leading to unpredictable results. If no other thread
can alter the data, then no synchronization is needed, and the data can be accessed safely and concurrently.

Knowing that certain data will not change allows the compiler to make optimize code better.

But these benefits can be taken away by using the `mut` keyword, which allows you to create mutable variables that can be changed after they are assigned.

Therefore, to ensure that your code is safe and efficient, you should use immutable variables whenever possible, and only use mutable variables when absolutely necessary.

```rust
let x = 5; // immutable variable
let mut y = 10; // mutable variable
x = 20; // this will cause a compile-time error because x is immutable
y = 20; // this is allowed because y is mutable
```

# Jargon 1 - Stack vs Heap

Rust has clear rules about stack and heap data management:

- **Stack**: Fast allocation and deallocation. Rust uses the stack for most primitive data types and for data where the size is known at compile time (eg: numbers). Since size of data is predictable at compile time and there is no scope for it change during runtime, it is organized and managed efficiently in a last-in-first-out (LIFO) manner. This makes stack memory very fast to access and manage.

We don't just put something on the stack. A stack frame is created for each function call, and when the function returns, the stack frame is popped off the stack. This means that the memory used by the function's local variables is automatically freed when the function returns.

- **Heap**: Used for data that can grow at runtime, such as vectors or strings. Heap memory is more flexible but also slower to access and manage because it requires more complex bookkeeping to keep track of allocated and deallocated memory.

A heap is just some address in RAM

# Jargon 2 - Ownership

![alt text](https://www.notion.so/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F085e8ad8-528e-47d7-8922-a23dc4016453%2F3b89e5a1-8b80-4bfb-b674-d2bddeb7cc55%2FScreenshot_2024-04-27_at_2.52.31_AM.png?table=block&id=99599f19-9f02-4272-b721-e9164a6668f5&cache=v2)

Ref - https://doc.rust-lang.org/book/ch04-01-what-is-ownership.html

## Meet Rihana

She always wants to keep a `boyfriend` (or owner) and can never remain single. She says if I ever become single (have no owners), I will die. She also can only have a single boyfriend at a time.

## Stack Variables

#### Example 1 - Passing stack Variables inside functions

```rust
fn main() {
		let x = 1; // crated on stack,owner is main
		let y = 3; // created on stack,owner is main
    println!("{}", sum(x, y));//Is it passed by value or by reference?
    println!("Hello, world!");
}

//Are these x and y new variables or references to the original x and y?
fn sum(a: i32, b: i32) -> i32 {
    let c = a + b;
    return c;
}
// In this case, x and y are passed by value, meaning that a and b are new variables created on the stack with the same values as x and y.
//Why? Because for things we know the size of at compile time, Rust uses the stack for memory management.
```

#### Example 2 - Scoping variables in same fn

```rust
fn main() {
    let x = 1; // crated on stack
    {
        let y = 3; // created on stack
    }

    println!("{}", y); // throws error, as y is not in scope here
}
```

## Heap Variables

Heap variables are like Rihana. They always wanna have a `single` owner, and if their owner goes out of scope, they get deallocated.

Any time the owner of a `heap variable` goes out of scope, the heap variable is deallocated. This is done by the Rust compiler at compile time, so there is no need for a garbage collector.

Earlier in C, we had to manually allocate and deallocate memory, which may result in dangling pointers, memory leaks, double free errors, etc. But in Rust, the compiler ensures that these issues do not occur by enforcing the ownership rules.

Rust says every variable created on Heap will have a pointer to it on the stack, and the pointer will be the owner of the heap variable. The pointer is created on the stack, and it points to the heap variable.

But what is new here? Isn't it obvious that a pointer dies when it goes out of scope?

let's say you try do this in Rust:

```rust
fn main() {
    let s1 = String::from("hello");
    let s2 = s1;
    println!("{}", s1); // This line would cause a compile error because ownership has been moved.(Rihana has a new boyfriend now, she cannot have two boyfriends at the same time)
}
```

Why does Rust do this? Because it wants to ensure that there is only one owner of the heap variable at a time, so that it can be deallocated safely when the owner goes out of scope. This is done to prevent memory leaks and dangling pointers. Earlier this was a major issue in C and C++.As shown in below image:

![alt text](https://www.notion.so/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F085e8ad8-528e-47d7-8922-a23dc4016453%2F165f9686-4e14-4160-bde4-08c3340c14e3%2Ftrpl04-04.svg?table=block&id=d5d261e4-d8f5-48fb-b92f-fe2bbcfc2306&cache=v2)

Another example of same concept:

```rust
fn main() {
    let my_string = String::from("hello");
    takes_ownership(my_string);
    println!("{}", my_string); // This line would cause a compile error because ownership has been moved.
}

fn takes_ownership(some_string: String) {
    println!("{}", some_string); // `some_string` now owns the data.
}
```

Here, `my_string` is passed to the `takes_ownership` function, and ownership is transferred to `some_string`. After this function call, `my_string` is no longer valid, and trying to use it will result in a compile-time error.

### Fix?

Clone the string (Explicitly create a new copy of the string data as cloning in Rust is expensive, so Rust does not do it automatically):

```rust
fn main() {
    let s1 = String::from("hello");
    let s2 = s1.clone();
    println!("{}", s1); // Compiles now
}
```

![alt text](https://www.notion.so/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F085e8ad8-528e-47d7-8922-a23dc4016453%2F2eace7ca-252a-4eea-96fc-78deef6b586b%2FScreenshot_2024-04-26_at_9.08.01_AM.png?table=block&id=036c4833-6e1a-4c72-a64c-dc80124fd1c7&cache=v2)

But what if you want to pass the same string over to the function? You don’t want to clone it, and you want to return back ownership to the original function?
You can either do the following -

```rust
fn main() {
    let s1 = String::from("hello");
    let s2 = takes_ownership(s1);
    println!("{}", s2);
}

fn takes_ownership(some_string: String) -> String {
    println!("{}", some_string);
    return some_string; // return the string ownership back to the original main fn
}
```

OR

```rust
fn main() {
    let mut s1 = String::from("hello");
    s1 = takes_ownership(s1);
    println!("{}", s2);
}

fn takes_ownership(some_string: String) -> String {
    println!("{}", some_string);
    return some_string; // return the string ownership back to the original main fn
}
```

Is there a better way to pass strings (or generally heap related data structures) to a function without passing over the ownership?
Yes - References

# Jargon 3 - Borrowing and References

## Rihana Upgrades

Rihana now says I’d like to be borrowed from time to time. I will still have a single owner, but I can still be borrowed by other variables temporarily. What rules do you think should apply to her?
She can be borrowed by multiple people that she’s friends with but does no hanky panky
If she does want to do hanky panky, she can only have 1 borrower that she does it with. She cant simultaneously be with other borrowers (even with no hanky panky)

What does it mean?
CASE 1: owner -> rihana, borrower -> friend1, friend2
CASE 2: owner -> rihana, borrower -> friend1 (no extra borrower)

If borrowers die, rihana does not die, but if owner dies, rihana dies.

## References

In Rust, references are like pointers in C/C++.
References mean giving the address of a string rather than the ownership of the string over to a function
For example

```rust
fn main() {
    let s1 = String::from("Hello");
    let s2 = &s1; //no concept of length or capacity, simply dependent on the owner (here s1)

    println!("{}", s2);
    println!("{}", s1);    // This is valid, The first pointer wasn't invalidated
}
```

![alt text](https://www.notion.so/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F085e8ad8-528e-47d7-8922-a23dc4016453%2F01536509-0350-4ee4-ba6e-fcb838cc32ae%2FScreenshot_2024-04-26_at_9.27.08_AM.png?table=block&id=d2216029-bfeb-41f7-81b4-a04762520203&cache=v2)


## Borrowing
You can transferring ownership of variables to fns. By passing a reference to the string to the function ```borrow_variable```, the ownership of the string remains with the original variable, in the ```main``` function. This allows you to use ```my_string```again after the function call.

```rust
fn main() {
    let my_string = String::from("Hello, Rust!");
    borrow_variable(&my_string);  // Pass a reference to my_string
    println!("{}", my_string);    // This is valid because ownership was not transferred
}

fn borrow_variable(some_string: &String) {
    println!("{}", some_string);  // some_string is borrowed and not moved
}
```

## Mutable References

What if you want a function to ```update``` a variable.
(rihanna's hanky panky)

```rust

fn main() {
    let mut s1 = String::from("Hello");
    update_word(&mut s1);
    println!("{}", s1);
}

fn update_word(word: &mut String) {
    word.push_str(" World");
}
```
Try having more than one mutable reference at the same time - 

```rust
fn main() {
    let mut s1 = String::from("Hello");
    let s2 = &mut s1;
    update_word(&mut s1);
    println!("{}", s1);
    println!("{}", s2);
}

fn update_word(word: &mut String) {
    word.push_str(" World");
}
```

NOTE:Only one mutable reference to a piece of data is allowed in a particular scope. But if you declare a mutable reference but do not use it, the compiler understands that you are not using it and allows you to create another mutable or immutable reference.


## Rules of Borrowing

1. There can be many ```immutable references``` at the same time.

```rust

fn main() {
    let  s1 = String::from("Hello");
    let s2 = &s1;
    let s3 = &s1;
    
    println!("{}", s1);
    println!("{}", s2);
    println!("{}", s3);
}
// No errors
```

2. There can be only one ```mutable reference``` at a time.

```rust
fn main() {
    let mut s1 = String::from("Hello");
    let s2 = &mut s1;
    let s3 = update_word(&mut s1);
    
    println!("{}", s1);
    println!("{}", s2);
}

fn update_word(word: &mut String) {
    word.push_str(" World");
}
// Error: You can only have one mutable reference to a piece of data in a particular scope
```
3. If there is a ```mutable reference```, you can’t have another ```immutable reference``` either.
```rust
fn main() {
    let mut s1 = String::from("Hello");
    let s2 = &mut s1;
    let s3 = &s1;
    
    println!("{}", s1);
    println!("{}", s2);
}

fn update_word(word: &mut String) {
    word.push_str(" World");
}
```
// Error: You can only have one mutable reference to a piece of data in a particular scope


-  This to avoid any ```data races/inconsistent``` behaviour
- If someone makes an ```immutable reference```, they don’t expect the value to change suddenly
- If more than one ```mutable reference``` happen, there is a possibility of a data race and synchronization issues


# Structs

Structs in rust let you structure data together. Similar to ```objects``` in javascript.

```rust
struct User {
    active: bool,
    username: String,
    email: String,
    sign_in_count: u64,
}

fn main() {
    //tuple struct, unit struct (not used much but good to know - read rustbook)
    let user1 = User {
        active: true,
        username: String::from("someusername123"),
        email: String::from("someone@example.com"),
        sign_in_count: 1,
    };
    print!("User 1 username: {:?}", user1.username);
}
```

You cannot create objects directly in Rust because Rust is very strict about ownership and borrowing, you need to create a struct first.

- **Unit Structs**: These are structs without any fields, used for type-level information. Why use when no attributes are needed? They can have some functions associated with them. (implementations without attributes)

- **Tuple Structs**: These are structs that have a fixed number of fields, but the fields do not have names. They are similar to tuples in Rust, but they are named types.

```rust
struct Color(u8, u8, u8); // RGB color
struct Point(i32, i32); // 2D point
```
#### How is data stored in memory for structs?

![alt text](https://www.notion.so/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F085e8ad8-528e-47d7-8922-a23dc4016453%2F6f31247f-792b-4c8e-b966-351b2d9e4a7a%2FScreenshot_2024-04-26_at_10.04.21_AM.png?table=block&id=e45bc154-8082-42a8-b9d2-c981eeb53260&cache=v2)

Each attribute in a struct is stored in memory sequentially, meaning that the first attribute is stored at the lowest address, the second attribute is stored at the next address, and so on. This means that the size of a struct is the sum of the sizes of its attributes. Those with data types for stack memory are stored on the stack, and those with data types for heap memory are stored on the heap.

NOTE: Traits to be discussed later.

# Implementations of Structs
You can also ```implement structs``` , which means you can attach functions to instances of structs
(Very similar to classes in TS)

```rust
struct Rect {
   width: u32,
   height: u32,
};

impl Rect {
    fn area(&self) -> u32 {
         self.width * self.height
    }
}

fn main() {
    let rect = Rect {
        width: 30,
        height: 50,
    };
    print!("The area of the rectangle is {}", rect.area());
}
```


# Enums

Enums in rust are similar to enums in Typescript. They allow you to define a type by enumerating its possible variants.

Ref - https://doc.rust-lang.org/book/ch06-01-defining-an-enum.html


```rust
enum Direction {
    North,
    East,
    South,
    West,
};

fn main() {
    let my_direction = Direction::North;
    let new_direction = my_direction; // No error, because Direction is Copy
    move_around(new_direction);
}

fn move_around(direction: Direction) {
    // implements logic to move a character around
}
```

Why not simple use actual strings? Because we don’t enforce the 4 variants of directions. So this is much looser than strictly allowing only 4 variants for direction

## Enums with Values/Variants/Data

Enums can also have values associated with them. This allows you to store additional data with each variant of the enum.

```rust
// Define an enum called Shape
enum Shape {
    Circle(f64),  // Variant with associated data (radius)
    Square(f64),  // Variant with associated data (side length)
    Rectangle(f64, f64),  // Variant with associated data (width, height)
}

// Function to calculate area based on the shape
fn calculate_area(shape: Shape) -> f64 {
    // calculates the area of the shape 
    return 0
}

fn main() {
    // Create instances of different shapes
    let circle = Shape::Circle(5.0);
    let square = Shape::Square(4.0);
    let rectangle = Shape::Rectangle(3.0, 6.0);
    
}
```

What about calculating the area of these shapes? 

## Pattern Matching with Enums

```rust
// Function to calculate area based on the shape
fn calculate_area(shape: Shape) -> f64 {
    match shape {
        Shape::Circle(radius) => std::f64::consts::PI * radius * radius,
        Shape::Square(side_length) => side_length * side_length,
        Shape::Rectangle(width, height) => width * height,
    }
};
```


# Error Handling

1. **Compilation Errors**: These are errors that occur at compile time, such as syntax errors or type mismatches, borrow checker errors, etc where the final binary cannot be created.

2. **Runtime Errors**: Final binary was created, but while this binary is running, it encounters an error that causes it to crash or behave unexpectedly. These errors can be caused by things like invalid input, network failures, etc.

```Didn't you say Rust handles everything and avoids errors?```
Yes, Rust does a great job of catching many errors at compile time, but it cannot catch all possible runtime errors. For example, if you try to access an index that is out of bounds in an array, Rust will panic at runtime. The catch is how gracefully you handle these errors.

Different languages have different ways to handle errors. 
Javascript, for example, has the concept of try catch blocks

```javascript
try {
    const data = fs.readFileSync('example.txt', 'utf8');
    console.log("File content:", data);
} catch (err) {
    console.error("Error reading the file:", err);
}
```

The reason we put the code inside a try catch block is that ```reading a file``` is ```unpredictable```. 
The file might not exist, the file might be locked by another process, and hence there is a possibility of this code ```throwing an error```
The same is true for a rust program trying to access a file. But the way rust does error handling is slightly different.

## Result Enum

```rust
enum Result<T, E> { //T &  E are generic in Rust(Generics are a way to write code that can work with any data type,always in Capital letters)
    Ok(T),
    Err(E),
}
```

If you look at the code above, it is an enum (with generic types)
This enum is what a function can return/returns when it has a possibility of throwing an error

For example
```rust
use std::fs::File;

fn main() {
	let greeting_file_result = fs::read_to_string("hello.txt");
}
```

Notice the type of `greeting_file_result` in VSCode's type hints.

 
It returns an enum that looks as follows. It’s an enum with the ```Ok``` variant having a string value and ```Err``` variant having an Error value 

```rust
enum Result{
    Ok(String),
    Err(Error),
}
```

Complete code example:

```rust
use std::fs;

fn main() {
    let greeting_file_result = fs::read_to_string("hello.txt");

    match greeting_file_result {
        Ok(file_content) => {
            println!("File read successfully: {:?}", file_content);
        },
        Err(error) => {
            println!("Failed to read file: {:?}", error);
        }
    }
}
```

Incase you write a function yourself, you can also return a ```Result``` from it. As the name suggests, ```Result``` holds the ```result``` of a function call that might lead to an error. 

## Unwraps

Incase you are ok with runtime errors (crashing the process while it runs if an error happens), then you can ```unwrap``` a ```Result```

```rust
use std::fs;

fn main() {
    let greeting_file_result = fs::read_to_string("hello.txt");
    print!("{}", greeting_file_result.unwrap());
}
```


## Returning Custom Errors

```rust
use core::fmt;
use std::{fmt::{Debug, Formatter}, fs};

pub struct FileReadError {

}

fn main() {
    let contents = read_file("hello.txt".to_string());
    match contents {
        Ok(file_content) => {
            println!("File content: {}", file_content);
        },
        Err(error) => {
            println!("Error reading file: {:?}", error);
        }
    }
}

fn read_file(file_path: String) -> Result<String, FileReadError> {
    let greeting_file_result = fs::read_to_string("hello.txt");
    match greeting_file_result {
        Ok(file_content) => {
            Ok(file_content)
        },
        Err(error) => {
            let err = FileReadError {};
            Err(err)
        }
    }
}
```


## Option Enum

In other languages, we have the concept of ```null``` or ```undefined``` to represent the absence of a value, which is believed to be a billion dollar mistake in programming languages that can lead to many runtime errors and bugs.
Rust does not have a null value, but it has an ```Option``` enum that can be used to represent the absence of a value in a safe way.



- Ref- https://viblo.asia/p/billion-dollar-mistake-RQqKLopr57z

The Option enum was introduced in Rust to handle the concept of nullability in a safe and expressive way. Unlike many programming languages that use a null or similar keyword to represent the absence of a value, Rust doesn't have null.
 

```rust
pub enum Option<T> {
    None, //no value present
    Some(T), //some value present
}
```


If you ever have a function that should return ```null```, return an ```Option``` instead.

```rust
fn find_first_a(s: String) -> Option<i32> {
    for (index, character) in s.chars().enumerate() {
        if character == 'a' {
            return Some(index as i32);
        }
    }
    return None;
}

fn main() {
    let my_string = String::from("raman");
    match find_first_a(my_string) {
        Some(index) => println!("The letter 'a' is found at index: {}", index),
        None => println!("The letter 'a' is not found in the string."),
    }
}
```


# Cargo, packages and external deps

Just like the ```nodejs``` ecosystem has ```npm```, the rust ecosystem has ```cargo```
Cargo is a ```package manager ```for rust, which means we can use it to bring packages (crates in case of rust) to our projects
 
Examples:

## Generate random numbers

```rust
use rand::{Rng, thread_rng};

fn main() {
    let mut rng = thread_rng();
    let n: u32 = rng.gen();
    println!("Random number: {}", n);
}
```

## Store time in DB as a variable
Use crate https://crates.io/crates/chrono
Run ```cargo add chrono```

```rust
use chrono::{Local, Utc};

fn main() {
    // Get the current date and time in UTC
    let now = Utc::now();
    println!("Current date and time in UTC: {}", now);

    // Format the date and time
    let formatted = now.format("%Y-%m-%d %H:%M:%S");
    println!("Formatted date and time: {}", formatted);

    // Get local time
    let local = Local::now();
    println!("Current date and time in local: {}", local);
}
```

### What all libraries does rust have?
A lot of them
https://actix.rs/ - Extremely fast http server
https://serde.rs/ - Serializing and deserialising data in rust
https://tokio.rs/ - Asynchronous runtime in rust
https://docs.rs/reqwest/latest/reqwest/ - Send HTTP requests
https://docs.rs/sqlx/latest/sqlx/ - Connect to sql database


- You can add an external crate to your project by running `cargo add <crate_name>`, which will automatically update your `Cargo.toml` file with the new dependency.