# المحاضرة 9 — الواجب | Machine Learning Models

**Engineer Ibrahim — كورس Python & AI (12 محاضرة) · أسئلة المحاضرة 9**

الملف ده موجود في **نسختين بنفس المحتوى**: `L09_assignment.md` تنسخ منه بلوك
بلوك بضغطة من GitHub أو Colab، و `L09_assignment.pdf` لو بتحب تطبع وتشتغل على
ورق. اختار اللي يريحك.

**التسليم على GitHub:**

- `lectures/lecture_09/exercises.ipynb` → **الجزء A + الجزء C + تحقّقات B**
- `tracker/predictor.py` → **الجزء B (الكود)** — أول موديل لمشروعك

**تحقّق ذاتي:** كل تمرين تقني ليه **assert** — كل واحد بيقول `ممتاز` أو
يوضّح الغلط.

**سياسة AI:** تشرحلك وتلاقي الغلط — **ما تكتبشلك الحل**. الهدف إنك أنت
تتعلم.

مدة الواجب 3–4 ساعات — يتسلّم قبل المحاضرة 10.

> **ملاحظة:** رسائل الخطأ بالإنجليزي — وبعدها اقرأ **آخر سطر** فيها، فيه
> السبب غالبًا (`could not convert string to float` يعني لسه فيه كلمة في
> `X`).

---

## الجزء A — تمارين التدريب (5 تمارين)

### A1 — خطّي: ساعات الدراسة ← الدرجة

**النص:** درّب Linear Regression على ساعات الدراسة والدرجة — ولاحظ إن
`X` لازم يفضل **جدول** حتى لو ميزة واحدة.

```python
import pandas as pd
from sklearn.linear_model import LinearRegression

study = pd.DataFrame({
    "hours": [1, 2, 3, 4, 5, 6, 7, 8],
    "score": [52, 55, 61, 63, 70, 72, 78, 81],
})
X = study[["hours"]]   # ميزة واحدة بس - ويفضل جدول
y = study["score"]

model = LinearRegression().fit(X, y)
print(round(float(model.coef_[0]), 2))     # 4.26
print(round(float(model.intercept_), 2))   # 47.32

assert 3.5 < model.coef_[0] < 4.5, "الميل حوالي 4"
print("A1: ممتاز")
```

**الجملة المطلوبة (اكتبها في `exercises.ipynb`):** كل ساعة دراسة زيادة
تزيد الدرجة بحوالي **4.26** — ولو شفت الميل سالب، راجع اتجاه البيانات.

**Checklist:** طلعت `4.26` و `47.32` · طلعت `A1: ممتاز` · كتبت جملة
المعنى بجنبها.

---

### A2 — تشفير فئة بـ `get_dummies`

**النص:** طلبات توصيل فيها عمود فئوي `method` — اشفّره وشوف الأعمدة
الجديدة بأسمائها.

```python
import pandas as pd

orders = pd.DataFrame({
    "method": ["express", "standard", "express", "pickup", "standard",
               "pickup", "express", "standard"],
    "minutes": [25, 45, 30, 15, 50, 18, 22, 40],
    "fee": [12.0, 7.0, 13.0, 4.0, 6.0, 4.5, 14.0, 7.5],
})

X = pd.get_dummies(orders[["minutes", "method"]],
                   columns=["method"], dtype=float)
print(list(X.columns))    # ['minutes', 'method_express', 'method_pickup', 'method_standard']

assert "method_express" in X.columns, "عمود الفئة اتعمل"
assert X.shape == (8, 4), "8 صفوف - 4 أعمدة كلها أرقام"
print("A2: ممتاز")
```

**السؤال (سطرين):** ليه رمينا `method` الأصلي وحطّينا مكانه تلات أعمدة؟
(الإجابة في النيوتبوك بأسلوبك.)

**Checklist:** الأربعة أعمدة طلعوا بأسماء `method_*` · مفيش عمود نصّي فاضل
· `A2: ممتاز`.

---

### A3 — شجرة قرار: سيارات مستعملة

