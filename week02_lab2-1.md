# ใบงานการทดลองที่ 2-1
# Dart Programming Fundamentals
### วิชา: การพัฒนาซอฟต์แวร์สำหรับอุปกรณ์เคลื่อนที่

| | |
|--|--|
| **สัปดาห์ที่** | 2 |
| **ใบงานที่** | 2-1 จาก 2 |
| **เวลา** | 2 ชั่วโมง 30 นาที |
| **เครื่องมือ** | DartPad (dartpad.dev) |

---

## วัตถุประสงค์

เมื่อสิ้นสุดการทดลอง นักศึกษาสามารถ:

1. เขียนโปรแกรม Dart ที่มี Type System, Null Safety, Collection ได้ถูกต้อง
2. ออกแบบและเขียน Function ทั้งแบบ Named Parameter, Optional Parameter และ Higher-order Function ได้
3. ออกแบบ Class ที่มี Constructor, Getter, Inheritance, Abstract Class และ Mixin ได้
4. เขียนโปรแกรม Async ด้วย Future, async/await และ Future.wait ได้ พร้อมจัดการ Error
5. อ่าน Error Message ของ Dart และแก้ไขได้ด้วยตนเอง

---

## การเตรียมตัวก่อนทดลอง

ใบงานนี้ใช้ **DartPad** ทั้งหมด ไม่จำเป็นต้องติดตั้งโปรแกรมใดๆ เพิ่มเติม

1. เปิด Browser (แนะนำ Chrome หรือ Edge)
2. ไปที่ **https://dartpad.dev**
3. ตรวจสอบว่าหน้าตาเป็นดังนี้

```
┌─────────────────────────────────────────────────────┐
│  DartPad                              [Dart ▼] [Run]│
├───────────────────────────┬─────────────────────────┤
│                           │                         │
│   บริเวณเขียนโค้ด            │   บริเวณแสดงผล           │
│   (Code Editor)           │   (Console Output)      │
│                           │                         │
└───────────────────────────┴─────────────────────────┘
```

> **หมายเหตุ:** ตรวจสอบว่าเลือก **Dart** (ไม่ใช่ Flutter) ที่ Dropdown มุมบนขวา ก่อนเริ่มทุกการทดลอง

---

## ส่วนที่ 1 — ทฤษฎีและการทดลอง: Variables, Types และ Null Safety

### ทฤษฎี 1.1 — ระบบชนิดข้อมูล (Type System)

Dart เป็นภาษา **Strongly Typed** หมายความว่าทุกตัวแปรมีชนิดข้อมูลที่แน่นอน และชนิดนั้นไม่เปลี่ยนตลอดอายุการใช้งาน ซึ่งต่างจากภาษา Dynamic เช่น Python ที่ตัวแปรเดียวกันสามารถเป็นได้ทั้ง String และ Number

**ชนิดข้อมูลพื้นฐานใน Dart:**

| ชนิด | ความหมาย | ตัวอย่างค่า |
|------|----------|-----------|
| `int` | จำนวนเต็ม | `0`, `42`, `-10` |
| `double` | จำนวนทศนิยม | `3.14`, `2.0`, `-0.5` |
| `String` | ข้อความ | `"สวัสดี"`, `'hello'` |
| `bool` | ค่าจริง/เท็จ | `true`, `false` |
| `List<T>` | รายการ (Array) | `[1, 2, 3]` |
| `Map<K,V>` | คู่ Key-Value | `{"name": "ชาย"}` |
| `Set<T>` | ชุดไม่ซ้ำ | `{1, 2, 3}` |

**การประกาศตัวแปร:**

```dart
// แบบที่ 1: ระบุ Type ชัดเจน — อ่านง่าย รู้ Type ทันที
String name = "สมชาย";
int age = 20;
double gpa = 3.75;

// แบบที่ 2: ใช้ var — Dart อนุมาน Type จากค่าที่กำหนด (Type Inference)
var city = "กรุงเทพฯ";  // Dart รู้ว่าเป็น String
var score = 95;           // Dart รู้ว่าเป็น int

// แบบที่ 3: final — กำหนดค่าได้ครั้งเดียว (Runtime Constant)
final birthYear = 2004;    // เปลี่ยนหลังกำหนดไม่ได้
final now = DateTime.now(); // ค่าถูกกำหนด ณ Runtime

// แบบที่ 4: const — ค่าคงที่ Compile Time (ต้องรู้ค่าก่อนรัน)
const pi = 3.14159;
const maxScore = 100;
// const now = DateTime.now(); ← ❌ Error! DateTime.now() ไม่รู้ก่อนรัน
```

**String Interpolation** — การฝังตัวแปรใน String:

```dart
String name = "สมชาย";
int age = 20;

// วิธีที่ 1: $variable — ใช้กับตัวแปรเดี่ยว
print("ชื่อ: $name");           // → ชื่อ: สมชาย

// วิธีที่ 2: ${expression} — ใช้กับ Expression ที่ซับซ้อน
print("อายุ: ${age} ปี");       // → อายุ: 20 ปี
print("ปีเกิด: ${2025 - age}"); // → ปีเกิด: 2005
print("ชื่อ: ${name.toUpperCase()}"); // → ชื่อ: สมชาย (ตัวพิมพ์ใหญ่)
```

---

### ทฤษฎี 1.2 — Null Safety

ก่อนที่ Dart จะมี Null Safety โปรแกรมเมอร์มักพบ Error ที่ทำให้แอป Crash แบบนี้:

```
Unhandled Exception: Null check operator used on a null value
```

Dart 2.12+ แก้ปัญหานี้ด้วยระบบ **Sound Null Safety** — Compiler จะ**ไม่ยอม**ให้โค้ดที่อาจเกิด Null Error ผ่านได้

```
ตัวแปร Dart แบ่งเป็น 2 ประเภท:

Non-nullable (default):          Nullable (เพิ่ม ?):
┌─────────────────────┐         ┌─────────────────────┐
│  String name        │         │  String? nickname   │
│  ┌───────────────┐  │         │  ┌───────────────┐  │
│  │ ต้องมีค่า        │  │         │  │ มีค่า หรือ       │  │
│  │ เสมอ!         │  │         │  │ null ก็ได้      │  │
│  └───────────────┘  │         │  └───────────────┘  │
└─────────────────────┘         └─────────────────────┘
```

**Null-aware Operators — เครื่องมือจัดการ Null อย่างปลอดภัย:**

```dart
String? nickname = null;

// 1. ?? (Null Coalescing) — "ถ้า null ให้ใช้ค่าขวาแทน"
String display = nickname ?? "ไม่มีชื่อเล่น";
print(display); // → ไม่มีชื่อเล่น

// 2. ?. (Null-aware method call) — "เรียก method เฉพาะเมื่อไม่ null"
int? length = nickname?.length;
print(length);  // → null (ไม่ Crash)

// 3. ??= (Null-aware assignment) — "กำหนดค่าเฉพาะถ้าตัวแปรเป็น null"
nickname ??= "ชื่อเล่นเริ่มต้น";
print(nickname); // → ชื่อเล่นเริ่มต้น

// 4. ! (Null Assertion) — "ฉันมั่นใจว่าไม่ null" ⚠️ ใช้ด้วยความระวัง
String definitelyNotNull = "ค่าจริง";
String? maybeNull = definitelyNotNull;
print(maybeNull!.length); // → 8 (ถ้าผิดพลาดและเป็น null จะ Crash)
```

**Collections:**

```dart
// List — รายการที่มีลำดับ เพิ่ม/ลบได้
List<String> fruits = ["แอปเปิล", "กล้วย", "ส้ม"];
fruits.add("มะม่วง");
fruits.remove("กล้วย");
print(fruits.length);    // → 3
print(fruits[0]);        // → แอปเปิล
print(fruits.first);     // → แอปเปิล
print(fruits.last);      // → ส้ม

// Map — คู่ Key-Value
Map<String, int> scores = {
  "คณิตศาสตร์": 85,
  "วิทยาศาสตร์": 92,
  "ภาษาไทย": 78,
};
scores["ภาษาอังกฤษ"] = 88;  // เพิ่ม entry ใหม่
print(scores["คณิตศาสตร์"]); // → 85
print(scores["ชีววิทยา"]);   // → null (ไม่มี Key นี้)

// วนซ้ำใน Map
scores.forEach((subject, score) {
  print("$subject: $score");
});

// Set — ชุดข้อมูลที่ไม่มีซ้ำ
Set<String> tags = {"dart", "flutter", "mobile"};
tags.add("dart");  // ไม่เพิ่ม เพราะซ้ำอยู่แล้ว
print(tags.length); // → 3
```

---

### การทดลอง 1.1 — Variables, Types และ Collections

**⏱ เวลา:** 20 นาที

**ขั้นตอนที่ 1** เปิด dartpad.dev เลือก Dart แล้วล้างโค้ดเดิมออก

**ขั้นตอนที่ 2** พิมพ์โค้ดต่อไปนี้ทีละบล็อก อ่านทำความเข้าใจก่อนกด Run

```dart
void main() {
  // === บล็อกที่ 1: ชนิดข้อมูลพื้นฐาน ===
  String studentName = "สมชาย ดีใจ";
  int studentAge = 20;
  double gpa = 3.75;
  bool isEnrolled = true;

  print("=== ข้อมูลนักศึกษา ===");
  print("ชื่อ: $studentName");
  print("อายุ: $studentAge ปี");
  print("GPA: $gpa");
  print("ลงทะเบียนแล้ว: $isEnrolled");
  print("ปีเกิด (ประมาณ): ${2026 - studentAge}");
}
```

