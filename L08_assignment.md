# المحاضرة 8 — الواجب | Data Visualization

**Engineer Ibrahim — كورس Python & AI (12 محاضرة) · أسئلة المحاضرة 8**

الملف ده موجود في **نسختين بنفس المحتوى**: `L08_assignment.md` تنسخ منه بلوك
بلوك بضغطة من GitHub أو Colab، و `L08_assignment.pdf` لو بتحب تطبع وتشتغل على
ورق. اختار اللي يريحك.

**التسليم على GitHub:**

- `lectures/lecture_08/exercises.ipynb` → **الجزء A + الجزء C + تحقّقات B**
- `tracker/charts.py` → **الجزء B (الكود)** — 4 دوال رسم على داتا مشروعك
- `reports/figures/*.png` → **الجزء B (الرسوم)** — كل رسمة محفوظة على الريبو
- `reports/findings.md` → **الجزء B** — تتوسّع بقسم الرسوم الجديد (v2)

**تحقّق ذاتي:** كل تمرين تقني ليه **assert** — كل واحد بيقول `ممتاز` أو
يوضّح الغلط · والرسوم نفسها بتتشاف بعينك.

**سياسة AI:** تشرحلك وتلاقي الغلط — **ما تكتبشلك الحل**. الهدف إنك أنت
تتعلم.

مدة الواجب 3–4 ساعات — يتسلّم قبل المحاضرة 9.

> **ملاحظة مهمة على النصوص في الرسوم:** عناوين الرسوم والمحاور اكتبها
> **بالإنجليزي** — البايثون بيجيب الحروف العربية مقطّعة من غير مكتبات
> إضافية. التعليقات والتقرير بالعربي زي ما انت.

---

## الجزء A — تمارين الرسم (5 تمارين)

### A1 — Bar chart من dict

**النص:** لكل يوم إيراد — ارسمه أعمدة، مع عنوان وعناوين محاور. لو الرسمة
طلعت من غير عنوان، صلّحها قبل ما تكمل.

```python
import matplotlib.pyplot as plt

sales = {"Mon": 120, "Tue": 90, "Wed": 150, "Thu": 110, "Fri": 180}
plt.bar(sales.keys(), sales.values())
plt.title("Weekly sales")
plt.xlabel("Day")
plt.ylabel("Sales (EGP)")
plt.show()

assert len(sales) == 5 and max(sales.values()) == 180
print("A1: ممتاز")
```

**Checklist:** الرسمة طلعت بأعمدة خمسة · فيها title و xlabel و ylabel ·
أعلى عمود هو الجمعة (180) — لو مختلف، انت مقلّب المحاور.

---

### A2 — Line chart بعناوين وشبكة

**النص:** خط بيبيّن الترند — والـ `marker="o"` بيحط نقطة على كل قيمته.

```python
import numpy as np
import matplotlib.pyplot as plt

days = np.arange(1, 11)
visitors = [45, 52, 61, 58, 70, 75, 72, 80, 88, 95]
plt.plot(days, visitors, marker="o")
plt.title("Visitors per day")
plt.xlabel("Day")
plt.ylabel("Visitors")
plt.grid(True)
plt.show()

assert visitors[-1] > visitors[0], "الخط صاعد من الأول للآخر"
print("A2: ممتاز")
```

**Checklist:** الرسمة خط صاعد مع نقاط · فيها شبكة `grid` · وضّحت في سطر:
الاتجاه العام للزوار؟ (صاعد — مع تذبذب بسيط في اليوم 4 و7.)

---

### A3 — Histogram لتوزيع رقمي

**النص:** الـ histogram بيجاوب: **القيم بتتوزّع إزاي؟** — كام مرة وقعت
في كل مدى؟ (مختلف عن الـ bar: المحور X هنا مدى مستمر مش فئات.)

```python
import numpy as np
import matplotlib.pyplot as plt

values = np.random.default_rng(42).normal(45, 15, 244)
plt.hist(values, bins=20)
plt.title("Tip distribution")
plt.xlabel("Tip (EGP)")
plt.ylabel("Count")
plt.show()

assert 40 < values.mean() < 50, "المتوسط قرب 45"
print("A3: ممتاز")
```

**Checklist:** الرسمة شكل جرسي متوازن · وضّحت في سطر: فين أكبر تجمّع
ومدى القيم الشائعة؟

---

### A4 — Scatter + تفسير العلاقة

**النص:** العلاقة بين عمودين رقميين بيترسم scatter — وبعدها احسب
الارتباط وبتوّضّح في سطرين.

```python
import numpy as np
import matplotlib.pyplot as plt

rng = np.random.default_rng(7)
hours = rng.uniform(1, 10, 60)
scores = hours * 7 + 30 + rng.normal(0, 6, 60)
plt.scatter(hours, scores)
plt.title("Hours vs score")
plt.xlabel("Hours studied")
plt.ylabel("Score")
plt.show()

corr = float(np.corrcoef(hours, scores)[0, 1])
print(round(corr, 2))
assert corr > 0.7, "ارتباط إيجابي قوي"
print("A4: ممتاز")
```

