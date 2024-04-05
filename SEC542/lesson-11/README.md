# جمع آوری اطلاعات در وب

در این بخش از دوره آموزشی SEC542 شما با ابزار Nmap آشنا شده و بخش هایی از استاندارد OWASP که مربوط به جمع آوری اطلاعات در وب می باشد را فراخواهید گرفت.

### نرم افزار Nmap

ابزار Nmap یکی از ابزارهای محبوب و کاربردی در زمینه اسکن شبکه و پورت می باشد که دارای قابلیت های مختلفی برای شناسایی پورت های باز، سرویس ها موجود بر روی سیستم و همچنین تشخیص سیستم عامل هدف است. یکی از مشکلاتی که نفوذگر با آن درگیر می باشد، امکان شناسایی وی است چراکه Nmap برای شناسایی پورت ها و موارد دیگر، یک درخواست را به هر پورت ارسال می کند و بر اساس پاسخ دریافتی، تعیین می کند که پورت مورد نظر باز یا بسته بوده و یا اینکه توسط فایروال فیلتر شده است.

البته شما با پیکربندی زمان ارسال بسته ها توسط Nmap و همچنین روش اسکن می توانید احتمال تشخیص توسط سیستم های امنیتی را کاهش دهید. با توجه به این موضوع اگر شما اسکن را به صورت آهسته تر انجام دهید به دلیل حجم ترافیکی تولیدی کمتر، امکان تشخیص شما کمتر خواهد شد.

یکی از موارد دیگر در هنگام اسکن استفاده از SYN Scan می باشد. در این نوع اسکن ارتباط TCP کامل نمی شود و این امر موجب می شود که ارتباط شما کامل نشده و اسکن به صورت مخفیانه انجام شود. البته امروزه اکثر سیستم های تشخیص نفوذ این نوع اسکن ها را شناسایی می کنند ولی چون اسکن پورت امروزه بسیار رایج می باشد، اغلب افراد هشدارهای مربوط به چنین اسکنی را نادیده می گیرند.

همچنین شما می توانید با استفاده از سوییچ های Nmap مانند -sV سرویس های موجود بر روی یک سیستم را شناسایی نمایید و با استفاده از سوییچ -O می توانید سیستم عامل هدف را تشخیص دهید.

### انجام Server Profiling

پس از شناسایی پورت و سیستم عامل هدف، مرحله بعدی Server Profiling است. این تست عمیق تر از تشخیص سیستم عامل هدف می باشد و بر روی بررسی پیکربندی و شناسایی دستگاه های زیرساختی پشتیبانی شده تمرکز می کند.

هر سرور با سرور دیگر متفاوت می باشد. نه تنها در ابزارها و نرم افزارهای در حال اجرا بلکه در رابطه با توپولوژی شکه و ارتباط با سیستم های دیگر نیز تفاوت وجود دارد. در این حالت شما برای هر سرور باید ارائه دهنده نرم افزار HTTP را شناسایی نمایید و علاوه بر این باید پلاگین ها، ویژگی های اضافی، پشتیبانی از SSL، نوع سرور میزبان وب سایت(مبتنی بر IP یا HOST) و اینکه آیا سایت در پشت یک Load Balancer می باشد یا خیر را نیز باید شناسایی نمایید.

هنگامی که شما نرم افزارهای موجود بر روی سرور را شناسایی می کنید، اطلاعات به دست آمده بر روی سطوح حمله و تکنیک های مورد استفاده توسط شما بسیار تاثیرگذار می باشد. به عنوان مثال نحوه حمله در سرور Apache با سیستم عامل FreeBSD و PHP4 با حمله در IIS و FrontPage متفاوت خواهد بود.

همانطور که وب سرورها یک هدف اصلی در طول آزمون تست نفوذ هستند، تعیین نوع و نسخه آن بسیار حائز اهمیت هست زیرا ممکن است نسخه خاصی از وب سرور دارای آسیب پذیر باشد و بتوان از آن استفاده نمود.

همچنین در صورت شناسایی نوع سیستم عامل و زبان برنامه نویسی بکار رفته، نحوه انجام فرآیندهایی مانند Injection و اکسپلویت نمودن آن متفاوت خواهد بود. از آنجایی که نتایج این بخش بسیار مهم می باشد پس باید سعی کنیم تا چندین تست را برای تایید نتایج انجام دهیم تا از صحت نتایج اطمینان حاصل نماییم.

ابزار Nmap در مورد مذکور فوق العاده عمل می کند و می تواند اطلاعات مربوط به سرور و سرویس های آن را به ما نمایش دهد. برای این منظور ما از سوییچ -sV استفاده می کنیم که بیانگر سرویس اسکن می باشد. البته سوییچ -A نیز به منظور شناسایی سرویس و سیستم عامل هدف مورد استفاده قرار می گیرد.

