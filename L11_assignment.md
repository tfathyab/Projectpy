# المحاضرة 11 — الواجب | Train vs Eval

**Engineer Ibrahim — كورس Python & AI (12 محاضرة) · أسئلة المحاضرة 11**

الملف ده موجود في **نسختين بنفس المحتوى**: `L11_assignment.md` تنسخ منه بلوك
بلوك بضغطة من GitHub أو Colab، و `L11_assignment.pdf` لو بتحب تطبع وتشتغل على
ورق. اختار اللي يريحك.

**التسليم على GitHub:**

- `lectures/lecture_11/exercises.ipynb` → **الجزء A + الجزء C + تحقّق B**
- `tracker/predictor.py` → **الجزء B (الكود: `evaluate_model`)**
- `reports/evaluation.md` → **التقرير بنتائجك وحدودك**

**تحقّق ذاتي:** كل تمرين تقني ليه **assert** — كل واحد بيقول `ممتاز` أو
يوضّح الغلط.

**سياسة AI:** تشرحلك وتلاقي الغلط — **ما تكتبشلك الحل**. الهدف إنك أنت
تتعلم.

مدة الواجب 3–4 ساعات — يتسلّم قبل المحاضرة 12.

> **ملاحظة مهمة عن المقياس:** الواجب كله بيقيس على **المقياس اللي كتبته
> في L6** (`problem_definition.md`) — مش مقياس جديد. الأمثلة هنا تستخدم
> **MAE بالجنيه** كمثال واحد؛ لو مشروعك دقة (classification) — استبدل
> `mean_absolute_error` بـ `accuracy_score` في نفس المكان.

---

## الجزء A — تمارين التقييم (4 تمارين)

### A1 — قسّم الداتا واشرح الأحجام

**النص:** نفس داتا المحل الـ 11 سجل — النهاردة بتتقسم: 7 تدريب و4
ما يشوفهاش الموديل.

```python
import pandas as pd
from sklearn.model_selection import train_test_split

coffee = [
    {"drink": "espresso", "cups": 12, "revenue": 36.0},
    {"drink": "latte",    "cups": 18, "revenue": 90.0},
    {"drink": "espresso", "cups": 10, "revenue": 30.0},
    {"drink": "latte",    "cups": 22, "revenue": 110.0},
    {"drink": "espresso", "cups": 14, "revenue": 42.0},
    {"drink": "tea",      "cups": 9,  "revenue": 27.0},
    {"drink": "latte",    "cups": 20, "revenue": 100.0},
    {"drink": "espresso", "cups": 16, "revenue": 48.0},
    {"drink": "latte",    "cups": 25, "revenue": 125.0},
    {"drink": "latte",    "cups": 30, "revenue": 150.0},
    {"drink": "tea",      "cups": 12, "revenue": 36.0},
]
df = pd.DataFrame(coffee)
X = pd.get_dummies(df[["cups", "drink"]], columns=["drink"], dtype=float)
y = df["revenue"]

X_tr, X_va, y_tr, y_va = train_test_split(X, y, test_size=0.3, random_state=42)
print(len(X_tr), len(X_va))     # 7 4

assert len(X_tr) == 7 and len(X_va) == 4, "11 سجل = 7 تدريب + 4 تحقق"
print("A1: ممتاز")
```

**السؤال (سطرين):** 30% من 11 = 3.3 — طيب ليه طلعوا **4 سجلات مش 3**؟
ورمّي `random_state` وشغّل تاني — إيه اللي بيحصل للأحجام وللتقييم؟

**Checklist:** طلعت `7 4` · طلعت `A1: ممتاز` · كتبت السطرين.

---

### A2 — MAE بإيدك الأول وبعدين sklearn

**النص:** نفس الـ 5 نقاط من L10 — الاتنين لازم يطلعوا **4.3**.

