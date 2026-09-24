## 8. Exercises

> จัดทำแบบฝึกหัด **2 ข้อ** ที่สอดคล้องกับ Topic และมีระดับความยากเหมาะสม

### Exercise 1 — `Let me out!`

**Problem**

`ให้เขียนโปรแกรมที่ประกาศตัวแปรข้อความว่า "I'm in the block" ไว้หนึ่งตัวภายใน block { } จากนั้นนำค่าของตัวแปรนั้นไปพิมพ์ภายนอก block ให้ได้`

**Hint**

`ต้องทำให้ข้อความภายใน Block มีอายุต่อหลังออกจาก Block ให้ได้`

**Solution**

```rust
fn main() 
{
    let outer;
    {
        let inner = String::from("I'm in the block"); // inner เป็นเจ้า "I'm in the block" อยู่
        outer = inner;   // ย้าย ownership ของ "I'm in the block" ให้ outer ซึ่งถูกประกาศอยู่ main
    }
    println!("{}", outer);
}
```

**Explanation**

`inner เป็นตัวแปรที่เป็นเจ้าของ "I'm in the block" ซึ่งถูกสร้างภายใน block หากพยายาม print ตัว inner โดยตรงหรือให้ outer เก็บค่าผ่านการอิง inner (&inner) แล้ว print outer จะไม่สามารถทำได้ เพราะหลังจากออก Block "I'm in the block" จะถูก Drop เพราะ inner ซึ่งเป็น Owner ถูกโปรแกรมมองว่าหลุด scope ไปแล้วทำให้ใน inner จะไม่มีค่าเก็บอยู่ (Dangling pointer) แก้ปัญหาได้โดยการย้ายความเป็นเจ้าของ (Ownership) ของ "I'm in the block" จาก inner สู่ outer แทน ทำให้โปรแกรม Run ได้ตามปกติ`

---

### Exercise 2 — `Which one's longer?`

**Problem**

`ให้เขียนฟังก์ชันชื่อ longer ที่รับข้อความ 2 ค่าเข้ามาผ่านการอ้างอิง (reference) แล้วคืนค่าข้อความที่ยาวกว่าออกไป (ถ้ายาวเท่ากันคืนค่าไหนก็ได้) จากนั้นเขียน main ที่เรียกใช้ฟังก์ชันนี้และแสดงผลให้ถูกต้อง`

**Hint**

`Rust ทราบหรือไม่ว่าต้อง Return ตัวไหน ? แล้วทั้ง 2 ตัวจำเป็นต้องมีอายุเท่ากันหรือไม่ ?`

**Solution**

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

**Explanation**

`เนื่องจาก longer() รับตัวแปรเข้าไป 2 ตัวและต้องผ่านกระบวนการ if else เพื่อตัดสินว่า a หรือ b จะถูก return กลับมา ทำให้ rust ไม่สามารถเดาได้ว่าตัวไหนกันแน่ที่จะถูก return มา ซึ่งอาจมีปัญหาได้หากตัวที่จะถูก return เกิดมีอายุสั้นเกินไป (ในกรณีนี้เช่น let b = String::from("Apple"); ซึ่งเป็นตัวที่จะ return ถูกประกาศภายใน block แล้ว print ที่นอก block ทำให้ค่า "Apple" ถูก drop ไปก่อน) ทำให้ต้องมีการประกาศ Lifetime specifier ('a) เพื่อให้ rust ตรวจสอบและมั่นใจว่าทุกตัวแปรที่ส่งเข้าไปจะมีอายุอยู่นานพอในขณะที่ longer() ยังสามารถถูกเรียกใช้ได้อยู่`

---