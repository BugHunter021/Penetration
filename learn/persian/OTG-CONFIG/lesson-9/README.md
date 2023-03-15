# بررسی File Permission

در این بخش از دوره آموزشی OWASP-WSTG به دومین بخش از استاندارد **WSTG** با شناسه WSTG-CONFIG-009 می پردازیم که مربوط به بررسی File Permission می باشد.

### خلاصه

هنگامی که دسترسی به یک منبع یا فایل برای رنج وسیعی از افراد تعریف شده باشد، این امر می‌تواند منجر به دسترسی غیرمجاز، افشای اطلاعات حساس، تغییر در ساختار این فایل و یا موارد دیگر شود.

این موضوع به ویژه هنگامی خطرناک‌تر می‌شود که منبع مورد نظر مربوط به پیکربندی برنامه، اجرا یا داده‌های حساس کاربر باشد.

یک مثال واضح از این موضوع، یک فایل اجرایی است که توسط کاربران غیرمجاز قابل اجرا باشد.

مثال دیگر، می‌تواند ذخیره اطلاعات اکانت یا یک Token Value برای دسترسی به یک API (این موضوع به طور فزاینده‌ای در سرویس‌های وب مدرن دیده می‌شود) در یک فایل پیکربندی باشد که مجوزهای آن به طور پیش‌فرض (از هنگام نصب اولیه) به گونه‌ای تنظیم شده است که برای کلیه افراد قابل خواندن می‌باشد.

چنین اطلاعات حساسی می‌تواند توسط عوامل مخرب داخلی به خطر بیافتد. همچنین این فایل‌ها ممکن است توسط یک نفوذگر از راه دور که بوسیله یک آسیب‌پذیری به سامانه نفوذ نموده است (که تنها سطح دسترسی یک کاربر عادی را دارد) نیز به خطر بیافتد.
### اهداف تست

بررسی و شناسایی سطوح دسترسی هر یک از فایل‌ها

### چگونگی تست را انجام دهیم

در سیستم عامل لینوکس، از دستور ls می‌توان برای بررسی سطح دسترسی فایل‌ها استفاده نمود. علاوه بر این از دستور namei نیز می‌توان برای بررسی سطوح دسترسی به صورت Recursive استفاده نمود:
```bash
namei -l /PathToCheck/
```
فایل‌ها و دایرکتورهایی که نیاز به بررسی مجوزهای دسترسی دارند شامل موارد زیر هستند (البته تنها محدود به موارد زیر نیستند):

* دایرکتوری و فایل های مربوط به وب
* دایرکتوری و فایل های پیکربندی
* دایرکتوری و فایل هایی که شامل اطلاعات حساس هستند (داده های رمز شده، کلمات عبور، کلیدها)
* دایرکتوری و فایل های لاگ (security logs, operation logs, admin logs)
* دایرکتوری و فایل های قابل اجرا (scripts, EXE, JAR, class, PHP, ASP)
* دایرکتوری و فایل های مربوط به پایگاه داده
* دایرکتوری و فایل های موقتی (Temp)
* دایرکتوری و فایل های مربوط به آپلود
### بررسی Remediation

سطوح دسترسی فایل‌ها و دایرکتوری‌ها می‌بایست به درستی تنظیم شود تا کاربران غیر مجاز نتوانند بدون دلیل به منابع حیاتی سیستم دسترسی داشته باشند.

# ابزارها

* Windows AccessEnum
* Windows AccessChk
* Linux namei
