# المحاضرة 4 — الواجب | Functions

**Engineer Ibrahim — كورس Python & AI (12 محاضرة) · أسئلة المحاضرة 4**

الملف ده **نصّي عن قصد**: تفتحه على GitHub أو Colab، تنسخ أي بلوك كود
بضغطة واحدة (زرار النسخ فوق الكود)، وتلصقه في نيوتبوكك وتشتغل عليه.

**التسليم على GitHub — في نيوتبوكين من إيدك أنت:**

- `lectures/lecture_04/exercises.ipynb` → حلول **الجزء A**
- `lectures/lecture_04/menu_v2.ipynb` → **الجزء B + C**: المينيو بعد التفكيك بدوال (قطعة المشروع)

**تحقق ذاتي:** كل سؤال ليه **checklist** تتأكّد منه بعينك و/أو **assert** —
كل واحد بيقول `ممتاز` أو يوضّح الغلط.

**سياسة AI:** تشرحلك وتلاقي الغلط — **ما تكتبشلك الحل**. الهدف إنك أنت تتعلم.

مدة الواجب 2–3 ساعات — يتسلّم قبل المحاضرة 5.

---

## الجزء A — تمارين المهارات (5 تمارين)

### A1 — تلات دوال صغيرة

عرّف الدوال الثلاثة دي: `is_even` ترجّع صح لو الرقم بيقبل القسمة على 2،
و `square` ترجّع الرقم في نفسه، و `average` ترجّع متوسط
القائمة. (التركيز على `def` و `return` — الحساب نفسه سهل.)

```python
def is_even(n):
    # TODO: رجّع صح لو الرقم بيقبل القسمة على 2
    pass


def square(n):
    # TODO: رجّع الرقم في نفسه
    pass


def average(values):
    # TODO: رجّع متوسط القائمة
    pass

print(is_even(4), square(7))
```

**تحقق ذاتي:**

```python
assert is_even(4) == True and is_even(7) == False, "is_even"
assert square(7) == 49, "square"
assert abs(average([82, 90, 76]) - 82.67) < 0.01, "average"
print("A1: ممتاز")
```

---

### A2 — معامل افتراضي

الدالة ترجّع تحية للشخص: من غير معامل تاني التحية الافتراضية
`"Ahlan"`، ولو بعتله كلمة تانية يستخدمها هو.

```python
def greet(name, greeting="Ahlan"):
    # TODO: رجّع التحية ثم الاسم — مفصولين بفاصلة ومسافة
    pass

print(greet("Sara"))
```

**تحقق ذاتي:**

```python
assert greet("Sara") == "Ahlan, Sara", "الافتراضي"
assert greet("Omar", "Marhaba") == "Marhaba, Omar", "موضعي"
assert greet(name="Ali", greeting="Hi") == "Hi, Ali", "مفتاحي"
print("A2: ممتاز")
```

---

### A3 — قيم مرجّعة أكثر من واحدة

دالة `min_max` ترجع أصغر رقم وأكبر رقم في القائمة —
رجّعهم **tuple** وفكّهم بـ unpacking زي ما اتعلمنا في L2.

```python
def min_max(values):
    # TODO: لفّ على القائمة وتتبّع أكبر وأصغر — ورجّع (الأصغر, الأكبر)
    pass

low, high = min_max([82, 90, 76])
print(low, high)
```

**تحقق ذاتي:**

```python
assert min_max([82, 90, 76]) == (76, 90), "الأصغر الأول"
assert min_max([5]) == (5, 5), "عنصر واحد"
assert min_max([-3, -9, 2]) == (-9, 2), "سالب وكبير"
print("A3: ممتاز")
```

---

### A4 — احفظ في ملف واقرا بأمان

اكتب السطور الثلاثة في ملف، وبعدين اقرا الملف كله. وجرّب كمان قراية ملف
**مش موجود** — لازم تمسك الخطأ بـ `try / except` بدل ما النيوتبوك يقع.

