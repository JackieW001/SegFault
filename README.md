# Intro to Rust

## What is Rust?
Rust is a systems level programming language that was sponsored by Mozilla Research. It was started in 2006 by Graydon Hoare and in 2016 and 2017, it won "most loved programming language” in the StackOverflow Developer Survey.

## Syntax & Running Rust Files

Create a file named main.rs and input this into your file:

```
fn main() {
    println!("Hello, world!");
}
```
Then in your terminal, run these commands:

```
$ rustc main.rs
$ ./main
Hello, world!
```
You should see the same output as demonstrated above.

## Variable Binding

Variables in Rust are staticly typed which means they are given a type when they are declared. 

```
fn main() {
    println!("Hello, world!");
    
    let num = 10;
    
    let mut age: i32 = 40;
}
```

In this example, when you declare num to be 10, Rust will guess the type of num, in this case integer. In addition, it will automatically assume your variable is immutable but more on that later. In this example, when you declare age, you set its type to be a 32 byte integer and you declare to be mutable. 

## Mutability

Try running this code block:

```
fn main() {
    let x = 5;
    println!("The value of x is: {}", x);
    x = 6;
    println!("The value of x is: {}", x);
}
```

If you run this, you should encounter an error. This is because in Rust, variables are automatically immutable. This means their value is unable to be changed. In order to fix this, you can add the word `mut` when you declare x.


```
fn main() {
    let mut x = 5;
    println!("The value of x is: {}", x);
    x = 6;
    println!("The value of x is: {}", x);
}
```

## Functions

In Rust, function parameters are declared to be a certain type. In the below example, in the function `plus_one` the parameter x has a type of a 32 byte integer. In addition, in Rust functions, `->` is placed after the parameters to show what the return type is. In the below example, the function `plus_one` has a return type of a 32 byte integer. 

```
fn main() {
    let x = plus_one(5);

    println!("The value of x is: {}", x);
}

fn plus_one(x: i32) -> i32 {
    x + 1
}
```

One thing to note about functions in Rust is that instead of saying `return x+1;`, you can simply make the last line of the function `x+1`. However, when doing so, do not include the semicolon or else you will run into errors. Also, even though this is less typing, if you want, you can still use the keyword `return`.

## Loops

There are three main looping mechanisms in Rust: `while`, `loop`, and `for`.

### Loop
Looping with loop is pretty simple; it just goes forever.

```
loop {
    println!("Loop forever!");
}
```

This is equivalent to:

```
while true {
    println!("Loop forever!");
}
```

### While

While loops will go until a certain condition is no longer true. You should use while loops when you aren't sure how many times you should loop.

```
let mut x = 5; // mut x: i32
let mut done = false; // mut done: bool

while !done {
    x += x - 3;

    println!("{}", x);

    if x % 5 == 0 {
        done = true;
    }
}
```

In this example, the loop will run until x is divisible by 5 which will cause `!done` to evaluate to false which will break you out of your loop.

### For

For loops in Rust are formatted as follows:

```
for var in expression {
    code
}
```

One example of this formatting is:

```
for x in 0..10 {
    println!("{}", x); // x: i32
}
```

In this example, your loop will run ten times. One to note is that when you use `0..10`, the first value is inclusive while the second is exclusive. So in this example, it will print 0 to 9 instead of 0 to 10 or 1 to 10.

## Structs

To define a struct, we use the keyword `struct` and then name the struct. We can give the struct names and types of the data included which we call `fields`.

```
struct User {
    username: String,
    email: String,
    sign_in_count: u64,
    active: bool,
}
```

After defining a struct, we can create an instance of it as follows:

```
let user1 = User {
    email: String::from("someone@example.com"),
    username: String::from("someusername123"),
    active: true,
    sign_in_count: 1,
};
```

After creating an instance, we can access the information stored using dot notation. For example, in order to get user1's email, you would:

```
    user1.email;
```

In addition if your struct is mutable, you can change the value of the fields. For example, you could change their email.

```
let mut user1 = User {
    email: String::from("someone@example.com"),
    username: String::from("someusername123"),
    active: true,
    sign_in_count: 1,
};

user1.email = String::from("anotheremail@example.com");
```

## Ownership
All programs have a way to manage the computer's memory. Some programs use a garbage collector that constantly searches for memory that is no longer being used to clear it. Other programs require the programmer to specifically free up and allocate memory. Rust does it differently. It uses a concept called ownership to manage memory.

### Ownership Rules
Ownership is governed by three rules:
```
1. Each value in Rust has a variable that’s called its _owner_.
2. There can only be one owner at a time.
3. When the owner goes out of scope, the value will be dropped.
```

### Variable Scope
Since we have already covered basic syntax, we won't include the `fn main() {` code in examples. Just make note that the outer most set of `{}` deliniates the main function. This will help keep our code more concise and easier to read.

As a first example of ownership, we’ll look at the scope of some variables. A scope is the range within a program for which an item is valid. Let's say we have this:
```
{
  let s = "hello";
}
```