```python
from sklearn.metrics import mean_absolute_error

actual = [45.0, 140.0, 33.0, 65.0, 60.0]
pred = [48.1, 137.8, 33.8, 69.4, 71.0]

diffs = [abs(a - p) for a, p in zip(actual, pred)]
manual = round(sum(diffs) / len(diffs), 2)
print("manual:", manual)                                   # 4.3
print("sklearn:", round(float(mean_absolute_error(actual, pred)), 2))  # 4.3

assert manual == 4.3, "MAE = متوسط الفروق المطلقة"
print("A2: ممتاز")
```

**السؤال (سطرين):** لو جمعت الفروق **بالإشارات** (+ و −) بدل ما أخذ
المطلق — ممكن الرقم النهائي يطلع صغير والغلط كبير. ليه؟ وامتى تفضّل
RMSE على MAE؟

**Checklist:** طلعت `4.3` مرتين · `A2: ممتاز` · كتبت السطرين.

---

### A3 — جدول الأعماق: اعثر على الـ sweet spot

**النص:** شجرة من depth 1 لـ 10 على نفس التقسيم — المطلوب تشوف
التدريب والتحقق مع بعض.

```python
import pandas as pd
from sklearn.tree import DecisionTreeRegressor
from sklearn.model_selection import train_test_split
from sklearn.metrics import mean_absolute_error

coffee = [
    {"drink": "espresso", "cups": 12, "revenue": 36.0},
    {"drink": "latte",    "cups": 18, "revenue": 90.0},
    {"drink": "espresso", "cups": 10, "revenue": 30.0},
    {"drink": "latte",    "cups": 22, "revenue": 110.0},
    {"drink": "espresso", "cups": 14, "revenue": 42.0},
    {"drink": "tea",      "cups": 9,  "revenue": 27.0},
    {"drink": "latte",    "cups": 20, "revenue": 100.0},
    {"drink": "espresso", "cups": 16, "revenue": 48.0},
    {"drink": "latte",    "cups": 25, "revenue": 125.0},
    {"drink": "latte",    "cups": 30, "revenue": 150.0},
    {"drink": "tea",      "cups": 12, "revenue": 36.0},
]
df = pd.DataFrame(coffee)
X = pd.get_dummies(df[["cups", "drink"]], columns=["drink"], dtype=float)
y = df["revenue"]
X_tr, X_va, y_tr, y_va = train_test_split(X, y, test_size=0.3, random_state=42)

rows = []
for d in range(1, 11):
    tree = DecisionTreeRegressor(max_depth=d, random_state=0).fit(X_tr, y_tr)
    tr = round(float(mean_absolute_error(y_tr, tree.predict(X_tr))), 1)
    va = round(float(mean_absolute_error(y_va, tree.predict(X_va))), 1)
    rows.append((d, tr, va))
    print(f"depth {d} | train {tr} | val {va}")

best = min(rows, key=lambda r: r[2])[0]
print("sweet spot:", best)      # 3
assert best == 3, "أول توقف تحسّن على التحقق"
print("A3: ممتاز")
```

**السؤال (سطرين):** عند `depth 10` — احسب **فرق** التدريب والتحقق
بالرقم، وقول: إيه اللي الموديل ده بيعمله مع الداتا؟

**Checklist:** الجدول طلع 10 صفوف · `sweet spot: 3` · `A3: ممتاز` ·
جاوبت السؤال.

---

### A4 — ليه درجة تدريب 100% ممكن تكون سيئة؟

**النص:** شجرة عميقة بتذاكر — الرقم على التدريب صفر غلط، وعلى التحقق
قصاده الحقيقة.

