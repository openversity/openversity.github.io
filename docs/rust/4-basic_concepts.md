---
exclude: true
layout: default
title: Basic Concepts
parent: The Rust Workshop
nav_order: 4
---

exclude: true
1. TOC
{:toc}

---

name: title
class: middle, center
# Basic Concepts

---
name: agenda
## Agenda
* **Data Types and Variables**

* **Mutability & Shadowing**

* **Constants and Literals**

* **Compound & User Defined types**

* **Functions**

* **Control Flows**

* **Comments**

---
## Data Types & Variables
Rust is a _statically typed_ language; Variables must be declared at compile time with data type
* Scalar types
    * **Signed Integers** : `i8, i16, i32, i64, i128, isize`
    * **Un-Signed Integers** : `u8, u16, u32, u64, u128, usize`
    * **floats** : `f16, f32`
    * **Boolean** : `bool`
    * **Characters** : `char`

* Compound types: Tuples, Arrays, Structs and Enums

* Standard library types like `String`

**_Variables_** are declared using `let` keyword; Data type can be explicitely specified or inferred from assigned value.
```rust
let token_start = "100"; //Inferred type as String
let token_number:u32 = token_number.parse().expect("Not a number");

let next_token = token_number; //Inferred type as integer u32
```

---
## Variable are Immutabile
Variables in Rust are immutable by default; one of the safety that Rust offers while we write concurrent code;
* Following code will NOT compile
    ```rust
    fn main() {
        let x = 5;
        println!("The value of x is: {}", x);
        x = 6; //This line will not compile since x is immutable
        println!("The value of x is: {}", x);
    }
    ```
* Mutable variables are declared using `let mut`; Following code will work
    ```rust
    fn main() {
        let mut x = 5;
        println!("The value of x is: {}", x);
        x = 6;
        println!("The value of x is: {}", x);
    }
    ```

---
### Variable Shadowing
Same variable name can be re-declared with in the same scope with different type and value;
* Re-declaration will shadow the previous type and value
* Following code is valid and compiles;
    ```rust
    fn main() {
        let token_number = "100";
        let token_number:u32 = token_number.trim().parse().expect("Parse Failed!");
        let token_number = token_number + 1;
        println!("Next Token: {}", token_number);
    }
    ```
* Every time we use `let` with same name we are shadowing the previous value and data type;
* This is useful when the value needs to be transformed to a different data type like the case above; 
* It is clean to re-use the same variable name instead of creating two variables like `str_token_number` and `token_number`.

---
## Constants
A constant is declared using `const` keyword and must always be annotated with the data type
```rust
const PI: f32 = 3.141;
```
* PI is a float constant and is always immutable.
* A constants can NOT be re-bound to another value or redefined in the same scope
* Constant can be declared in any **scope** including the global scope.
    ```rust
    const PI: f32 = 3.141;
    fn main() {
        println!("PI={}", PI);
    }
    ```
* Constants live thru the entire time the program runs.
* Constant can be only set to a constant expression which can be evaluated at compile time.
 
_Note: Constant names are in uppercase in rust by convention._

---
## Integer literals
Integers can be expressed using any following bases
```rust
let num_dec = 98_222;     //decimal
let num_hex = 0xff;       //hex
let num_oct = 0o77;       //octal
let num_bin = 0b1101;     //binary; rust also supports binary literal
let num_byte:u8 = b`A`    //u8 only bytes
```
**Integer Overflow**
* Debug builds will panic the program in case of integer overflow due to the extra runtime checks added by rust.

* Release builds(`--release`) perform wrapping in case of overflows; For example, storing 256 in u8 integer will wrap around to 0, and 257 will wrap around to 1.

---
## Floats Literals
**Rust has two floating point types**

* `f32` : for single precision float
* `f64` : for double precision float
Example:
```rust
let one_fourth = 0.25; // f64 by default
let three_fourth: f32 = 0.75; // we need to specify explicitely for f32 
```

_Default float type in rust is_: `f64`