```python
lines = ["date,category", "2026-10-01,food", "2026-10-02,drink"]
path = "my_lines.txt"
content = ""

# TODO: with للكتابة — كل سطر في سطر جديد
# TODO: with للقراءة — النتيجة كلها في المتغيّر اللي فوق
# TODO: except FileNotFoundError لو الملف مش موجود

print(content)
```

**تحقق ذاتي:**

```python
assert "2026-10-01,food" in content, "السطور رجعت"
assert "2026-10-02,drink" in content, "كل السطور"
print("A4: ممتاز")
```

**Checklist:** الملف اتقفل لوحده بعد الـ with · قرايته من غير
نص محذوف · جربت قراية ملف غلط وشوفت رسالة الخطأ بعينك.

---

### A5 — نظّف الكود المكرّر (DRY)

نفس الحساب مكرّر تلات مرات. عرّف دالة `average3` بثلاث معاملات،
واستبدل الأسطر التلاتة بنادي الدالة.

```python
# الكود ده مكرّر — نظّفه بدالة
print((85 + 90 + 76) / 3)
print((100 + 55 + 60) / 3)
print((40 + 88 + 92) / 3)
```

```python
# TODO: عرّف الدالة بثلاث معاملات — واستبدل الأسطر بناديها
print(average3(85, 90, 76))
print(average3(100, 55, 60))
print(average3(40, 88, 92))
```

**تحقق ذاتي:**

```python
assert abs(average3(85, 90, 76) - 83.67) < 0.01, "الدالة شغالة"
print("A5: ممتاز")
```

---

## الجزء B — قطعة المشروع: tracker/functions.py + menu_v2.ipynb

**ده أهم جزء.** من L3 عندك مينيو شغّال — بس شفت إن كود الخيارات طويل
ومكرّر ومتناثر جوّا `while`. النهاردة نعمل **refactor**:
كل خيار يبقى **دالة** في ملف مستقل `tracker/functions.py`،
والمينيو الجديد `menu_v2.ipynb` بس بينادي الدوال. ونضيف الحفظ والقراءة
من `data/sample_records.csv`.

### الخطوات بالترتيب

1. خلية الإعداد (معطاة — شغّلها أول حاجة): تجهّز المجلدات.

```python
# menu_v2.ipynb — شغّلها أول حاجة
import os
os.makedirs("tracker", exist_ok=True)
os.makedirs("data", exist_ok=True)
```

2. اكتب محتوى `tracker/functions.py` في خلية جديدة —
   **أول سطر في الخلية** يكون `%%writefile tracker/functions.py`
   وبعده المحتوى ده كله:

```python
# tracker/functions.py
import csv


def add_record(records, record):
    """Append one record dict to the list."""
    # TODO: أضف السجل للقائمة وارجعها
    pass


def filter_records(records, category):
    """Return records matching the given category."""
    # TODO: لفّ واشرط — أعد قائمة جديدة بالمطابقين
    pass


def summarize(records):
    """Return count, total, average, biggest, smallest of amount_egp."""
    # TODO: نمط الملخص — لو القائمة فاضية رجّع أصفار
    pass


def save_records(records, path):
    """Write records to CSV with a header row."""
    # TODO: csv.writer — صف العناوين أول حاجة
    pass


def load_records(path):
    """Read CSV back into dicts; a missing file returns an empty list."""
    # TODO: except FileNotFoundError — لو الملف مش موجود رجّع قائمة فاضية
    # TODO: amount_egp -> float بعد القراءة
    pass
```

3. استورد الدوال وحمّل بياناتك. لو الملف لسه فاضي، الصق سجلاتك
   من المحاضرة اللي فاتت في القائمة وسيب الكود يحفظها:

```python
from tracker.functions import (add_record, filter_records, summarize,
                               save_records, load_records)

records = load_records("data/sample_records.csv")
if not records:
    records = []  # TODO: الصق هنا سجلاتك من المحاضرة اللي فاتت
    save_records(records, "data/sample_records.csv")
print(len(records))
```

