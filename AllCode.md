# 4th Key concepts
## Shared XOR Mutable

```rust
fn main(){
    let mut data = String::from("Rust");

    let r1 = &data; // ยืมแบบอ่าน
    let r2 = &data; // ยืมแบบอ่านตัวที่สอง (ทำได้)
    
    let r3 = &mut data; // ยืมแบบแก้ไข (Error: ทำไม่ได้เพราะ r1, r2 ยังใช้งานอยู่)
    
    println!("Read: {} and {}", r1, r2);
}
```

## Non-Lexical Lifetimes (NLL)

```rust
fn main() {
    let mut x = 10;

    let y = &mut x; // เริ่มยืมแบบแก้ไข
    *y += 5;        // y ถูกใช้งานครั้งสุดท้ายที่บรรทัดนี้

    // ด้วย NLL คอมไพเลอร์รู้ทันทีว่า y จบหน้าที่แล้ว
    // จึงอนุญาตให้ z เข้ามายืม x ต่อได้โดยไม่ต้องรอให้จบปีกกา
    let z = &x; 
    println!("x is now: {}", z); 
}
```

## Explicit Lifetime Annotation

```rust
fn longest<'a>(x: &'a str, y: &'a str) -> &'a str {
    if x.len() > y.len() {
        x
    } else {
        y
    }
}
```

# 6th Runnable Code Example
## 1st Example

```rust
fn main(){
    let x = 5;
    let a;
        {
            let y = 6;
            a = &y;
            print!("{}",a);
        }
    //println!("{} {}",x,a); **ถ้าเรียกข้างนอกขอบเขตอายุไขของ y จะหมดก่อน**
}
```

## 2nd Example

```Rust
fn higher<'a>(x: &'a i32, y: &'a i32) -> &'a i32 {
    if x > y{ 
        x
    }else{
        y
    }
}

fn main() {
    let x = 30;
    let result;
    {
        let y = 10;
        result = higher(&x,&y); // ใช้ได้ใน scope นี้ 
        println!("The higher value : {}", result);
    }
    //println!("The higher value : {}", result);
}
```

## 3rd Example

```rust
struct Card<'a> {
    name: &'a str,
}

impl<'a> Card<'a> {
    fn show(&self) -> &str {
        self.name  // return reference ที่มี lifetime เดียวกับ &self
    }
}

fn main() {
    let i;
    {
    let s = "FixHe4Rt".to_string();
    let username = &s;
    i = Card { name: username };
    println!("Name : {}",i.show());
    }
    //println!("Name : {}",i.show());
}
```
# 7th Common Mistake
## Dangling Pointer
### Wrong

```rust
fn dangle() -> &String 
            {
                let s = String::from("hi");
                &s
            }// s หมด scope ตรงนี้ ค่า "hi" จะถูก drop ทิ้งเพื่อคืนพื้นที่หน่วยความจำ

            fn main()
            {
                let a = dangle(); //อิงถึง s แล้วไม่เจอค่าเพราะถูก drop ไปแล้ว ทำให้ Complie ไม่ผ่าน
                println!("{}", a);
            }
```

### Correct

```rust
fn no_dangle() -> String 
{
    let s = String::from("hi");
    s
}//ส่งออก Ownership "hi" ให้ตัวแปรที่เรียก function

fn main()
{
    let a = no_dangle(); //a เป็นเจ้าของค่า "hi" แทน s แล้ว
    println!("{}", a); 
}
```

## Lifetime Specifier
### Wrong

```rust
struct Highlight
{
    text: &str,   // จะ Compile ไม่ผ่านตั้งแต่ตรงนี้
}

fn main() 
{
    let content = String::from("Rust is fun");
    let h = Highlight { text: &content };
    println!("{}", h.text);
}
```

### Correct

```rust
struct Highlight<'a>
{
    text: &'a str, //มีการเพิ่ม 'a เข้ามา
}

fn main() 
{
    let content = String::from("Rust is fun");
    let h = Highlight { text: &content };
    println!("{}", h.text);
}
```

# 8th Exercise
## 1st Exercise
ให้เขียนโปรแกรมที่ประกาศตัวแปรข้อความว่า "I'm in the block" ไว้หนึ่งตัวภายใน block { } จากนั้นนำค่าของตัวแปรนั้นไปพิมพ์ภายนอก block ให้ได้
### Solution

```rust
fn main() 
{
    let outer;
    {
        let inner = String::from("I'm in the block"); // inner เป็นเจ้าของ "I'm in the block" อยู่
        outer = inner;   // ย้าย ownership ของ "I'm in the block" ให้ outer ซึ่งถูกประกาศอยู่ main
    }
    println!("{}", outer);
}
```

## 2nd Exercise
ให้เขียนฟังก์ชันชื่อ longer ที่รับข้อความ 2 ค่าเข้ามาผ่านการอ้างอิง (reference) แล้วคืนค่าข้อความที่ยาวกว่าออกไป (ถ้ายาวเท่ากันคืนค่าไหนก็ได้) จากนั้นเขียน main ที่เรียกใช้ฟังก์ชันนี้และแสดงผลให้ถูกต้อง
### Solution

```rust
fn longer<'a>(x: &'a str, y: &'a str) -> &'a str //กำหนด 'a เพื่อให้ rust เช็คอายุของทั้ง 2 string
{
    if x.len() > y.len()
    {
        x
    }
    else
    {
        y
    }
}

fn main()
{
    let a = String::from("Pine");
    let b = String::from("Apple");
    let result = longer(&a, &b); //โยน reference เข้าไป
    println!("ยาวกว่าคือ: {}", result);
}
```

# 9th PPL Prespective
## 9.2 Semantics

```rust
fn main() {
    let mut number: i32 = 1;
    number += 2;
    println!("{}", number);
}
```

## 9.4 Memory / Resource management

```rust
fn main() {
    let number = Box::new(10);
    println!("{}", number);
    drop(number);
}
```

# 10th Rust Vs. Other PL
## Language Example
### Rust

```rust
fn main() {
    let mut number: i32 = 1;
    number += 2;
    println!("{}", number);
}
```

### Go

```go
package main

            import "fmt"

            func main() {
                number := 1
                number += 2
                fmt.Println(number)
            }
```

### Zig

```zig
const std = @import("std");

pub fn main() !void {
    var number: i32 = 1;
    number += 2;
    try std.io.getStdOut().writer().print("{d}\n", .{number});
}
```

### Swift

```swift
var number: Int = 1
    number += 2
    print(number)
```

---