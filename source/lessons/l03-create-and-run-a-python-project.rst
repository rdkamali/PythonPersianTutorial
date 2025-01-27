.. role:: emoji-size

.. meta::
   :description: پایتون به پارسی - کتاب آنلاین و آزاد آموزش زبان برنامه‌نویسی پایتون - درس سوم: چگونگی ایجاد و اجرای یک پروژه پایتون
   :keywords: پایتون,آموزش پایتون, آموزش برنامه نویسی, ایجاد پروژه پایتون, اسکریپت پایتون, ماژول پایتون, بسته پایتون, ساختار پایتون, پروژه پایتون, سورس کد, سورس کد پایتون, اجرای پایتون, اسکریپت, ماژول, pyvenv, virtualenv


.. _lesson-03: 

درس ۰۳: چگونگی ایجاد و اجرای یک پروژه پایتون
=============================================

.. figure:: /_static/pages/03-python-project-structure.jpg
    :align: center
    :alt: چگونگی ایجاد و اجرای یک پروژه پایتون
    :class: page-image

    Photo by `Sam Moqadam <https://unsplash.com/photos/UkwbRZkt8zM>`__

این درس به چگونگی ایجاد پروژه‌‌های برنامه‌نویسی پایتون و اجرای آن‌ها اختصاص یافته است. درس با بیان تعاریف و رسم ساختار معمول یک  پروژه شروع  و اشاره‌ای نیز به ساختار پروژه‌های قابل انتشار در PyPI می‌شود. در بخش یکم تلاش شده است که تصویر کاملی از ساختار یک پروژه در ذهن خواننده ایجاد و از این طریق او با تعاریف «بسته»، «ماژول» و «اسکریپت» در زبان پایتون آشنا شود. در دو بخش‌ بعدی نیز ضمن اشاره به دو شیوه‌ اجرای دستورات پایتون، به شیوه ایجاد اسکریپت و چگونگی اجرای آن تمرکز شده است؛ چرا که پروژه‌های پایتون به این شیوه اجرا می‌گردند. در ادامه هم به روند اجرای کد توسط مفسر پایتون و همچنین به معرفی بایت‌کد و در نهایت نیز به معرفی virtualenv و pyvenv پرداخته شده است.

:emoji-size:`✔` سطح: پایه


----

.. contents:: سرفصل‌ها
    :depth: 2

----

.. _definations: 

تعاریف
--------------

سورس کد یک پروژه به زبان پایتون در قالب یک یا چند «ماژول» (Module) توسعه می‌یابد که در سورس کدهایی با بیش از یک ماژول بهتر است ماژول‌هایی که از نظر منطقی با یکدیگر مرتبط هستند را درون دایرکتوری‌هایی مجزا قرار دهیم که به این نوع دایرکتوری‌ها در زبان پایتون «بسته» (Package) گفته می‌شود.

.. tip::
    یک یا چند ماژول درون یک دایرکتوری مشخص تشکیل یک بسته را می‌دهند و هر بسته خود می‌تواند حاوی بسته‌(های) دیگری باشد. 

.. note::
    از نسخه 3.3 پایتون با افزوده شدن ویژگی جدیدی به نام «بسته فضانام» (Namespace Package - `PEP 420 <http://www.python.org/dev/peps/pep-0420>`_)، تعریف بسته پایتون به دو شاخه «بسته عادی» (Regular Package) که همان تعریف قدیمی از بسته می‌باشد و بسته فضانام گسترش یافته است.

    در واقع، تا قبل از پایتون 3.3 هر بسته پایتون می‌بایست حاوی یک فایل ``init__.py__`` باشد ولی دیگر نیازی به این فایل نیست و مفسر پایتون اکنون می‌تواند تمامی بسته‌های داخل پرو‌ژه را محل‌یابی کند. در این درس به منظور شفاف‌سازی بسته‌ها از روش Regular Package استفاده خواهد شد.

در تعریف زبان پایتون دو نوع ماژول وجود دارد:

۱- Pure Module (ماژول عادی)، همان تعریف عادی از ماژول پایتون است؛ فایل‌هایی با پسوند py که کد (تعاریف و دستورات) به زبان پایتون در آن‌ها نوشته می‌شود.

۲- Extension Module (ماژول توسعه)، ماژول‌هایی که توسط زبان‌های برنامه‌نویسی دیگری  به غیر از پایتون  تهیه شده‌اند. از درس یکم به خاطر داریم که پایتون یک زبان توسعه‌پذیر است و در کنار آن می‌توان از کد‌های نوشته شده با دیگر زبان‌های برنامه‌نویسی استفاده نمود: به مانند C و ++C در پیاده‌سازی CPython یا Java در پیاده‌سازی Jython یا #C در پیاده‌سازی IronPython - بررسی این نوع ماژول خارج از حوزه این کتاب است. در صورت علاقه برای مطالعه بیشتر می‌توانید به مستندات پایتون مراجعه (`Python/C API Reference <https://docs.python.org/3/c-api/index.html>`_) یا کتاب Python 3.6 Extending and Embedding Python نوشته Guido Van Rossum را تهیه نمایید.