4. المينيو الجديد — **8 خيارات** (خروج بقت 8 — لأنها بتحفظ
   قبل ما تقف): كل خيار ناديه بس — الشغل كله جوّا الدوال.

```python
while True:
    print("1) عرض الكل")
    print("2) إضافة سجل")
    print("3) تصفية حسب التصنيف")
    print("4) ملخص")
    print("5) بحث بالكلمة")
    print("6) حذف سجل")
    print("7) تعديل سجل")
    print("8) خروج وحفظ")
    choice = input("اختار: ")

    if choice == "1":
        for r in records:
            print(r)
    elif choice == "2":
        # TODO: اجمع الحقول وتحقق من الأرقام — ثم نادي الدالة
        pass
    elif choice == "3":
        category = input("التصنيف: ")
        for r in filter_records(records, category):
            print(r)
    elif choice == "4":
        print(summarize(records))
    elif choice == "5":
        word = input("كلمة: ")
        for r in records:
            if word in str(r):
                print(r)
    elif choice == "6":
        # TODO: اطبع السجلات بالرقم — ونادي دالة الحذف
        pass
    elif choice == "7":
        # TODO: اسأل رقم السجل والحقول الجديدة — ونادي دالة التعديل
        pass
    elif choice == "8":
        save_records(records, "data/sample_records.csv")
        print("اتحفظت! مع السلامة!")
        break
    else:
        print("اختيار غلط — جرّب تاني")
```

**ملاحظات:** خيار الإضافة زي ما كان في L3 بس بـ `try/except` على
الأرقام، وبعدين `add_record(records, new)`. والدالة
`summarize` ترجع قاموس فيه أرقام الملخص كله — اطبعه وهتشوف
العدد والمجموع والمتوسط والأعلى والأدنى.

**اعرض ملف الـ CSV قبل ما تلصقه على GitHub:**

```python
with open("data/sample_records.csv", "r", encoding="utf-8") as f:
    print(f.read())
```

### تحقّق ذاتي للجزء B

**Checklist — العب كل خيار من التمانية وتأكد بنفسك:**

| الخيار | لازم يحصل |
|---|---|
| 1 | يعرض كل السجلات من غير خطأ |
| 2 | يضيف سجل حقيقي — ويرفض حقل رقمي فيه حروف برسالة واضحة |
| 3 | يعرض سجلات التصنيف المطلوب بس |
| 4 | الأرقام مطابقة للحساب اليدوي على سجلاتك |
| 5 | يلاقي سجلاً تحتويه الكلمة اللي تكتبها |
| 6 | يحذف السجل اللي تختاره فعلًا |
| 7 | يغيّر الحقل المطلوب فعلًا |
| 8 | يحفظ في الملف ويقف (break) |
| اختيار غلط | يرجع القائمة من غير ما يقع |

**تحقّق برمجي — الصقه في آخر `menu_v2.ipynb` بعد ما تخلّص لعب:**

```python
assert isinstance(records, list) and len(records) >= 10, "records"
s = summarize(records)
assert s["count"] == len(records), "count"
assert s["smallest"] <= s["average"] <= s["biggest"], "bounds"
save_records(records, "data/sample_records.csv")
loaded = load_records("data/sample_records.csv")
assert len(loaded) == len(records), "roundtrip count"
assert loaded[0]["category"] == records[0]["category"], "roundtrip value"
assert isinstance(loaded[0]["amount_egp"], float), "amount float"
print("Part B: ممتاز")
```

**قبل التسليم:** **Run All** — واختار **8 (خروج وحفظ)** عشان النيوتبوك
يقف نضيف بعد ما يحفظ.

---

## الجزء C — حذف وتعديل (إلزامي)

ضيف دالتين جداد لـ `tracker/functions.py`، ووصّلهم بخيارات 6 و 7
في المينيو. الحذف والتعديل بالفهرس (رقم السجل — زي ما اتعلمنا في L2 إن
الفهرس بيبدأ من صفر).

