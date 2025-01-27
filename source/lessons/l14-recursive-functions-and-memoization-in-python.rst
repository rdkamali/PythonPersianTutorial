.. role:: emoji-size

.. meta::
   :description: پایتون به پارسی - کتاب آنلاین و آزاد آموزش زبان برنامه‌نویسی پایتون - درس چهاردهم: تابع در پایتون - تابع بازگشتی (Recursive) و Memoization


.. _lesson-14: 

درس ۱۴: تابع در پایتون: تابع بازگشتی (Recursive) و Memoization
==============================================================================


.. figure:: /_static/pages/14-python-function-recursive-memoization.jpg
    :align: center
    :alt: تابع در پایتون: تابع بازگشتی (Recursive) و Memoization
    :class: page-image

    Photo by `Dan Freeman <https://unsplash.com/photos/WHPsxhB4mWQ>`__

این درس بخش پایانی از بررسی تابع در پایتون می‌باشد و به شرح **تابع بازگشتی (Recursive)** و تکنیک **Memoization** در برنامه‌نویسی اختصاص یافته است. 

در پایان نیز به منظور جمع‌بندی تابع به معرفی برخی دیگر از امکانات تابع در زبان برنامه‌نویسی پایتون مانند **Function Attributes** پرداخته شده است.



:emoji-size:`✔` سطح: متوسط

----


.. contents:: سرفصل‌ها
    :depth: 2

----


.. _recursive-function: 

تابع بازگشتی
------------

از درس نهم با دستورات کنترلی ``for`` و ``while`` آشنا شده‌ایم، این دستورات تنها ابزار ما برای تکرار قسمتی از کد بودند. اکنون با پیاده‌سازی شیوه‌ای جدید در تکرار آشنا می‌شویم.

به بیانی ساده، **تابع بازگشتی** (Recursive function) به تابعی گفته می‌شود که خود را از داخل بدنه خود فراخوانی می‌کند. پیاده‌سازی تابع به صورت بازگشتی شیوه‌ای است که از آن برای حل برخی مسائل بهره گرفته می‌شود و باید بدانیم که توابع بازگشتی، یک سینتکس یا دستور خاص در زبان پایتون نیست بلکه یک شیوه حل مسئله می‌باشد که با استفاده از تابع در زبان برنامه‌نویسی پایتون (همچون بسیاری از زبان‌های دیگر) قابل پیاده‌سازی است. 

برای مثال در نمونه کد پایین مقدار فاکتوریل (`Factorial <https://en.wikipedia.org/wiki/Factorial>`_) عدد پنج را به شیوه بازگشتی محاسبه می‌کنیم::


  >>> def factorial(n):
  ...     if n <= 1:
  ...         return 1 
  ...     else:
  ...         return n * factorial(n - 1)
  ... 
  >>> 
  >>> factorial(5)
  120
  >>>

عموما می‌توان مسئله‌هایی که از **توالی** انجام یک **کار یکسان** قابل حل هستند را به صورت بازگشتی پیاده‌سازی کرد. مراحل اجرای نمونه کد بالا به صورت زیر است:

.. image:: /_static/lessons/l14-factorial-relation.png
    :align: center

:: 

  factorial(5)
  |--> 5 * factorial(4)
           |--> 4 * factorial(3)
                    |--> 3 * factorial(2)
                             |--> 2 * factorial(1)
                                      |--> 1

  120 = 5 * (4 * (3 * (2 * 1)))

**توضیح:** هنگامی ``factorial(5)`` فراخوانی می‌شود (``n == 5``)، شرط ``1 => n`` رد و بخش ``else`` اجرا می‌شود. در این مرحله نمونه دیگری از تابع با آرگومان ``4`` فراخوانی‌ می‌شود و اجرای ``factorial(5)`` منتظر پایان اجرای ``factorial(4)`` و دریافت نتیجه آن می‌ماند. به همین ترتیب چندین نمونه از یک تابع اجرا می‌شوند که منتظر دریافت نتیجه از نمونه بعد از خود هستند. در نهایت شرط ``1 => n`` برقرار می‌شود و نمونه ``factorial(1)`` نتیجه خود را به ``factorial(2)`` برمی‌گرداند. به همین ترتیب نتایج بازگشت داده می‌شوند تا به نمونه نخست اجرا شده یعنی ``factorial(5)`` برسد و اجرای مورد نظر کاربر به پایان برسد.