**ขั้นตอนที่ 3** กด **Run** ตรวจสอบผลลัพธ์ที่ได้
<img width="2352" height="990" alt="image" src="https://github.com/user-attachments/assets/6fbb5a59-f877-4ce3-8c75-49bd81bebf0e" />

**ขั้นตอนที่ 4** เพิ่มโค้ดต่อไปนี้ **ต่อท้าย** ภายใน `main()` ก่อนปิด `}`

```dart
  // === บล็อกที่ 2: Null Safety ===
  print("\n=== Null Safety ===");
  String? nickname = null;
  print("ชื่อเล่น: ${nickname ?? 'ไม่มี'}");  // → ไม่มี

  nickname = "ชาย";
  print("ชื่อเล่น: ${nickname ?? 'ไม่มี'}");  // → ชาย
  print("ความยาว: ${nickname?.length}");       // → 3
  print("ตัวพิมพ์ใหญ่: ${nickname?.toUpperCase()}"); // → ชาย
```


**ขั้นตอนที่ 5** กด Run อีกครั้ง สังเกตผลลัพธ์ที่เพิ่มขึ้น
<img width="3004" height="1296" alt="image" src="https://github.com/user-attachments/assets/27dc8b91-67c8-4a12-89c7-70af7bd24f80" />

**ขั้นตอนที่ 6** เพิ่มโค้ด Collections ต่อท้าย

```dart
  // === บล็อกที่ 3: List ===
  print("\n=== รายวิชาที่ลงทะเบียน ===");
  List<String> courses = ["Mobile Dev", "Web Dev", "AI"];
  Map<String, int> courseScores = {
    "Mobile Dev": 90,
    "Web Dev": 85,
    "AI": 92,
  };

  // วนซ้ำแสดงรายวิชาและคะแนน
  for (int i = 0; i < courses.length; i++) {
    String course = courses[i];
    int? score = courseScores[course];
    print("${i + 1}. $course: ${score ?? 'ยังไม่มีคะแนน'} คะแนน");
  }

  // คำนวณเฉลี่ย
  int total = courseScores.values.reduce((a, b) => a + b);
  double avg = total / courseScores.length;
  print("คะแนนเฉลี่ย: ${avg.toStringAsFixed(2)}");
```


**ขั้นตอนที่ 7** กด Run และบันทึกผลลัพธ์ทั้งหมด

<img width="3022" height="1344" alt="image" src="https://github.com/user-attachments/assets/ac20dedf-6dd3-4cf6-a11d-6c52cc652f95" />
---

### 🎯 โจทย์ฝึกทำ 1.1 — แก้ไขและเพิ่มเติมโค้ดด้วยตนเอง

แก้ไขโค้ดที่มีอยู่ให้ทำสิ่งต่อไปนี้ได้ครบ:

1. เพิ่มรายวิชา "Database" ที่มีคะแนน 88 ลงใน `courses` และ `courseScores`
2. หาวิชาที่มีคะแนนสูงสุดโดยใช้ `courseScores.entries` และ `.reduce()` แล้วพิมพ์ว่า "วิชาที่ได้คะแนนสูงสุด: ..."
3. นับจำนวนวิชาที่ได้คะแนน >= 90 แล้วพิมพ์ผล
4. สร้าง `Set<String>` ชื่อ `passedCourses` ที่เก็บเฉพาะวิชาที่ได้คะแนน >= 80 แล้วพิมพ์รายการ

**ผลลัพธ์ที่คาดหวัง (ตัวอย่าง):**
```
วิชาที่ได้คะแนนสูงสุด: AI (92 คะแนน)
จำนวนวิชาที่ได้ >= 90: 2 วิชา
วิชาที่ผ่าน: {Mobile Dev, Web Dev, AI, Database}
```
**บันทึกผลการทดลอง: บันทึกโค้ดคำสั่งที่ได้**
```dart
void main() {
  // 1. ข้อมูลเริ่มต้น (สมมติข้อมูลเริ่มต้นสำหรับทำโจทย์)
  List<String> courses = ['Mobile Dev', 'Web Dev', 'AI', 'UX/UI'];
  Map<String, int> courseScores = {
    'Mobile Dev': 85,
    'Web Dev': 90,
    'AI': 92,
    'UX/UI': 75
  };

  // --- เริ่มแก้ไขและเพิ่มเติมโค้ดตรงนี้ ---

  // ข้อ 1: เพิ่มรายวิชา "Database" ที่มีคะแนน 88
  courses.add('Database');
  courseScores['Database'] = 88;

  // ข้อ 2: หาวิชาที่มีคะแนนสูงสุดโดยใช้ courseScores.entries และ .reduce()
  var highestCourse = courseScores.entries.reduce((current, next) {
    return current.value > next.value ? current : next;
  });
  print('วิชาที่ได้คะแนนสูงสุด: ${highestCourse.key} (${highestCourse.value} คะแนน)');

  // ข้อ 3: นับจำนวนวิชาที่ได้คะแนน >= 90
  int countTopScores = courseScores.values.where((score) => score >= 90).length;
  print('จำนวนวิชาที่ได้ >= 90: $countTopScores วิชา');

  // ข้อ 4: สร้าง Set<String> ชื่อ passedCourses ที่เก็บเฉพาะวิชาที่ได้คะแนน >= 80
  Set<String> passedCourses = courseScores.entries
      .where((entry) => entry.value >= 80)
      .map((entry) => entry.key)
      .toSet();
  print('วิชาที่ผ่าน: $passedCourses');
}

```
<img width="2772" height="1428" alt="image" src="https://github.com/user-attachments/assets/54e3ca21-15a6-4610-96fc-36493939383c" />

---

## ส่วนที่ 2 — ทฤษฎีและการทดลอง: Functions

### ทฤษฎี 2.1 — รูปแบบ Function ใน Dart

Function คือหน่วยของโค้ดที่แยกออกมาเพื่อทำงานเฉพาะอย่าง ช่วยให้ไม่ต้องเขียนโค้ดซ้ำ

**รูปแบบ Function พื้นฐาน:**

```dart
// โครงสร้าง: ReturnType functionName(ParameterType paramName) { ... }

// Function ที่คืนค่า String
String greet(String name) {
  return "สวัสดี $name!";
}

// Function ที่ไม่คืนค่า (void)
void printDivider(int length) {
  print("─" * length);
}

// Arrow Function: ย่อเมื่อ body มีแค่ return expression เดียว
String greetArrow(String name) => "สวัสดี $name!";
double square(double x) => x * x;
bool isAdult(int age) => age >= 18;
```

**Positional Parameters — ต้องส่งตามลำดับ:**

```dart
// Required positional — ต้องส่งครบทุกตัว
double calculateBMI(double weight, double height) {
  return weight / (height * height);
}
print(calculateBMI(70, 1.75)); // → 22.86 ✅
// print(calculateBMI(1.75, 70)); // ✅ รัน แต่ผิดความหมาย!

// Optional positional — ใส่ [] รอบ Parameter ที่ไม่บังคับ
String formatName(String firstName, [String? lastName, String title = ""]) {
  if (lastName != null) {
    return "$title $firstName $lastName".trim();
  }
  return "$title $firstName".trim();
}
print(formatName("สมชาย"));              // → สมชาย
print(formatName("สมชาย", "ดีใจ"));      // → สมชาย ดีใจ
print(formatName("สมชาย", "ดีใจ", "นาย")); // → นาย สมชาย ดีใจ
```

**Named Parameters — ต้องระบุชื่อเมื่อเรียก:**

```dart
// Named parameters ใส่ {} รอบ — ลำดับไม่สำคัญ
void createProfile({
  required String name,    // required = บังคับส่ง
  required String email,   // required = บังคับส่ง
  int age = 0,             // optional + default value
  String? bio,             // optional nullable
}) {
  print("ชื่อ: $name");
  print("Email: $email");
  print("อายุ: $age ปี");
  if (bio != null) print("Bio: $bio");
}

// เรียกใช้ — ไม่ต้องสนใจลำดับ แต่ต้องระบุชื่อ
createProfile(
  name: "สมชาย",
  email: "somchai@example.com",
  age: 20,
  bio: "นักศึกษา KMITL",
);

createProfile(
  email: "guest@example.com",  // ลำดับต่างกันก็ได้
  name: "ผู้เยี่ยมชม",
  // age ไม่ส่งก็ได้ มีค่า default เป็น 0
);
```

---

### ทฤษฎี 2.2 — Higher-Order Functions

**Higher-order Function** คือ Function ที่รับ Function อื่นเป็น Parameter หรือคืนค่าเป็น Function ซึ่งทำให้เขียนโค้ดที่ยืดหยุ่นและกระชับมากขึ้น

```
แนวคิด:
ปกติเราส่ง ตัวเลข, ข้อความ เป็น Parameter
Higher-order Function ส่ง Function เป็น Parameter ได้ด้วย!

f(x) = x * 2          ← Function ธรรมดา
g(f, x) = f(x) + 1    ← Higher-order Function (รับ f เป็น parameter)
```

**Collection Methods ที่ใช้บ่อย:**

