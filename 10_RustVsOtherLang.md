# 10. Rust vs. Other Languages

เลือกเปรียบเทียบ Rust กับ **Go, Zig และ Swift** เนื่องจากแต่ละภาษามีลักษณะคล้าย Rust ในบางด้าน เช่น Static typing, Performance หรือการพัฒนาโปรแกรมระบบ แต่ไม่จำเป็นต้องเหมือนกันทุกหัวข้อ โดยเฉพาะแนวทางการจัดการหน่วยความจำ

## 10.1 Comparison Table

| Aspect | Rust | Go | Zig | Swift |
|---|---|---|---|---|
| Syntax | ใช้ `let`, `mut`, `fn` และ Macro เช่น `println!` | ใช้ `func`, `:=` และคำสั่ง `go` สำหรับ Concurrency | ใช้ Syntax เรียบง่าย เน้น Explicit control และมี `defer` | ใช้ `let`, `var`, Function และ Property พร้อม Syntax ที่อ่านง่าย |
| Semantics / Behavior | ตรวจสอบ Ownership และ Borrowing ตอน Compile | ใช้ Goroutine และ Channel พร้อม Garbage Collector | ควบคุมการทำงานใกล้ Hardware และจัดการ Error แบบ Explicit | ใช้ Value semantics และตรวจสอบ Memory ด้วย ARC |
| Type System | Static และ Strong typing พร้อม Type inference | Static typing พร้อม Type inference และ Interface | Static typing พร้อม Compile-time checking และ Optional | Static และ Strong typing พร้อม Type inference และ Protocol |
| Memory Management | จัดการอัตโนมัติด้วย Ownership โดยทั่วไปไม่ใช้ Garbage Collector | ใช้ Garbage Collector จัดการหน่วยความจำ | ให้ผู้พัฒนาควบคุมหน่วยความจำอย่างชัดเจน โดยไม่มี Garbage Collector | ใช้ Automatic Reference Counting (ARC) |
| Safety | ป้องกันหลายปัญหาตั้งแต่ Compile เช่น Use-after-free และ Data Race | มี Memory Safety จาก Garbage Collector แต่ตรวจบางปัญหาตอน Runtime | มีเครื่องมือช่วยตรวจสอบ แต่ผู้พัฒนายังต้องระวัง Pointer | ลดปัญหา Memory ด้วย ARC และตรวจสอบชนิดข้อมูลตอน Compile |
| Performance | สูงและเหมาะกับงานระบบ | สูงและเหมาะกับงาน Concurrent Server | สูงและควบคุมทรัพยากรได้ใกล้เคียง C | สูง เหมาะกับงาน Application และระบบของ Apple |

## 10.2 Rust Example

```rust
fn main() {
	let mut number: i32 = 1;
	number += 2;
	println!("{}", number);
}
```

## 10.3 Go Example

```go
package main

import "fmt"

func main() {
	number := 1
	number += 2
	fmt.Println(number)
}
```

## 10.4 Zig Example

```zig
const std = @import("std");

pub fn main() !void {
	var number: i32 = 1;
	number += 2;
	try std.io.getStdOut().writer().print("{d}\n", .{number});
}
```

## 10.5 Swift Example

```swift
var number: Int = 1
number += 2
print(number)
```

## 10.6 Analysis

Rust, Go, Zig และ Swift มีบางลักษณะที่คล้ายกัน เช่น เป็นภาษา Static typing และเหมาะกับงานที่ต้องการ Performance แต่ไม่ได้เหมือนกันทุกด้าน โดย Go เน้นการพัฒนา Concurrent Application ที่ง่ายขึ้นด้วย Goroutine และ Garbage Collector ส่วน Zig เน้นการควบคุมทรัพยากรและความเรียบง่ายใกล้เคียงภาษาระดับระบบ ขณะที่ Swift ใช้ ARC เพื่อช่วยจัดการหน่วยความจำและมีระบบชนิดข้อมูลที่ปลอดภัย

แม้ทั้งสามภาษาจะมีลักษณะคล้าย Rust แต่แนวทางด้าน Memory Safety แตกต่างกัน Rust ใช้ Ownership, Borrowing และ Lifetime ตรวจสอบการจัดการหน่วยความจำตั้งแต่ตอน Compile โดยไม่ต้องใช้ Garbage Collector ขณะที่ Go ใช้ Garbage Collector, Zig ให้ผู้พัฒนาควบคุมหน่วยความจำเอง และ Swift ใช้ ARC

ดังนั้น Rust จึงโดดเด่นด้วยการผสาน Performance ระดับภาษา System Programming เข้ากับ Memory Safety ที่ตรวจสอบได้ตั้งแต่ Compile ทำให้เหมาะกับงานที่ต้องการทั้งความเร็วและความน่าเชื่อถือ

---