مدیریت توالی تابع (شیوه بازگشتی) در حافظه با استفاده از ساختمان داده پشته (Stack) [`ویکی‌پدیا <https://en.wikipedia.org/wiki/Stack_(abstract_data_type)>`__] انجام می‌شود.

هر تابع بازگشتی شامل دو بخش مهم است:

* یک عبارت حاوی فراخوانی خود تابع
* یک شرط برای انتخاب بین فراخوانی مجدد و پایان

.. note::
    پیاده‌سازی شیوه بازگشتی شاید به نظر هیجان‌انگیز باشد اما نباید فراموش کرد که میزان حافظه (Memory) زیادی مصرف می‌کند، اجرای آن زمان‌بر خواهد بود، درک جریان اجرای آن اغلب سخت است و اشکال‌زدایی (debug) آن ساده نخواهد بود!


.. _using-python-decorator:

استفاده از decorator
~~~~~~~~~~~~~~~~~~~~~

هنگام استفاده از decorator بر روی توابع بازگشتی باید به این نکته توجه داشته باشید که این decorator بر روی تمامی نمونه‌های فراخوانی شده از تابع اعمال خواهد شد و اینکه تنها یک نمونه از decorator ایجاد می‌شود و تمام نمونه‌‌های تابع به همان یک نمونه ارسال می‌شوند::

  >>> def logger(func):
  ...     print('Decorator is created!')
  ...     def func_wrapper(number):
  ...         print(f'New factorial call with parameter: {number}')
  ...         result = func(number)
  ...         print (f'factorial({number}) ==> {result}')
  ...         return result
  ...     return func_wrapper
  ... 
  >>> @logger
  ... def factorial(n):
  ...     if n <= 1:
  ...         return 1
  ...     else:
  ...         return n * factorial(n - 1)
  ... 
  >>> 
  >>> factorial(5)
  Decorator is created!
  New factorial call with parameter: 5
  New factorial call with parameter: 4
  New factorial call with parameter: 3
  New factorial call with parameter: 2
  New factorial call with parameter: 1
  factorial(1) ==> 1
  factorial(2) ==> 2
  factorial(3) ==> 6
  factorial(4) ==> 24
  factorial(5) ==> 120
  120
  >>> 

*به خروجی نمونه کد بالا حتما توجه نمایید!.*

.. _python-set-recursion-limit:

تنظیم عمق بازگشتی
~~~~~~~~~~~~~~~~~~~~

در زبان برنامه‌نویسی پایتون در عمق پیاده‌سازی توابع بازگشتی (تعداد نمونه‌های فراخوانی شده از تابع و موجود در پشته) یک محدودیت قابل تنظیم وجود دارد. تابع ``()getrecursionlimit`` از ماژول ``sys`` این مقدار را برمی‌گرداند [`اسناد پایتون <https://docs.python.org/3/library/sys.html#sys.getrecursionlimit>`__]. این مقدار به صورت پیش‌فرض برابر با ``1000`` 	می‌باشد که با استفاده از تابع ``(limit)setrecursionlimit`` از ماژول ``sys`` می‌توان آن را تغییر داد [`اسناد پایتون <https://docs.python.org/3/library/sys.html#sys.setrecursionlimit>`__]::

  >>> import sys

  >>> sys.getrecursionlimit()
  1000

  >>> sys.setrecursionlimit(50)

  >>> sys.getrecursionlimit()
  50

با رد شدن از محدودیت عمق توابع بازگشتی یک استثنا ``RecursionError`` گزارش خواهد شد::

  
  >>> factorial(9)
  362880

  >>> sys.setrecursionlimit(10)

  >>> factorial(9)
  Traceback (most recent call last):
    File "<stdin>", line 1, in <module>
    File "<stdin>", line 5, in factorial
    File "<stdin>", line 5, in factorial
    File "<stdin>", line 5, in factorial
    [Previous line repeated 5 more times]
    File "<stdin>", line 2, in factorial
  RecursionError: maximum recursion depth exceeded in comparison