```dart
List<int> numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];

// where() — กรองรายการตามเงื่อนไข (คืน Iterable)
var evens = numbers.where((n) => n % 2 == 0).toList();
print(evens); // → [2, 4, 6, 8, 10]

// map() — แปลงทุก element (คืน Iterable ของ Type ใหม่)
var doubled = numbers.map((n) => n * 2).toList();
print(doubled); // → [2, 4, 6, 8, 10, 12, 14, 16, 18, 20]

var asString = numbers.map((n) => "No.$n").toList();
print(asString); // → [No.1, No.2, ...]

// reduce() — รวมทั้งหมดเป็นค่าเดียว
int sum = numbers.reduce((acc, n) => acc + n);
print(sum); // → 55

int max = numbers.reduce((a, b) => a > b ? a : b);
print(max); // → 10

// any() — มี element ใดสอดคล้องเงื่อนไขไหม?
bool hasNegative = numbers.any((n) => n < 0);
print(hasNegative); // → false

// every() — ทุก element สอดคล้องเงื่อนไขไหม?
bool allPositive = numbers.every((n) => n > 0);
print(allPositive); // → true

// sort() — เรียงลำดับ (แก้ List เดิม)
List<int> scores = [85, 42, 96, 71, 58];
scores.sort((a, b) => b.compareTo(a)); // เรียงจากมากไปน้อย
print(scores); // → [96, 85, 71, 58, 42]
```

**Function เป็น Variable:**

```dart
// เก็บ Function ในตัวแปร
String Function(String) shout = (s) => s.toUpperCase() + "!!!";
print(shout("hello")); // → HELLO!!!

// ส่ง Function เป็น Parameter
void applyToList(List<int> list, void Function(int) action) {
  for (var item in list) {
    action(item);
  }
}

applyToList([1, 2, 3], (n) => print("ค่า: $n"));
// → ค่า: 1
// → ค่า: 2
// → ค่า: 3
```

---

### การทดลอง 2.1 — Functions พื้นฐาน

**⏱ เวลา:** 20 นาที

**ขั้นตอนที่ 1** ล้างโค้ดใน DartPad แล้วพิมพ์โค้ดต่อไปนี้

```dart
// === Function พื้นฐาน ===
String gradeLabel(double gpa) {
  if (gpa >= 3.5) return "เกียรตินิยมอันดับ 1";
  if (gpa >= 3.25) return "เกียรตินิยมอันดับ 2";
  if (gpa >= 3.0) return "ดีมาก";
  if (gpa >= 2.5) return "ดี";
  if (gpa >= 2.0) return "พอใช้";
  return "ต่ำกว่าเกณฑ์";
}

// === Arrow Function ===
double average(List<double> nums) =>
    nums.reduce((a, b) => a + b) / nums.length;

bool isHonors(double gpa) => gpa >= 3.25;

// === Named Parameters ===
void printStudent({
  required String name,
  required double gpa,
  int year = 1,
  String? major,
}) {
  print("─────────────────────────");
  print("ชื่อ: $name (ปีที่ $year)");
  if (major != null) print("สาขา: $major");
  print("GPA: $gpa → ${gradeLabel(gpa)}");
  print("เกียรตินิยม: ${isHonors(gpa) ? "✅ ใช่" : "❌ ไม่"}");
}

void main() {
  print("=== รายชื่อนักศึกษา ===\n");

  printStudent(name: "สมชาย", gpa: 3.75, year: 3, major: "เทคโนโลยีคอมพิวเตอร์");
  printStudent(name: "สมหญิง", gpa: 2.90, year: 2);
  printStudent(name: "สมศักดิ์", gpa: 3.30, year: 4, major: "เทคโนโลยีคอมพิวเตอร์");

  print("\n=== GPA เฉลี่ยทั้งชั้น ===");
  List<double> allGpas = [3.75, 2.90, 3.30];
  print("เฉลี่ย: ${average(allGpas).toStringAsFixed(2)}");
}
```

**ขั้นตอนที่ 2** กด Run สังเกตผลลัพธ์
<img width="2776" height="1506" alt="image" src="https://github.com/user-attachments/assets/715a2195-7391-43db-ac3e-0818eb443f37" />


**ขั้นตอนที่ 3** ทดลองเปลี่ยนค่า `gpa` ในแต่ละ `printStudent()` เพื่อดูว่า Label เปลี่ยนอย่างไร

---

### การทดลอง 2.2 — Higher-Order Functions

**⏱ เวลา:** 25 นาที

**ขั้นตอนที่ 1** ล้างโค้ดแล้วพิมพ์โค้ดต่อไปนี้

```dart
void main() {
  List<Map<String, dynamic>> students = [
    {"name": "สมชาย",  "gpa": 3.75, "year": 3, "faculty": "วิศวกรรม"},
    {"name": "สมหญิง", "gpa": 2.50, "year": 1, "faculty": "วิทยาศาสตร์"},
    {"name": "สมศักดิ์","gpa": 3.10, "year": 2, "faculty": "วิศวกรรม"},
    {"name": "สมใจ",  "gpa": 1.80, "year": 4, "faculty": "บริหาร"},
    {"name": "สมปอง", "gpa": 3.50, "year": 2, "faculty": "วิทยาศาสตร์"},
    {"name": "สมศรี", "gpa": 2.90, "year": 3, "faculty": "บริหาร"},
  ];

  // === where() — กรองนักศึกษาที่ GPA >= 3.0 ===
  print("=== นักศึกษาที่ GPA >= 3.0 ===");
  var honorStudents = students
      .where((s) => (s["gpa"] as double) >= 3.0)
      .toList();
  for (var s in honorStudents) {
    print("  ${s["name"]}: ${s["gpa"]}");
  }

  // === map() — แปลงเป็น String รายงาน ===
  print("\n=== รายงานนักศึกษา ===");
  var report = students
      .map((s) => "${s["name"]} (${s["faculty"]}) GPA: ${s["gpa"]}")
      .toList();
  report.forEach(print);

  // === sort() + reduce() ===
  print("\n=== วิเคราะห์คะแนน ===");
  List<double> gpas = students.map((s) => s["gpa"] as double).toList();

  double maxGpa = gpas.reduce((a, b) => a > b ? a : b);
  double minGpa = gpas.reduce((a, b) => a < b ? a : b);
  double avgGpa = gpas.reduce((a, b) => a + b) / gpas.length;

  print("GPA สูงสุด: $maxGpa");
  print("GPA ต่ำสุด: $minGpa");
  print("GPA เฉลี่ย: ${avgGpa.toStringAsFixed(2)}");

  // === any() และ every() ===
  bool anyFailing = students.any((s) => (s["gpa"] as double) < 2.0);
  bool allPassing = students.every((s) => (s["gpa"] as double) >= 2.0);
  print("มีนักศึกษาที่ GPA < 2.0: $anyFailing");
  print("ทุกคน GPA >= 2.0: $allPassing");
}
```

**ขั้นตอนที่ 2** กด Run และอ่านผลลัพธ์ทุกส่วน
<img width="2952" height="1528" alt="image" src="https://github.com/user-attachments/assets/d4893d2c-23b4-44f0-81d2-dca09356412c" />


**ขั้นตอนที่ 3** เพิ่มโค้ดต่อท้ายใน `main()` เพื่อกรองนักศึกษาเฉพาะคณะ "วิศวกรรม" แล้วแสดงผล

```dart
  // ทดลองเพิ่มเอง: กรองเฉพาะคณะวิศวกรรม
  print("\n=== นักศึกษาคณะวิศวกรรม ===");
  var engineeringStudents = students
      .where((s) => s["faculty"] == "วิศวกรรม")
      .toList();
  // พิมพ์ชื่อและ GPA ของนักศึกษาแต่ละคน
  for (var s in engineeringStudents) {
    print("  ${s["name"]}: ${s["gpa"]}");
  }
```
<img width="2870" height="1526" alt="image" src="https://github.com/user-attachments/assets/ec06d2dd-5061-417c-a573-98a2a72ab756" />

---

### 🎯 โจทย์ฝึกทำ 2 — เขียน Function ด้วยตนเอง

เขียนโค้ดโดยใช้ List ของนักศึกษาจากการทดลอง 2.2 เพิ่มเติม:

1. เขียน Function `findTopStudentByFaculty(List students, String faculty)` ที่คืนชื่อนักศึกษาที่ GPA สูงสุดในคณะที่ระบุ
2. เขียน Function `groupByFaculty(List students)` ที่คืน `Map<String, List>` โดยจัดกลุ่มนักศึกษาตามคณะ
3. ใช้ `sort()` เรียงนักศึกษาตาม GPA จากสูงไปต่ำ แล้วพิมพ์ข้อมูลนักศึกษาที่มี GPA สูงสุด 3 อันดับแรก