.. note::
    از این پس هر جایی از این کتاب که گفته شود «ماژول» منظور Pure Module خواهد بود، مگر اینکه نام «ماژول توسعه» به صراحت ذکر گردد.

در ایجاد یک پروژه از پایتون هیچ اجباری به رعایت ساختار خاصی نیست و سورس کد یک پروژه می‌تواند تنها شامل یک ماژول (فایل با پسوند py.) باشد. به عنوان نمونه، شمای پایین از پروژه فرضی SampleProject را در نظر بگیرید:

.. code::
    
    SampleProject
    .
    └── sample_project.py

یا کمی پیچیده‌تر:

.. code::
    
    SampleProject
    .
    ├── sample_project.py
    └── pakage/
        ├── __init__.py
        ├── module_two.py
        └── module_one.py

.. tip::
    در پایتون هر **Regular Package** می‌بایست حاوی فایل ویژه‌‌‌ ``init__.py__``  باشد که البته الزامی به کدنویسی درون این فایل وجود ندارد. این فایل دایرکتوری خود را به عنوان یک بسته (محلی برای یافتن ماژول‌های مرتبط) به مفسر پایتون معرفی می‌کند.

در ایجاد سورس کد باید به صورتی عمل شود که با اجرای یک ماژول‌ مشخص تمام برنامه اجرا گردد. این ماژول معمولا هم نام پروژه در نظر گرفته و با عنوان «اسکریپت» (Script) از آن یاد می‌شود (اینجا:‌ sample_project.py). در واقع اسکریپت، ماژولی است که با هدف اجرای برنامه توسعه می‌یابد و ایجاد سورس کد نیز از آن شروع می‌گردد. 

.. tip::
  هر پروژه پایتون باید حاوی یک اسکریپت باشد ولی می‌تواند هیچ، یک یا چند ماژول داشته باشد  که این ماژول‌ها نیز می‌توانند در قالب بسته‌هایی جداگانه سازمان‌دهی شوند. 

.. tip::
    [`PEP 8 <http://www.python.org/dev/peps/pep-0008/>`_]: در نام‌گذاری ماژول‌ها تنها از حروف کوچک استفاده می‌شود و در صورت نیاز می‌توان از کاراکتر خط زیرین (``_``) نیز استفاده نمود (اما پیشنهاد نمی‌شود). نام بسته‌ها کوتاه بوده و از حروف کوچک تشکیل می‌گردد.

.. _source-code: 

ایجاد سورس کد
---------------
برای ایجاد فایل‌های سورس کد (ماژول‌ها و اسکریپت) نیاز به هیچ برنامه یا ابزار خاصی نیست و تنها با استفاده از یک ویرایشگر ساده متن (مانند برنامه Notepad در ویندوز) می‌توانید آن‌ها را ایجاد و ویرایش نمایید.

در ادامه پروژه‌ای (یا در واقع یک فولدر) به نام FirstProject که  تنها حاوی یک فایل اسکریپت (first_project.py) خواهد بود را ایجاد می‌نماییم. وظیفه این اسکریپت فرستادن حاصل عبارت ``4÷(6×5-50)`` به خروجی  (Output) خواهد بود.

.. code::
    
    FirstProject
    .
    └── first_project.py

داخل فولدر پروژه فایل first_project.py را ایجاد و سپس به کمک یک برنامه‌ ویرایشگر متن آن را ویرایش و کد پایین را در آن درج و سپس ذخیره می‌نماییم.

.. code-block:: python
    :caption: first_project.py
    
    # This script prints a value to the screen.

    print("(50-5×6)÷4 =", (50-5*6)/4)

در بخش بعدی به اجرای پروژه FirstProject خواهیم پرداخت؛ در این بخش بهتر است کمی به بررسی کدهای آن بپردازیم:

در زبان پایتون هر متنی که بعد از کاراکتر ”Number sign“ یا # (در همان سطر) قرار بگیرد توسط مفسر پایتون نادیده گرفته می‌شود و تاثیری در روند ترجمه و اجرای کدها ندارد، به این نوع متن‌ «توضیح» (کامنت Comment) گفته می‌شود و از آن برای مستندسازی (Documentation) ماژول، یعنی ارایه توضیح در مورد بخشی از کد استفاده می‌گردد. ارایه توضیح نقش زیادی در خوانایی ماژول دارد و کمک می‌کند تا افراد دیگر - حتی خودتان - بتوانند عملکرد کدهای ماژول (یا اسکریپت) را بفهمند.

سطرهای خالی (Blank Lines) نیز توسط مفسر پایتون نادیده گرفته می‌شوند و تاثیری در روند ترجمه و اجرای کدها ندارند. استفاده درست از سطرهای خالی بر خوانایی کدهای ماژول می‌افزاید.