**النص:** درّب Decision Tree على سنة الصنع والكيلومترات عشان تنبّأ
بالسعر — وشوف قربها من الحقيقة.

```python
import pandas as pd
from sklearn.tree import DecisionTreeRegressor

cars = pd.DataFrame({
    "year": [2015, 2018, 2012, 2020, 2016, 2019, 2013, 2017, 2021, 2014],
    "km": [120000, 80000, 150000, 40000, 110000, 60000,
           140000, 90000, 30000, 130000],
    "price": [180, 260, 130, 330, 200, 290, 140, 240, 350, 150],
})
X = cars[["year", "km"]]
y = cars["price"]

tree = DecisionTreeRegressor(max_depth=3, random_state=0).fit(X, y)
pred = tree.predict(X)
print([round(float(p)) for p in pred[:4]])   # أول 4 تنبؤات

mae = float(abs(y - pred).mean())
print(round(mae, 2))     # 3.0
assert mae < 10, "الشجرة قربت من داتا التدريب"
print("A3: ممتاز")
```

**السؤال (سطرين):** الشجرة طبّعت تنبؤ قريب جدًا من الحقيقة — بس على
**نفس الداتا اللي درّبت عليها**. ليه ده مش دليل إنها هتنبّأ صح على
سيارة تشوفها بكرة؟ (كلمة واحدة هتفتكرها في L11.)

**Checklist:** طلعت `A3: ممتاز` · كتبت السطرين وفيهم إن التقييم ده على
التدريب · ما قلتش «الشجرة أحسن» من غير رقم.

---

### A4 — تنبّأ باليد ثم قارنه بـ `predict`

**النص:** ده الميل والنقطة — احسب التنبؤ **بإيدك** لـ 45 دقيقة تمرين
وقارنه بمخرجات الموديل.

```python
import pandas as pd
from sklearn.linear_model import LinearRegression

gym = pd.DataFrame({
    "minutes": [10, 20, 30, 40, 50, 60, 70, 80],
    "calories": [52, 105, 161, 208, 250, 310, 355, 405],
})
X = gym[["minutes"]]
y = gym["calories"]

model = LinearRegression().fit(X, y)
w = float(model.coef_[0])
b = float(model.intercept_)

hand = w * 45 + b
auto = float(model.predict(pd.DataFrame({"minutes": [45]}))[0])
print(round(hand, 1))    # 230.8

assert abs(hand - auto) < 0.01, "التنبؤ باليد زي ما طلع من الموديل"
print("A4: ممتاز")
```

**الجملة المطلوبة:** معادلة الموديل بالعربي — «كل دقيقة تمرين تزيد
السعرات حوالي **5.01**، ومعاها نقطة بداية **(الرقم من print)**».

**Checklist:** طلعت `230.8` · `A4: ممتاز` · كتبت المعادلة بكل رقمين.

---

### A5 — حوّل `coef_` إلى جملة معنى

**النص:** المبيعات مقابل درجة الحرارة — اقرأ الميل وحوّله جملة واحدة
بالعربي. الرقم من غير جملة معلومة ميتة.

```python
import pandas as pd
from sklearn.linear_model import LinearRegression

shop = pd.DataFrame({
    "temp": [18, 21, 24, 27, 30, 33],
    "sales": [120, 150, 175, 210, 245, 280],
})
X = shop[["temp"]]
y = shop["sales"]

model = LinearRegression().fit(X, y)
temp_coef = float(model.coef_[0])
print(round(temp_coef, 2))    # 10.67

assert temp_coef > 0, "اتجاه إيجابي - حرارة أعلى مبيعات أعلى"
print("A5: ممتاز")
```

**الجملة المطلوبة:** مثال: «كل درجة حرارة زيادة تزيد المبيعات ≈ **10.67**
وحدة» — ولو طلع سالب، اكتب المعنى الصح بدل ما تفرض الصواب.

**Checklist:** طلعت `10.67` · `A5: ممتاز` · الجملة فيها الرقم واتجاهه.

---