با استفاده از سوییچ های مذکور، Nmap سعی در اتصال به پورت های مورد نظر و جست و جو برای مشاهده بنرهای آن ها را دارد. همچنین با استفاده از تطبیق پاسخ های ارسالی از سیستم با پایگاه داده خود را دارد تا مشخص کنید که سیستم عامل هدف از چه نوعی می باشد.

### مرحله  Using Netcat to Grab Server Connection Strings

یکی از ابزارهای ساده اما قدرتمند، ابزار Netcat است. به این ابزار لقب چاقوی سوئیسی یا Swiss Army knife داده شده است. دلیل این نام گذاری تا حدودی به این دلیل است که می تواند به هر سرویس شبکه متصل شود و به نفوذگر اجازه می دهد که علاوه بر این، پورت های شبکه را اسکن نموده، فایل های مورد نظر را به یک سیستم آپلود و یا فایلی را از آن دانلود نماید.

برای یک تست نفوذگر وب، Netcat به منظور اتصال به وب سرور و وارد نمودن HTTP Verb استفاده شده تا داده های مربوط به صفحات را بازیابی نماید و اطلاعات مورد نظر در مورد نسخه سرور را افشا نماید. در داده های بازگشت داده شده در این حالت، بخش هایی با عنوان X-Powered-By و Server برای تست نفوذگر مفید می باشد.

توجه داشته باشید که اگرچه اطلاعات مربوط به بنر سرور اغلب دقیق می باشد ولی ممکن است این بنرها توسط مدیر سرور به منظور گمراه نمودن نفوذگر، تغییر داده شده باشند. به عنوان مثال می توان سایت را به گونه ای پیکربندی کرد که در بنر خود اطلاعاتی مانند IIS v6 را نمایش دهد در حالی که وب سرور واقعی آن آپاچی می باشد.

در مثال بالا یک درخواست برای اتصال به وب سرور و بازیابی اطلاعات آن ارسال شده است. در این مثال با استفاده از Printf اطلاعات مربوطه را به Netcat ارسال نموده و منتظر پاسخ آن می مانیم. سپس Response صفحه به ما نمایش داده می شود که شامل اطلاعات در مورد سرور و نسخه آن است. همچنین در قسمت دوم با توجه به استفاده سرور از ماژول امنیتی Apache Mode Security، بنر سرور به Atari/2600 تغییر پیدا کرده است.

در دوره آموزشی SEC504 اطلاعات کامل تری در خصوص ابزار Netcat، ویژگی های آن و همچنین نحوه کار با این ابزار قدرتمند ارائه می گردد که پیشنهاد می کنیم به لینک زیر جهت مشاهده بخش های این دوره مراجعه نمایید.

### مرحله Netcraft Detection

شرکت Netcraft یک شرکت خدمات اینترنتی است که نتایج تجزیه و تحلیل را برای بهره برداری وب سرورهای مختلف در اینترنت و نیز اطلاعات مربوط به سایت های حمله فیشینگ ارائه می دهد. این شرکت با جمع آوری از طریق روش های نمونه برداری و همچین از طریق نوار ابزار Netcraft چندین سال است که اطلاعات مربوط به توزیع های مختلف وب سرورها و اطلاعات نسخه های اجرا شده بر روی آن ها را از درصد زیادی از اینترنت جمع آورد نموده است.

برای برخی از سایت ها، Netcraft می تواند اطلاعات مربوط به آن، نسخه سرور و تاریخچه تغییرات وب سرور را به شما نمایش دهد. توجه داشته باشید که اگرچه با جست و جوی یک سرور در Netcraft برای شناسایی وب سرور، به صورت مستقیم ارتباطی با آن وب سرور انجام نمی گیرد ولی آدرس IP و مشخصات شما در Netcraft را ثبت می شود. به همین خاطر پیشنهاد می شود که قبل از استفاده از Netcraft محدوده تست خود را مشخص نموده و مراحل ایجاد قرارداد را طی نمایید تا اطمینان حاصل کنید که فعالیت های شما به صورت قانونی صورت می گیرد.

در ادامه نمونه ای از نتایج مربوط به سایت isc.sans.org در سایت Netcraft می باشد.

در این دوره، بخش هایی از متدولوژی OWASP که به منظور تست برنامه های تحت وب طراحی شده است، آورده می شود که این بخش هم نمونه ای از همین مورد می باشد.

بخش Fingerprint Web Server یا شناسایی وب سرور، یکی از موارد مهم در فرآیند تست نفوذ می باشد. شناسایی نسخه و نوع وب سرور در حال اجرا، موجب تسریع در شناسایی آسیب پذیری های شناخته شده و بکارگیری اکسپلویت های مناسب می گردد. توجه داشته باشید که یک وب سرور قدیمی یا وصله نشده می تواند برنامه تحت وب را تحت تاثیر قرار دهد.