Here, the variable `s` is a string literal that points to the data `"hello"`. The variable is valid from the point at which it’s declared until the end of the current _scope_. Below shows commented code where `s` is valid:
```
{                      // s is not valid here, it’s not yet declared
    let s = "hello";   // s is valid from this point forward

    // do stuff with s
}                      // this scope is now over, and s is no longer valid
```

When `s` comes into scope, it is valid, and remains so until it goes out of scope.

### Ways Variables and Data Interact: Move
Multiple variables can interact with the same data in different ways in Rust.
Let's say we have this:
```
let x = 5;
let y = x;
```

From previous programming experience, we know that the value `5` is binded to the variable `x`. Then, the data stored in variable `x` is copied to variable `y`. Now, we have two variables, `x` and `y`, that are both equal to `5`. 

Now let's look at the `String` version:

But before we do that, a little note about Rust `Strings`:

The Rust String is considered a complex data type. It is different from string literals
1. The declaration of a String and a string literal is different
2. `String` can be mutated
The `String` in Rust can be thought as the `String` class in Java.

This is how to declare a `String` in Rust:
```
let s = String::from("hello");
```

And it can be mutated by doing this:
```
let mut s = String::from("hello");

s.push_str(", world!"); // push_str() appends a literal to a String

println!("{}", s); // This will print `hello, world!`
```

Now that we know a little bit more about `Strings`, let's look back at the `String` version of variable declaration.
Let's say we have something like this:
```
let s1 = String::from("hello");
let s2 = s1;
```

We might assume that the way it works would be the same as the previous example: that is, the second line would make a copy of the value in `s1` and bind it to `s2`. But this isn’t quite what happens.

Let's look at what a `String` looks like "under the hood."