روش رایج فرستادن داده به خروجی (اینجا:‌ چاپ بر روی صفحه نمایش) در پایتون، استفاده از تابع ``()print`` است. تابع در برنامه‌نویسی همان مفهوم ریاضی خود را دارد، موجودیتی که مقادیری را به عنوان ورودی دریافت و بر اساس آن خروجی تولید می‌کند، بحث تابع بسیار گسترده است که طی دروس دوازدهم تا چهاردهم به صورت کامل شرح داده خواهد شد.

تابع print توانایی دریافت هر تعداد داده و از هر نوع را دارد و در صورت دریافت یک عبارت محاسباتی (Arithmetic) یا منطقی (Logical) ابتدا حاصل آن را محاسبه یا ارزیابی کرده و پس از تبدیل به نوع داده string در خروجی قرار می‌دهد. در هنگام فرستادن چندین داده گوناگون به خروجی می‌بایست آن‌ها را توسط کاما (Comma) از یکدیگر جدا نماییم. در اینجا نیز print دو داده برای فرستادن به خروجی دریافت کرده است؛ یک نوع داده string و یک عبارت محاسباتی.


به دنباله‌ای از کاراکترها که بین دو نماد نقل قول (Quotation): ``" "`` یا ``' '`` محصور شده‌ باشند، نوع داده string گفته می‌شود.


.. _running: 


اجرای سورس کد
---------------
در حالت کلی به دو شیوه می‌توان به زبان پایتون کد نوشت و اجرا نمود: ۱- به حالت تعاملی (Interactive) با مفسر پایتون ۲- با ایجاد اسکریپت پایتون.

شیوه تعاملی: در این روش می‌بایست ابتدا دستور فراخوانی مفسر پایتون (حالت عمومی دستور: ``python``) را در رابط خط فرمان سیستم عامل وارد نمایید؛ توسط این دستور خط فرمان وارد حالت تعاملی پایتون می‌شود و اکنون به سادگی می‌توانید شروع به کد‌نویسی نمایید. در این حالت هر کدی که وارد شود بلافاصله اجرا شده و در صورت لزوم نتیجه آن نیز نمایش داده می‌شود. از آنجا که در این روش امکان برگشت و ویرایش کدهای وارد شده وجود ندارد، در عمل زیاد کارآمد نبوده و از آن بیشتر در مواردی مانند گرفتن نتیجه‌ قطعه کدهای کوچک، اهداف آموزشی، دریافت راهنمایی یا ماشین حساب! استفاده می‌گردد. چگونگی کار با حالت تعاملی پایتون در درس بعدی بررسی می‌شود.

.. code::
    
    > python
    Python 3.10.5 (main, Jul 22 2022, 17:09:35) [GCC 9.4.0] on linux
    Type "help", "copyright", "credits" or "license" for more information.
    >>>
    >>> 
    >>> a = 3
    >>> b = 2
    >>> a * b
    6
    >>>

شیوه دیگر که موضوع همین بخش است، ایجاد اسکریپت می‌باشد. می‌دانیم که اسکریپت، ماژولی است که برای اجرای سورس کد توسعه یافته و اجرای یک برنامه پایتونی همیشه از اسکریپت شروع می‌شود.

برای اجرای اسکریپت می‌بایست در خط فرمان سیستم عامل دستور فراخوانی مفسر پایتون را به همراه نام کامل اسکریپت (نشانی + نام + پسوند) وارد نمایید.

نمونه‌های پایین،‌ نتیجه اجرای اسکریپت بخش پیش را از طریق رابط خط فرمان گنولینوکس نمایش می‌دهد:
   
.. code:: 
 
    user> python /home/user/Documents/FirstProject/first_project.py
    (50-5×6)÷4 = 5.0

یا:

.. code:: 
 
    user> cd /home/user/Documents/FirstProject
    user> python first_project.py
    (50-5×6)÷4 = 5.0

چنانچه کاربر سیستم عامل ویندوز هستید به این نکته توجه داشته باشید که به دلیل وجود کاراکترهای خاصی (÷ و ×) که قرار است توسط print بر روی خط فرمان نمایش داده شوند و همچنین امکان عدم پشتیبانی پیش‌فرض خط فرمان ویندوز از کدگذاری UTF-8، به هنگام اجرای اسکریپت خطایی گزارش می‌شود که ارتباطی با کد پایتون ندارد. در این مواقع پیشنهاد می‌شود از برنامه PowerShell استفاده نموده و پیش از اجرای اسکریپت دستور ``chcp 65001`` را وارد نمایید - به صورت پایین:

.. code::
    
    PS > chcp 65001
    Active code page: 65001
    
    PS > python C:\Users\user\Documents\FirstProject\first_script.py
    (50-5×6)÷4 = 5.0

چگونگی اجرای اسکریپت‌های پایتون چیزی بیش از این نیست، البته می‌توان در هنگام اجرای اسکریپت داده‌هایی را نیز به عنوان آرگومان به آن ارسال نمود که این مورد در درس بعدی بررسی می‌شود.