**บันทึกผลการทดลอง: บันทึกโค้ดคำสั่งที่ได้**
```dart
class Student {
  String name;
  String faculty;
  double gpa;

  Student(this.name, this.faculty, this.gpa);

  @override
  String toString() {
    return '$name (GPA: $gpa)';
  }
}

// 1. Function หาชื่อนักศึกษาที่ GPA สูงสุดในคณะที่ระบุ
String? findTopStudentByFaculty(List<Student> students, String faculty) {
  // กรองเฉพาะนักศึกษาในคณะที่ระบุ
  var studentsInFaculty = students.where((s) => s.faculty == faculty).toList();
  
  if (studentsInFaculty.isEmpty) {
    return "ไม่พบข้อมูลนักศึกษาในคณะนี้";
  }

  // เรียงลำดับจาก GPA สูงไปต่ำ แล้วคืนค่าชื่อคนแรก
  studentsInFaculty.sort((a, b) => b.gpa.compareTo(a.gpa));
  return studentsInFaculty.first.name;
}

// 2. Function จัดกลุ่มนักศึกษาตามคณะ
Map<String, List<Student>> groupByFaculty(List<Student> students) {
  Map<String, List<Student>> grouped = {};
  
  for (var student in students) {
    if (!grouped.containsKey(student.faculty)) {
      grouped[student.faculty] = [];
    }
    grouped[student.faculty]!.add(student);
  }
  
  return grouped;
}

void main() {
  // ข้อมูลจำลองอ้างอิงจากการทดลอง
  List<Student> students = [
    Student("Krittinai", "Engineering", 3.85),
    Student("Nawapon", "Engineering", 3.40),
    Student("Alice", "Science", 3.92),
    Student("Bob", "Science", 3.15),
    Student("Charlie", "Engineering", 3.95),
    Student("David", "Business", 3.60),
    Student("Eve", "Business", 3.80),
  ];

  print("--- ผลการทดสอบ findTopStudentByFaculty ---");
  String targetFaculty = "Engineering";
  String? topStudent = findTopStudentByFaculty(students, targetFaculty);
  print("นักศึกษาที่ได้ GPA สูงสุดในคณะ $targetFaculty คือ: $topStudent");
  print("");

  print("--- ผลการทดสอบ groupByFaculty ---");
  Map<String, List<Student>> groupedStudents = groupByFaculty(students);
  groupedStudents.forEach((faculty, studentList) {
    print("คณะ $faculty:");
    for (var student in studentList) {
      print("  - ${student.name} (GPA: ${student.gpa})");
    }
  });
  print("");

  print("--- ผลการเรียงลำดับและแสดง Top 3 GPA ---");
  // 3. ใช้ sort() เรียงนักศึกษาตาม GPA จากสูงไปต่ำ
  // สร้าง List ใหม่เพื่อไม่ให้กระทบกับ List ต้นฉบับ
  List<Student> sortedStudents = List.from(students);
  sortedStudents.sort((a, b) => b.gpa.compareTo(a.gpa));

  // พิมพ์ข้อมูลนักศึกษาที่มี GPA สูงสุด 3 อันดับแรก
  for (int i = 0; i < 3 && i < sortedStudents.length; i++) {
    print("อันดับ ${i + 1}: ${sortedStudents[i].name} - GPA: ${sortedStudents[i].gpa} (คณะ ${sortedStudents[i].faculty})");
  }
}

```
<img width="3012" height="1502" alt="image" src="https://github.com/user-attachments/assets/af6d7085-e752-4df8-920e-f02c3af74a4b" />

---

## ส่วนที่ 3 — ทฤษฎีและการทดลอง: OOP

### ทฤษฎี 3.1 — Class และ Object

**Class** คือ "แบบพิมพ์" หรือ "Blueprint" ของ Object ส่วน **Object** คือ "สิ่งของ" ที่สร้างจาก Class นั้น

```
Class (แบบพิมพ์):          Object (สิ่งของ):
┌───────────────────┐      ┌───────────────────┐
│  class Student {  │  →   │  student1         │
│    String name;   │      │    name: "สมชาย"  │
│    int age;       │      │    age: 20        │
│    study() {...}  │      │    study() → ทำงาน│
│  }                │      └───────────────────┘
└───────────────────┘
                           ┌───────────────────┐
                       →   │  student2         │
                           │    name: "สมหญิง"  │
                           │    age: 21        │
                           └───────────────────┘
```

**โครงสร้าง Class ใน Dart:**

```dart
class BankAccount {
  // === Fields (ตัวแปรของ Object) ===
  final String accountNumber;   // final = เปลี่ยนหลัง Constructor ไม่ได้
  final String ownerName;
  double _balance;               // _ นำหน้า = private (ใช้ได้ใน Class นี้เท่านั้น)
  List<String> _transactions = [];

  // === Constructor (สร้าง Object) ===
  // this.xxx = กำหนดค่า field โดยตรง ไม่ต้องเขียน body
  BankAccount({
    required this.accountNumber,
    required this.ownerName,
    double initialBalance = 0,
  }) : _balance = initialBalance; // initializer list

  // Named Constructor — สร้าง Object แบบพิเศษ
  BankAccount.savings({required String owner})
      : accountNumber = "SAV${DateTime.now().millisecondsSinceEpoch}",
        ownerName = owner,
        _balance = 0;

  // === Getter — อ่านค่าแบบ Property ===
  double get balance => _balance;
  bool get isEmpty => _balance == 0;
  List<String> get transactions => List.unmodifiable(_transactions);

  // === Methods ===
  bool deposit(double amount) {
    if (amount <= 0) return false;
    _balance += amount;
    _transactions.add("ฝาก: +${amount.toStringAsFixed(2)}");
    return true;
  }

  bool withdraw(double amount) {
    if (amount <= 0 || amount > _balance) return false;
    _balance -= amount;
    _transactions.add("ถอน: -${amount.toStringAsFixed(2)}");
    return true;
  }

  // toString — แสดงเมื่อ print(object)
  @override
  String toString() =>
      "บัญชี[$accountNumber] ของ $ownerName ยอด: ${_balance.toStringAsFixed(2)}";
}
```

---

### ทฤษฎี 3.2 — Inheritance (การสืบทอด) และ Abstract Class

**Inheritance** คือการสร้าง Class ใหม่จาก Class เดิม โดยสืบทอดทุกอย่างมา แล้วเพิ่มหรือแก้ไขเฉพาะส่วนที่ต่าง

**Abstract Class** คือ Class ที่กำหนด "สัญญา" ว่า Subclass ต้อง implement อะไรบ้าง ไม่สามารถสร้าง Object โดยตรงได้

```
Abstract Class (สัญญา):
┌───────────────────────────────────────────┐
│  abstract class Shape {                   │
│    double get area;       ← ต้อง implement │
│    double get perimeter;  ← ต้อง implement │
│    void describe() {...}  ← มาให้แล้ว       │
│  }                                        │
└───────────────────────────────────────────┘
              ↑ extends
   ┌──────────┴──────────┐
   │                     │
Circle               Rectangle
┌──────────┐       ┌──────────────┐
│ radius   │       │ width,height │
│ area=πr² │       │ area=w*h     │
│ perim=2πr│       │ perim=2(w+h) │
└──────────┘       └──────────────┘
```

```dart
abstract class Shape {
  // Abstract getter — Subclass ต้อง implement
  double get area;
  double get perimeter;

  // Concrete method — มาให้แล้ว Subclass ใช้ได้เลย
  void describe() {
    print("${runtimeType}:");
    print("  พื้นที่: ${area.toStringAsFixed(2)} ตร.หน่วย");
    print("  เส้นรอบรูป: ${perimeter.toStringAsFixed(2)} หน่วย");
  }

  bool isLargerThan(Shape other) => area > other.area;
}

class Circle extends Shape {
  final double radius;
  Circle(this.radius);

  @override
  double get area => 3.14159 * radius * radius;

  @override
  double get perimeter => 2 * 3.14159 * radius;
}

class Rectangle extends Shape {
  final double width;
  final double height;
  Rectangle(this.width, this.height);

  @override
  double get area => width * height;

  @override
  double get perimeter => 2 * (width + height);

  // Subclass สามารถเพิ่ม method เองได้
  bool get isSquare => width == height;
}
```

---

### ทฤษฎี 3.3 — Mixin

**Mixin** คือชุด Method ที่เพิ่มให้กับ Class ได้ โดยไม่ต้อง Inherit (เหมาะเมื่อต้องการเพิ่ม Behavior หลายชุดให้กับ Class เดียว)

```
ปัญหา: Dart ให้ extends ได้แค่ 1 Class
แต่บางครั้งต้องการ Behavior จากหลายที่

Mixin แก้ปัญหาโดย:
class Duck extends Animal with Swimmable, Flyable {
  // Duck ได้ทั้ง swim() และ fly() โดยไม่ต้อง inherit สองชั้น
}
```

```dart
mixin Printable {
  // Mixin สามารถมี Method และ Getter
  void printInfo() {
    print(toString()); // เรียก toString() ของ Class ที่ใช้ Mixin
  }
}

mixin Saveable {
  Map<String, dynamic> toJson(); // บังคับให้ Class ที่ใช้ implement

  String toJsonString() {
    var json = toJson();
    return json.entries.map((e) => '"${e.key}": "${e.value}"').join(", ");
  }
}

// ใช้ Mixin หลายตัวพร้อมกัน
class Product with Printable, Saveable {
  final String name;
  final double price;
  final int stock;

  Product({required this.name, required this.price, required this.stock});

  @override
  Map<String, dynamic> toJson() => {
    "name": name,
    "price": price,
    "stock": stock,
  };

  @override
  String toString() => "$name (฿${price.toStringAsFixed(2)}) เหลือ $stock ชิ้น";
}
```

---

### การทดลอง 3.1 — Class และ Inheritance

**⏱ เวลา:** 30 นาที

**ขั้นตอนที่ 1** ล้างโค้ดแล้วพิมพ์โค้ด BankAccount:

```dart
class BankAccount {
  final String ownerName;
  double _balance;
  List<String> _history = [];

  BankAccount({required this.ownerName, double initial = 0})
      : _balance = initial;

  double get balance => _balance;
  List<String> get history => List.unmodifiable(_history);

  bool deposit(double amount) {
    if (amount <= 0) {
      print("❌ จำนวนเงินต้องมากกว่า 0");
      return false;
    }
    _balance += amount;
    _history.add("+ ฝาก ${amount.toStringAsFixed(2)} บาท (ยอดคงเหลือ: ${_balance.toStringAsFixed(2)})");
    print("✅ ฝาก ${amount.toStringAsFixed(2)} บาท สำเร็จ");
    return true;
  }

  bool withdraw(double amount) {
    if (amount <= 0) {
      print("❌ จำนวนเงินต้องมากกว่า 0");
      return false;
    }
    if (amount > _balance) {
      print("❌ ยอดเงินไม่เพียงพอ (มี ${_balance.toStringAsFixed(2)} บาท)");
      return false;
    }
    _balance -= amount;
    _history.add("- ถอน ${amount.toStringAsFixed(2)} บาท (ยอดคงเหลือ: ${_balance.toStringAsFixed(2)})");
    print("✅ ถอน ${amount.toStringAsFixed(2)} บาท สำเร็จ");
    return true;
  }

  void printStatement() {
    print("\n=== สรุปบัญชี: $ownerName ===");
    print("ยอดปัจจุบัน: ${_balance.toStringAsFixed(2)} บาท");
    print("ประวัติรายการ:");
    if (_history.isEmpty) {
      print("  (ยังไม่มีรายการ)");
    } else {
      _history.forEach((h) => print("  $h"));
    }
  }

  @override
  String toString() => "BankAccount(${ownerName}, ยอด: ${_balance.toStringAsFixed(2)})";
}
```

**ขั้นตอนที่ 2** เพิ่ม Subclass SavingsAccount ที่มีดอกเบี้ย:

```dart
class SavingsAccount extends BankAccount {
  final double interestRate; // อัตราดอกเบี้ยต่อปี เช่น 0.03 = 3%

  SavingsAccount({
    required String ownerName,
    required this.interestRate,
    double initial = 0,
  }) : super(ownerName: ownerName, initial: initial);

  // Override withdraw เพื่อเพิ่มกฎพิเศษ
  @override
  bool withdraw(double amount) {
    if (_balance - amount < 500) {
      print("❌ บัญชีออมทรัพย์ต้องมียอดขั้นต่ำ 500 บาท");
      return false;
    }
    return super.withdraw(amount); // เรียก withdraw() ของ BankAccount
  }

  // Method พิเศษของ SavingsAccount
  void applyMonthlyInterest() {
    double interest = _balance * interestRate / 12;
    _balance += interest;
    _history.add("+ ดอกเบี้ยรายเดือน ${interest.toStringAsFixed(2)} บาท");
    print("✅ ดอกเบี้ยเดือนนี้: ${interest.toStringAsFixed(2)} บาท");
  }
}
```

**ขั้นตอนที่ 3** เพิ่ม `main()` และรัน:

```dart
void main() {
  print("=== ทดสอบ BankAccount ===\n");
  var acc = BankAccount(ownerName: "สมชาย", initial: 1000);

  acc.deposit(500);
  acc.withdraw(200);
  acc.withdraw(2000); // เกินยอด
  acc.withdraw(-100); // ค่าไม่ถูก
  acc.printStatement();

  print("\n=== ทดสอบ SavingsAccount ===\n");
  var savings = SavingsAccount(
    ownerName: "สมหญิง",
    interestRate: 0.03,
    initial: 1000,
  );

  savings.deposit(5000);
  savings.withdraw(5600); // เหลือน้อยกว่า 500
  savings.withdraw(3000); // ได้
  savings.applyMonthlyInterest();
  savings.printStatement();

  // Polymorphism — ใช้ BankAccount แทนทั้งคู่ได้
  print("\n=== Polymorphism ===");
  List<BankAccount> accounts = [acc, savings];
  for (var account in accounts) {
    print(account); // เรียก toString() ของแต่ละ Object
  }
}
```

**ขั้นตอนที่ 4** กด Run และอ่านผลลัพธ์ทุกบรรทัด
<img width="2960" height="1502" alt="image" src="https://github.com/user-attachments/assets/e407c774-e91e-455e-a0bc-71bd0b7f1867" />

---

### 🎯 โจทย์ฝึกทำ 3 — เขียน Class ด้วยตนเอง

1. สร้าง `CheckingAccount extends BankAccount` ที่อนุญาตให้ถอนเกินยอดได้ไม่เกิน 500 บาท (Overdraft) และคิดค่าธรรมเนียม 50 บาท เมื่อ Overdraft

2. สร้าง Abstract Class `Vehicle` ที่มี Abstract getter `fuelEfficiency` (กม./ลิตร), method `refuel(double liters)`, method `drive(double km)` ที่คำนวณการใช้น้ำมัน จากนั้นสร้าง `Car` และ `Truck` ที่ extend `Vehicle` โดยมี `fuelEfficiency` ต่างกัน

3. สร้าง Mixin `Discountable` ที่มี method `applyDiscount(double percent)` แล้วนำไปใช้กับ Class `Product` ที่มี `name` และ `price`


**บันทึกผลการทดลอง: บันทึกโค้ดคำสั่งที่ได้**
```dart

class BankAccount {
  String accountHolder;
  double balance;

  BankAccount(this.accountHolder, this.balance);

  void deposit(double amount) {
    balance += amount;
    print('ฝากเงิน $amount บาท | ยอดคงเหลือปัจจุบัน: $balance บาท');
  }

  void withdraw(double amount) {
    if (balance >= amount) {
      balance -= amount;
      print('ถอนเงิน $amount บาท | ยอดคงเหลือปัจจุบัน: $balance บาท');
    } else {
      print('ยอดเงินไม่พอสำหรับการถอนทั่วไป');
    }
  }
}

class CheckingAccount extends BankAccount {
  final double overdraftLimit = 500.0;
  final double overdraftFee = 50.0;

  CheckingAccount(String accountHolder, double balance) : super(accountHolder, balance);

  @override
  void withdraw(double amount) {
    // คำนวณว่าเงินในบัญชีพอไหม ถ้าไม่พอ ต้องใช้สิทธิ์ถอนเกินบัญชี (Overdraft)
    if (balance >= amount) {
      balance -= amount;
      print('[Checking] ถอนเงิน $amount บาท | ยอดคงเหลือ: $balance บาท');
    } else if (balance + overdraftLimit >= amount) {
      // ยอดเงิน + วงเงินถอนเกิน พอชำระเงินต้น
      balance -= amount; // ยอดเงินจะติดลบ
      balance -= overdraftFee; // หักค่าธรรมเนียมเพิ่มเติม
      print('[Checking] ถอนเงินเกินบัญชี! จำนวน $amount บาท (หักค่าธรรมเนียม Overdraft $overdraftFee บาท) | ยอดคงเหลือ: $balance บาท');
    } else {
      print('[Checking] ไม่สามารถถอนเงินได้เนื่องจากเกินวงเงิน Overdraft ($overdraftLimit บาท)');
    }
  }
}

abstract class Vehicle {
  double fuelRemaining; // ปริมาณน้ำมันที่มีอยู่ (ลิตร)

  Vehicle(this.fuelRemaining);

  // Abstract getter ที่บังคับให้ Class ลูกไปกรอกตัวเลขเอง
  double get fuelEfficiency; 

  void refuel(double liters) {
    fuelRemaining += liters;
    print('${runtimeType}: เติมน้ำมัน $liters ลิตร | น้ำมันคงเหลือ: $fuelRemaining ลิตร');
  }

  void drive(double km) {
    // คำนวณปริมาณน้ำมันที่ต้องใช้ = ระยะทาง / อัตราประหยัดน้ำมัน
    double fuelNeeded = km / fuelEfficiency;

    if (fuelRemaining >= fuelNeeded) {
      fuelRemaining -= fuelNeeded;
      print('${runtimeType}: ขับขี่เป็นระยะทาง $km กม. | ใช้น้ำมันไป ${fuelNeeded.toStringAsFixed(2)} ลิตร | น้ำมันคงเหลือ: ${fuelRemaining.toStringAsFixed(2)} ลิตร');
    } else {
      print('${runtimeType}: น้ำมันไม่พอสำหรับระยะทาง $km กม. (ต้องการน้ำมันอีก ${(fuelNeeded - fuelRemaining).toStringAsFixed(2)} ลิตร)');
    }
  }
}

class Car extends Vehicle {
  Car(double fuelRemaining) : super(fuelRemaining);

  @override
  double get fuelEfficiency => 15.0; // รถยนต์ทั่วไปวิ่งได้ 15 กม./ลิตร
}

class Truck extends Vehicle {
  Truck(double fuelRemaining) : super(fuelRemaining);

  @override
  double get fuelEfficiency => 6.0; // รถบรรทุกกินน้ำมัน วิ่งได้ 6 กม./ลิตร
}

mixin Discountable {
  // ฟังก์ชันคำนวณส่วนลดโดยอิงจาก field price ที่จะถูกเรียกใช้ใน Class ปลายทาง
  double calculateDiscountedPrice(double currentPrice, double percent) {
    double discount = currentPrice * (percent / 100);
    return currentPrice - discount;
  }
}

class Product with Discountable {
  String name;
  double price;

  Product(this.name, this.price);

  void applyDiscount(double percent) {
    double oldPrice = price;
    price = calculateDiscountedPrice(price, percent);
    print('สินค้า: $name | ราคาเดิม: $oldPrice บาท | รับส่วนลด $percent% | ราคาใหม่: $price บาท');
  }
}

void main() {
  print('--- ทดสอบข้อ 1: CheckingAccount ---');
  var myAccount = CheckingAccount('Phuwish', 1000.0);
  myAccount.withdraw(800);  // ถอนปกติ ยอดเหลือ 200
  myAccount.withdraw(500);  // ถอนเกินบัญชี (ใช้ Overdraft) ติดลบ 300 + ค่าธรรมเนียม 50 -> ยอดเหลือ -350
  myAccount.withdraw(200);  // เกินวงเงิน Overdraft 500
  print('');

  print('--- ทดสอบข้อ 2: Car & Truck ---');
  Car myCar = Car(20.0);    // มีน้ำมัน 20 ลิตร
  myCar.drive(150);         // ขับ 150 กม. ใช้ 10 ลิตร (เหลือ 10)
  
  Truck myTruck = Truck(20.0); // มีน้ำมัน 20 ลิตร
  myTruck.drive(150);       // ขับ 150 กม. ใช้ 25 ลิตร (น้ำมันไม่พอ)
  print('');

  print('--- ทดสอบข้อ 3: Mixin Discountable ---');
  Product gadget = Product('Gaming Mouse', 2500.0);
  gadget.applyDiscount(15); // ลดราคา 15%
}


```
<img width="2924" height="1430" alt="image" src="https://github.com/user-attachments/assets/89ae0385-e8c5-4084-bcc5-f3b8e9641d58" />

