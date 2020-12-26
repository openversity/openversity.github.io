---
exclude: true
layout: default
title: Ownership
parent: The Rust Workshop
nav_order: 4
---

exclude: true
1. TOC
{:toc}

---
name: agenda
## Agenda
* **Ownership**
* **References & Borrowing**
* **Slices**
* **Memory Layout**

---
## Ownership of Heap Memory
**There can only be one owner at a time**
* No explicite memory management is required;
* No garbage collecor involved
* Memory is manged through system of ownership with rules checked at compile time.
  * _Each value in Rust has a variable that's called its owner_
  * _There can only be one owner at a time_
  * _When the owner goes out of scope, the value will be dropped_
* Object memory is automatically freed when the object goes out of scope;
    ```rust
    {
        let s1 = String::from("welcome")
    }
    ```
* Memory for `s1` will get freed automatically at `'}'`
* Rust calls a special function called `drop` for each object going out of scope

> In C++, this techniq is called __RAII__

---
## Transfer of Ownership
* Ownership is moved or transfered when you assign a heap object to another variable
    ```rust
    {
        let s1 = String::from("WELCOME");
        let msg = s1; // data ownership is moved from s1 to msg
        println!("Message:{}", s1); //This line will not compile since s1 is invalid
    }
    ```
* object `s1` is no longer exist and ownership has been moved to `msg`
* Memory free is done only once for `msg`; Since owner `s1` is no longer exist, no double free for `s1`

---
## Ownership and Data copy  
* Rust never deep copies the objects; It only transfer the data ownership
> _No hidden performance impacts of data copies_

* We can `clone()` the obejct to do the deep copy
    ```rust
    {
        let s1 = String::from("WELCOME");
        let msg = s1.clone(); // data is copied to msg
        println!("Message:{}", s1); //This line is ok since s1 has ownership to its own copy
    }
    ```
* We can also implement `copy` trait on user defined types to explicitely tell the rust to deep copy when we assign or pass an object to a function.

* Any object whose datatype has `copy` trait implemnted is automatically copied instead of transfering ownership.
* Scalar type values are also copied since they are allocated on stack and there is no difference between shalow copy and deep copy. 

---
## Ownership and Functions
* Passing an object to a function moves ownership into function parameter
* Simalary the ownership is moved out of the function to the caller when the function returns an object.
    ```rust
    fn main() {
        let msg = String::from("Welcome");        
        display(msg); //ownership of msg is moved into display
        println!("Message :{}", msg); //This line will not compile
    
        //ownership of return value is moved to variable banner
        let banner = prepare_banner();
        println!("Banner :{}", banner);
    }
    
    fn display(str: String) {
        println!("Message :{}", str);
    }
    
    fn prepare_banner() -> String {
        let str = String::from("HELLO; WELCOME");
        str
    }
    ```

---
## References to borrow the access
References are a way of accessing the data by borrow, i.e. without transfer of ownership.
* Reference to a variable/object is denoted using `&`
    ```rust
    let msg = String::from("WELCOME");
    display(&msg); //Only reference is passed; Ownership not moved
    println!("Message:{}", msg); //valid as the msg has still ownership to its data
    
    fn diaply(s: &String) {
        println!("message:{}", s); // s is borrowing data from msg; so memory is not freed here
    }
    ```

---
## References to modify 
* References to the data are immutable by default
* We can use mutable reference to borrow the access to data as well as modify
    ```rust
    let msg = String::from("WELCOME");
    display(&msg); //Only reference is passed; Ownership not moved
    println!("Message:{}", msg); //valid as the msg has still ownership to its data
    
    fn diaply(s: &String) {
        println!("message:{}", s);
        s.push_str("TO RUST!"); //Modify the data owned by msg
    } //memory for 's' is not freed here
    ```

---
## References and race condtions
* We can have only one mutable reference in the given scope.
    ```rust
    {
        let msg = String::from("WELCOME");
        let s1 = &mut msg; 
        let s2 = &mut msg; //This line will fail
    }
    ```
* In a given scope, we cannot have both mutable and immutable references to an object.
    ```rust
    {
        let msg = String::from("WELCOME");
        let s1 = &msg; //read reference
        let s2 = &mut msg; //write reference; this will fail
        println!("message:{}", s1); //Cannot access s1
    }
    ```

* Scope of a reference ends after its last usage
    ```rust
    {
        let msg = String::from("WELCOME");
        let s1 = &msg; //read reference
        println!("message:{}", s1); //This is OK
        let s2 = &mut msg;
    }
    ```

---
## Dangling References


---
##Summary