معمولا در گنولینوکس سطری به مانند پایین به ابتدای اسکریپت‌های پایتون (فقط در سطر یکم) اضافه می‌کنند، در این صورت به هنگام اجرا دیگر نیازی به فراخوانی مفسر پایتون نبوده و تنها می‌بایست پس از تغییر حالت (Mode) اسکریپت مورد نظر به حالت قابل اجرا (توسط دستور `chmod <http://en.wikipedia.org/wiki/Chmod#Symbolic_modes>`_)، آن را به روش معمول در یونیکس اجرا نماییم:

::
    
    #!/usr/bin/env python

``env`` یک دستور شل (Shell) یونیکس است که در زمان اجرای اسکریپت مفسر پایتون را می‌یابد و نشانی آن را جایگزین می‌کند. به جای استفاده از ``env`` می‌توان نشانی مفسر پایتون مورد نظر را به صورت صریح مانند ``usr/bin/python/!#`` نوشت که البته در مواردی که پایتون به صورت جداگانه نصب شده باشد (نشانی مفسر در این حالت: usr/local/bin/python/)، کارایی ندارد و موجب شکست در اجرا می‌گردد.

اکنون برای نمونه اگر اسکریپت first_script.py را برای اجرا در گنولینوکس کامل‌تر سازیم:

.. code-block:: python
    
    #!/usr/bin/env python
    
    # This script prints a value to the screen.
    print "(50-5×6)÷4 =", (50-5*6)/4

پس از تغییر حالت، می‌توان آن را به صورت زیر در توزیع‌های گنولینوکس اجرا نمود:
    
.. code::

    user> cd /home/user/Documents/FirstProject

    user> chmod +x first_project.py

    user> ./first_project.py
    (50-5×6)÷4 = 5

.. note::
    نباید نماد !# (`shebang <http://en.wikipedia.org/wiki/Shebang_(Unix)>`_) را با نماد کامنت در پایتون (#) اشتباه گرفت.


|

ایجاد اسکریپت پایتون و اجرای آن همان‌طور که مشاهده کردید بسیار ساده است و وابسته به وجود هیچ ابزار خاصی نیست ولی برای پایتون نیز مانند هر زبان پر کاربرد دیگری تعداد زیادی `IDE <https://en.wikipedia.org/wiki/Integrated_development_environment>`_ توسعه داده شده است که در ادامه به معرفی چند نمونه محبوب از آن‌ها خواهیم پرداخت.

* `PyCharm <https://www.jetbrains.com/pycharm/>`_: محصولی از شرکت فوق‌العاده JetBrains است که البته نسخه کامل آن فروشی است ولی نسخه کامیونیتی (Community) آن رایگان و متن باز می‌باشد که از بسیاری ویژگی‌ها و امکانات ویژه برخوردار است. (`مقایسه نسخه‌ها <https://www.jetbrains.com/pycharm/features/editions_comparison_matrix.html>`_)

* `PyDev <http://www.pydev.org/>`_: یک IDE کامل، متن باز و رایگان است که برای پلتفرم `Eclipse <http://www.eclipse.org>`_ ارایه می‌شود.

* `NetBeans <https://netbeans.apache.org/>`_: یک IDE کامل، متن باز و رایگان است که طرفداران بسیاری دارد. NetBeans به صورت پیش‌فرض از پایتون پشتیبانی نمی‌کند و باید پلاگین مربوط به آن نصب گردد. (`پلاگین nbPython <http://nbpython.org/>`_)



.. tip::
    IDE یا Integrated development environment به ابزارهایی گفته می‌شود که علاوه‌بر یک ویرایشگر متن پیشرفته، امکانات بسیار کاربردی دیگری را نیز به مانند دیباگر (`Debugger <https://en.wikipedia.org/wiki/Debugger>`__) در اختیار برنامه‌نویس قرار می‌دهد.


.. admonition:: تمرین
    
  یک اسکریپت پایتون ایجاد کنید که نام و سن شما را تنها با یکبار استفاده از تابع ``()print`` به صورت پایین بر روی خروجی نمایش دهد::
      
      Name: Hideyoshi Nagachika - Age: 19  

  ** حالت‌های مختلفی که می‌توان به این ساختار از خروجی رسید را امتحان نمایید (ورودی‌های متفاوت)


.. _modular-programming: 

برنامه‌نویسی ماژولار
-----------------------
یک الگوی توصیه شده در برنامه‌نویسی، توسعه برنامه در واحدهایی کوچک از کد است. به گونه‌ای که هر واحد یک نقش مشخص و مستقل از دیگر واحدها داشته باشد. مستقل به این معنی که تغییر یا جایگزینی یک واحد نباید بر روی عملکرد دیگر واحدها تاثیرگذار باشد. به این ترتیب فرآیند خطایابی، نگهداری و توسعه برنامه به مراتب بهبود می‌یابد. یکی از امکانات پایتون در رسیدن به این اصل، استفاده درست از مفاهیم بسته و ماژول است. 
[برای مطالعه بیشتر: `Modular programming <https://en.wikipedia.org/wiki/Modular_programming>`_ و `Loose coupling <https://en.wikipedia.org/wiki/Loose_coupling>`_]