---

## ส่วนที่ 4 — ทฤษฎีและการทดลอง: Async/Await และ Future

### ทฤษฎี 4.1 — ทำไม Mobile App ถึงต้องมี Async?

ลองนึกถึงแอปสั่งอาหาร เมื่อผู้ใช้กด "สั่งอาหาร" แอปต้องส่งข้อมูลไปยัง Server แล้วรอการยืนยัน ซึ่งอาจใช้เวลา 1-3 วินาที

```
ถ้าใช้ Synchronous (รอแบบบล็อก):
────────────────────────────────────────────────────
Main Thread: [วาด UI][รอ Server 3 วินาที.........][วาด UI]
                          ↑
             ผู้ใช้กดอะไรก็ไม่ตอบสนอง
             ระบบแจ้ง "App Not Responding"

ถ้าใช้ Asynchronous (รอแบบไม่บล็อก):
────────────────────────────────────────────────────
Main Thread: [วาด][แสดง Loading][วาด][วาด][แสดงผล]
Background:  [ส่งไป Server──────────►รับผล]
```

**Future คืออะไร:**

```
Future<T> = "คำสัญญาว่าจะได้ T ในอนาคต"

Future<String>  → จะได้ String ในภายหลัง
Future<int>     → จะได้ int ในภายหลัง
Future<void>    → จะเสร็จในภายหลัง (ไม่มีค่าคืน)

สถานะของ Future:
┌─────────────┐    ┌──────────────┐    ┌─────────────┐
│  Pending    │ →  │  Completed   │ or │   Error     │
│  (รอผล)     │    │  (มีผลแล้ว)    │    │  (เกิดข้อ     │
│             │    │              │    │   ผิดพลาด)   │
└─────────────┘    └──────────────┘    └─────────────┘
```

---

### ทฤษฎี 4.2 — async/await และ Error Handling

```dart
// ประกาศ async function
Future<String> fetchUserName(int userId) async {
  // await หยุดรอ Future โดยไม่บล็อก Thread หลัก
  await Future.delayed(Duration(seconds: 1)); // จำลองการรอ Network

  if (userId <= 0) {
    // throw ส่ง Error ออกไป
    throw ArgumentError("userId ต้องมากกว่า 0");
  }
  return "ผู้ใช้ #$userId";
}

// เรียกใช้ด้วย await
void main() async {
  // try/catch จัดการ Error
  try {
    String name = await fetchUserName(1);
    print("ได้รับ: $name");

    // จะ throw ArgumentError
    await fetchUserName(-1);

  } on ArgumentError catch (e) {
    // จับ Error เฉพาะประเภท
    print("Argument Error: $e");
  } catch (e, stackTrace) {
    // จับ Error ทุกประเภท
    print("Error: $e");
    print("Stack: $stackTrace");
  } finally {
    // รันเสมอ ไม่ว่าจะ Error หรือไม่
    print("เสร็จสิ้น");
  }
}
```

**Sequential vs Parallel Execution:**

```dart
// Sequential — รอทีละอย่าง (เสียเวลา)
Future<void> loadSequential() async {
  var user    = await fetchUser(1);      // รอ 1 วินาที
  var posts   = await fetchPosts(1);     // รออีก 0.8 วินาที
  var friends = await fetchFriends(1);   // รออีก 0.5 วินาที
  // รวม ~2.3 วินาที
}

// Parallel — รอพร้อมกัน (เร็วกว่า)
Future<void> loadParallel() async {
  var results = await Future.wait([
    fetchUser(1),       // ╗
    fetchPosts(1),      // ╠═ รันพร้อมกันทั้งหมด
    fetchFriends(1),    // ╝
  ]);
  // รวม ~1 วินาที (เท่ากับ task ที่นานสุด)
  var user    = results[0];
  var posts   = results[1];
  var friends = results[2];
}
```

**Stream — ข้อมูลที่ไหลต่อเนื่อง:**

```dart
// Stream คือ "ท่อ" ที่ส่งข้อมูลหลายๆ ครั้งตามเวลา
Stream<int> countDown(int from) async* {
  for (int i = from; i >= 0; i--) {
    await Future.delayed(Duration(seconds: 1));
    yield i; // ส่งค่าออกทาง Stream
  }
}

// รับข้อมูลจาก Stream
void main() async {
  await for (int count in countDown(5)) {
    print("นับถอยหลัง: $count");
  }
  print("🚀 ปล่อย!");
}
```

---

### การทดลอง 4.1 — Future และ async/await

**⏱ เวลา:** 25 นาที

**ขั้นตอนที่ 1** ล้างโค้ดแล้วพิมพ์โค้ดจำลอง API:

```dart
import 'dart:async';

// จำลอง Database/API functions
Future<Map<String, dynamic>> fetchUser(int id) async {
  print("  [API] กำลังดึงข้อมูล User $id...");
  await Future.delayed(Duration(milliseconds: 800)); // จำลอง Network delay

  if (id <= 0) throw Exception("User ID ไม่ถูกต้อง");

  return {
    "id": id,
    "name": "ผู้ใช้ที่ $id",
    "email": "user$id@example.com",
    "role": id == 1 ? "admin" : "user",
  };
}

Future<List<String>> fetchUserPosts(int userId) async {
  print("  [API] กำลังดึง Posts ของ User $userId...");
  await Future.delayed(Duration(milliseconds: 600));

  return [
    "โพสต์ที่ 1 ของ User $userId",
    "โพสต์ที่ 2 ของ User $userId",
    "โพสต์ที่ 3 ของ User $userId",
  ];
}

Future<int> fetchUserFollowers(int userId) async {
  print("  [API] กำลังดึงจำนวน Follower ของ User $userId...");
  await Future.delayed(Duration(milliseconds: 500));
  return userId * 42; // จำลอง
}
```

**ขั้นตอนที่ 2** เพิ่ม `main()` ทดสอบแบบ Sequential:

```dart
void main() async {
  print("=== Sequential (รอทีละอย่าง) ===");
  var stopwatch = Stopwatch()..start();

  var user      = await fetchUser(1);
  var posts     = await fetchUserPosts(1);
  var followers = await fetchUserFollowers(1);

  stopwatch.stop();
  print("ชื่อ: ${user['name']}");
  print("จำนวนโพสต์: ${posts.length}");
  print("Followers: $followers คน");
  print("เวลาที่ใช้: ${stopwatch.elapsedMilliseconds}ms\n");

  // === Parallel ===
  print("=== Parallel (Future.wait) ===");
  stopwatch = Stopwatch()..start();

  var results = await Future.wait([
    fetchUser(2),
    fetchUserPosts(2),
    fetchUserFollowers(2),
  ]);

  stopwatch.stop();
  var user2      = results[0] as Map<String, dynamic>;
  var posts2     = results[1] as List<String>;
  var followers2 = results[2] as int;

  print("ชื่อ: ${user2['name']}");
  print("จำนวนโพสต์: ${posts2.length}");
  print("Followers: $followers2 คน");
  print("เวลาที่ใช้: ${stopwatch.elapsedMilliseconds}ms");
}
```

**ขั้นตอนที่ 3** กด Run สังเกตความแตกต่างของเวลา
<img width="3004" height="1466" alt="image" src="https://github.com/user-attachments/assets/cfb0c872-cce7-4de5-a9ac-448a8c97c5fc" />

**ขั้นตอนที่ 4** เพิ่ม Error Handling ต่อท้าย `main()`:

```dart
  // === Error Handling ===
  print("\n=== Error Handling ===");

  try {
    print("ลองดึง User ID = -1:");
    var badUser = await fetchUser(-1);
    print("ได้รับ: $badUser"); // ไม่ถึงบรรทัดนี้
  } on Exception catch (e) {
    print("❌ Exception: $e");
  }

  print("โปรแกรมยังทำงานต่อได้หลัง Error ✅");
```

**ขั้นตอนที่ 5** กด Run อีกครั้ง บันทึกผลเวลาของ Sequential vs Parallel

```
บันทึกผลการทดลอง:
Sequential ใช้เวลา: 3611ms
Parallel ใช้เวลา:   999ms
ประหยัดเวลาได้:    2612ms (72.33 %)
```

---

### การทดลอง 4.2 — Stream

**⏱ เวลา:** 15 นาที

**ขั้นตอนที่ 1** ล้างโค้ดแล้วพิมพ์:

```dart
import 'dart:async';

// Stream Generator ด้วย async*
Stream<double> simulateStockPrice(String symbol) async* {
  double price = 100.0;
  int ticks = 0;

  while (ticks < 5) {
    await Future.delayed(Duration(milliseconds: 500));

    // จำลองการเปลี่ยนราคา
    double change = (ticks % 2 == 0) ? 2.5 : -1.5;
    price += change;
    ticks++;

    yield price; // ส่งค่าออกทาง Stream
  }
}

void main() async {
  print("=== ราคาหุ้น (Stream) ===");
  print("Symbol: DART\n");

  double? lastPrice;

  await for (double price in simulateStockPrice("DART")) {
    String direction = "";
    if (lastPrice != null) {
      direction = price > lastPrice! ? "📈 ขึ้น" : "📉 ลง";
    }
    print("ราคา: ${price.toStringAsFixed(2)} บาท  $direction");
    lastPrice = price;
  }

  print("\nสิ้นสุดการแสดงราคา");
}
```

**ขั้นตอนที่ 2** กด Run สังเกตว่าราคาออกมาทีละค่า ไม่ใช่ทั้งหมดพร้อมกัน
<img width="2924" height="1478" alt="image" src="https://github.com/user-attachments/assets/18ce654b-b799-48c2-8bd4-6c06eacec776" />

---

### 🎯 โจทย์ฝึกทำ 4 — เขียน Async ด้วยตนเอง

1. สร้าง `Future<double> calculateTax(double income)` ที่มี delay 0.5 วินาที คืนค่าภาษีตามอัตราก้าวหน้า (income <= 150,000 → 0%, <= 300,000 → 5%, <= 500,000 → 10%, อื่นๆ → 20%)

2. เขียน `main()` ที่ดึงข้อมูลรายได้ของผู้ใช้ 3 คนพร้อมกัน (ใช้ `Future.wait`) แล้วคำนวณภาษีแต่ละคน และแสดงผลรวมภาษีทั้งหมด

3. สร้าง `Stream<String>` ที่จำลองการส่ง Chat Message ทุก 1 วินาที เป็นเวลา 5 ครั้ง แล้วแสดงผลผ่าน `await for`

**บันทึกผลการทดลอง: บันทึกโค้ดคำสั่งที่ได้**
```dart
import 'dart:async';

void main() async {
  print('=== ข้อ 2: คำนวณภาษีของนักศึกษา/ผู้ใช้ 3 คนพร้อมกัน (Parallel) ===');
  Stopwatch stopwatch = Stopwatch()..start();

  List<double> incomes = [250000, 450000, 800000];

  // ใช้ Future.wait เพื่อคำนวณภาษีของทุกคนไปพร้อมๆ กัน
  List<double> taxes = await Future.wait(
    incomes.map((income) => calculateTax(income))
  );

  double totalTax = 0;
  for (int i = 0; i < incomes.length; i++) {
    print('ผู้ใช้คนที่ ${i + 1} รายได้: ${incomes[i]} บาท -> ภาษีที่ต้องจ่าย: ${taxes[i]} บาท');
    totalTax += taxes[i];
  }
  
  print('💰 รวมภาษีทั้งหมดที่ต้องจ่าย: $totalTax บาท');
  print('⏱️ เวลาที่ใช้ในการคำนวณรวม: ${stopwatch.elapsedMilliseconds} ms');
  print('--------------------------------------------------\n');

  print('=== ข้อ 3: จำลองการรับ Chat Message ผ่าน Stream (ทุก 1 วินาที) ===');
  // เรียกใช้ Stream และใช้ await for ในการดึงข้อมูลที่ทยอยส่งมา
  await for (String message in getChatStream()) {
    print(message);
  }
  print('✨ สิ้นสุดการรับข้อความ');
}

// -------------------------------------------------------------
// ข้อ 1: Function คำนวณภาษีอัตราก้าวหน้า (มี Delay 0.5 วินาที)
// -------------------------------------------------------------
Future<double> calculateTax(double income) async {
  // จำลอง Delay 0.5 วินาที (500 มิลลิวินาที)
  await Future.delayed(Duration(milliseconds: 500));

  // คำนวณภาษีแบบขั้นบันได (Progressive Tax)
  if (income <= 150000) {
    return 0.0;
  } else if (income <= 300000) {
    return (income - 150000) * 0.05;
  } else if (income <= 500000) {
    // 150k แรก 0% + 150k ต่อมา 5% (7,500) + ส่วนที่เกิน 300k คูณ 10%
    return 7500 + (income - 300000) * 0.10;
  } else {
    // 150k แรก 0% + 150k ต่อมา 5% (7,500) + 200k ต่อมา 10% (20,000) + ส่วนที่เกิน 500k คูณ 20%
    return 7500 + 20000 + (income - 500000) * 0.20;
  }
}

// -------------------------------------------------------------
// ข้อ 3: Stream Generator สำหรับส่ง Chat Message 5 ครั้ง
// -------------------------------------------------------------
Stream<String> getChatStream() async* {
  List<String> messages = [
    '📥[User]: สวัสดีครับ มีเรื่องสอบถามหน่อยครับ',
    '📥[User]: ระบบ Docker ของผมทำงานช้าผิดปกติ',
    '📥[User]: ไม่แน่ใจว่าเป็นเพราะการจัดการ Volume หรือเปล่า',
    '📥[User]: รบกวนช่วยตรวจสอบโครงสร้าง docker-compose ให้หน่อยครับ',
    '📥[User]: ขอบคุณล่วงหน้าครับสำหรับคำแนะนำ!'
  ];

  for (var msg in messages) {
    await Future.delayed(Duration(seconds: 1)); // รอ 1 วินาที
    yield msg; // ส่งข้อความออกไปใน Stream ทีละข้อความ
  }
}

```
---
<img width="2832" height="1476" alt="image" src="https://github.com/user-attachments/assets/a13a203f-2959-4735-9dd3-8538aec6b626" />


### **ข้อ 1: อธิบายความแตกต่างระหว่าง `final` และ `const` พร้อมยกตัวอย่าง**

| คุณสมบัติ | `final` | `const` |
| :--- | :--- | :--- |
| **การกำหนดค่า** | กำหนดค่าได้**ครั้งเดียว** (หลังจากนั้นเปลี่ยนไม่ได้) | กำหนดค่าได้**ครั้งเดียว** และต้องเป็นค่าที่คงที่แน่นอน |
| **ช่วงเวลาที่รู้ค่า (Evaluation)** | **Run-time** (รู้ค่าเมื่อรันโปรแกรมไปถึงจุดนั้น) | **Compile-time** (ต้องรู้ค่าทันทีตั้งแต่ตอนคอมไพล์โค้ด) |
| **ความสัมพันธ์ใน Class** | เป็น Instance variable ของ Class ได้ปกติ | เป็น Instance variable ตรงๆ ไม่ได้ (ต้องประกาศร่วมกับ `static`) |

* **ตัวอย่างการใช้ `final`:** เหมาะกับข้อมูลที่เรายังไม่รู้ค่าในขณะเขียนโค้ด แต่จะรู้เมื่อรันโปรแกรม เช่น ข้อมูลที่ดึงมาจาก API, เวลาปัจจุบัน (`DateTime.now()`), หรือค่าที่รับมาจาก User Input
    ```dart
    final String username = fetchUsernameFromDatabase(); 
    final DateTime loginTime = DateTime.now(); // รู้เวลาตอนรันโปรแกรมเท่านั้น
    ```
* **ตัวอย่างการใช้ `const`:** เหมาะกับค่าคงที่ทางคณิตศาสตร์, ค่า Configuration ของระบบ หรือ UI Widget ที่ไม่มีวันเปลี่ยนแปลง เพื่อให้คอมไพเลอร์ประหยัดหน่วยความจำ
    ```dart
    const double pi = 3.14159; // ค่าคงที่ทางคณิตศาสตร์ที่รู้แน่นอน
    const double gravity = 9.8;
    ```

---

### **ข้อ 2: Named Parameters และ Positional Parameters ต่างกันอย่างไร? ควรเลือกใช้แบบไหนเมื่อไหร่?**

* **Positional Parameters (พารามิเตอร์ตามตำแหน่ง):**
    * **ลักษณะ:** เวลาเรียกใช้ฟังก์ชัน ต้องส่งค่าเรียงตามลำดับตำแหน่งที่ประกาศไว้ หากสลับลำดับข้อมูลจะเพี้ยนทันที
    * **การใช้งาน:** `void greet(String name, int age)` -> เรียกใช้: `greet('Phuwish', 21)`
