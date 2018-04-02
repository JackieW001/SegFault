# Intro to Rust

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


------------------------------------------
You can use the [editor on GitHub](https://github.com/JackieW001/SegFault/edit/master/README.md) to maintain and preview the content for your website in Markdown files.

Whenever you commit to this repository, GitHub Pages will run [Jekyll](https://jekyllrb.com/) to rebuild the pages in your site, from the content in your Markdown files.

### Markdown

Markdown is a lightweight and easy-to-use syntax for styling your writing. It includes conventions for

```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/JackieW001/SegFault/settings). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://help.github.com/categories/github-pages-basics/) or [contact support](https://github.com/contact) and we’ll help you sort it out.


