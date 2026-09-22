# Rust Tutorial Project — Principles of Programming Languages

> **สำหรับนักศึกษา:** ใช้ไฟล์นี้เป็น Template สำหรับจัดทำบทเรียน Rust ของกลุ่ม
> **Topic No.:** `17`
> **Topic Name:** `[Lifetimes]`
> **Group No.:** `17`

---

## 1. Members

| # | Name | Student ID | GitHub Username | Main Responsibility |
|---|---|---|---|---|
| 1 | `[ชื่อ-นามสกุล]` | `[รหัส]` | `@[username]` | Concept + Code |
| 2 | `[ชื่อ-นามสกุล]` | `[รหัส]` | `@[username]` | Code + Demo |
| 3 | `[ชื่อ-นามสกุล]` | `[รหัส]` | `@[username]` | Rust vs Other Language + PPL |
| 4 | `[ชื่อ-นามสกุล]` | `[รหัส]` | `@[username]` | Exercises + Common Mistakes |

---

## 2. Learning Objectives

หลังจากศึกษา Topic นี้แล้ว ผู้เรียนสามารถ:

1. `[อธิบายแนวคิดสำคัญได้]`
2. `[เขียนโปรแกรม Rust ที่เกี่ยวข้องได้]`
3. `[วิเคราะห์พฤติกรรม/กฎของภาษาได้]`
4. `[เปรียบเทียบ Rust กับภาษาอื่นได้]`

---

## 3. Introduction

อธิบายว่า Topic นี้คืออะไร มีความสำคัญอย่างไร และใช้แก้ปัญหาอะไรในการเขียนโปรแกรม

การจัดการหน่วยความจำในภาษาโปรแกรมโดยทั่วไปแบ่งเป็นแบบจัดการเอง (C/C++) ที่รวดเร็วแต่เสี่ยงช่องโหว่ และแบบใช้ Garbage Collector (Java, Go) ที่ปลอดภัยแต่แลกมาด้วยความล่าช้า (Overhead) ภาษา Rust จึงทลายข้อจำกัดนี้ด้วยการจัดการหน่วยความจำตั้งแต่ขั้นตอนการคอมไพล์ผ่าน 2 แนวคิดหลัก ได้แก่:

1. **Lifetime (อายุขัยของข้อมูล):**
    คอมไพเลอร์จะตรวจสอบขอบเขตเวลาใช้งานของทุกการอ้างอิง (Reference) เพื่อป้องกันปัญหา Dangling Pointers (พอยน์เตอร์ชี้ค้าง) โดยมีกฎเหล็กคือตัวอ้างอิงต้องไม่มีอายุขัยยาวนานกว่าตัวข้อมูลจริง
2. **Ownership (ระบบความเป็นเจ้าของ):**
    ประยุกต์ใช้ตรรกะ Affine Type System ที่กำหนดให้ข้อมูลแต่ละชิ้นมีเจ้าของได้เพียงตัวเดียวเท่านั้น เมื่อตัวแปรเจ้าของสิ้นสุดการทำงาน (Out of scope) ระบบจะแทรกคำสั่งคืนค่าหน่วยความจำให้โดยอัตโนมัติ

ผลลัพธ์ที่ได้คือ Zero-cost Abstraction ที่การันตีความปลอดภัยของหน่วยความจำได้ 100% โดยไม่ต้องพึ่งพา Garbage Collector ทำให้โปรแกรมมีความเร็วและประสิทธิภาพเทียบเท่าภาษา C หรือ C++

---

## 4. Key Concepts

### 4.1 Shared XOR Mutable

Borrow Checker คือกลไกของคอมไพเลอร์ที่บริหารจัดการการยืมข้อมูล เพื่อตรวจสอบว่าการเข้าถึงหน่วยความจำในทุก ๆ ตำแหน่งสอดคล้องกับกฎของการยืมหรือไม่ เป้าหมายหลักในขั้นต้นคือเพื่อป้องกันปัญหา Data Races (การแย่งกันแก้ไขข้อมูลพร้อมกัน)

**Shared XOR Mutable** ในช่วงเวลาหนึ่ง ข้อมูลสามารถมีตัวอ้างอิงแบบอ่านอย่างเดียว (Immutable) ได้หลายตัว หรือ มีตัวอ้างอิงแบบแก้ไขได้ (Mutable) เพียง 1 ตัวเท่านั้น ห้ามมีทั้งสองแบบพร้อมกันโดยเด็ดขาด

**ตัวอย่าง**