پروژه پایین که تنها شامل یک اسکریپت است را در نظر بگیرید:

.. code::
    
    TokyoGhoul
    .
    └── tokyo_ghoul.py

.. code-block:: python
    :linenos:
    :caption: tokyo_ghoul.py
    
    print("Name:")
    print("Actor_1:", "Hideyoshi Nagachika")

    print("Name:")
    print("Actor_2:", "Ken Kaneki")

    print("Age:")
    print("Actor_1:", 19)

    print("Age:")
    print("Actor_2:", 18)

می‌توان این پروژه را با ساختاری به شکل پایین نیز توسعه داد:

.. code::

    TokyoGhoul
    .
    ├── tokyo_ghoul.py
    │
    └── actors/
        │
        ├── __init__.py
        │
        ├── ages/
        │   ├── __init__.py
        │   ├── actor1.py
        │   └── actor2.py
        │   
        └── names/
            ├── __init__.py
            ├── actor1.py
            └── actor2.py


.. code-block:: python
    :linenos:
    :caption: actors.ages.actor1.py
    
    print("Age:")
    print("Actor_1:", 19)

.. code-block:: python
    :linenos:
    :caption: actors.ages.actor2.py
    
    print("Age:")
    print("Actor_2:", 18)

.. code-block:: python
    :linenos:
    :caption: actors.names.actor1.py
    
    print("Name:")
    print("Actor_1:", "Hideyoshi Nagachika")

.. code-block:: python
    :linenos:
    :caption: actors.names.actor2.py
    
    print("Name:")
    print("Actor_2:", "Ken Kaneki")

.. code-block:: python
    :linenos:
    :caption: tokyo_ghoul.py
    
    import actors.names.actor1

    from actors.names import actor2

    import actors.ages.actor1

    from actors.ages import actor2

خروجی اجرای ``tokyo_ghoul.py`` در هر دو حالت یکی است::

  Name:
  Actor_1: Hideyoshi Nagachika
  Name:
  Actor_2: Ken Kaneki
  Age:
  Actor_1: 19
  Age:
  Actor_2: 18

اما در حالت دوم امکان توسعه بیشتر بوده و به راحتی می‌توان اطلاعات (نام و سن) جدید را به برنامه افزود. همچنین موارد سن و نام  را می‌توان به دفعات در برنامه استفاده نمود بدون آنکه مجبور به نوشتن کدهای تکراری شویم (`DRY - Don't repeat yourself <https://en.wikipedia.org/wiki/Don%27t_repeat_yourself>`_). البته این مثال با توجه به اطلاعات پایتونی جاری ارایه شده است و دارای ضعفی است که در آینده خود متوجه آن خواهید شد :) 

همانطور که مشاهده می‌شود در پایتون برای ایجاد دسترسی به دیگر ماژول‌ها می‌بایست ابتدا آن‌ها را ``import`` کرد و این عمل در هر جایی از بدنه ماژول یا اسکریپت جاری ممکن است. البته باید توجه داشت که اجرای کدهای پایتون سطر به سطر از بالا به پایین می‌باشد و در صورت نیاز به هر ماژول می‌بایست پیش‌تر آن را ``import`` نمایید.

برای ``import`` هر ماژول می‌بایست نام تمام بسته‌های موجود از ابتدا تا ماژول مورد نظر به ترتیب ذکر گردد به صورتی که همگی با یک کاراکتر ``.`` به یکدیگر متصل شده باشند. در پایان نیز نام ماژول مورد نظر ذکر می‌گردد::

  import actors.names.actor1


ماژول‌های داخل هر بسته را می‌توان به صورت زیر نیز ``import`` کرد::

  from actors.names import actor1

استفاده از روش دوم مزایایی دارد که در کامیونیتی پایتون آن را به مراتب پر استفاده‌تر ساخته است. برای مثال فراخوانی اجزای داخلی ماژول را ساده‌تر می‌سازد و دیگر نیازی به ذکر نام بسته ها نخواهد بود. این مورد را به همراه نکات دیگر پیرامون ``import`` کردن ماژول‌ها طی دروس آتی خواهید دید. ذکر تمام ویژگی‌ها و کاربردهای موجود بدون آشنایی با دیگر اجزای زبان برنامه‌نویسی پایتون بیهوده است و در ادامه این کتاب به ترتیب با این اجزا آشنا خواهید شد.


.. tip::
    مفسر پایتون دستورات داخل هر ماژول را یکبار به صورت کامل در نخستین ``import`` اجرا می‌کند.


.. admonition:: تمرین
    
  تمرین قبل را در ساختار چند ماژولی پیاده‌سازی کنید به گونه‌ای که نام در سطر یکم و سن در سطر دوم چاپ شود.

  ** سعی کنید مسیر ماژول‌های خود را چندین بار تغییر دهید و متناسب با آن‌ها برنامه خود را اصلاح و تست نمایید.