## الجزء B — قطعة المشروع: `tracker/predictor.py`

**ده أهم جزء.** من L6 عندك baseline بـ if/else، ومن L7/L8 داتا نظيفة
ورسوم. النهاردة أول **موديل مدرّب** لمشروعك بدالتين بس.

### الخطوة 1 — `tracker/predictor.py`

من صفحة الريبو: **Add file → Create new file** → الاسم
`tracker/predictor.py` → الصق الهيكل ده واملاه بأعمدة **مشروعك انت**:

```python
"""Project first ML model - L9."""
import pandas as pd
from sklearn.linear_model import LinearRegression
from sklearn.tree import DecisionTreeRegressor

TARGET = "YOUR_TARGET_COL"       # عمود الهدف في مشروعك
CAT_COLS = ["YOUR_CAT_COL"]      # عموداتك الفئوية - عدّلها


def prepare_data(df):
    """يجهّز X و y: صفوف نظيفة + أعمدة فئوية مشفّرة."""
    # TODO: dropna(subset=[TARGET]) - نظّف صفوف الهدف الناقصة
    # TODO: pd.get_dummies(...) - اشفّر كل عمود فئوي
    # TODO: df[TARGET] - الهدف عمود واحد بس
    pass


def train_model(X, y, kind="linear"):
    """يدرب ويرجّع الموديل - نوع التدريب حسب kind"""
    # TODO: kind بيحدّد الكلاس المناسب
    # TODO: model.fit(X, y) - التدريب هنا
    # TODO: return model
    pass
```

**قواعد الجزء B:**

| القاعدة | ليه |
|---|---|
| `dropna` قبل `fit` | الموديل مبيكمّلش ناقص — بيقع |
| `get_dummies` على فئات X بس | الهدف `y` عمود واحد رقمي مايتشفّرش |
| كل أعمدة X أرقام | غلط `could not convert string to float` = نسيان التشفير |
| `return model` من `train_model` | من غير رجوع مافيش تنبؤ بعدين |

### الخطوة 2 — تحقّق ذاتي للجزء B

**الصقه في `exercises.ipynb` من جذر الريبو (بعد ما ترفع predictor.py):**

```python
import pandas as pd
from tracker.analysis import load_dataset
from tracker.predictor import prepare_data, train_model

df = load_dataset()
X, y = prepare_data(df)
model = train_model(X, y, kind="linear")

print(X.shape)                      # عدد الصفوف النظيفة
print(list(X.columns))              # كل الأعمدة أرقام
print([round(float(p), 2) for p in model.predict(X)[:5]])   # أول 5 تنبؤات

assert len(X) == len(y), "X و y بنفس الطول"
assert X.dtypes.map(lambda d: d != object).all(), "مفيش عمود نصّي فاضل"
print("Part B: ممتاز")
```

> استبدل `YOUR_TARGET_COL` و `YOUR_CAT_COL` بأسماء أعمدة مشروعك.

**Checklist للجزء B:**

| العنصر | لازم يحصل |
|---|---|
| `predictor.py` | دالتان بالـ docstring · كل أعمدة X أرقام |
| التنبؤات | طبعت أول 5 تنبؤات من داتا مشروعك |
| `Part B: ممتاز` | التحقّق الاتنين عدّوا من غير خطأ |
| مقاسات | `len(X) == len(y)` — وطولهم = الصفوف النظيفة |

---


## الجزء C — قارن عائلتين على داتا مشروعك

دلوقتي درّب **نوع تاني** على نفس الداتا وقارنهم على 5 صفوف — ده تمرين
التفكير، مش الزوّد:

```python
import pandas as pd
from tracker.analysis import load_dataset
from tracker.predictor import prepare_data, train_model

df = load_dataset()
X, y = prepare_data(df)
lin = train_model(X, y, kind="linear")
tree = train_model(X, y, kind="tree")

comp = pd.DataFrame({
    "actual": list(y[:5]),
    "linear": [round(float(p), 2) for p in lin.predict(X[:5])],
    "tree": [round(float(p), 2) for p in tree.predict(X[:5])],
})
print(comp)
assert lin.predict(X).shape == tree.predict(X).shape, "نفس عدد التنبؤات"
print("C: ممتاز")
```

