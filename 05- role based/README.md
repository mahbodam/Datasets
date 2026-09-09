<div dir="rtl">
# دیتاست نقش‌محور (مشاور و تحلیل‌گر کسب‌وکار — Carin)

ساختار: برای هر نقش یک زیرپوشه با فایل‌های زیر (طبق `prompt-business-analyst-dataset.md`):

```
datasets/role based/<role>/
├── train.jsonl              # ۸۰٪ نمونه‌ها (شکافت طبقه‌بندی‌شده بر اساس خانوادهٔ سناریو)
├── validation.jsonl         # ۱۰٪
├── test.jsonl               # ۱۰٪
├── negative_examples.jsonl  # نمونه‌های منفی با برچسب نوع خطا (جدا)
├── pilot.jsonl              # دستهٔ آزمایشی ۱۰تایی (پیش از تولید انبوه)
└── batch_01..05.jsonl       # دسته‌های ۲۰۰تایی تولید
```

## نقش‌ها (هرکدام ۱۰۰۰ نمونه — مجموعاً ۱۰٬۰۰۰)

| شناسهٔ نقش | نقش | حوزه | وضعیت |
|---|---|---|---|
| cfo | مدیر مالی | مالی + بین‌دامنه‌ای | ✓ |
| mining_production_manager | مدیر تولید معدن | تولید معدنی | ✓ |
| warehouse_manager | مدیر انبار و زنجیرهٔ تأمین | موجودی + خرید | ✓ |
| hr_manager | مدیر منابع انسانی | منابع انسانی | ✓ |
| hse_manager | مدیر ایمنی، بهداشت و محیط زیست | ایمنی + محیط زیست | ✓ |
| commercial_director | مدیر بازرگانی | فروش + صادرات | ✓ |
| procurement_manager | مدیر خرید/تدارکات | خرید + تأمین‌کننده | ✓ |
| processing_manager | مدیر کارخانه/فرآوری | فرآوری + کیفیت | ✓ |
| maintenance_manager | مدیر تعمیرات و قابلیت اطمینان | تعمیرات + قطعات | ✓ |
| ceo | مدیرعامل | سطح سازمانی + استراتژیک | ✓ |

## خلاصهٔ آماری

- **مجموع نمونه‌ها:** ۱۰٬۰۰۰ (۱۰ نقش × ۱٬۰۰۰)
- **شکافت:** ۸۰۰/۱۰۰/۱۰۰ (آموزش/اعتبارسنجی/آزمون) در هر نقش
- **میانگین امتیاز:** ۴٫۵۸ در هر نقش
- **نمونه‌های منفی:** ۴ تا ۱۲ مورد به ازای هر نقش (پوشش ۶ برچسب خطای سند)
- **شناسه‌ها:** یکتا و پیوسته در هر نقش (`case_<role>_NNNNNN`)
- **توزیع دشواری:** دقیقاً ۱۰۰/۱۵۰/۲۵۰/۲۵۰/۱۵۰/۱۰۰
- **چندمرحله‌ای:** ≥۳۵۰ (≥۳۵٪) در هر نقش
- **عدم‌قطعیت:** ≥۱۰۰ (≥۱۰٪) در هر نقش

## ساختار هر نمونه

- شناسه: `case_<role>_NNNNNN` — یکتا و پیوسته
- کلیدها: نقش، دامنه، نوع، برچسب نوع مورد، دشواری ۱ تا ۶، درخواست کاربر، بافت در دسترس (کاتالوگ داده و متریک جاسازی‌شده)، گفتگو، امتیازها
- گفتگو: خلاصهٔ تحلیل → فراخوانی رسمی ابزار با نام و آرگومان → مشاهده → (چندمرحله در صورت نیاز) → پاسخ نهایی با شش بخش
- هیچ متریک یا دیتاستی خارج از کاتالوگ جاسازی‌شده در فراخوانی‌ها نمی‌آید (کنترل در جانشین)
- امتیازها: ۱۰ معیار ۰ تا ۵؛ آستانه: میانگین ≥ ۳٫۵ و درستی عددی و «بدون فرض بی‌پشتوانه» ≥ ۳
- برچسب نوع مورد: استاندارد / عدم‌قطعیت / چندمرحله‌ای

## الزام‌های سند (برای هر نقش)

- توزیع دشواری: ۱۰ / ۱۵ / ۲۵ / ۲۵ / ۱۵ / ۱۰ درصد
- چندمرحله‌ای ≥ ۱۵٪ و عدم‌قطعیت ≥ ۱۰٪
- شکافت ۸۰/۱۰/۱۰ بر اساس خانوادهٔ سناریو (نوع × دشواری)، نه رندوم
- خط قرمز: عدد اختراعی ممنوع؛ همبستگی به‌جای علیت ممنوع؛ با دادهٔ ناکافی نتیجهٔ قطعی ممنوع

## پایپ‌لاین

```bash
# تولید هر نقش
python tools/generate_role_based.py --role cfo          # مدیر مالی
python tools/generate_role_mining.py                     # مدیر تولید معدن
python tools/generate_role_warehouse.py                  # مدیر انبار
python tools/generate_role_hr.py                         # مدیر منابع انسانی
python tools/generate_role_hse.py                        # مدیر HSE
python tools/generate_role_commercial.py                 # مدیر بازرگانی
python tools/generate_role_procurement.py                # مدیر خرید
python tools/generate_role_processing.py                 # مدیر کارخانه
python tools/generate_role_maintenance.py                # مدیر تعمیرات
python tools/generate_role_ceo.py                        # مدیرعامل

# نهایی‌سازی (شکافت + نمونه‌های منفی)
python tools/finalize_role_based.py <role>

# جانشین ساختاری
python tools/verify_role_based.py "datasets/role based/<role>/batch_01.jsonl" \
  "datasets/role based/<role>/batch_02.jsonl" "datasets/role based/<role>/batch_03.jsonl" \
  "datasets/role based/<role>/batch_04.jsonl" "datasets/role based/<role>/batch_05.jsonl"
```

## ابزارها

```
tools/role_common.py                ابزارهای مشترک
tools/generate_role_based.py        نقش مالی
tools/generate_role_mining.py       نقش معدن
tools/generate_role_warehouse.py    نقش انبار
tools/generate_role_hr.py           نقش منابع انسانی
tools/generate_role_hse.py          نقش HSE
tools/generate_role_commercial.py   نقش بازرگانی
tools/generate_role_procurement.py  نقش خرید
tools/generate_role_processing.py   نقش کارخانه
tools/generate_role_maintenance.py  نقش تعمیرات
tools/generate_role_ceo.py          نقش مدیرعامل
tools/finalize_role_based.py        شکافت و نمونه‌های منفی
tools/verify_role_based.py          جانشین ساختاری
```

</div>