---
## Boolean & Character Literals
**Boolean Type: `bool`**
* `bool` type is one byte size
* `true` and `false` are boolean literals
```rust
let is_signaled = false; //default
let is_shutdown: bool = true; // explicit type annotation
```
* All control-flow expressions in rust should result into boolean value

**Character type: `char`**
* Rust `char` type is unicode scalar type and 4 bytes in size
* It can store unicode scalar values ranging from `U+0000` to `U+D7FF` and `U+E000` to `U+10FFFF` inclusive
* `char` literals are specified using single quotes
```rust
let c = 'A';
let z = 'ℤ';
```

---
## Compound & User Defined Types

* Tuple : Group of related values with differet types

* Array : Set of values of same type

* Struct: Used to create Custom Datatype out of other scalar or struct types 

* Enums: Defines possible value variants for a given result

---
### Tuples
* A tuple is a group of related values; values can be of different types
* Tuple is fixed in length and cannot grow or shrink once created
    ```rust
    let readings_today: (i32, f64, bool) = (25, 65.5, false); 
    ```
* Variable `readings_today` is a tuple that represents 3 values
* Values can be accessed using **_destructuring_**
    ```rust
    let (temp, wind, is_rain) = readings_today;
    println!("Temprature today:{}" temp);
    ```

* Values can also be accessed using member index
    ```rust
    let (temp, wind, is_rain) = readings_today;
    println!("Temprature today:{}", readings_today.0);
    println!("Wind speed today:{}", readings_today.1);
    ```

---
### Arrays
* Array is a fixed size collection of values of similar type
    ```rust
    fn main() {
        let days = ["sun", "mon", "tue", "wed", "thu", "fri", "sat"];
        let values = [1, 2, 3, 4, 5, 6, 7];
    }
    ```
* Array is a single chunk of memory on stack
* Elements are accessed using index
    ```rust
    let first  = values[0];
    let second = values[1];
    let last   = values[6];
    ```

* Rust protects from accessing array elements out of bounds and will panic immediately if index is out of range.

* Arrays can not grow in size; We can use standard library collection `Vector` which is allocated on heap and can grow and shrink in size 

---
### Structs
* structs are user defined types
    ```rust
    struct User {
        username: String,
        email: String,
        sign_in_count: u64,
        active: bool,
    }
    
    fn main() {
        let user1 = User {
            email: String::from("someone@example.com"),
            username: String::from("someusername123"),
            active: true,
            sign_in_count: 1,
        };
    }
    ```

---
### Tuple Structs
* Tuple Structs are structs without field names
    ```rust
    fn main() {
        struct Color(u32, u32, u32);
        struct Point(i32, i32, i32);
    
        let black = Color(0, 0, 0);
        let origin = Point(0, 0, 0);
    }
    ```

---
### Enums
Enums are used to define a possible variants/instances for a given type.
```rust
#[allow(dead_code)] //supress wanring since only Saturday is instatiated
enum Day {
    Sunday,
    Monday,
    Tuesday,
    Wednesday,
    Thursday,
    Friday,
    Saturday,
}

fn main() {
    let today = Day::Saturday;
    let day_type = match today {
        Day::Sunday  | Day::Saturday => "WEEK_END",
        
        Day::Monday | Day::Tuesday | Day::Wednesday | 
        Day::Thursday | Day::Friday => "WEEK_DAY",
    };
    println!("Today is a {}", day_type);
}
```

---
## Functions
* Functions are defined in rust using keyword `fn`
* `fn main()` is the entry point of a rust program
* functions can be defined any where in the program
* function body block has statements and expressions
* return type is specified using arrow **`->`**
    ```rust
    fn main() {
        let x = 6;
        let y = 4;
        println!("Sum of {} and {}  = {}", x, ,y, sum(x,y));
    }

    //Function returns 32-bit integer
    fn sum(x: i32, y:i32) -> i32 {
        x + y
    }
    ```
* Function return value is implicite with last expression `x + y`
* To return early from the function we can use explicite `return`