.. tip::
    علاوه بر این محدودیت، یک محدودیت جدی‌تر دیگری نیز وجود دارد و آن هم میزان فضایی است که توسط سیستم عامل برای پشته در نظر گرفته شده است. با رد شدن از این مقدار فضا، برنامه با خطای زمان اجرا مواجه می‌گردد (``RuntimeError``).


.. _python-generator-recursive:

تابع Generator بازگشتی
~~~~~~~~~~~~~~~~~~~~~~

در پیاده‌سازی توابع Generator و Coroutine نیز می‌توان شیوه بازگشتی را در نظر گرفت، در این صورت ممکن است نتایج کمی برخلاف انتظار شما باشد. نمونه کد زیر یک شی لیست تو در تو را دریافت و تک تک اعضای درون هر لیست را چاپ می‌کند::

  >>> def flatten(lists):
  ...     for sub in lists:
  ...         if isinstance(sub,list):
  ...             flatten(sub)
  ...         else:
  ...             print(sub)
  ... 
  >>> items = [[1,2,3],[4,5,[5,6]],[7,8,9]]
  >>> flatten(items)
  1
  2
  3
  4
  5
  5
  6
  7
  8
  9
  >>> 

اکنون برای تبدیل تابع ``flatten`` به یک  Generator کافی است به جای ``print`` از ``yield`` استفاده کنیم::

  >>> def genflatten(lists):
  ...     for sub in lists:
  ...         if isinstance(sub,list):
  ...             genflatten(sub)
  ...         else:
  ...             yield sub
  ... 
  >>> items = [[1,2,3],[4,5,[5,6]],[7,8,9]]

  >>> genflatten(items)
  <generator object genflatten at 0x7eff06d40150>

  >>> list(genflatten(items))
  []


اتفاقی نیفتاد! و خروجی یک لیست خالی است. از درس پیش به خاطر داریم، فراخوانی تابع ``genflatten`` (که در واقع یک تابع Generator است) تنها باعث ایجاد یک شی Generator می‌شود و می‌بایست در نقطه‌ای که تابع خودش را فراخوانی می‌کند نیز مقدمات پردازش خروجی یک شی Generator را فراهم کنیم. اکنون با اصلاح کد بالا::

  >>> def genflatten(lists):
  ...     for sub in lists:
  ...         if isinstance(sub,list):
  ...             for item in genflatten(sub):
  ...                 yield item
  ...         else:
  ...             yield sub
  ... 
  >>> items = [[1,2,3],[4,5,[5,6]],[7,8,9]]

  >>> genflatten(items)
  <generator object genflatten at 0x7f6cee349258>

  >>> list(genflatten(items))
  [1, 2, 3, 4, 5, 5, 6, 7, 8, 9]


.. _memoization:

Memoization
~~~~~~~~~~~~~

**Memoization** یا یادآوری، یک تکنیک برای نگهداری از نتایج به دست آمده به منظور جلوگیری از تکرار محاسبات است [`ویکی‌پدیا <https://en.wikipedia.org/wiki/Memoization>`__]. این تکنیک را می‌توان در زبان برنامه‌نویسی پایتون با استفاده از **decorator** پیاده‌سازی کرد.

برای توضیح این بخش اجازه دهید یک مثال بازگشتی دیگر را بررسی کنیم. محاسبه مقدار فیبوناچی [`ویکی‌پدیا <https://en.wikipedia.org/wiki/Fibonacci_number>`__] یک عدد مشخص:

.. image:: /_static/lessons/l14-fibonacci-relation.png
    :align: center

::

  >>> def fibonacci(n):
  ...     if n <= 1:
  ...         return n
  ...     else:
  ...         return fibonacci(n-1) + fibonacci(n-2)
  ... 
  >>> for number in range(10):
  ...    print(fibonacci(number))
  ... 
  0
  1
  1
  2
  3
  5
  8
  13
  21
  34

  
در این مثال ما از عدد ``9`` جلوتر نرفتیم چرا که محاسبه برای اعداد بزرگتری به مانند ``50`` واقعا زمان‌بر خواهد بود و این فرصتی است تا ما کارایی تکنیک Memoization را محک بزنیم. اکنون تابع بازگشتی فیبوناچی خود را با استفاده از تکنیک Memoization و یک Decorator بهینه‌سازی می‌کنیم::

  >>> def memoize_fibonacci(func):
  ...     memory = {} 
  ...     def func_wrapper(number): 
  ...         if number not in memory: 
  ...             memory[number] = func(number)
  ...         return memory[number]
  ...     return func_wrapper
  ... 
  >>> @memoize_fibonacci
  ... def fibonacci(n):
  ...     if n <= 1:
  ...         return n
  ...     else:
  ...         return fibonacci(n-1) + fibonacci(n-2)
  ... 
  >>> 

