## 7. Common Mistakes

### Mistake 1 — `[Dangling Pointer]`

**Problem**

`[เผลอส่งออกค่า Reference ของตัวที่มี Lifetime แค่ใน Function]`

**Incorrect Code**

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

**Correct Code**

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

**Why?**

`[เนื่องจากใน Code ที่ผิดเป็นการเข้าถึงค่า Reference ซึ่งใน Rust หากตัว Owner หลุด Scope ไปแล้วค่าที่ตัว Owner own อยู่จะถูก Drop ทิ้งเพื่อคืนพื้นที่หน่วยความจำ ทำให้เมื่อจบ dangle() ตัวแปร s ซึ่งเป็น Owner ของค่า "hi" นั้นหลุด scope ไปแล้ว โปรแกรมจึง drop ค่า "hi" ทิ้ง เมื่อ a พยายามอิงถึงค่าภายใน s จึงมองไม่เห็นอะไรทำให้ compile ไม่ผ่าน แต่ใน code ที่ถูกต้อง no_dangle() ส่งออก Ownership แทนการ reference เฉยๆ ทำให้ a เป็น Owner ของ "hi" แทน s จึงสามารถใช้งานได้ปกติ]`

---

### Mistake 2 — `[Lifetime Specifier กับ Struct]`

**Problem**

`[ไม่ได้กำหนด Lifetime specifier ให้ Struct ที่เก็บ Reference]`

**Incorrect Code**

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

**Correct Code**

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

**Why?**

`[เนื่องจาก Struct ที่เก็บค่าโดยการ Reference บางที Rust ไม่สามารถทราบได้ว่าตัวแปรที่ถูกอิงถึงนั้นจะมีอายุอยู่พอสำหรับตลอดการเรียกใช้งานของ Struct หรือไม่ เพื่อป้องกัน Dangling pointers Rust จึงบังคับให้ประกาศ Lifetime specifier ('a ในที่นี้แทนความยาวของ Lifetime หนึ่งที่ชื่อ a) เป็นตัวช่วยในตอน Compile เพื่อยืนยันว่า Lifetime ของตัวที่ถูกอิงถึงจะมีอายุยืนพอตลอดระยะเวลาที่สามารถเรียกใช้งาน Struct ได้ โดยถ้าตรวจแล้วว่า Owner อาจตายก่อน Code จะไม่ผ่านตั้งแต่ตอน Compile]`

---