---
## Blocks as Expressions
* In rust, a block itself can be an expression if it has an expression at the end without semicolan
* So the following statement is valid in rust
    ```rust
    let avg = {
        let sum = 700;
        let count = 7; 
        sum / count
    }
    ```

> Above variable `avg` will be set to the result of the block which is `sum / count`

* Even control blocks like `if`, `loop` etc., can be represented as expressions in rust and they can be assigned to a varible
 
---
## Control Flows
* Branching Controls
  * `if`
  * `match`

* Reptetion Controls
    * `loop`
    * `while`
    * `for`

---
### Branching with `if`
* if condition must always be a boolean expression; rust does not convert other types to boolean
    ```rust
    if n % 2 == 0 {
        println!("{} is an even number", n);
    } else {
        println!("{} is an odd number", n)
    }
    ```
* similar to other languages, we can chain with multiple `else if` blocks
* if blocks can also be an expression and can be assinged to a variable
    ```rust
    let days = if year % 100 == 0 {365} else {366} 
    println!("Year:{} has Days:{}", year, days);
    ```

---
### Branching with `match`
* `match` expression is more powerful than `if` with multiple blocks
    ```rust
    let day = 1;
    match day {
        2..=6 => println!("Weekday"), //Inclusive Range match
        1|7 => println!("WeekEnd"),   //Individual Value match
        _ => println!("Invalid day")  //Rest of the cases
    }
    ```

* `match` is NOT similar to `switch` in other languages

* `match` block can be an expression and can be assigned
    ```rust
    let day_type = match day {
        2..=6 => "Weekday",
        1|7 => "WeekEnd",
        _ => "Invalid day",
    };
    println!("Day {} is a {}", day, day_type);
    ```

---
### Repetition using `loop`
* `loop { ... }` can be used to run a block forever until we break
    ```rust
    loop {
        println!("Welcome")
    }
    ```
* A break statement can be used to stop the loop as well as to return a value as loop result
    ```rust
    fn main() {
        let mut n = 0;
        let mut sum = 0;
        sum = loop {
            n += 1;
            sum += n;
            if n >= 10 {
                break sum;
            }
        };
        println!("sum of {} is {}", n ,sum);
    }
    ```

---
### Repetition using `while`
* `while condition { ... }` can be used to repeat the code block until condition becomes false
    ```rust
    fn main() {
        let mut n = 1;
        let mut sum = 0;
        while n <= 10 {
            sum += n;
            n += 1;
        };
        println!("sum of {} is {}", n-1 ,sum);
    }
    ```

---
### Repetition using `for`
* `for` is the safer control to iterate over the elements of a collection like array or vector
    ```rust
    fn main() {
        let a = [10, 20, 30, 40, 50];
    
        for element in a.iter() {
            println!("the value is: {}", element);
        }
    }
    ```
* `for` loop limits the iterations within the index range of the collection and hence it is safer than using `while` with index

* `for` also can be used to iterate over a `Range`, a standard library type
* Following code does countdown from 10; `rev()` specifies to reverse the range from 10 to 1
    ```rust
    fn main() {
        for number in (1..10).rev() {
            println!("{}!", number);
        }
        println!("DONE!");
    }
    ```

---
## Comments
* _Code comments_
    ```rust
        // This is a simple comment
        let counter = 1;
        
        // This is a multiline comment
        // each line should prefix with //
        // token reset to 100
        let mut token = 100;
        
        token += counter; // This is a comment at the end of statement
    ```

* _Documentation Comments - Supports markdown syntax_
    ```rust
    /// **Returns the next Token**
    /// _Example_
    ///    let next = get_token();
    ///    println!("Next token: {}", next);
    pub fn get_token() -> i32 {
        counter += 1;
        counter
    }
    ```
    
---
## Summary
* Rust is statically typed language and variables are immutable by default

* Rust has 4 scalar types: integers, floats, boolean, char

* Rust has 3 compound types: tuple, array and struct

* Rust has `if` and `macth` for branching

* `loop` and `while` are used to repeate statements

* `for` is suitable to iterate over Collecions and Ranges