```python
# tracker/functions.py — ضيف الدالتين دول
def delete_record(records, index):
    """Delete the record at index and return it."""
    # TODO: None لو الرقم برّا — احذف بالفهرس وارجّع المحذوف
    pass


def update_record(records, index, field, value):
    """Change one field of the record at index."""
    # TODO: غيّر الحقل المطلوب في السجل رقم الفهرس
    pass
```

**في المينيو:** خيار 6 يعرض السجلات مرقّمة بـ `enumerate` ويطلب
الرقم وينادي `delete_record` · وخيار 7 يطلب الرقم والحقول الجديدة
وينادي `update_record` — والاتنين ضيفهم في سطر الاستيراد في الخلية 3.
وبعد الحذف/التعديل: خيار 8 هو اللي بيعمل الحفظ.

**تحقق ذاتي:** الصقه في `menu_v2.ipynb` بعد الاستيراد.

```python
sample = [
    {"date": "2026-10-01", "category": "food", "amount_egp": 85.5,
     "payment_method": "cash", "quantity": 1},
    {"date": "2026-10-02", "category": "drink", "amount_egp": 40.0,
     "payment_method": "card", "quantity": 2},
]
t = sample.copy()

assert len(t) == 2, "sample"
deleted = delete_record(t, 0)
assert len(t) == 1, "delete"
assert deleted["category"] == "food", "return deleted"
update_record(t, 0, "amount_egp", 99.9)
assert t[0]["amount_egp"] == 99.9, "update"
print("Part C: ممتاز")
```

**Checklist:** حذفت سجل بالرقم واتأكدت إن العدّاد نقص ·
عدّلت حقل وشفت النتيجة في خيار 1 · جربت رقم برّا القائمة وشوفت رسالة واضحة
من غير ما النيوتبوك يقع.

---

## التسليم — معايير القبول

| المعيار | التفاصيل |
|---|---|
| الملفات في مكانها | `tracker/functions.py` + `data/sample_records.csv` في جذر الريبو · `lectures/lecture_04/exercises.ipynb` + `lectures/lecture_04/menu_v2.ipynb` |
| الدوال كاملة | **7 دوال** كلها شغالة ومعاها **docstring** — وواحد على الأقل ليه `try/except` |
| Run All نضيف | ينتهي بخروج من المينيو (خيار 8) — من غير أخطاء غير متوقعة |
| علامات ممتاز | `A1..A5` و `Part B` و `Part C` كلها طلعت |
| الخيارات التمانية | كلها شغالة فعلًا (checklist فوق) |

### إزاي ترفع الملفين الجداد على GitHub

النيوتبوكين بيترفعوا زي كل مرة: **File → Save a copy in GitHub** في
ريبو `smart-tracker`. أما الملفين الآخرين (`.py` و`.csv`) فبيرفعوا من متصفح GitHub:

1. افتح ريبوك على github.com واضغط **Add file → Create new file**
2. اكتب المسار كامل في خانة الاسم: `tracker/functions.py` (المجلدات بتعمل نفسها)
3. لصق المحتوى زي ما هو من النيوتبوك — و**Commit changes**
4. كرّر نفس الخطوة لـ `data/sample_records.csv` (المحتوى اللي طبعه
   بلوك عرض الـ CSV فوق)

---

## Hand-off للمحاضرة 5

دلوقتي عندك: **الكود متجمّع في دوال** و**بياناتك بتحفظ في ملف وترجع** —
الكود والبيانات لسه منفصلين عن بعض. المحاضرة 5 — Intro to OOP: نجمّع
**البيانات والسلوك مع بعض** في `class` واحدة، فيبقى لكل شيء
بياناته وطرقه. شوفت بنفسك إن الدوال طول الوقت بتاخد بيانات وترجّع
بيانات — ده بالذات اللي بيدفعنا للـ class.