.. _interpreter-background: 

پشت صحنه اجرا
---------------
زمانی که اقدام به اجرای یک اسکریپت می‌کنید؛ ابتدا، اسکریپت و تمام ماژول‌های ``import`` شده در آن به بایت‌کد کامپایل و سپس بایت‌کد‌های حاصل جهت تفسیر به زبان ماشین و اجرا، به ماشین مجازی فرستاده می‌شوند. آنچه ما از آن به عنوان مفسر پایتون (پیاده‌سازی CPython) یاد می‌کنیم در واقع ترکیبی از یک کامپایلر و یک ماشین مجازی است. تصویر پایین به خوبی روند اجرای کدهای پایتون را نمایش می‌دهد.


.. image:: /_static/lessons/l03-interpreter.png
    :align: center
    :target: http://trizpug.org/Members/cbc/wyntkap/compiler.html

بایت‌کد هر ماژول‌ پایتون در قالب فایلی با پسوند pyc که یاد‌آور py Compiled است، ذخیره می‌گردد. این فایل در یک زیردایرکتوری با نام __pycache__ داخل همان دایرکتوری ماژول ذخیره می‌شود و نام‌گذاری آن نیز با توجه به نام ماژول و نسخه‌ مفسر پایتون مورد استفاده، انجام می‌گیرد (نمونه: module.cpython-34.pyc). مفسر پایتون از این فایل ذخیره شده جهت افزایش سرعت اجرا در آینده بهره خواهد برد؛ به این صورت که در نوبت‌های بعدی اجرا چنانچه تغییری در کدهای ماژول یا نسخه‌ مفسر پایتون صورت نگرفته باشد، مفسر با بارگذاری فایل بایت‌کد موجود از کامپایل مجدد صرف نظر می‌کند.

.. note::
    مفسر پایتون تنها برای ماژول‌های وارد شده در اسکریپت اقدام به ذخیره کردن فایل بایت‌کد بر روی دیسک می‌کند و برای اسکریپت‌ این عمل صورت نمی‌گیرد. 

    بایت‌کد سورس کدهایی که تنها شامل یک اسکریپت هستند در حافظه‌ (Memory) نگهداری می‌شود.

.. note::
    زمانی که به هر دلیلی (به مانند: عدم وجود فضای کافی) مفسر پایتون قادر به ذخیره‌ فایل بایت‌کد بر روی دیسک ماشین نباشد، مفسر بایت‌کد را داخل حافظه‌ قرار می‌دهد و مشکلی در اجرا به وجود نخواهد آمد. البته بدیهی است که پس از اتمام اجرا یا قطع ناگهانی منبع تغذیه، بایت‌کد حذف می‌گردد.

.. note::
    در نسخه‌های پیش از 3.2، دایرکتوری __pycache__ ایجاد نمی‌گردد و فایل بایت‌کد با نامی برابر نام ماژول و در همان دایرکتوری قرار داده می‌شود (نمونه: module.pyc). در این شیوه قدیمی علاوه بر  وجود بی‌نظمی در میان فایل‌ها، تمایز بین ترجمه‌ نسخه‌های متفاوت مفسر پایتون نیز ممکن نمی‌باشد.

کدنویسی در حالت تعاملی را در درس بعدی خواهید آموخت ولی به یاد داشته باشید که مفسر پایتون محیط کدنویسی در این حالت را به مانند یک اسکریپت در نظر می‌گیرد.


.. _virtual-environments: 

ایجاد محیط مجازی
------------------
حالتی را در نظر بگیرید که در ایجاد پروژه‌های مختلف به نسخه‌های متفاوتی از برخی کتابخانه‌ها نیاز دارید؛ در این صورت چگونه می‌توانید چندین نسخه‌ متفاوت از یک کتابخانه‌ را در پایتون نصب نمایید؟ برای نمونه، فرض نمایید می‌خواهیم بر روی توسعه دو وب‌سایت؛ یکی توسط نسخه جدید (1.8) وب فریم‌ورک جنگو (`Django <http://www.djangoproject.com/>`_) و دیگری بر روی یک نسخه قدیمی (0.96) از آن کار کنیم، ولی نمی‌توانیم!؛ زیرا که نمی‌شود هر دوی این نسخه‌ها را با هم در پایتون (دایرکتوری site-packages) نصب داشت. در این وضعیت راه حل ایجاد محیط‌هایی مجازی (Virtual Environments) برای توسعه پروژه‌های مورد نظر است؛ محیطی که توسعه و اجرای هر پروژه پایتون را به همراه تمام وابستگی‌های (Dependencies) آن از پروژه‌های دیگر جدا یا ایزوله (isolate) می‌کند. در ادامه به بررسی دو ابزار رایج در این رابطه می‌پردازیم.

virtualenv
~~~~~~~~~~~

