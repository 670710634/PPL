# 10. Rust vs. Other Languages

เลือกเปรียบเทียบ Rust กับ **C, C++ และ Python** เนื่องจากทั้งสามภาษาแสดงแนวคิดด้าน Syntax, Type System และ Memory Management ที่แตกต่างกันอย่างชัดเจน

## 10.1 Comparison Table

| Aspect | Rust | C | C++ | Python |
|---|---|---|---|---|
| Syntax | ใช้ `let`, `mut`, `fn` และ Macro เช่น `println!` | ใช้ฟังก์ชันและคำสั่งแบบ C เช่น `printf` | ใช้ Class, Function และ Template | ใช้ Syntax ที่สั้นและไม่ต้องใส่ `;` ท้ายคำสั่ง |
| Semantics / Behavior | ตรวจสอบ Ownership และ Borrowing ตอน Compile | ทำงานตามคำสั่งและให้ผู้เขียนจัดการ Pointer เอง | รองรับ Object-oriented และจัดการ Pointer ได้ | ทำงานแบบ Dynamic และตรวจชนิดข้อมูลส่วนใหญ่ตอน Runtime |
| Type System | Static และ Strong typing พร้อม Type inference | Static typing แต่เปิดให้แปลงชนิดข้อมูลได้ค่อนข้างยืดหยุ่น | Static typing และรองรับหลายรูปแบบ เช่น Template | Dynamic typing และไม่ต้องประกาศชนิดตัวแปรล่วงหน้า |
| Memory Management | จัดการอัตโนมัติด้วย Ownership โดยทั่วไปไม่ใช้ Garbage Collector | ต้องจัดการด้วย `malloc()` และ `free()` เอง | ใช้ทั้งการจัดการเองและ Smart Pointer เช่น `unique_ptr` | ใช้ Garbage Collector และการนับ Reference |
| Safety | ป้องกันหลายปัญหาตั้งแต่ Compile เช่น Use-after-free และ Data Race | ผู้เขียนต้องระวัง Pointer และ Memory เอง | ปลอดภัยกว่า C บางส่วน แต่ยังเกิด Memory Error ได้ | ปลอดภัยด้าน Memory มากกว่า C/C++ แต่มี Runtime Error ได้ |
| Performance | สูงและใกล้เคียง C/C++ | สูงและควบคุมเครื่องได้โดยตรง | สูงและมี Abstraction ได้หลากหลาย | โดยทั่วไปช้ากว่าเพราะเป็นภาษาระดับสูงและทำงานผ่าน Runtime |

## 10.2 Rust Example

```rust
fn main() {
	let mut number: i32 = 1;
	number += 2;
	println!("{}", number);
}
```

## 10.3 C Example

```c
#include <stdio.h>

int main() {
	int number = 1;
	number += 2;
	printf("%d\n", number);
	return 0;
}
```

## 10.4 C++ Example

```cpp
#include <iostream>

int main() {
	int number = 1;
	number += 2;
	std::cout << number << std::endl;
	return 0;
}
```

## 10.5 Python Example

```python
number = 1
number += 2
print(number)
```

## 10.6 Analysis

Rust, C และ C++ เป็นภาษา Static typing ที่เหมาะกับงานซึ่งต้องการ Performance สูง แต่ Rust แตกต่างจาก C และ C++ ตรงที่ใช้ Ownership, Borrowing และ Lifetime ตรวจสอบการจัดการหน่วยความจำตั้งแต่ตอน Compile จึงลดปัญหา Memory Error ได้โดยไม่ต้องใช้ Garbage Collector

Python มี Syntax ที่สั้นและเขียนง่ายกว่า แต่เป็น Dynamic typing จึงตรวจสอบชนิดข้อมูลส่วนใหญ่ตอน Runtime และใช้ Garbage Collector ช่วยจัดการหน่วยความจำ ทำให้พัฒนาได้รวดเร็ว แต่โดยทั่วไปควบคุมหน่วยความจำและ Performance ได้น้อยกว่า Rust

ดังนั้น Rust เป็นจุดกึ่งกลางระหว่างความเร็วและการควบคุมแบบ C/C++ กับความปลอดภัยของ Memory โดยมีระบบตรวจสอบจาก Compiler ช่วยลดข้อผิดพลาดก่อนนำโปรแกรมไปใช้งานจริง

---