```rust
fn main() {
    let mut data = String::from("Rust");

    let r1 = &data; // ยืมแบบอ่าน
    let r2 = &data; // ยืมแบบอ่านตัวที่สอง (ทำได้)
    
    let r3 = &mut data; // ยืมแบบแก้ไข (Error: ทำไม่ได้เพราะ r1, r2 ยังใช้งานอยู่)
    
    println!("Read: {} and {}", r1, r2);
}
```

**Explanation**

เมื่อตัวแปร r1 และ r2 กำลังถือสิทธิ์การอ่านแบบ Shared อยู่ Borrow Checker จะไม่อนุญาตให้ r3 ขอสิทธิ์แบบ Mutable เพื่อเข้ามาแก้ไขข้อมูลเด็ดขาดจนกว่า r1 และ r2 จะทำงานเสร็จสิ้น เพื่อรับประกันว่าจะไม่มีใครแอบเปลี่ยนข้อมูลในขณะที่คนอื่นกำลังอ่านอยู่

---

### 4.2 Non-Lexical Lifetimes (NLL)

เพื่อให้การจัดการหน่วยความจำยืดหยุ่นขึ้นและป้องกันปัญหา Use-after-free 

Borrow Checker จะทำงานอยู่บน Region Variables โดยวิเคราะห์ กราฟเส้นทางการทำงานของโปรแกรม (Control Flow Graph - CFG) แทนที่จะดูแค่ขอบเขตของโค้ด

ด้วย NLL อายุขัยของการอ้างอิงจะสิ้นสุดลงทันทีเมื่อ สิ้นสุดการเรียกใช้งานจริงในบรรทัดสุดท้าย ไม่ต้องรอให้จบโครงสร้างบล็อกปีกกา **{}** เพื่อลดการรายงานข้อผิดพลาดที่เข้มงวดเกินจำเป็น

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

**Explanation**
กลไก CFG ของคอมไพเลอร์ติดตามเส้นทางการใช้งานของตัวแปร y และพบว่ามันไม่ได้ถูกใช้งานอีกเลยหลังจากบรรทัด *y += 5; 

Borrow Checker จึงปลดล็อกสิทธิ์การยืมทันที ทำให้ z สามารถอ้างอิงตัวแปร x ต่อได้โดยไม่ผิดกฎ Shared XOR Mutable


---

### 4.3 Explicit Lifetime Annotation

มักจะถูกเขียนแทนด้วยสัญลักษณ์ Apostrophe ตามด้วยชื่อตัวแปรขนาดเล็ก เช่น 'a (อ่านว่า ทิก-เอ)

แม้ว่าในหลายฟังก์ชัน คอมไพเลอร์จะสามารถอนุมานอายุขัยของตัวแปรภายในฟังก์ชัน (Intra-procedural analysis) ได้อย่างแม่นยำโดยไม่ต้องพึ่งพาไวยากรณ์เหล่านี้ แต่เมื่อโปรแกรมมีความซับซ้อนมากขึ้น โดยเฉพาะเมื่อมีการรับค่าอ้างอิงเข้ามาหลายตัวและมีการคืนค่าอ้างอิงกลับออกไป การระบุ Lifetime จึงกลายเป็นสิ่งจำเป็นอย่างหลีกเลี่ยงไม่ได้ ใช้เพื่อเชื่อมโยงว่าค่าอ้างอิง (Reference) ขาออกนั้น มีความเกี่ยวข้องกับค่าอ้างอิงขาเข้าตัวใด

การระบุ Lifetime ไม่ได้ช่วยยืดอายุขัยของข้อมูลจริง แต่เป็นเพียงการอธิบายตรรกะให้ Borrow Checker มั่นใจว่าจะไม่มีการเข้าถึงข้อมูล (Dangling pointer) หลังจากที่เจ้าของข้อมูลนั้นถูกทำลายไปแล้ว

```rust
fn longest<'a>(x: &'a str, y: &'a str) -> &'a str {
    if x.len() > y.len() {
        x
    } else {
        y
    }
}
```

**Explanation**
เครื่องหมาย 'a เป็นค่า Reference ที่ฟังก์ชันนี้ส่งคืนกลับไป จะมีอายุการใช้งานเท่ากับตัวแปรขาเข้า (x หรือ y) ตัวที่มีอายุสั้นที่สุด เพื่อให้คอมไพเลอร์ตรวจสอบฟังก์ชันผู้เรียก (Caller) ได้อย่างปลอดภัยโดยไม่ต้องเข้ามาวิเคราะห์โค้ดข้างในฟังก์ชันนี้ซ้ำ

---

## 5. Important Syntax / Rules