در اینجا برای نصب `virtualenv <http://virtualenv.pypa.io>`_  (ویرچوال اِنو) از pip استفاده می‌کنیم. [`برای اطلاعات بیشتر به درس پیش مراجعه نمایید </lessons/l02.html#id8>`_] - پیش از شروع هر نصبی بهتر است pip را آپدیت نماییم؛ این مراحل را در سیستم عامل گنو لینوکس به صورت پایین دنبال می‌کنیم::

    user> sudo pip install -U pip

    [...]
    Successfully installed pip[...]
    
    user>

*نصب virtualenv:* ::

    user> sudo pip install virtualenv
    
    [...]
    Successfully installed virtualenv[...]
    
    user>

.. note::
    چنانچه بر روی سیستم عاملی هر دو نسخه 2x یا 3x نصب است؛ این موضوع که virtualenv را توسط pip کدام نسخه نصب نمایید، اهمیت چندانی ندارد. چرا که امکان استفاده از آن برای دیگر نسخه‌ها نیز وجود دارد.

اکنون برای ایجاد یک محیط مجازی از دستور ``virtualenv ENV`` استفاده می‌شود که منظور از ``ENV`` در آن، نشانی دایرکتوری دلخواهی است که قصد داریم محیط مجازی در آن ایجاد گردد::

     user> virtualenv Documents/SampleENV/

دستور بالا موجب ایجاد یک محیط مجازی در مسیر ``/Documents/SampleENV`` سیستم عامل، بر پایه مفسر پایتونی که از pip آن برای نصب virtualenv استفاده کردیم می‌شود و چنانچه بخواهیم محیط مجازی خود را بر پایه‌ نسخه‌ موجود دیگری از پایتون ایجاد نماییم، لازم است با استفاده از گزینه ``python--`` نشانی مفسر آن مشخص گردد [`صفحه راهنما <http://virtualenv.pypa.io/en/latest/reference.html#cmdoption-p>`_]::

    user> virtualenv --python=python2 ENV
    
::

    user> virtualenv --python=python3 ENV
    
::

    user> virtualenv --python=/opt/python3.3/bin/python ENV


*در نمونه کد‌ بالا، نسخه‌های 2.7 و 3.4 پایتون از پیش بر روی سیستم عامل نصب بوده و نسخه 3.3 توسط کاربر در مسیر opt/python3.3/ نصب شده است.*

مثالی دیگر برای کاربران ویندوز::

    > virtualenv --python=C:\Python25\python.exe Documents\SampleENV\

اکنون می‌توانیم در پروژه خود به کتابخانه‌ها، pip، دایرکتوری site-packages و مفسری اختصاصی دسترسی داشته باشیم. البته پیش از شروع کار با یک محیط مجازی می‌بایست آن را ``activate`` (فعال) و پس از اتمام کار نیز آن را ``deactivate`` (غیر فعال) نماییم. فعال کردن در اینجا به معنای تنظیم متغیر Path سیستم عامل بر روی مفسر محیط مجازی مورد نظر است که با غیر فعال کردن، این وضعیت از بین می‌رود.

*در گنولینوکس:* ::

    user> cd Documents/SampleENV/
    user> source bin/activate 
    (SampleENV)$ 

::

    (SampleENV)$ deactivate
    user>

*در ویندوز:* ::

    > cd Documents\SampleENV\
    > Scripts\activate.bat
    (SampleENV)>

::

    (SampleENV)> deactivate.bat
    >


pyvenv
~~~~~~
از نسخه 3.3 پایتون به بعد ماژولی با نام `venv <http://docs.python.org/3/library/venv.html>`_ برای ایجاد محیط مجازی به کتابخانه استاندارد پایتون افزوده شده است که می‌توان از آن به جای نصب virtualenv استفاده نمود؛ برای این منظور از دستور pyvenv (پای وی اِنو) و با الگویی مشابه ``pyvenv ENV`` استفاده می‌گردد.

*در گنولینوکس:* ::

    user> pyvenv Documents/SampleENV/
    
    user> cd Documents/SampleENV/
    user> source bin/activate 
    (SampleENV)$ 

::

    (SampleENV)$ deactivate
    user>

*در ویندوز:* ::

    > C:\Python34\python C:\Python34\Tools\Scripts\pyvenv.py Documents\SampleENV\

یا ::

    > C:\Python34\python -m venv Documents\SampleENV\

[*در درس بعد با ساختار نمونه کد بالا آشنا می‌شوید*]

::

    > cd Documents\SampleENV\
    > Scripts\activate.bat
    (SampleENV)>

::

    (SampleENV)> deactivate.bat
    >


.. note::
    به عنوان یک رفتار توصیه شده بهتر است، در ازای هر پروژه پایتونی که آغاز می‌کنید یک محیط مجازی مخصوص نیز ایجاد نمایید. همچنین هر پروژه نیز بهتر است همواره حاوی فایل requirements.txt مخصوص به خود باشد تا مراحل آماده‌سازی (نصب بسته‌های پیش‌نیاز یا مورد استفاده در پروژه - dependencies) در این محیط به مراتب ساده‌تر گردد.