```python
import pandas as pd
from sklearn.tree import DecisionTreeRegressor
from sklearn.model_selection import train_test_split
from sklearn.metrics import mean_absolute_error

coffee = [
    {"drink": "espresso", "cups": 12, "revenue": 36.0},
    {"drink": "latte",    "cups": 18, "revenue": 90.0},
    {"drink": "espresso", "cups": 10, "revenue": 30.0},
    {"drink": "latte",    "cups": 22, "revenue": 110.0},
    {"drink": "espresso", "cups": 14, "revenue": 42.0},
    {"drink": "tea",      "cups": 9,  "revenue": 27.0},
    {"drink": "latte",    "cups": 20, "revenue": 100.0},
    {"drink": "espresso", "cups": 16, "revenue": 48.0},
    {"drink": "latte",    "cups": 25, "revenue": 125.0},
    {"drink": "latte",    "cups": 30, "revenue": 150.0},
    {"drink": "tea",      "cups": 12, "revenue": 36.0},
]
df = pd.DataFrame(coffee)
X = pd.get_dummies(df[["cups", "drink"]], columns=["drink"], dtype=float)
y = df["revenue"]
X_tr, X_va, y_tr, y_va = train_test_split(X, y, test_size=0.3, random_state=42)

tree = DecisionTreeRegressor(max_depth=10, random_state=0).fit(X_tr, y_tr)
train_mae = round(float(mean_absolute_error(y_tr, tree.predict(X_tr))), 1)
val_mae = round(float(mean_absolute_error(y_va, tree.predict(X_va))), 1)
print("train:", train_mae, "| val:", val_mae)   # train: 0.0 | val: 37.0

assert train_mae == 0.0 and val_mae > 30, "تدريب صفر + تحقق عالي = تذكّر"
print("A4: ممتاز")
```

**السؤال (3 أسطر):** الرقم على التدريب `0.00` — ليه ده **مش فرح**؟ ابدأ
بكلمة «الموديل…» وخلّص بـ «…عشان كده نقيّم على».

**Checklist:** طلعت `train: 0.0 | val: 37.0` · `A4: ممتاز` · كتبت 3
أسطر.

---


## الجزء B — قطعة المشروع: Evaluation report (أغلى جزء)

**النص:** من L9 عندك `prepare_data` و `train_model`. ومن L6 عندك
الـ baseline والمقياس. النهاردة تربطهم في **دالة تقييم واحدة** +
**تقرير بيقول الحقيقة**: الموديل ده يتحطّ في المشروع ولا لأ.

### الخطوة 1 — `predictor.py`: دالة `evaluate_model`

أضفها في `tracker/predictor.py` جنب دوال L9 و L10:

```python
# predictor.py — دالة التقييم الجديدة
def evaluate_model(X, y, kind="linear", test_size=0.3, random_state=42):
    """تقسيم صادق + قياس على المخبّأ - يرجع مقياس المحاضرة 6."""
    # TODO: train_test_split(X, y, test_size=test_size, random_state=random_state)
    # TODO: درّب الموديل على الجزء التدريبي
    # TODO: تنبّأ على الجزء المخبّأ
    # TODO: return {"train_mae": ..., "mae": ..., "rmse": ..., "r2": ...}
    # mae/rmse/r2 ← accuracy لو مشروعك تصنيف
    pass
```

### الخطوة 2 — القواعد (جدول واحد يمنع الرجوع)

| القاعدة | ليه |
|---|---|
| `random_state=42` في كل تقسيم | من غيره كل تشغيل نتاجي جديدة |
| نموذجان على الأقل + baseline | المقارنة مش رأي — أرقام في جدول |
| المقياس = اللي في L6 | مش مقياس جديد من عندك |
| فرق train↔val يتحسب ويتنكتب | هو دليل الـ overfitting |
| حدود صادقة مكتوبة | 4 سجلات تحقق = تقدير صغير — قلها |

### الخطوة 3 — تحقّق ذاتي للجزء B

**الصقه في `exercises.ipynb` من جذر الريبو:**

