---
 title: "برنامه‌نویسی برای عملیات CRUD در لیست‌های شیرپوینت" 
 date: 2013-08-24T20:19:34+03:30
 draft: false 
 categories: ["SharePoint Development"]
 cover: "/img/sharepoint-logo.png"
---



### افزودن اطلاعات به لیست شیرپوینت



برای افزودن اطلاعات به لیست شیرپوینت از کدی مانند کد زیر استفاده می‌کنیم. در این کد هم همان‌طور که مشاهده می‌کنید بحث استفاده از display name ستون‌ها مطرح است. در کد زیر لیستی داریم که دو ستون Title و Birthday دارد و با کد به آن‌ها مقدار داده‌ایم.


{{< gist hameds 3e81ecb10b735525b8b78ea60dcb7439 >}}



در کد بالا نکته قابل توجه اول در خط 6 است جایی که ابتدا یک شی از کلاس `SPListItem` ایجاد کرده‌ایم. این شی را با متد `Add` مقدار داده‌ایم که ممکن است کمی گیج کننده به نظر برسد. واقعیت این است که چون ساختار یک آیتم لیست شیرپوینت می‌تواند به شکل‌های گوناگونی باشد (ستون‌های متفاوتی داشته باشد) استفاده از متد `Add` کمک می‌کند تا ساختار مربوط به آیتم این لیست خاص (MyList) در شی item در حافظه ایجاد شود. بعداً و مثلاً در خط 7 می‌توانید ستون‌های مختلف این ساختار را مقداردهی کنید.  
در خط 9 هم برای ذخیره کردن شی ایجاد شده در حافظه از متد `Update` استفاده می‌کنیم. از این متد برای ویرایش هم که در ادامه به آن اشاره می‌شود استفاده خواهیم کرد.



### خواندن آیتم‌های لیست شیرپوینت



جهت خواندن آیتم‌های لیست شیرپوینت باید ابتدا آیتم‌های مورد نظر را مشخص کنیم. یک راه استفاده از روش زیر است که تمام آیتم‌های یک لیست را برمی‌گرداند.


{{< gist hameds a2d7e63b1bbf2a138e0cfd8e2220fe24 >}}



در دنیای واقعی ما باید آیتم‌های مورد نظرمان را پیدا کنیم. برای دریافت یک آیتم خاص از متد `GetItemById` می‌توانیم استفاده کنیم. راه دیگری هم وجود دارد که استفاده از کوئری‌های CAML است که در مطلب دیگری به آن خواهم پرداخت.


{{< gist hameds 448362cc96a637b577953fd2042e9169 >}}



در کد بالا آیتمی با شناسه 5 را دریافت کرده و محتوای ستون Title آن را در کنسول نوشته‌ایم.



### به روز رسانی آیتم‌های لیست شیرپوینت



برای به روز رسانی ابتدا باید آیتم را دریافت و سپس ستون‌های آن را مقداردهی کنیم. مانند مثال زیر:



{{< gist hameds 95c954283f29293339c0b7336c155658 >}}    



در مثال بالا آیتمی با شناسه 5 را دریافت و ستون Title آن را به عبارت جدیدی تغییر داده‌ایم. همانطور که مشاهده می‌کنید باز هم از متد Update استفاده کرده‌ایم.



### تفاوت Update و SystemUpdate



در خط 8 کد بالا از متد update استفاده کرده‌ایم. یک گزینه دیگر استفاده از متد `SystemUpdate` است. این دو متد تفاوتی اساسی با هم دارند. به صورت کلی در شیرپوینت هر زمان شما آیتمی را اضافه یا ویرایش می‌کنید چند ستون سیستمی مقدار می‌گیرند. ستون‌هایی مثل ایجاد کننده، ویرایش کننده،‌ تاریخ ایجاد، تاریخ آخرین ویرایش. اگر از متد `Update` استفاده کنید این ستون‌ها مقدار خواهند گرفت و اگر از متد `SystemUpdate` استفاده کنید مقدار قبلی این ستون‌ها تغییری نمی‌کند. در واقع می‌توانید آیتمی را ویرایش کنید بدون اینکه مثلاً ستون تاریخ ویرایش آن تغییر کند.



### تایید آیتم‌های لیست شیرپوینت با برنامه‌نویسی



در بعضی لیست‌های شیرپوینت ممکن است پروسه تایید اطلاعات فعال شده باشد. در این جریان هر آیتم می‌تواند شامل سه وضعیت تایید شده، رد شده یا معلق باشد. چنانچه بخواهیم آیتمی را تایید کنیم از کد زیر می‌توان استفاده کرد. در واقع تایید هم نوعی ویرایش ستون در لیست شیرپوینت است.



{{< gist hameds 58534c207b5952ef0c386f4f8bef7b70 >}}



### حذف آیتم‌ها از لیست شیرپوینت



برای حذف با برنامه‌نویسی بعد از دریافت آیتم، خیلی راحت می‌توانیم از متد `Delete` استفاده کنیم و نیازی به استفاده از متد `Update` نیست. مانند مثال زیر:



{{< gist hameds 2ba331cf89a5a42f28a6ca433c8d3d67 >}}



در نوشته‌های بعدی مثال‌های کاربردی‌تری از استفاده object model شیرپوینت را با هم بررسی خواهیم کرد.