![useful image](https://jackiew001.github.io/SegFault/assets/move1.png)

The most important parts to take away from this is that
1. The variable name is not directly connected to the `String` data, and
2. The variable name has a pointer that points to the `String` data

When we assign s1 to s2, the String data is copied, meaning we copy the pointer, the length, and the capacity. The data the `s1` variable is pointin to is **NOT** copied! The data representation in memory looks like this:

![useful image](https://jackiew001.github.io/SegFault/assets/move2.png)

Note that `s1` and `s2` now are both pointing to the same `String` data.

Earlier, we said that when a variable goes out of scope, Rust automatically tries to free up the memory the variable was pointing to. However, in the figure shown above, `s1` and `s2` are pointing to the same data. This is a problem: when `s2` and `s1` go out of scope, they will both try to free the same memory. This is known as a _double free error_ and is one of the memory safety bugs programmers encounter. Freeing memory twice can lead to memory corruption, which can potentially lead to security vulnerabilities.

To ensure memory saftey, there is one more thing Rust does. Instead of trying to copy the allocated memory, Rust moved the data from `s1` to `s2` and invalidated the `s1` variable. If you try doing something like this: 
```
let s1 = String::from("hello");
let s2 = s1;

println!("{}, world!", s1);
```

You will get an error because the `s1` reference to the `String` data was invalidated: 
```
error[E0382]: use of moved value: `s1`
 --> src/main.rs:5:28
  |
3 |     let s2 = s1;
  |         -- value moved here
4 |
5 |     println!("{}, world!", s1);
  |                            ^^ value used here after move
  |
  = note: move occurs because `s1` has type `std::string::String`, which does
  not implement the `Copy` trait
```

As mentioned before, the `s1` data was moved to `s2`. Visually, it looks like this: 

![useful image](https://jackiew001.github.io/SegFault/assets/move3.png)

It is similar to a shallow copy, where we are copying the pointer, length, and capacity without copying the data.
This solves our problem! With only `s2` valid, when it goes out of scope, it alone will free the memory, and we’re done.

The semantics for passing a value to a function are similar to assigning a value to a variable. Passing a variable to a function will move or copy, just like assignment. Here's an example of variables going into and out of scope:
```
fn main() {
    let s = String::from("hello");  // s comes into scope.

    takes_ownership(s);             // s's value moves into the function...
                                    // ... and so is no longer valid here.

    let x = 5;                      // x comes into scope.

    makes_copy(x);                  // x would move into the function,
                                    // but i32 is Copy, so it’s okay to still
                                    // use x afterward.

} // Here, x goes out of scope, then s. But since s's value was moved, nothing
  // special happens.

fn takes_ownership(some_string: String) { // some_string comes into scope.
    println!("{}", some_string);
} // Here, some_string goes out of scope and `drop` is called. The backing
  // memory is freed.

fn makes_copy(some_integer: i32) { // some_integer comes into scope.
    println!("{}", some_integer);
} // Here, some_integer goes out of scope. Nothing special happens.
```

Returning values can also transfer ownership. Here's an example:
```
fn main() {
    let s1 = gives_ownership();         // gives_ownership moves its return
                                        // value into s1.

    let s2 = String::from("hello");     // s2 comes into scope.

    let s3 = takes_and_gives_back(s2);  // s2 is moved into
                                        // takes_and_gives_back, which also
                                        // moves its return value into s3.
} // Here, s3 goes out of scope and is dropped. s2 goes out of scope but was
  // moved, so nothing happens. s1 goes out of scope and is dropped.

fn gives_ownership() -> String {             // gives_ownership will move its
                                             // return value into the function
                                             // that calls it.

    let some_string = String::from("hello"); // some_string comes into scope.

    some_string                              // some_string is returned and
                                             // moves out to the calling
                                             // function.
}

// takes_and_gives_back will take a String and return one.
fn takes_and_gives_back(a_string: String) -> String { // a_string comes into
                                                      // scope.

    a_string  // a_string is returned and moves out to the calling function.
}
```

The ownership of a variable follows the same pattern every time: assigning a value to another variable moves it. When a variable that includes data goes out of scope, the value will be cleaned up unless the data has been moved to be owned by another variable.

Taking ownership and then returning ownership with every function is a bit tedious. What if we want to let a function use a value but not take ownership? It’s quite annoying that anything we pass in also needs to be passed back if we want to use it again, in addition to any data resulting from the body of the function that we might want to return as well. This is where borrowing comes in.

## Borrowing
Let's say we want to create a function that returns the length of a `String`. We would do something like this:
```
fn main() {
    let s1 = String::from("hello");

    let len = calculate_length(&s1);

    println!("The length of '{}' is {}.", s1, len);
}

fn calculate_length(s: &String) -> usize {
    s.len()
}
```

Note that hat we pass `&s1` into calculate_length, and in its definition, we take `&String` rather than String. The ampersands `&` delinate _references_, and they allow you to refer to some value without taking ownership of it. Visually, references look like this:

![useful image](https://jackiew001.github.io/SegFault/assets/ref1.png)

Let's take a closer look at the function call:
```
let s1 = String::from("hello");

let len = calculate_length(&s1);
```

The `&s1` syntax lets us create a reference that refers to the value of `s1` but does not own it. Because it does not own it, the value it points to will not be dropped when the reference goes out of scope.

Likewise, the signature of the function uses `&` to indicate that the type of the parameter `s` is a reference.
```
fn calculate_length(s: &String) -> usize { // s is a reference to a String
    s.len()
} // Here, s goes out of scope. But because it does not have ownership of what
  // it refers to, nothing happens.
```

The scope in which the variable s is valid is the same as any function parameter’s scope, but we don’t drop what the reference points to when it goes out of scope because we don’t have ownership. Functions that have references as parameters instead of the actual values mean we won’t need to return the values in order to give back ownership, since we never had ownership. 

We call having references as function parameters _borrowing_.

What if we want to modify something we are borrowing? As in real life, borrowing something doesn't mean you own it, and therefore can't change or mutate it.

If we try something like this:
```
fn main() {
    let s = String::from("hello");

    change(&s);
}

fn change(some_string: &String) {
    some_string.push_str(", world");
}
```
This error will occur:
```
error[E0596]: cannot borrow immutable borrowed content `*some_string` as mutable
 --> error.rs:8:5
  |
7 | fn change(some_string: &String) {
  |                        ------- use `&mut String` here to make mutable
8 |     some_string.push_str(", world");
  |     ^^^^^^^^^^^ cannot borrow as mutable
```
We aren't allowed to modify something we have a reference to.

### Mutable References
With a small tweak, we can fix this:
```
fn main() {
    let mut s = String::from("hello");

    change(&mut s);
}

fn change(some_string: &mut String) {
    some_string.push_str(", world");
}
```
Note that instead of having just `&s`, it is now `&mut s`. In the function, `&String` is now `&mut String`. This tells Rust that we are making a mutable reference. 

But mutable references have one big restriction: you can only have one mutable reference to a particular piece of data in a particular scope.
Here's an example of having more than one mutable reference:
```
let mut s = String::from("hello");

let r1 = &mut s;
let r2 = &mut s;
```
This error will occur:
```
error[E0499]: cannot borrow `s` as mutable more than once at a time
 --> borrow_twice.rs:5:19
  |
4 |     let r1 = &mut s;
  |                   - first mutable borrow occurs here
5 |     let r2 = &mut s;
  |                   ^ second mutable borrow occurs here
6 | }
  | - first borrow ends here

```

This restriction allows for mutation but in a very controlled fashion. The benefit of having this restriction is that Rust can prevent data races at compile time.

A _data race_ occurs when these three behaviors occur:
1. Two or more pointers access the same data at the same time.
2. At least one of the pointers is being used to write to the data.
3. There’s no mechanism being used to synchronize access to the data.

Data races cause undefined behavior and can be difficult to diagnose and fix when you’re trying to track them down at runtime; Rust prevents this problem from happening because it won’t even compile code with data races!

As always, we can use curly brackets to create a new scope, allowing for multiple mutable references, just not simultaneous ones:
```
fn main() {
let mut s = String::from("hello");

{
    let r1 = &mut s;

} // r1 goes out of scope here, so we can make a new reference with no problems.

let r2 = &mut s;
}
```
### Rules of References
TL;DR of references:
1. At any given time, you can have _either_ but **not** both of:
  - One mutable reference.
  - Any number of immutable references.
2. References must be vaild.

## The Future of Rust

# Resources
[Rust Documentation](https://doc.rust-lang.org/book/second-edition/ch01-00-introduction.html)
