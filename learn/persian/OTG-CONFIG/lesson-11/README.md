 # بررسی  Cloud Storage

در این بخش از دوره آموزشی OWASP-WSTG به دومین بخش از استاندارد **WSTG** با شناسه WSTG-CONFIG-11 می پردازیم که مربوط به بررسی Cloud Storage می باشد.

### خلاصه


سرویس‌های ذخیره‌سازی ابری، نرم‌افزار و سرویس‌های وب را برای ذخیره و دسترسی به اشیا در سرویس ذخیره‌سازی، تسهیل می‌کنند. با این حال، پیکربندی نامناسب کنترل دسترسی ممکن است منجر به در معرض قرار گرفتن اطلاعات حساس، دستکاری داده‌ها یا دسترسی غیر مجاز شود.

یک مثال شناخته‌شده در مواردی است که یک باکت S3 آمازون درست پیکربندی نشده باشد، اگرچه دیگر خدمات ذخیره‌سازی ابری نیز ممکن است در معرض ریسک‌های مشابه قرار گیرند. به طور پیش‌فرض، تمام باکت‌های S3 خصوصی هستند و تنها توسط کاربرانی قابل‌دسترسی هستند که دسترسی به آن‌ها به صراحت اعطا شده‌است.

کاربران می‌توانند دسترسی عمومی به خود باکت و اشیا منحصر به فرد ذخیره‌شده در آن باکت را فراهم کنند. این امر ممکن است منجر به این شود که یک کاربر غیر مجاز قادر به آپلود فایل‌های جدید، اصلاح یا خواندن فایل‌های ذخیره‌شده باشد.
### اهداف تست

ارزیابی کنید که پیکربندی کنترل دسترسی برای خدمات ذخیره‌سازی به درستی در جای خود قرار داده شده است.

### چگونه تست را انجام دهیم

ابتدا URL را برای دسترسی به داده‌ها، در سرویس ذخیره‌سازی شناسایی نموده و سپس تست‌های زیر را در نظر بگیرید:

* خواندن داده‌های غیر مجاز
* آپلود یک فایل دل‌خواه جدید

شما ممکن است از curl با دستورات زیر برای انجام آزمایش‌ها استفاده نموده و مشاهده نمایید که آیا اقدامات غیر مجاز می‌توانند با موفقیت انجام شوند یا خیر.

برای تست توانایی خواندن یک شی از دستور زیر استفاده نمایید:
```bash
curl -X GET https://<cloud-storage-service>/<object>
```
برای تست توانایی بارگذاری یک فایل از دستور زیر استفاده نمایید:
```bash
curl -X PUT -d 'test' 'https://<cloud-storage-service>/test.txt'
```
تست پیکربندی نادرست باکت S3 آمازون

موارد URL های باکت S3 آمازون یکی از دو فرمت Virtual Hosted Style و Path-Style را دنبال می‌کنند.

*  دسترسی به Virtual Hosted Style

https://bucket-name.s3.Region.amazonaws.com/key-name


در مثال زیر، نام باکت my-bucket می‌باشد، us-west-2 نام Region بوده و puppy.png نام کلید است.

https://my-bucket.s3.us-west-2.amazonaws.com/puppy.png

* دسترسی به Path-Style

https://s3.Region.amazonaws.com/bucket-name/key-name

مانند مثال بالا، در مثال زیر، نام باکت my-bucket است،us-west-2 نام Region بوده و puppy.jpg نام کلید است:

https://s3-us-west-2.amazonaws.com/my-bucket/puppy.jpg


برای برخی Region ها،legacy global endpoint که یک نقطه پایانی خاص منطقه‌ای را مشخص نمی‌کند، می‌تواند مورد استفاده قرار گیرد. فرمت آن هم virtual hosted style یا path-style است.

* دسترسی به Virtual Hosted Style

https://bucket-name.s3.amazonaws.com

* دسترسی به Path-Style

https://s3.amazonaws.com/bucket-name

### شناسایی Bucket URL

برای تست جعبه سیاه، می‌توانURL های S3 را در پیغام‌های HTTP پیدا کرد. مثال زیر نشان می‌دهد که یک Bucket URL در برچسب img در پاسخ HTTP فرستاده می‌شود:
```html
<img src="https://my-bucket.s3.us-west-2.amazonaws.com/puppy.png">
```

برای تست جعبه خاکستری، می‌توانید URL های باکت را از رابط وب آمازون، اسناد، کد منبع، یا هر منبع موجود دیگری به دست آورید.

### تست با AWS-CLI

علاوه بر تست با curl، می‌توانید با ابزار خط فرمان AWS نیز تست را انجام دهید. در این مورد از پروتکل s3:// استفاده شده‌است.

### حالت List

دستور زیر تمام اشیا باکت را زمانی که پیکربندی عمومی انجام گرفته است، لیست می‌کند:

aws s3 1s s3://<bucket-name>

### حالت Upload

در مثال زیر نیز، فرمانی برای آپلود یک فایل آورده شده‌است:

 aws s3 cp arbitrary-file s3://bucket-name/path-to-save

زمانی که آپلود، موفقیت آمیز باشد، پاسخی مانند مثال زیر نمایش داده می‌شود:

$ aws s3 cp test.txt s3://bucket-name/test.txt 
upload: ./test.txt to s3://bucket-name/test.txt

زمانی که آپلود، با شکست مواجه شود، پاسخی مانند مثال زیر نمایش داده می‌شود:

 $ aws s3 cp test.txt s3://bucket-name/test.txt
upload failed: ./test2.txt to s3://bucket-name/test2.txt An error occurred (AccessDenied) when calling
the PutObject operation: Access Denied

### حالت Remove

همچنین در مثال زیر، فرمانی برای حذف شی آورده شده‌است:

 aws s3 rm s3://bucket-name/object-to-remove

# ابزارها

AWS CLI