.. _packaging-projects: 

انتشار پروژه
--------------


اکنون اطلاعات کافی برای شروع یک پروژه از پایتون را دارید ولی چنانچه می‌خواهید با ساختار مناسب پروژه‌ای که قرار است برای استفاده افراد دیگر از طریق PyPI یا سرویس‌هایی نظیر github.com منتشر شود (مانند یک کتابخانه کاربردی) آشنا شوید، ادامه این بخش را نیز مطالعه نمایید. 

در یک پروژه کامل که میخواهیم آن را در کامیونیتی پایتون منتشر سازیم، جدا از سورس کد لازم است موارد دیگری نیز در ساختار آن در نظر گرفته شود؛ به ساختار پایین توجه نمایید:


.. code::
    
    my_project
    .
    ├── pyproject.toml
    │
    ├── LICENSE.txt
    ├── README.rst
    ├── requirements.txt
    │
    ├── src/
    │   └── unique_pakage_name/
    │       ├── __init__.py
    │       └── main.py
    │   
    ├── docs/
    └── test/


ساختار ابتدایی تنها شامل سورس کد می‌بود ولی در این ساختار تمام سورس کد در قالب یک بسته پایتون بخشی از مجموعه بزرگتری است که در آن یک سری فایل به مانند requirements.txt ،README.rst و pyproject.toml افزوده شده است.  تاکید می‌شود که در حال حاضر نیازی به رعایت این ساختار و ایجاد تمامی این فایل‌ها نیست. 

**pyproject.toml**: تمام اطلاعات مورد نیاز برای فرآیند تولید (Build) یک Distribution یا یک بسته قابل انتشار از پروژه مورد نظر توسط این فایل تعریف می‌گردد. [`اسناد پایتون <https://pip.pypa.io/en/stable/reference/build-system/pyproject-toml/>`__]


**README.rst**: تمام پروژه‌ها می‌بایست شامل سندی برای توصیف خود باشند. در پایتون برای ایجاد اسناد معمولا از زبان نشانه‌گذاری `reStructuredText <http://en.wikipedia.org/wiki/ReStructuredText>`_ استفاده می‌گردد و به همین دلیل این اسناد پسوند rst دارند که البته اجباری به این مورد نیست و می‌توانید برای ایجاد این فایل از `Markdown <http://en.wikipedia.org/wiki/Markdown>`_ (پسوند md) نیز استفاده نمایید.

**requirements.txt**: از این فایل برای معرفی کتابخانه‌های خاصی که در پروژه استفاده شده‌اند و در زمان نصب یا اجرای سورس کد، وجود یا نصب بودن آن‌ها نیز ضروری است، استفاده می‌گردد.

**LICENSE.txt**: این فایل پروانه‌ انتشار پروژه را مشخص می‌کند و اغلب حاوی یک کپی از متن پروانه‌های متن باز رایج به مانند `MIT <http://opensource.org/licenses/MIT>`_ ،`GPL <http://opensource.org/licenses/GPL-3.0>`_ یا `BSD <http://opensource.org/licenses/BSD-3-Clause>`_ می‌باشد.

.. note::
    لازم است تمامی فایل‌های یاد شده و دایرکتوری docs در بالاترین شاخه از دایرکتوری پروژه قرار داده شوند.

**docs**: در این دایرکتوری اسناد (راهنما، آموزش و...)  پروژه قرار داده می‌شوند. ایجاد این اسناد در کامیونیتی پایتون معمولا توسط `Sphinx <http://sphinx-doc.org/>`_ انجام می‌شود. قابل ذکر است که در تولید این کتاب نیز از همین ابزار استفاده شده است.

**test**: این دایرکتوری محل نگهداری کدهای مربوط به تست پروژه می‌باشد. تست‌نویسی یک امر ضروری در روند توسعه هر پروژه برنامه‌نویسی است. این دایرکتوری می‌تواند هم  در بالا ترین شاخه از پروژه و هم در داخل دایرکتوری سورس کد قرار داده شود.

اکنون می توان از روی این پروژه یک توزیع (Distribution) ایجاد و آن را با استفاده از ابزارهایی به مانند  `Twine <https://twine.readthedocs.io/en/stable/>`_ یا `Poetry <https://python-poetry.org/>`_ بر روی PyPI منتشر ساخت. [برای کسب اطلاعات بیشتر می‌توانید از `اسناد پایتون <https://packaging.python.org/en/latest/tutorials/packaging-projects/>`_ استفاده نمایید]

در صورت علاقه می‌توانید نگاهی نیز به پروژه `saeiddrv/PackagingPythonProjects <https://github.com/saeiddrv/PackagingPythonProjects>`_ بیاندازید که تمامی مراحل را به صورت کاربردی بر پایه Poetry پیاده‌سازی کرده است.



|

----

:emoji-size:`😊` امیدوارم مفید بوده باشه

  
  