**اكتب في `exercises.ipynb` سطرين:**

- **الأقرب لداتاك:** أنهى موديل (خطّي ولا شجرة) — **بأي رقم؟** (فرق أو
  خطأ، مش «أحلى»).
- **ليه:** جملة واحدة بتشرح بالمنطق (خط مستقيم؟ فئات؟ قيم متطرّفة؟).

**Checklist:** طلعت `C: ممتاز` · الجدول فيه `actual` جنب التنبؤات ·
سطرك الأول فيه رقم · ما قلتش «أحسن» من غير دليل.

---


## الاختبار الذاتي — 5 أسئلة

**أجوبتك بإيدك الأول — وبعدها صحّح بنفسك.** تصحيح ذاتي فقط، مش متقيّم.

1. إيه الفرق بين `X` و `y`؟ (سطر لكل واحد — الشكل والدور.)
2. ليه لازم نشفّر الأعمدة الفئوية بـ `get_dummies` قبل `fit`؟
3. كم سطر بيتكرر مع **أي** موديل في scikit-learn؟ اكتبهم الثلاثة.
4. لو `coef_` بتاع ميزة طلع `0.02` — اكتب جملة معناها بالعربي.
5. ليه الـ baseline بتاع L6 مهم قبل ما نقول «الموديل نجح»؟

### الإجابات (صحّح بعد ما تكتب)

1. `X` جدول الصفوف × الأعمدة (الميزات، كله أرقام) · `y` عمود واحد بس هو
   الرقم اللي عايز نتنبّأ بيه.
2. لأن شغل الموديل ضرب وجمع — كلمة زي `espresso` مالهاش معنى حسابي،
   و`fit` بيوقع بـ `could not convert string to float`.
3. `model = Model()` · `model.fit(X, y)` · `model.predict(X_new)`.
4. مثال: «كل وحدة زيادة في الميزة دي تضيف 0.02 في الهدف» — ولو السالب،
   تزيد بالعكس. المطلوب جملة فيها الرقم والاتجاه.
5. لأنه المسطرة الموضوعية: خطأ الموديل لازم يكون **أقل** من خطأ النمط
   القديم (38.93 في حصة النهاردة) — وإلا الموديل مالوش قيمة عملية.

---


## التسليم — معايير القبول

| المعيار | التفاصيل |
|---|---|
| الملفات في مكانها | `tracker/predictor.py` · `lectures/lecture_09/exercises.ipynb` |
| الجزء A | `A1` إلى `A5` طلعوا + الجمل المطلوبة مكتوبة |
| الجزء B | الدالتين شغالين · أول 5 تنبؤات اتطبعت · `Part B: ممتاز` |
| الجزء C | جدول المقارنة + سطرين بأرقام |
| الاختبار الذاتي | 5 إجابات مكتوبة قبل ما تروح للإجابات |

### إزاي ترفع كل حاجة

1. **النيوتبوك:** **File → Save a copy in GitHub** →
   `lectures/lecture_09/exercises.ipynb`.
2. **`predictor.py`:** من صفحة الريبو — **Add file → Create new file** →
   الاسم `tracker/predictor.py` → الصق الهيكل واملاه بأعمدتك → Commit.
3. شغّل **تحقّق الجزء B** من جذر الريبو — وبعدها **Run All** كله قبل
   ما تسيب النيوتبوك.

---

## Hand-off للماضرة 10 — Predictions

دلوقتي عندك **أول موديل مدرّب** في `tracker/predictor.py` — بينتج تنبؤات
لصفوف شفتها. المحاضرة 10 — Predictions: نستخدمه على **داتا جديدة ما
شافهاش**، ونحطّه جوّا `Tracker`، ونقارنه **بالنظام القديم** baseline من L6
— يبقى عندك جدول: سجل · قاعدة قديمة · موديل جديد.