| Syntax / Rule | Meaning | Example |
|---|---|---|
| `[syntax/rule]` | `[ความหมาย]` | `[ตัวอย่าง]` |
| `[syntax/rule]` | `[ความหมาย]` | `[ตัวอย่าง]` |
| `[syntax/rule]` | `[ความหมาย]` | `[ตัวอย่าง]` |

### Important Rules

1. `[กฎสำคัญข้อที่ 1]`
2. `[กฎสำคัญข้อที่ 2]`
3. `[กฎสำคัญข้อที่ 3]`

---

## 6. Runnable Code Examples

> **ข้อกำหนด:** Code ทุกตัวต้อง Compile และ Run ได้จริงก่อนนำมาใส่ในเอกสาร

### Example 1 — `[ชื่อ Example]`

**Purpose:** `[ต้องการสาธิตอะไร]`

```rust
fn main() {
    // Write your runnable Rust code here
}
```

**Expected Output**

```text
[expected output]
```

**Explanation**

`[อธิบาย code ทีละส่วนที่สำคัญ]`

---

### Example 2 — `[ชื่อ Example]`

**Purpose:** `[ต้องการสาธิตอะไร]`

```rust
fn main() {
    // Write your runnable Rust code here
}
```

**Expected Output**

```text
[expected output]
```

**Explanation**

`[อธิบาย code]`

---

## 7. Common Mistakes

### Mistake 1 — `[ชื่อข้อผิดพลาด]`

**Problem**

`[อธิบายปัญหา]`

**Incorrect Code**

```rust
// Incorrect example
```

**Correct Code**

```rust
// Correct example
```

**Why?**

`[อธิบายสาเหตุ]`

---

### Mistake 2 — `[ชื่อข้อผิดพลาด]`

**Problem**

`[อธิบายปัญหา]`

**Incorrect Code**

```rust
// Incorrect example
```

**Correct Code**

```rust
// Correct example
```

**Why?**

`[อธิบายสาเหตุ]`

---

## 8. Exercises

> จัดทำแบบฝึกหัด **2 ข้อ** ที่สอดคล้องกับ Topic และมีระดับความยากเหมาะสม

### Exercise 1 — `[ชื่อโจทย์]`

**Problem**

`[เขียนโจทย์]`

**Hint**

`[คำใบ้]`

**Solution**

```rust
// Solution code
```

**Explanation**

`[อธิบายแนวทางแก้]`

---

### Exercise 2 — `[ชื่อโจทย์]`

**Problem**

`[เขียนโจทย์]`

**Hint**

`[คำใบ้]`

**Solution**

```rust
// Solution code
```

**Explanation**

`[อธิบายแนวทางแก้]`

---

## 9. PPL Perspective

> **ส่วนนี้เป็นหัวใจของรายวิชา Principles of Programming Languages**

วิเคราะห์ Topic นี้ในมุมมองของ Programming Languages

### 9.1 Syntax

`[Topic นี้เกี่ยวข้องกับ syntax อย่างไร]`

### 9.2 Semantics

`[คำสั่ง/construct เหล่านี้มีความหมายหรือพฤติกรรมอย่างไร]`

### 9.3 Type System

`[เกี่ยวข้องกับ type system อย่างไร ถ้ามี]`

### 9.4 Memory / Resource Management

`[เกี่ยวข้องกับ memory หรือ resource management อย่างไร ถ้ามี]`

### 9.5 Abstraction / Other PPL Concepts

`[อธิบาย abstraction, scope, binding, paradigm หรือแนวคิด PPL อื่นที่เกี่ยวข้อง]`

### 9.6 Why Rust?

`[Rust ใช้แนวคิดนี้เพื่อเพิ่ม safety, reliability หรือ performance อย่างไร]`

---

## 10. Rust vs. Other Language

**Comparison Language:** `[Python / C / C++ / Java / Kotlin / ...]`

| Aspect | Rust | Other Language |
|---|---|---|
| Syntax | `[อธิบาย]` | `[อธิบาย]` |
| Semantics / Behavior | `[อธิบาย]` | `[อธิบาย]` |
| Type System | `[อธิบาย]` | `[อธิบาย]` |
| Memory Management | `[อธิบาย]` | `[อธิบาย]` |
| Safety | `[อธิบาย]` | `[อธิบาย]` |

### Rust Example

```rust
// Rust code
```

### `[Other Language]` Example

```python
# Other language code
```

### Analysis

`[อธิบายความแตกต่างที่สำคัญ และเหตุผลด้านการออกแบบภาษา]`

---

## 11. Teach Your Topic

การนำเสนอมีสมาชิก **4 คน คนละประมาณ 5 นาที**