**التفسير (اكتب في `exercises.ipynb` سطرين):**

- **القوة والاتجاه:** ارتباط كذا (من print) — إيجابي: كل ما ساعات الدراسة
  زادت الدرجة زادت.
- **التحذير:** ده **ارتباط مش سببية** — ممكن في سبب تاني (زي المجموعة
  المذاكرة مع بعض).

**Checklist:** طلعت `A4: ممتاز` · كتبت السطرين قبل ما تروح لـ B ·
ذكرت كلمة «ارتباط مش سببية».

---

### A5 — إصلح الرسمة الغلط (شغّل الأول الغلط!)

**النص:** الكود ده بيرسم رسمة **مضلّلة** — شغّله وشوف الغلط بعينك، وبعدها
أصلحه بالكود الصح تحته.

**الرسمة الغلط (شغّلها):**

```python
import matplotlib.pyplot as plt

labels = ["Team A", "Team B", "Team C", "Team D"]
scores = [97, 98, 96, 99]
plt.bar(labels, scores)
plt.ylim(95, 100)
plt.show()
```

**إيه الغلط؟** المحور Y مبتدا من **95** — الفرق بين 96 و99 بقى شكل
عملاق والفرق الحقيقي 3% بس كأنه 500%. ده **محور مبتداز بيخدع**.

**أصلحه:**

```python
import matplotlib.pyplot as plt

labels = ["Team A", "Team B", "Team C", "Team D"]
scores = [97, 98, 96, 99]
plt.bar(labels, scores)
plt.ylim(0, 100)
plt.title("Scores by team")
plt.xlabel("Team")
plt.ylabel("Score")
plt.show()

assert plt.ylim()[0] == 0, "المحور لازم يبدأ من 0"
print("A5: ممتاز")
```

**Checklist:** شفت الرسمتين جنب بعض · الرسمة الصحيحة بدت من 0 وعندها
عنوان ومحاور · كتبت سطر: أي حاجة بتكدّب بالمحور، الرقم بتاعك هو اللي
يتظبط — مش المحور.

---


## الجزء B — قطعة المشروع: `tracker/charts.py` + رسوم findings v2

**ده أهم جزء.** من L7 عندك داتا نظيفة و5 نتايج. النهاردة نضيف **4–5
رسوم** كل واحدة بترد على سؤال — محفوظة في `reports/figures/` والجملة اللي
وراها بتتكتب في `reports/findings.md` (v2).

### الخطوة 1 — `tracker/charts.py`

من صفحة الريبو: **Add file → Create new file** → الاسم
`tracker/charts.py` → الصق الهيكل ده واملاه بأعمدة **مشروعك انت**:

```python
"""Project charts - L8. Every chart answers one question."""
import os
import matplotlib.pyplot as plt
import seaborn as sns

FIG_DIR = "reports/figures"


def chart_distribution(df, col, name="distribution.png"):
    # TODO: hist على عمود رقمي واحد - اسأل: التوزيع فين؟
    os.makedirs(FIG_DIR, exist_ok=True)
    # ... plt.hist(df[col]) + title/labels + savefig + plt.close()
    pass


def chart_categories(df, x_col, y_col, name="categories.png"):
    # TODO: bar لمتوسط كل فئة - اسأل: مين الأعلى؟
    os.makedirs(FIG_DIR, exist_ok=True)
    # ... groupby + plt.bar + title/labels + savefig + plt.close()
    pass


def chart_relationship(df, x_col, y_col, name="relationship.png"):
    # TODO: scatter للعلاقة مع الهدف - اسأل: اتجاهها إيه؟
    os.makedirs(FIG_DIR, exist_ok=True)
    # ... plt.scatter + title/labels + savefig + plt.close()
    pass


def chart_correlation(df, name="heatmap.png"):
    # TODO: heatmap للارتباط بين كل الأعمدة الرقمية - اسأل: مين متقارب؟
    os.makedirs(FIG_DIR, exist_ok=True)
    # ... df.corr(numeric_only=True) + sns.heatmap + savefig + plt.close()
    pass
```

**قواعد لكل رسمة:**

| القاعدة | ليه |
|---|---|
| عنوان + عناوين محاور (إنجليزي) | الرسمة من غير سؤال بقت زينة |
| `plt.savefig(...)` داخل `FIG_DIR` بعدها `plt.close()` | الملف يترفع على الريبو + الذاكرة تفضل خفيفة |
| **جملة رؤية واحدة** في `findings.md` | الرسمة لوحدها مش ناتج — الجملة هي الناتج |
| محور Y بيبدأ من 0 في الأعمدة | علّمنا في A5 — المضخّمات ممنوعة |

### الخطوة 2 — شغّلهم واحفظهم

الرسوم المطلوبة **4 بالأساس** (+ رسمة خامسة اختيارية زي box plot):