```python
import pandas as pd
from tracker.analysis import load_dataset
from tracker.predictor import prepare_data, evaluate_model
from sklearn.model_selection import train_test_split

df = load_dataset()
X, y = prepare_data(df)

# نموذجان على نفس التقسيم
res_lin = evaluate_model(X, y, kind="linear")
res_tree = evaluate_model(X, y, kind="tree")
print("linear:", {k: round(v, 2) for k, v in res_lin.items()})
print("tree:  ", {k: round(v, 2) for k, v in res_tree.items()})

# baseline L6 — نفس روح: متوسط التدريب
X_tr, X_va, y_tr, y_va = train_test_split(
    X, y, test_size=0.3, random_state=42)
base_mae = float(abs(y_va - y_tr.mean()).mean())
print("baseline MAE:", round(base_mae, 2))

# overfitting = فرق التدريب والتحقق
for name, res in (("linear", res_lin), ("tree", res_tree)):
    print(f"{name} gap:", round(res["mae"] - res["train_mae"], 2))

best = min(res_lin["mae"], res_tree["mae"])
assert base_mae > 0 and best > 0, "المقارنة اتعملت"
print("النتيجة:", "الموديل غلب الـ baseline" if best < base_mae
      else "الbaseline كسب — قولها بصراحة في التقرير")
print("Part B: ممتاز")
```

### الخطوة 4 — اكتب `reports/evaluation.md`

**الصقه بعد الخطوة 3 — وعدّل الأسطر بنتائج مشروعك انت:**

```python
from pathlib import Path

Path("reports").mkdir(exist_ok=True)
lines = ["# تقرير التقييم — المحاضرة 11", "",
         "| النموذج | MAE على التحقق |", "|---|---|",
         f"| baseline | {round(base_mae, 2)} |",
         f"| linear | {round(res_lin['mae'], 2)} |",
         f"| tree | {round(res_tree['mae'], 2)} |",
         "",
         "**فحص overfitting:** الفرق بين train و val في الجدول فوق.",
         "**الاختيار:** الموديل اللي غلب baseline على نفس المقياس.",
         "**حدود صادقة:** عدد سجلات التحقق صغير — النتيجة تقدير، وL12 يحكم على داتا أكبر."]
Path("reports/evaluation.md").write_text("\n".join(lines), encoding="utf-8")
print("saved: reports/evaluation.md")
```

**Checklist للجزء B:**

| العنصر | لازم يحصل |
|---|---|
| `predictor.py` | `evaluate_model` شغالة — تقسيم + نموذجان + قياس |
| الجدول | baseline ونموذجان بأرقامهم على نفس المقياس |
| فحص overfitting | الفرق train↔val محسوب ومكتوب |
| `evaluation.md` | محفوظ — فيه جدول + اختيار + **حدود صادقة** |
| `Part B: ممتاز` | التحقّق عدّى — والنتيجة اتطبعت بصراحة |

---


## الجزء C — cross-validation يأكّد النتيجة

**النص:** التقسيم الواحد حظّ — `cv=3` يعيد التقسيم 3 مرات وياخد
المتوسط. لو نفس الترتيب ظهر → ثقة أعلى.

```python
from sklearn.model_selection import cross_val_score
from sklearn.linear_model import LinearRegression
from sklearn.tree import DecisionTreeRegressor

lin_cv = -cross_val_score(LinearRegression(), X, y, cv=3,
                          scoring="neg_mean_absolute_error")
tree_cv = -cross_val_score(DecisionTreeRegressor(max_depth=3, random_state=0),
                           X, y, cv=3, scoring="neg_mean_absolute_error")
print("linear:", round(float(lin_cv.mean()), 2))   # 5.75
print("tree:  ", round(float(tree_cv.mean()), 2))  # 18.03

assert lin_cv.mean() < tree_cv.mean(), "نفس ترتيب التقسيم الواحد"
print("C: ممتاز")
```

**ملاحظة:** السالب (`neg_`) مش غلط مطبعي — sklearn بيدّي scorers اتجاه
«أعلى = أحسن» فالخطأ بيتسجّل سالب، وإحنا نرجّعه موجب بـ `-`.