در این بخش از متدولوژی OWASP با استفاده از ابزارهای مختلف از جمله Nmap و Netcraft که در مباحث پیشین به آن ها اشاره گردید، اقدام به شناسایی وب سرور هدف می نماییم. البته استفاده از افزونه Wappalyzer هم برای شناسایی برنامه های درحال اجرا بر روی سرور و نسخه وب سرور نیز مفید می باشد.

همچنین علاوه بر موارد مذکور، شناسایی سیستم عامل، سرویس های قابل اجرا و زبان برنامه نویسی وب سایت نیز باید در ادامه این بخش شناسایی شود که به دست آوردن متدهای HTTP و صفحات پیش فرض موجود بر روی وب سرور دو راه برای شناسایی بهتر وب سرور می باشد که در بخش بعدی به آن اشاره خواهد شد.

یکی از قابلیت های موجود در پروتکل HTTP امکان استفاده از متدهای مختلف است که در بخشی با همین نام در قسمت های پیشین به آن ها اشاره گردید. بسیاری از این متدها جهت کمک به توسعه دهندگان وب در استقرار و آزمایش برنامه های HTTP طراحی شده اند. در صورتی که تنظیمات مربوط به متدهای HTTP به درستی انجام نشده باشد، سوء استفاده از آن ها علیه وب سرور را امکان پذیر می سازد. به همین منظور تست نفوذگر وب باید متدهای پشتیبانی شده توسط وب سرور را شناسایی نماید.

اغلب وب سرورها از متدهای مشهور GET و POST استفاده می نمایند ولی به هر حال متدهای زیر می توانند در وب سرورها به کار برده شوند:

* **متد PUT:** این متد توسط WEBDAV برای نوشتن و قرار دادن فایل بر روی وب سرور استفاده می شود.
* **متد DELETE:** این متد توسط WEBDAV برای حذف فایل ها از وب سرور مورد استفاده قرار می گیرد.
* **متد CONNECT:** از این متد برای ایجاد یک TCP Tunnel از طریق پراکسی استفاده می شود.
* **متد TRACE:** از این متد برای بررسی تغییراتی که رابط های شبکه بر روی بسته می گذارند، استفاده می شود. استفاده از این متد باعث می شود که کاربر هر گونه اطلاعاتی را که توسط واسطه هایی مانند Load Balancer ها و پروکسی ها اضافه می شوند مشاهده نماید.
* **متد OPTIONS:** از این متد برای نمایش متدهای پشتیبانی شده استفاده می شود.


برای شناسایی این متدها می توان از ابزارهای مختلفی مانند Nmap، Netcat، Metasploit و ابزارهای دیگر استفاده نمود.

به عنوان مثال دستور زیر از ابزار nmap برای شناسایی متدهای HTTP مورد استفاده قرار می گیرد:

nmap –script=http-headers -p 80 -sS -sC -sV site.com

با استفاده از ابزار Netcat نیز می توان از شل اسکریپت زیر استفاده نمود:
Bash Script
Default Pages

مورد دیگری که باید آن را جست و جو نمود، صفحات پیش فرض است. این صفحات هنگام نصب سرور ساخته می شوند. نمونه هایی از آن ها، صفحات خوش آمدگویی وب سرور مانند Apache و IIS است. یکی از روش های شناسایی صفحات پیش فرض، دسترسی به سرور از طریق آدرس IP می باشد.

بسیاری از اوقات سرورها برای نمایش صفحات مختلف در سرور هنگام درخواست دامنه های موجود بر روی آن، صفحات سفارشی مورد نظر را نمایش می دهند، ولی در بیشتر موارد مدیران سرور فراموش می کنند تا صفحه پیش فرض را هنگامی که درخواست بر اساس IP انجام می شود، تغییر دهند.

یکی از ابزارهای عالی برای شناسایی محتوای پیش فرض در وب سرور، ابزار Nikto می باشد. این ابزار توسط Chris Sullo و با زبان Perl نوشته شده است.
Software Configuration Flaws

ارزیابی مربوط به مدیریت پیکربندی، بخش قابل توجهی از راهنمای تست نفوذ وب OWASP را شامل می شود. همانطور که در بخش پیشین به آن اشاره گردید، برای تست پیکربندی، یکی از ابزارهای مفید، Nikto می باشد.

اگرچه وجود مشکل در پیکربندی به اندازه مشکلاتی مانند SQL Injection، XSS و آسیب پذیری های مشابه خطرناک نمی باشد ولی با این حال وجود مشکل در پیکربندی می تواند منجر به افشای اطلاعات مختلف، شناسایی آسیب پذیری های مذکور و حتی در برخی از موارد موجب دسترسی به سرور شود.
