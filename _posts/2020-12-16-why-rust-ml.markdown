---
layout: post
title:  "Why is Rust a Good Language of ML?"
date:   2020-12-15 09:12:17 +0330
categories: rust
---
Languages like C and C++ are low-level systems programming languages which are used for, well, low-level programming. Memory management in C is manual (although not as *hard* as some *programming memes* want you to believe) and you have to keep the number of objects in heap under a tight visor (remember we talked about stack and heap in 6502 in one of the previous posts? Well, heap is where the the *objects* are located). In C/C++ we have *refrences* and *pointers*. In 6502 Assembly we talked about memory addresses. In these languages, a pointer is just a variable which *points* to a memory address and references refer to other variables. A reference to a pointer? Uh, let's not trigger a meme now! (I don't *hate* programming memes, it's just that rarely funny and they're very repetitive). All these are reasons that low-level languages are not good for machine learning. It's too complex, performant, but complex. 

On the other hand, we have high-level langauges such as Python, JavaScript, and Java. In these langagues, the memory is managed for the programmer through *garbage collection*. In GC, objects in the heap that are no longer used by the program are invalidated by the collector. When not done correctly, it may lead to *memory leak*. For example, the Python library *pandas* does it more than actual pandas don't do mating. It usually hapens when the library is not fully Pythonic and uses Python's C API. GC strategies are not perfect, and data requires to be stored in massive objects which occupy a large set of memory addresses in the heap. I'm not saying Python is not good for data science, I myself use Python and also, I'm currently writing a book which implements low-level ML and DNN algorithms in Python (add me to Discord so I can add you to the server in which I post the drafts, Chubak#7400). However, Python's automatic memory management is not *perfect*.

Now, ladies and gentleman, Rust. A language that's *somehow* low-level and *somewhat* high-level. 

Rust doesn't have garbage collection. None, nada, zilch. It achieves memory management by assocating an invariant *type* to objects. These invariant types prevent the objects in heap from racing each other in concurrent systems as it prohibits concurrent reads and writes to a single memory location. In Rust, every memory address is guaranteed to either have:

* 1 mutable reference and 0 immutable reference to it, or
* 0 mutable reference and n immutable references to it.

This invariant is not enough for memory safety though. But in tandem with owneship rules, we get total memory safety. In Rust, Ownership rules state that:

* each value in Rust has a variable that's called it's *owner*.
* There can only be one owner at a time
* When the owner goes out of scope, the value will be dropped

When we wanna use a variable outside its scope, we either have to *copy*, *move* or *borrow* it. Each object in Rust has a *trait* which defines these operations. Keep in mind that **Rust doesn't have classes**, and thusly, Rust doesn't have polymorphism. There are ways to emulate polymorphism in Rust, but ***why***? 

We should use Rust's unique syntax to come up with new patterns, new ways of doing things. I am fully against people trying to hack Rust into doing things you can do by default in C# or Java, as if it's Rust's shortcoming that it's a revolutionary language!

As we said, objects are stored in heap, and in ML, we can represent data through objects with specific traits. Rust's ability to micro-manage the memory with a high-level interface, and it's superb concurrency libraries, makes it an awesome tool for ML. But let's not stop there. There are high level features such as type inference so we will not need to write types for all variables. So your fear of "moooom I hate static typing!" will vanish in a second.

Since I wanna make a series of *ML in Rust* posts, what I'm gonna do is explaining Rust's basic features in a consice manner.

## Variables in Rust

Defining a variable in Rust is very easy:

```
let x  = "Hello world!";
println!("{}", x);
```

Here, `println` is not a function but a macro. Macros are denoted by ending in !. They are special metaprogramming constructs. 

Seeing the type of the variable is kinda verbose. 

```
#![feature(core_intrinsics)]


fn print_type_of<T>(_: &T) {

    println!("{}", unsafe {std::intrinsics::type_name::<T>()});

}

fn main() {

    let x = "Know my type, bitch!";

    println!("{}", x)
    print_type_of(&x);


}

```

Here `<T>` dentoes a *generic type* and `&T` denotes *reference* to that generic type.


# Mutation

When variables are created using `let` keyword, they are immutable. This is done due to typesafety. Keep ind mind that **typesafety is different from dynamic/static typing**. Typesafety is the extent to which a programming languages prevents type errors. A langauge being statically or dynamically typed plays a considerable part in it.

If we wanna make a variable mutable, we simply use the `mut` keyword:

```
let mut x = "Burritos!"

```

# Variable scoping

We explained that Rust has ownership rules and scopes are a part of this rule. To explain the scoping, get an online Rust compiler and run this code:

```
fn main() {
    let x = 5;

    if 4 < 10 {
        let x  = 10;
        println!("Inside if x = {:?}", x);
    }

    println!(Outside if x = {:?}, x);
}
```
# Data types

We saw that, like Kotlin, Rust blurs the lines between static and dynamic typing. Like Remi "Thirteen" Hadely, it goes both ways: you can initialize a variable without defining its type and it will infer it (called type inference) or you can define the type statically.

Rust has all the normal types:

* bool
* char
* signed integers: i8, i16, i32, i64
* unsigned integers: u8 etc
* floats: f32 and f64
* array, a fixed-size segment of memory
* slice: a dynamically sized view into a continouous sequence
* str: string slices
* tupe: a finite heterogenous sequence

```
let x: u64 = 12
```

## Functions

They are defined using the `fn` keyword. Function signatures in Rust are important. Their types can be generic.

```
fn square_of(x: i32) -> i32 {
    x * x
}
```
-> denotes the return type. The value after colon denotes the input parameter type. Notice that we didn't add a semicolon when we wanted to return x to the power of two. Because in Rust, expressions aren't entailed with semicolons. Any expression at the end of a function's block dentoes its return value.

## Conditionals

We have two ways of doing conditionals in Rust, a traditional `if` and pattern matching. We won't insult your intelligence by showing you how to do `if-else` statements. However, pattern matching neeeds to be brought up.

Pattenr matching is defined using the `match` keyword. They are the `switch` statements of Rust (something which modern hobbyist game designers should learn about! Looking at you, Toby Fox.) A match could be a function or an expression.

```
fn main() {
    let virus = "sars-cov-19";

    let infection = match virus {
        "cold" => "sniffles",
        "influenza" => "flu",
        "sars-cov-19" => "COVID-19",
        _ => "healthy",


    };


}

```

The default case `_` should always be present.


# References and Borrowing: str vs. String

We explained reference at the beginning, and borrowing as one of the ways to transfer ownership. Referencing is done with the `&` syntax. 

```
fn main() {
    let lang = "Rust";
    let rust1 = add_version(&lang);
    println!("{:?}, rust1)
}

fn add_version(s: &str) -> String {
    s.to_string() + "2018"
}
```
`String`s are mutable strings, they're closer to Java's string datatype. However, `str`s should be used as function parameters because they are immutable and can only be accessed by *referring* and *borrowing* which are similar operations. `str`s can live in the stack, the heap, or anyo ther place in the memory. However, `String`s are objects that can only live in the heap.

References can be mutable, as in `&mut var`. When you do that, you can mess with the variable it's refering to indirectly.

## Structures and Traits

Structures are essentially named tuples. They are syntactically not so different from C's structs. And just like C, that, and Traits, are all you're gonna get in terms of OOP. Traits are like Java's *interfaces* where you define a bunch of functions with their signatures and you later get to define them with `impl` keyword.


```

struct Planet {
    co2: f32,
    nitrogen: f32
}

struct Atmosphere {

    fn new(co2: f32, nitrogen: f32) -> Self;
    fn summarize(&self);

}


impl Atmosphere for Planet {

    fn new(co2: f32, nitrogen: f32) -> Planet {
        Planet {co2: co2, nitrogen: nitrogen}
    }

    fn summarize(&self) {

        println!("CO2: {}, Nitrogen: {}", self.co2, self.nitrogen);

    }

}

```

It's very easy and straightforward from here on out.

```
let mars = Planet {co2: 95.2, nitrogen: 2.7};

mars.summarize"();

```

## Enumerations

Enums are an interesting way to create a type that will hold of the values of few different variants. For example, one of the types of IP addresses, IPv4 or IPv6, but not both.

```
enum IP {
    IPv4,
    IPv6,
}


let my_ip = IP::IPv6;

```



Well, that's about it for now! I hope you've had a good time. **I'm writing an e-codex, call it a book**. If you wanna read new drafts, please follow me on Discord: Chubak#7400 and I will add you to the server.

Thank you!