| Member | Responsibility | Time |
|---|---|---:|
| Member 1 | Concept + Short Code Illustration | 5 min |
| Member 2 | Detailed Code + Live Demo | 5 min |
| Member 3 | Rust vs Other Language + PPL Analysis | 5 min |
| Member 4 | Exercises + Common Mistakes + Challenge | 5 min |

### Individual Contribution

**Member 1**

`[สิ่งที่รับผิดชอบ]`

**Member 2**

`[สิ่งที่รับผิดชอบ]`

**Member 3**

`[สิ่งที่รับผิดชอบ]`

**Member 4**

`[สิ่งที่รับผิดชอบ]`

> สมาชิกทุกคนต้องสามารถอธิบาย Code ของกลุ่มได้ ไม่ใช่เฉพาะส่วนที่ตนเองเขียน

---

## 12. References

> แนะนำให้มีอย่างน้อย **4 แหล่งอ้างอิง** และควรใช้เอกสารทางการเป็นหลัก

1. `[The Rust Programming Language — Rust Book]`
2. `[Rust by Example / Rust Reference]`
3. `[Official documentation ที่เกี่ยวข้องกับ Topic]`
4. `[แหล่งอ้างอิงเพิ่มเติม]`

---

## 13. AI Usage Declaration

สามารถใช้ AI เป็นเครื่องมือช่วยเรียนรู้และพัฒนาได้ แต่สมาชิกทุกคนต้องเข้าใจและสามารถอธิบายผลงานของกลุ่มได้

| AI Tool | Purpose | How the Result Was Verified |
|---|---|---|
| `[เช่น ChatGPT]` | `[ใช้เพื่ออะไร]` | `[ตรวจสอบอย่างไร]` |
| `[AI tool]` | `[ใช้เพื่ออะไร]` | `[ตรวจสอบอย่างไร]` |

### Declaration

- [ ] Code ทุกส่วนที่นำเสนอได้รับการ Compile และทดสอบแล้ว
- [ ] สมาชิกทุกคนสามารถอธิบาย Code ที่นำเสนอได้
- [ ] ตรวจสอบข้อมูลจากแหล่งอ้างอิงที่น่าเชื่อถือแล้ว
- [ ] ระบุการใช้ AI อย่างโปร่งใส

**รายละเอียดการใช้ AI**

`[อธิบายว่าใช้ AI ในขั้นตอนใด และสมาชิกตรวจสอบผลลัพธ์อย่างไร]`

---

## 14. GitHub Contribution

| Member | Issues | Commits | Pull Requests | Code Reviews | Contribution |
|---|---:|---:|---:|---:|---|
| Member 1 | `[จำนวน]` | `[จำนวน]` | `[จำนวน]` | `[จำนวน]` | `[รายละเอียด]` |
| Member 2 | `[จำนวน]` | `[จำนวน]` | `[จำนวน]` | `[จำนวน]` | `[รายละเอียด]` |
| Member 3 | `[จำนวน]` | `[จำนวน]` | `[จำนวน]` | `[จำนวน]` | `[รายละเอียด]` |
| Member 4 | `[จำนวน]` | `[จำนวน]` | `[จำนวน]` | `[จำนวน]` | `[รายละเอียด]` |

### Teamwork Reflection

**How did your team collaborate?**

`[อธิบายกระบวนการทำงานร่วมกัน]`

**Problems encountered**

`[ปัญหาที่พบ]`

**How did you solve them?**

`[วิธีแก้ปัญหา]`

---

## 15. Final Checklist

- [ ] Learning Objectives ครบ 3–4 ข้อ
- [ ] Key Concepts ครบถ้วน
- [ ] Syntax / Rules
- [ ] Runnable Code Examples
- [ ] Code Compile และ Run ได้จริง
- [ ] Common Mistakes
- [ ] Exercises 2 ข้อ พร้อม Solutions
- [ ] PPL Perspective
- [ ] Rust vs Other Language
- [ ] References อย่างน้อย 4 แหล่ง
- [ ] AI Usage Declaration
- [ ] GitHub Contribution
- [ ] สมาชิกทั้ง 4 คนมีส่วนร่วม
- [ ] สมาชิกทั้ง 4 คนพร้อมนำเสนอคนละ 5 นาที
- [ ] สมาชิกทุกคนสามารถอธิบาย Code ของกลุ่มได้

---

## Submission Information

**Repository:** `[GitHub repository URL]`

**Chapter Path:** `[เช่น chapters/01-introduction/]`

**Final PR:** `#[PR number]`

**Submitted by:** `[Group XX]`

**Date:** `[YYYY-MM-DD]`