* **Named Parameters (พารามิเตอร์ระบุชื่อ):**
    * **ลักษณะ:** ใช้เครื่องหมายปีกกา `{}` ครอบพารามิเตอร์ เวลาเรียกใช้ต้องระบุชื่อตัวแปรคู่กับค่า สลับลำดับการส่งได้ และสามารถใช้คีย์เวิร์ด `required` เพื่อบังคับให้ต้องส่งค่านั้นมาได้
    * **การใช้งาน:** `void greet({required String name, int? age})` -> เรียกใช้: `greet(age: 21, name: 'Phuwish')`

#### **แนวทางการเลือกใช้:**
* **เลือก Positional Parameters เมื่อ:** ฟังก์ชันมีพารามิเตอร์น้อยมาก (1-2 ตัว) และลำดับของมันเข้าใจง่ายชัดเจนในตัวเอง เช่น `add(int a, int b)`
* **เลือก Named Parameters เมื่อ:** ฟังก์ชันหรือ Constructor นั้นมีพารามิเตอร์จำนวนมาก (ตั้งแต่ 3 ตัวขึ้นไป) เพื่อเพิ่มความชัดเจนในการอ่านโค้ด (Readability) ป้องกันการส่งสลับประเภทข้อมูล และช่วยให้โค้ดฝั่ง Flutter UI มีความยืดหยุ่นสูงเวลาปรับแต่ง Widget

---

### **ข้อ 3: Abstract Class และ Mixin มีจุดประสงค์ต่างกันอย่างไร?**

* **Abstract Class (คลาสเสมือน):**
    * **จุดประสงค์:** ออกแบบมาเพื่อทำหน้าที่เป็น **"พิมพ์เขียว/โครงร่างหลัก"** สำหรับกำหนดโครงสร้างความสัมพันธ์แบบสายเลือดเดียวกัน (Is-A Relationship) โดยไม่สามารถนำมาสร้าง Object ตรงๆ ได้ แต่บังคับให้ Class ลูกที่จะสืบทอด (`extends`) ต้องนำคุณสมบัติเหล่านี้ไปเขียนต่อให้สมบูรณ์
    * **สถานการณ์ที่เหมาะ:** การทำระบบขนส่งที่ต้องการโครงหลักของพาหนะ เช่น สร้าง `Vehicle` เป็น Abstract class แล้วให้ `Car`, `Truck`, หรือ `Motorcycle` สืบทอดคุณสมบัติร่วมไปใช้งาน
* **Mixin (มิกซ์อิน):**
    * **จุดประสงค์:** ออกแบบมาเพื่อเป็น **"ชุดความสามารถเสริม"** ที่นำไปปะ/แปะ ร่วมกับ Class อื่นๆ ได้หลากหลายโดยไม่ต้องมีความสัมพันธ์ทางสายเลือด (Has-A / Share behavior) ทำให้ Class หนึ่งสามารถมีความสามารถจากหลายๆ แหล่งได้ (ช่วยเลี่ยงปัญหา Single Inheritance ของภาษา Dart)
    * **สถานการณ์ที่เหมาะ:** การทำระบบคำนวณส่วนลด เช่น สร้าง Mixin `Discountable` ขึ้นมา จากนั้นเราสามารถเอาไปแปะร่วมกับ Class `Product`, คลาส `ServiceFee` หรือคลาส `PremiumMember` เพื่อเพิ่มฟังก์ชันการคำนวณส่วนลดให้คลาสเหล่านั้นได้ทันทีโดยไม่ต้องสืบทอด Class แม่เดียวกัน

---

### **ข้อ 4: การเปรียบเทียบ Sequential และ Parallel (Future.wait) ในการรันแบบ Asynchronous**

> **บันทึกจากการทดลอง:**
> * **Sequential (ทำงานทีละขั้นตอน):** ใช้เวลาประมาณ **`3611 ms`**
> * **Parallel (ใช้ Future.wait ทำงานพร้อมกัน):** ใช้เวลาประมาณ **`999 ms`**

#### **เหตุผลที่ Parallel เร็วกว่า:**
การทำงานแบบ **Sequential** จะบังคับให้ Task ที่ 1 ทำงานเสร็จสมบูรณ์และได้ผลลัพธ์กลับมาก่อน ถึงจะเริ่มสั่งรัน Task ที่ 2 และ 3 ตามลำดับ เวลาที่ใช้ทั้งหมดจึงเกิดจาก**ผลรวมของเวลาทุก Task** ($1000\text{ms} + 1200\text{ms} + 1400\text{ms} \approx 3600\text{ms}$)
ในขณะที่ **Parallel (`Future.wait`)** จะสั่งยิง Network Request หรือทำงานทุกอย่างออกไปพร้อมๆ กันในเวลาเดียวกัน ทำให้เวลาที่ประหยัดไปได้คิดเป็น **`72.33%` (ลดลงถึง `2612 ms`)** โดยเวลาที่โปรแกรมต้องรอนั้นจะเท่ากับ**เวลาของ Task ที่ช้าที่สุดเพียงก้อนเดียวเท่านั้น** (ในที่นี้คือประมาณ `999 ms`)

#### **กรณีที่จำเป็นต้องใช้ Sequential แทน:**
เมื่อหนึ่งในงานนั้น**มีเงื่อนไขพึ่งพากันและกัน (Dependency)** หมายความว่าคุณจำเป็นต้องรอผลลัพธ์จาก Task แรกเพื่อนำไปใช้เป็น Parameter หรือเงื่อนไขในการรัน Task ถัดไป 
* **ตัวอย่าง:** คุณต้องการดึงประวัติการสั่งซื้อของผู้ใช้ ขั้นแรกต้องยิง API ไปยืนยันตัวตนเพื่อรับ `UserID` ก่อน (Sequential รอจนได้ไอดี) แล้วค่อยเอา `UserID` นั้นส่งไปขอข้อมูลการซื้อสินค้าจาก API ตัวที่สอง หากทำแบบขนานกันตั้งแต่แรก API ตัวที่สองจะทำงานไม่ได้เพราะยังไม่รู้ว่าใครเป็นคนขอข้อมูล

---

### **ข้อ 5: Future และ Stream ต่างกันอย่างไร? พร้อมตัวอย่างการพัฒนา Mobile App จริง**

| คุณสมบัติ | `Future` | `Stream` |
| :--- | :--- | :--- |
| **การส่งคืนผลลัพธ์** | คืนค่ากลับมาเพียง **"ครั้งเดียว" (Single Value)** ไม่ว่าจะเป็นผลลัพธ์สำเร็จหรือ Error แล้วจบงานทันที | ทยอยส่งผลลัพธ์กลับมาได้ **"หลายครั้งต่อเนื่อง" (Sequence of Values)** เป็นท่อส่งข้อมูล |
| **สถานะการสังเกต** | รอจนเสร็จสิ้นกระบวนการ (Pending -> Fulfilled/Rejected) | เปิดรับและจับตารอฟังข้อมูลที่เข้ามาเรื่อยๆ (Active / Listening) |

#### **ตัวอย่างการใช้จริงในการพัฒนา Mobile App:**
* **สถานการณ์ที่เหมาะกับ `Future`:**
    * **การเข้าสู่ระบบ (Login Request):** ส่ง Username/Password ไปเช็กที่ Server แล้วรอรับค่า Token กลับมาเพียงครั้งเดียวว่าผ่านหรือไม่ผ่าน
    * **การดาวน์โหลดรูปภาพ:** การยิง API ไปดึงข้อมูลรูปโปรไฟล์ผู้ใช้มาแสดงผลบน UI
* **สถานการณ์ที่เหมาะกับ `Stream`:**
    * **ระบบห้องแชท (Chat Application):** ต้องการดักฟังสัญญาณตลอดเวลา หากเพื่อนส่งข้อความใหม่เข้ามา โค้ดจะต้องดักรับข้อความที่ทยอยไหลเข้ามาใน Stream เพื่ออัปเดตหน้าจอแชททันทีโดยไม่ต้องกดรีเฟรชหน้าจอเอง
    * **การระบุตำแหน่ง GPS (Location Tracking):** การจับพิกัดแผนที่แบบ Real-time ของคนขับไรเดอร์ส่งอาหารที่พิกัดละติจูด/ลองจิจูดจะมีการเปลี่ยนแปลงและส่งอัปเดตเข้ามาทุกๆ วินาที
---

## ข้อผิดพลาดที่พบบ่อย

| Error Message | สาเหตุ | วิธีแก้ |
|---|---|---|
| `A value of type 'Null' can't be assigned...` | กำหนด null ให้ตัวแปร non-nullable | เพิ่ม `?` หรือกำหนดค่าเริ่มต้น |
| `The getter '...' isn't defined` | เรียก method ที่ไม่มีใน Type นั้น | ตรวจสอบ Type ของตัวแปร |
| `Non-abstract class '...' missing concrete implementation` | ไม่ได้ implement abstract method | เพิ่ม `@override` และ implement method |
| `Uncaught Error: ...` | Future throw Error แต่ไม่มี try/catch | ห่อด้วย try/catch |
| `setState() or markNeedsBuild()...` (บน Flutter) | เรียก setState หลัง dispose | เช็ค `if (mounted)` ก่อน setState |

---

*ใบงานการทดลองที่ 2-1 | Dart Programming*
*วิชา: การพัฒนาซอฟต์แวร์สำหรับอุปกรณ์เคลื่อนที่*