1. **توزيع** لعمود رقمي (hist) — مثل: توزيع الـ target بتاعك.
2. **مقارنة فئات** (bar) — متوسط رقمي لكل فئة.
3. **علاقة بالهدف** (scatter) — feature مقابل الـ target.
4. **خريطة ارتباط** (heatmap) — كل الأعمدة الرقمية مع بعض.
5. *(اختياري)* **box plot** — توزيع الـ target على فئتين.

### الخطوة 3 — `reports/findings.md` v2: قسم الرسوم

افتح التقرير بـ **✏️ Edit** وأضف قسمًا جديدًا بعد القسم القديم:

```markdown
## Charts (L8) — reports/figures/

### Chart 1 — distribution.png
- Question: اسأل: ...
- Insight: (جملة واحدة بالعربي - فيها رقم)

### Chart 2 — categories.png
- Question: ...
- Insight: ...

### Chart 3 — relationship.png
- Question: ...
- Insight: ...

### Chart 4 — heatmap.png
- Question: ...
- Insight: ...
```

### تحقّق ذاتي للجزء B

**الصقه في `exercises.ipynb` من جذر الريبو (بعد ما ترفع charts.py):**

```python
import os
from tracker.analysis import load_dataset
from tracker.charts import (chart_distribution, chart_categories,
                            chart_relationship, chart_correlation)

df = load_dataset()
chart_distribution(df, "YOUR_NUMERIC_COL")
chart_categories(df, "YOUR_CATEGORY_COL", "YOUR_VALUE_COL")
chart_relationship(df, "YOUR_FEATURE_COL", "YOUR_TARGET_COL")
chart_correlation(df)

for f in ["distribution.png", "categories.png",
          "relationship.png", "heatmap.png"]:
    p = os.path.join("reports/figures", f)
    assert os.path.exists(p) and os.path.getsize(p) > 1000, f
print("Part B: ممتاز")
```

> استبدل كل `YOUR_*` بأسماء الأعمدة بتاعتك.

**Checklist للجزء B:**

| العنصر | لازم يحصل |
|---|---|
| `charts.py` | 4 دوال · كل واحدة بتحفظ PNG في `reports/figures/` |
| الرسوم على الريبو | 4 ملفات PNG اترفعوا بـ **Upload files** (تدفع متعددة) |
| insights v2 | 4 أسئلة + 4 جمل رؤية — كل جملة فيها رقم |
| تحميل خفيف | كل دالة فيها `plt.close()` بعد الحفظ |

---

## الجزء C — رسمة تسأل سؤالك الصعب

اختار **افتراض كنت متأكد منه** في داتا مشروعك (مثل: «الفئة الفلانية
أعلى دايمًا» · «العلاقة خطيّة» · «مفيش قيم شاذة») وارسم **رسمة واحدة
تقدّر تكسر أو تأكّد** الافتراض ده.

اكتب في `exercises.ipynb`:

- **الافتراض:** جملة واحدة.
- **الرسمة:** أنهى نوع وليه (hist أو bar أو scatter أو box).
- **الحكم:** الرسمة قالت إيه — لو الافتراض اتكسر، اعترف بيه بصراحة.

**Checklist:** الافتراض محدّد مش عام · الرسمة من نوع **يناسب السؤال** ·
الحكم فيه رقم أو ملاحظة واضحة · مكتوب بأسلوبك.

---


## التسليم — معايير القبول

| المعيار | التفاصيل |
|---|---|
| الملفات في مكانها | `tracker/charts.py` · `reports/figures/*.png` (4 ملفات) · `reports/findings.md` متوسّع · `lectures/lecture_08/exercises.ipynb` |
| الرسوم الأربع | توزيع · مقارنة فئات · علاقة بالهدف · heatmap — كلها بعنوان ومحاور |
| insights v2 | 4 أسئلة + 4 جمل رؤية — كل جملة فيها رقم من الرسمة |
| علامات ومراجع | `A1` إلى `A5` طلعوا · و checklist بتوع C متأشر |

### إزاي ترفع كل حاجة

1. **النيوتبوك:** **File → Save a copy in GitHub** →
   `lectures/lecture_08/exercises.ipynb`.
2. **الرسوم PNG:** من صفحة الريبو — **Add file → Upload files** ←
   **اختار 4 ملفات مع بعض** (Ctrl/Cmd + كليك) وارفعهم في مجلد
   `reports/figures/` (اكتب المسار في خانة الاسم قبل الـ commit).
3. **`charts.py`:** **Add file → Create new file** → لصق الهيكل واملاه.
4. **`findings.md`:** افتحه بـ **✏️ Edit** → الصق قسم Charts تحت القسم
   القديم → Commit.

---

## Hand-off للماضرة 9 — Machine Learning Models

دلوقتي عندك: داتا نظيفة · 5 نتايج مكتوبة · و4 رسوم تشرح القصة. بس لسه
كل ده **وصف للماضي**. المحاضرة 9 — ML Models: نعلّم الكمبيوتر **يتعلّم
من الداتا دي** ويتوقّع حاجات ما شافهاش — بأول نموذج حقيقي في
`tracker/predictor.py`.