**Checklist:** `linear` و `tree` طلعوا · نفس ترتيب التقسيم الواحد ·
`C: ممتاز`.

---


## الاختبار الذاتي — 5 أسئلة

**أجوبتك بإيدك الأول — وبعدها صحّح بنفسك.** تصحيح ذاتي فقط، مش متقيّم.

1. «نقيّم الموديل على نفس التدريب = كذبة» — إيه السبب بجملة واحدة؟
2. `random_state=42` بتعمل إيه بالظبط — وليه من غيرها المقارنة بلا معنى؟
3. كل واحد من MAE و RMSE و R² بيجاوب على إيه؟ وامتى R² أقل من صفر؟
4. إزاي تعرف إن الموديل overfitting من جدول train/val؟ (بالرقم)
5. تدريب `0.00` و تحقق `37.0` — أول خطوة عملية تنفّذها عشان تحسّن؟

### الإجابات (صحّح بعد ما تكتب)

1. لأن الموديل **شاف الصفوف دي قبل كده** — الدرجة على التدريب بتقيس
   التذكّر مش القدرة على سجل جديد (النتيجة: رقمك الجميل بيضحك عليك).
2. بتحدد **نفس التقسيم بالظبط** في كل تشغيل — من غيرها كل مرة سجلات
   تحقق مختلفة، والنتيجة تتغيّر من غير ما الموديل يتغيّر → مقارنة عمياء.
3. **MAE**: متوسط الخطأ بوحدتك (جنيه) · **RMSE**: بيعظّم الخطأ الكبير
   بعد التربيع · **R²**: كام من التشتت اتشرح — **أقل من صفر = أسوأ من
   إنك تخمّن المتوسط**.
4. التدريب **أقل بكتير من التحقق** (مثل 0.0 مقابل 37.0) — الفرق هو
   دليل التذكّر؛ والsweet spot أول نقطة توقف تحسّن التحقق.
5. **أنزّل `max_depth` للـ sweet spot** (أو داتا أكتر / أعمدة أقل) وشوف
   التحقق بيتحرك ولا لأ — إصلاح واحد في كل مرة.

---


## التسليم — معايير القبول

| المعيار | التفاصيل |
|---|---|
| الملفات في مكانها | `tracker/predictor.py` · `reports/evaluation.md` · `lectures/lecture_11/exercises.ipynb` |
| الجزء A | `A1` إلى `A4` طلعوا + كل الأسئلة المكتوبة مجاب عليها |
| الجزء B | `evaluate_model` شغالة · جدول المقارنة · الفرق محسوب · التقرير فيه حدود صادقة |
| الجزء C | `cross_val_score` شغال ونفس الترتيب ظهر |
| الاختبار الذاتي | 5 إجابات مكتوبة قبل ما تروح للإجابات |

### إزاي ترفع كل حاجة

1. **النيوتبوك:** **File → Save a copy in GitHub** →
   `lectures/lecture_11/exercises.ipynb`.
2. **`predictor.py`:** من صفحة الريبو — افتح الملف → **✏️ Edit** →
   الصق `evaluate_model` بالمسافة البادئة الصح → Commit.
3. **`evaluation.md`:** من النيوتبوك (الخطوة 4 بتحفظه) أو **Add file →
   Create new file** لصق المحتوى.
4. **Run All** كله قبل ما تسيب النيوتبوك.

---

## Hand-off للماضرة 12 — Final Project

دلوقتي عندك **`evaluate_model`** + تقرير **`reports/evaluation.md`
بيقول الحقيقة**: جدول (baseline · نموذجان) + فرق overfitting + اختيار
بحدود صادقة. المحاضرة 12 — Final Project: نجمّع كل القطع في
`notebooks/main.ipynb` واحد + README يبيع المشروع + عرض 5 دقائق —
ومشروعك **بيتبنى على كل بلوكاتك من L1 لـ L11**.
