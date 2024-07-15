+++
title = 'mdBook'
date = '2024-07-15'
author = 'aiclr'
categories = ['tools']
tags = ['mdbook','rust']
Summary='mdBook is a command line tool to create books with Markdown. It is ideal for creating product or API documentation, tutorials, course materials or anything that requires a clean, easily navigable and customizable presentation.'
+++

[https://rust-lang.github.io/mdBook/format/markdown.html](https://rust-lang.github.io/mdBook/format/markdown.html)


```sh
# install
cargo install mdbook

# tools
## init --theme --title --ignore --force
mdbook init
mdbook init path/to/book

## The build command is used to render your book --open --dest-dir
mdbook build
mdbook build path/to/book

## The watch command is useful when you want your book to be rendered on every file change.
mdbook watch path/to/book

## The serve command is used to preview a book by serving it via HTTP at localhost:3000 by default:
mdbook serve
mdbook serve path/to/book

## mdBook supports a test command that will run all available tests in a book
mdbook test path/to/book

## The clean command is used to delete the generated book and any other build artifacts.
mdbook clean
mdbook clean path/to/book
mdbook clean --dest-dir=path/to/book 
```


## Hiding code lines

&#x60;&#x60;&#x60;rust \
&#x23; fn main() { \
    let x = 5; \
    let y = 6; \
 \
    println!("{}", x + y); \
&#x23; } \
&#x60;&#x60;&#x60;


&#x60;&#x60;&#x60;java,hidelines=// \
public int getId(){ \
    //this line will hide \
    return this.id; \
} \
&#x60;&#x60;&#x60;

&#x60;&#x60;&#x60;python,hidelines=~ \
~hidden() \
nothidden(): \
~    hidden() \
    ~hidden() \
    nothidden() \
&#x60;&#x60;&#x60;

&#x60;&#x60;&#x60;python,hidelines=!!! \
!!!hidden() \
nothidden(): \
!!!    hidden() \
    !!!hidden() \
    nothidden() \
&#x60;&#x60;&#x60;

## Rust Playground

&#x60;&#x60;&#x60;rust,playground \
#![allow(unused)] \
fn main() { \
println!("Hello, World!"); \
} \
&#x60;&#x60;&#x60;

&#x60;&#x60;&#x60;rust,noplayground \
let mut name = String::new(); \
std::io::stdin().read_line(&mut name).expect("failed to read line"); \
println!("Hello {}!", name); \
&#x60;&#x60;&#x60;

## Rust code block attributes

&#x60;&#x60;&#x60;rust,ignore \
&#x23; This example won't be tested. \
panic!("oops!"); \
&#x60;&#x60;&#x60;

## Including files

&#x60;&#x60;&#x60;rust \
{{#include file.rs}} \
&#x60;&#x60;&#x60;

&#x60;&#x60;&#x60;java \
{{#include ../../../hello.java}} \
&#x60;&#x60;&#x60;

### Including portions of a file

&#x60;&#x60;&#x60;rust \
{{#include file.rs:2}} \
{{#include file.rs::10}} \
{{#include file.rs:2:}} \
{{#include file.rs:2:10}} \
&#x60;&#x60;&#x60;



## book.toml

```toml
[output.html.code.hidelines]
java = "//"
python = "~"
[output.html.playground]
runnable = false
```