حالا مقدار ``50`` که هیچ، مقدار فیبوناچی برای عدد ``500`` را محاسبه کنید (``(500)fibonacci``). تفاوت در زمان اجرا را خودتان متوجه خواهید شد!


به کمک Decorator در این مثال (``memoize_fibonacci``) نتایج حاصل از فراخوانی هر نمونه تابع در جایی ذخیره می‌شود (شی دیکشنری ``memory``) و پیش از فراخوانی مجدد یک نمونه جدید از تابع بررسی می‌شود که آیا قبلا این مقدار محاسبه شده است یا خیر. در صورت وجود جواب از تکرار فراخوانی تابع صرف نظر و مقدار از پیش موجود به عنوان نتیجه برگردانده می‌شود. بنابراین بدیهی است که با جلوگیری از ایجاد نمونه توابع جدید و محاسبات تکراری، سرعت اجرا افزایش یابد.


Function Attributes
---------------------

از دروس پیش مشاهده کردیم که اشیا در پایتون بر حسب نوع خود شامل یک سری صفات یا ویژگی‌های (Attributes) پیش‌فرض هستند؛ برای مثال صفت ``__name__`` که دربردارنده نام تابع است [`اسناد پایتون <https://docs.python.org/3/library/stdtypes.html#definition.__name__>`__]. 

علاوه بر این؛‌ توابع در پایتون می‌توانند صفات دلخواه کاربر را نیز دریافت کنند که به این صورت می‌توان یک سری اطلاعات اضافی را به توابع پیوست کرد [`PEP 232 <https://www.python.org/dev/peps/pep-0232/>`__]. به نمونه کد پایین توجه نمایید::

  >>> def foo():
  ...     pass
  ... 
  >>> foo.is_done = True
  >>> 
  >>> if foo.is_done:
  ...     print('DONE!')
  ... 
  DONE!
  >>> 

همانطور که قابل مشاهده است با استفاده از سینتکس زیر می‌توان یک Attribute به تابع اضافه کرد::

  function_name.attribute_name = attribute_value

همچنین برای این منظور می‌توان از تابع آماده ``setattr`` استفاده کرد [`اسناد پایتون <https://docs.python.org/3/library/functions.html#setattr>`__]. این تابع سه آرگومان دریافت می‌کند؛ شی‌ای که می‌خواهید یک Attribute به آن اضافه کنید (در اینجا تابع)، نام (از نوع رشته - string) و مقدار Attribute مورد نظر::

          setattr(object, name, value)

::

  >>> setattr(foo, 'name', 'Saeid')
  >>> setattr(foo, 'age', 32)
  >>> 
  >>> foo.name
  'Saeid'
  >>> foo.age
  32

این صفات در قالب یک شی دیکشنری ذخیره می‌شوند که با استفاده از صفت ``__dict__`` در دسترس هستند [`اسناد پایتون <https://docs.python.org/3/library/stdtypes.html#object.__dict__>`__]::

  >>> foo.__dict__
  {'is_done': True, 'name': 'Saeid', 'age': 32}

برای دریافت مقدار یک Attribute مشخص می‌توانید از تابع آماده ``getattr`` نیز استفاده کرد [`اسناد پایتون <https://docs.python.org/3/library/functions.html#getattr>`__]. این تابع دو پارامتر اجباری (``object`` و ``name``) و یک پارامتر اختیاری (``default``) دارد. در صورتی که شی مورد نظر (در اینجا تابع) فاقد صفت مورد نظر باشد مقدار default (در صورت ارسال) برگردانده خواهد شد::


   getattr(object, name[, default])

::

  >>> getattr(foo, 'is_done')
  True
  >>> getattr(foo, 'is_publish', False)
  False

