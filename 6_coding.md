## 6. Runnable Code Examples

> **ข้อกำหนด:** Code ทุกตัวต้อง Compile และ Run ได้จริงก่อนนำมาใส่ในเอกสาร

### Example 1 — `การเรียกใช้ตัวแปรที่หมดอายุเเล้ว`

**Purpose:** `ให้ดูว่า lifetime ในขอบเขตมีระยะเวลาเท่าใด`

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

**Expected Output**

ได้ค่า 6 

**Explanation**

เริ่มมาเราจะสร้างเเละกำหนดค่าของ x เป็น 5 จากนั้นสร้างตัวแปร a ขึ้นมาหลังจากนั้นเราจะกำหนดขอบเขตขึ้นมาเเล้วสร้างเเละกำหนดค่าของ y ข้างในนั้นพร้อมทั้งนำค่า a ไป Reference ถึงค่า y ก็คือ 6 จากนั้นจะให้เรียก print a ออกมาจะได้ค่าที่เก็บไว้คือ 6(Reference จาก y)

---

### Example 2 — `lifetime ของ Function`

**Purpose:** `ระยะเวลาเเละชื่อของ life time ของ Function`

```rust
fn higher<'a>(x: &'a i32, y: &'a i32) -> &'a i32 {
    if x > y{ 
        x
    }else{
        y
    }
}

fn main() {
    let X = 30;
    let result;
    {
        let Y = 10;
        result = higher(&X,&Y); // ใช้ได้ใน scope นี้ 
        println!("The higher value : {}", result);
    }
    //println!("The higher value : {}", result);
}
```

**Expected Output**

```text
The higher value : 30
```

**Explanation**

เราจะสร้าง function higher ขึ้นมาก่อนเพื่อเช็คว่าค่าไหนมากกว่ากันตัว function ก็จะรับ parameter มา 2 ตัวคือ x,y จากนั้นก็เทียบค่าว่าค่าใดมากกว่าเเละส่งค่าที่มากกว่าออกไป
ใน function main เราจะกดหนดค่า x = 30 เเละสร้างตัวแปล result ไว้ จากในนั้นเราจะกำหนดขอบเขตขึ้นมาเเละสร้างตัวแปล Y ขึ้นมาให้มีค่า 10 จากนั้นเราจะทำการกำหนดให้ result เก็บค่าที่เรียกใช้ function higher มาหลังจากนั้นให้ print ค่า result จะได้ค่าที่มากกว่าเป็นคำตอบ
---
### Example 3 — `lifetime ของ struct`

**Purpose:** `[ต้องการสาธิตอะไร]`

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
    let i
    {
    let S = "FixHe4Rt".to_string();
    let username = &S;
    i = Card { name: username };
    println!("Name : {}",i.show());
    }
    //println!("Name : {}",i.show());
}

```

**Expected Output**

```text
Name : FixHe4Rt
```

**Explanation**

เริ่มจากการที่เราจะสร้าง struct ชื่อ Card ขึ้นมาพร้อมตั้งชื่อ lifetime ไว้ให้ด้วยเเละในนั้นจะมี field ชื่อ name จากนั้นเราจะสร้าง impl block เพื่อเพิ่ม method ให้กับ Card พร้อมตั้งชื่อ lifetime เดียวกันกับ struct เเละข้างในจะมี method ชื่อ show เเละมี self คือตัวแทนของ instance ที่เรียก method นั้นอยู่ เเละให้ output ออกมาเป็น self.name หรือก็คือ name ที่เก็บไว้ใน struct เเละ ใน Function main เราจะกำหนดค่า s เป็น "FixHe4Rt" เเละ username คือตัวที่ Reference ถึง s เเละ สร้าง i เป็น Card ที่เก็บ name เป็น username เเละเรียกใช้ print i.show() ซึ่งRust จะส่ง i เข้าไปเป็น self โดยอัตโนมัติ ฟังก์ชันเลยเข้าถึง self.ืname ได้ จึงได้ค่าออกมาเป็นค่าที่เก็บไว้ใน name ของ Card ถ้าในส่วนที่คอมเม้นอยู่นำไปใช้งานเเทนจะทำให้เปิด error เพราะ lifetime ของ ตัวที่เป็น field name หมดไปเเล้วทำให้เรียกใช้ไม่ได้

---