---
 title: "۳۰ روز با TDD: روز هجدهم - بازبینی Refactoring قسمت اول" 
 date: 2016-12-11T00:00:00+03:30
 draft: false 
 categories: ["Test Driven Development"]
 cover: "/oldimg/tdd_cycle.jpg"
---


توجه: قبل از این نوشته، آزمون‌های واحد (Unit testها) مربوط به تغییرات PlaceOrder نوشته قبلی را از [اینجا](https://gist.github.com/JamesBender/7321252) می‌توانید دانلود کنید.

در چند نوشته گذشته، متد PlaceOrder را از OrderService بیرون بردیم. برای مرور، متد فعلی این شکلی است:

{{< gist hameds 84a4e1f9de333a2923a47e143b173a40 >}}


این متد کمی طولانی شده و همچنین داریم به محدوده نقض Single Responsibility Principel (برای مرور SRP [روز پنجم](/post/82-30-روز-با-tdd--روز-پنجم---کد-solid-ایجاد-کنید/) را مطالعه کنید) وارد می‌شویم. در حال حاضر شش دلیل برای اینکه این متد باید تغییر کند شمردم:



۱. اعتبارسنجی هر آیتم سبد خرید  
۲. روش باز شدن session ها با سرویس order fulfillment  
۳. روش چک کردن انبار با سرویس order fulfillment  
۴. روش ثبت سفارش با سرویس order fulfillment  
۵. بسته شدن session ها با order fulfillment  
۶. ایجاد و ذخیره یک سفارش



در لیست بالا «ایجاد و ذخیره یک سفارش» موردی است که کمترین نگرانی را درباره‌اش دارم. کد زیادی درباره ایجاد و ذخیره سفارش نیست پس فعلاً جای نگرای نیست. این موضوع بعداً در جریان توسعه نرم‌افزار قطعاً تغییر می‌کند اما فعلاً در رادار من نیست. مورد اول هم گرچه جای نگرانی دارد ولی در حال حاضر بدترین مشکل نقض SRP نیست، بنابراین می‌تواند منتظر بماند.



چیزی که الان می‌خواهم رویش تمرکز کنم موارد ۲ تا ۵ هستند. همه این موارد با سرویس order fulfillment در ارتباطند و بخش زیادی از کد را تشکیل می‌دهند.



توسعه‌دهنده‌هایی که درباره ایده SRP یا refactoring تازه‌وارد هستند احتمالاً به لیست بالا نگاه می‌کنند و فکر می‌کنند احتمالاً به ۴ متد جدید برای هر یک از مسائل ۲ تا ۵ نیاز داریم. این اشتباه نیست، اما بخشی از داستان است. اینکه خیلی ساده ۴ متد جدید اضافه کنیم، کد را خواناتر و قابل استفاده مجددتر می‌کند، اما همچنین شش دلیل برای تغییر PlaceOrder خواهیم داشت. SRP‌ و refactoring فقط این نیست که کدها را به متدهای خصوصی (private methods) ببریم تا متدهای اصلی را کوتاه‌تر کنیم، بلکه درباره ایجاد تجرد منطقی (logical abstraction) در کد است تا مرا در برابر تغییرات آینده محافظت کند.



آنچه می‌بینم، حجم زیادی پروسه کار با سرویس order fulfillment است که می‌خواهم از متد PlaceOrder جدایش کنم. وقتی این کار انجام شد، برای کل این پروسه طولانی کار با order fulfillment فقط یک متد باید فراخوانی کنم. این کار مرا در برابر تغییرات مرتبط در هر یک از مراحل کار با سرویس order fulfillment محافظت می‌کند. پس قدم اول استخراج همه کدهایی که با سرویس order fulfillment کار می‌کنند برای ایجاد یک متد خصوصی جداگانه است. چون از Just Code (محصول شرکت تلریک) استفاده می‌کنم این کار ساده است: می‌توانم یک بلوک کد را انتخاب و از منوی کمکی Just Code گزینه Extract Method را مشابه تصویر زیر انتخاب کنم یا از کلید میانبر (CTRL + R, CTRL + M) استفاده کنم و Just Code بخش انتخاب شده را برای یک متد خصوصی جدید استخراج می‌کند.







![](/oldimg/image_thumb29c575ca485f8-png.png)

Just Code کد مورد نیاز را به همراه پارامترهای مورد نیاز استخراج می‌کند و از من درباره نام متد سوال می‌کند و من هم نام PlaceOrdeWithFulfillmentService را انتخاب می‌کنم. همچنین کد اصلی را با فراخوانی این متد تازه ساخته شده تغییر می‌دهد.

{{< gist hameds d363637dd93751f7444f1e879efdec6f >}}

تاثیر این کار بر روی متد PlaceOrder خیلی زیاد است و همین حالا هم بسیار خواناتر شده. متوجه شدم که JustCode متد تازه ساخته شده را با نوع بازگشتی void ایجاد کرده است. این به خاطر آن است که در هیچ کجای کد استخراج شده مقداری تولید نمی‌شود که در ادامه کد اصلی از آن استفاده شده باشد. در این مورد در نوشته بعدی بیشتر صحبت می‌کنیم. برای تایید این کارها و اینکه این تغییرات کد را دچار مشکل نکرده، باید unit test ها را اجرا کنم.



![](/oldimg/34image_thumb4-png.png)

نتیجه اجرای تست‌ها به این معنی است که اولین refactor من کار کرده و نوع بازگشتی متد PlaceOrderWithFulfillmentService فعلاً می‌تواند void باشد. حالا اجازه بدهید به سراغ متد تازه ایجاد شده برویم و refactor کنیم.





متد PlaceOrderWithFulfillmentService باعث شده که کد ما کمتر شکننده باشد، اما خودش هم مشکلاتی دارد. ما به refactor بیشتری نیاز داریم و حالا می‌خواهم هر یک از مراحل را به متدهای خودشان منتقل کنم. به سراغ اولین مرحله می‌روم: OpenOrderFuilfillmentService



{{< gist hameds fee7c4a8f74d44f2bf68675534d614bd >}}


اجرای تست‌ها نشان می‌دهد که این refactor هم موفق بوده است. حالا مرحله بستن session را هم به متد جدیدی به نام CloseOrderFulfillmentService می‌برم

{{< gist hameds d6cecf1b08ce55baead4c7a2d8a9561c >}}

اجرای تست‌ها نشان می‌دهد که همچنان کد بدون مشکل کار می‌کند.


![](/oldimg/22image_thumb-png.png)

دو مرحله باقیمانده کمی سخت‌تر هستند، بنابراین در نوشته بعدی درباره‌اش صحبت می‌کنیم.



ادامه دارد...