::

  >>> getattr(foo, 'is_publish')
  Traceback (most recent call last):
    File "<stdin>", line 1, in <module>
  AttributeError: 'function' object has no attribute 'is_publish'

  >>> foo.is_publish
  Traceback (most recent call last):
    File "<stdin>", line 1, in <module>
  AttributeError: 'function' object has no attribute 'is_publish'

در صورت تلاش برای دریافت صفتی که برای تابع مورد نظر تعریف نشده باشد یک استثنای ``AttributeError`` گزارش خواهد شد. البته همانطور که بیان شد در صورت استفاده از تابع ``getattr`` و تنظیم پارامتر ``default`` این اتفاق رخ نخواهد داد. همچنین برای جلوگیری از بروز این استثنا می‌توان پیش از استفاده از صفت، وجود آن را با استفاده از تابع آماده ``(hasattr(object, name`` بررسی کرد [`اسناد پایتون <https://docs.python.org/3/library/functions.html#hasattr>`__]::

  >>> if hasattr(foo, 'is_publish'):
  ...     print(foo.is_publish)
  ... else:
  ...     print(f"{foo.__name__!r} has no attribute 'is_publish'")
  ... 
  'foo' has no attribute 'is_publish'
  >>> 

برای **حذف** یک Attribute نیز می‌توان از تابع آماده ``(delattr(object, name`` استفاده کرد [`اسناد پایتون <https://docs.python.org/3/library/functions.html#delattr>`__]::

  >>> delattr(foo, 'age')
  >>> 
  >>> foo.age
  Traceback (most recent call last):
    File "<stdin>", line 1, in <module>
  AttributeError: 'function' object has no attribute 'age'

و یا از دستور ``del`` ::

  >>> del foo.is_done
  >>> 
  >>> foo.is_done
  Traceback (most recent call last):
    File "<stdin>", line 1, in <module>
  AttributeError: 'function' object has no attribute 'is_done'
  >>> 

.. note::
    در انتهای این بخش باید خاطر نشان کرد که در صورت تعریف Attribute برای توابع خود و استفاده از decorator، همانطور که در درس پیش نیز توضیح داده شد استفاده از ``functools.wraps@`` فراموش نشود [`درس سیزدهم </lessons/l13-decorator-generator-and-lambda-with-python-functions.html#functools-wraps>`__].


Built-in Functions
--------------------

مفسر پایتون تعدادی تابع کاربردی را بدون نیاز به import کردن ماژول خاصی در اختیار برنامه‌نویسان قرار می‌دهد. از این توابع با عنوان **Built-in Functions** (توابع آماده یا **توابع داخلی**) یاد می‌شود. فهرست کامل این توابع به همراه توضیح در `اسناد پایتون <https://docs.python.org/3/library/functions.html>`__ موجود است. در طی دروس پیشین و حتی همین درس با برخی از آن‌ها آشنا شده‌اید، در این بخش نیز به بررسی چند مورد دیگر می‌پردازیم.

eval
~~~~~~

این تابع یک (و تنها یک) عبارت پایتونی را در قالب شی رشته دریافت، اجرا و نتیجه را برمی‌گرداند [`اسناد پایتون <https://docs.python.org/3/library/functions.html#eval>`__].

::

  >>> eval('3*4 + 7.2')
  19.2

::

  >>> import math
  >>> x = 2
  >>> eval('math.sin(3.5+x) + 7.2')
  6.494459674429608

بر اساس تعریف موجود در اسناد پایتون ``eval``، این تابع شامل دو پارامتر  ``globals`` و ``locals`` نیز می‌شود که ارسال آرگومان به آن‌ها اختیاری است. هر دو از نوع دیکشنری (dict) هستند که Scope یا حوزه‌های global و  local کدی که باید اجرا شود (پارامتر یکم تابع) را  ارايه می‌دهند::


    eval(object[, globals[, locals]])

::

  >>> globals_env = {'x': 7, 'y': 10, 'birds': ['Parrot', 'Swallow', 'Albatross']}
  >>> locals_env = {}
  >>> a = eval("3 * x + 4 * y", globals_env, locals_env)
  >>> a
  61




exec
~~~~~~

این تابع همانند ``eval`` است ولی با این تفاوت که می‌تواند چندین عبارت یا دستور پایتونی را در قالب یک شی رشته دریافت و اجرا کند. خروجی ``exec`` همیشه برابر با ``None`` است [`اسناد پایتون <https://docs.python.org/3/library/functions.html#exec>`__].

::

  >>> exec('import math; x=2; print(math.sin(3.5+x) + 7.2)')
  6.494459674429608

::

  >>> exec("for i in range(5): print(i)")
  0
  1
  2
  3
  4


این تابع همانند ``eval`` شامل دو پارامتر  ``globals`` و ``locals`` نیز می‌شود::

  exec(object[, globals[, locals]])

::

  >>> exec("for b in birds: print(b)", globals_env, locals_env)
  Parrot
  Swallow
  Albatross



compile
~~~~~~~~~

هر بار که یک شی رشته حاوی کد پایتون به توابع ``eval`` و ``exec`` ارسال می‌شود، مفسر پایتون ابتدا این کد را به بایت‌کد کامپایل و سپس اجرا می‌کند که تکرار این کار باعث تحمیل سربار به سیستم می‌شود. می‌توان با یک بار کامپیال و استفاده مجدد از اعمال این سربار اجتناب کرد.

تابع ``compile`` برای همین منظور است [`اسناد پایتون <https://docs.python.org/3/library/functions.html#compile>`__]. تعریف این تابع به صورت زیر است::

  compile(source, filename, mode, flags=0, dont_inherit=False, optimize=-1)


* **source**: کدی است که می‌خواهیم آن را کامپیال و در نهایت اجرا کنیم که می‌تواند یک شی از نوع رشته (str)، بایت (bytes) یا AST [`اسناد پایتون <https://docs.python.org/3/library/ast.html>`__] باشد.

* **filename**: نام فایلی که کد باید از آن خوانده شود؛ چنانچه کد مورد نظر شما از فایل خوانده نمی‌شود، یک نام به دلخواه خود قرار دهید یا آن را با یک رشته خالی مقداردهی کنید.

* **mode**: نوع کد را مشخص می‌کند. می‌تواند یکی از مقادیر ``exec``، ``eval`` یا ``single`` باشد. شرایط اجرای دو تابع ``eval`` (تنها شامل یک عبارت) و ``exec`` (یک یا چند عبارت و دستور) را برسی کردیم و از ``single`` نیز در مواقعی که کد مورد نظر تنها شامل یک دستور باشد، استفاده می‌شود.

* **flags**, **dont_inheritmode**: این دو پارامتر اختیاری هستند و در این مرحله می‌توانید از آنها گذر کنید. از این دو برای تعیین اینکه کدام یک از دستورات Future در کامپایل کد مورد نظر تاثیر دارد [`اسناد پایتون <https://docs.python.org/3/reference/simple_stmts.html#future>`__]، مورد استفاده قرار می‌گیرند.

* **optimize**: میزان سطح بهینه‌سازی کد را برای کامپایلر تنطیم می‌کند و ارسال آروگومان به آن نیز اختیاری است - مطالعه بیشتر: [`PEP 488 <https://www.python.org/dev/peps/pep-0488/>`__]. 


به نمونه کدهای پایین توجه نمایید::

  >>> # compile() with string source

  >>> code_str = 'x=5\ny=10\nprint("sum =",x+y)'
  >>> code = compile(code_str, 'sum_test.py', 'exec')
  >>> print(type(code))
  <class 'code'>
  >>> exec(code)
  sum = 15



.. code-block:: text
    :linenos:

    # File Name: test_code.py
    # Directory: /home/saeid/Desktop

    x = 10
    y = 20
    print('Multiplication = ', x * y)

::

  >>> # reading code from a file
 
  >>> file = open('/home/saeid/Desktop/test_code.py', 'r')
  >>> code_str = file.read()
  >>> file.close()
  >>> code = compile(code_str, 'test_code.py', 'exec')
  >>> print(type(code))
  <class 'code'>
  >>> exec(code)
  Multiplication =  200


locals, globals
~~~~~~~~~~~~~~~~~

خروجی هر دو تابع یک شی دیکشنری (dict) است. تابع ``()locals`` یک دیکشنری حاوی متغیرهای موجود در حوزه local [`اسناد پایتون <https://docs.python.org/3/library/functions.html#locals>`__] و تابع ``()globals`` نیز یک دیکشنری حاوی متغیرهای موجود در حوزه global را برمی‌گرداند [`اسناد پایتون <https://docs.python.org/3/library/functions.html#globals>`__]::

  >>> a = 0
  >>> def func():
  ...     x = 'text'
  ...     print('-' * 10)
  ...     print('locals():')
  ...     print(locals())
  ...     print('-' * 10)
  ...     print('globals():')
  ...     print(globals())
  ... 
  >>> func()
  ----------
  locals():
  {'x': 'text'}
  ----------
  globals():
  {'__name__': '__main__', '__doc__': None, '__package__': None, '__loader__': <class '_frozen_importlib.BuiltinImporter'>, '__spec__': None, '__annotations__': {}, '__builtins__': <module 'builtins' (built-in)>, 'a': 0, 'func': <function func at 0x7f2b29f1ec80>}
  >>> 

توجه داشته باشید در سطح ماژول خروجی این دو تابع با هم یکسان می‌شود::

  >>> a = 5
  >>> b = 10
  >>> def func():
  ...     pass
  ... 
  >>> locals()
  {'__name__': '__main__', '__doc__': None, '__package__': None, '__loader__': <class '_frozen_importlib.BuiltinImporter'>, '__spec__': None, '__annotations__': {}, '__builtins__': <module 'builtins' (built-in)>, 'a': 5, 'b': 10, 'func': <function func at 0x7f1dd0218c80>}
  >>> globals()
  {'__name__': '__main__', '__doc__': None, '__package__': None, '__loader__': <class '_frozen_importlib.BuiltinImporter'>, '__spec__': None, '__annotations__': {}, '__builtins__': <module 'builtins' (built-in)>, 'a': 5, 'b': 10, 'func': <function func at 0x7f1dd0218c80>}
  >>> 


.. caution::
    همانطور که در درس‌های سوم و چهارم نیز بیان شده است، تمام محیط تعاملی پایتون از دید مفسر پایتون همانند یک ماژول (یا اسکریپت) است.


Documentation Strings
------------------------

از درس ششم با `docstring </lessons/l06.html#docstring>`__ آشنا شده‌ایم؛ در این بخش با رویکرد تابع به این مبحث می‌پردازیم [`PEP 257 <https://www.python.org/dev/peps/pep-0257/>`__].

.. tip::
   استفاده از **docstring** در ابتدای ماژول‌ها، کلاس‌ها و توابع یک شیوه مناسب در زبان پایتون برای ارايه چگونگی ارتباط و رفتار با این عناصر است.

به نمونه کد پایین توجه نمایید::

  >>> def factorial(n):
  ...     """Computes n factorial. For example:
  ... 
  ...     >>> factorial(5)
  ...     120
  ...     >>>
  ...     """
  ...
  ...     if n <= 1: return 1
  ...     else: return n*factorial(n-1)
  ... 
  >>>

::

  >>> factorial.__doc__
  'Computes n factorial. For example:\n\n    >>> factorial(5)\n    120\n    >>>\n    '

::

  >>> print(factorial.__doc__)
  Computes n factorial. For example:

      >>> factorial(5)
      120
      >>>
  >>>

::

  >>> help(factorial)

  Help on function factorial in module __main__:

  factorial(n)
      Computes n factorial. For example:
    
      >>> factorial(5)
      120
      >>>
  (END)

مقدار docstring در attribute یا صفت ``__doc__`` تابع قرار می‌گیرد. همچنین این مقدار از طریق تابع ``help`` در محیط تعاملی (interactive) پایتون نیز قابل دسترس است.

برنامه‌های موسوم به IDE از جمله PyCharm نیز docstring‌ها را مورد پردازش قرار می‌دهند و با استفاده از اطلاعات موجود در آن‌ها به برنامه‌نویس امکانات کمکی بیشتری ارايه می‌دهند. برای مثال می‌توان نوع ورودی‌های یک تابع یا مقدار خروجی آن را با استفاده از docstring تشریح کرد. برای اطلاعات بیشتر و مشاهده نمونه کد می‌توانید به مستندات PyCharm مراجعه نمایید: `PyCharm - Document source code <https://www.jetbrains.com/help/pycharm/documenting-source-code.html>`__



|

----

:emoji-size:`😊` امیدوارم مفید بوده باشه




