.. role:: emoji-size

.. meta::
   :description: پایتون به پارسی - کتاب آنلاین و آزاد آموزش زبان برنامه‌نویسی پایتون - درس سیزدهم: تابع در پایتون - Coroutine ،Generator ،Decorator‌ و lambda


.. _lesson-13: 

درس ۱۳: تابع در پایتون: Generator ،Decorator‌ و lambda
==================================================================

.. figure:: /_static/pages/13-python-function-decorator-generator-coroutine-lambda.jpg
    :align: center
    :alt: تابع در پایتون: Coroutine ،Generator ،Decorator‌ و lambda
    :class: page-image

    Photo by `Bill Oxford <https://unsplash.com/photos/-fGqsewtsJY>`__


این درس در ادامه درس پیش است و به معرفی برخی از کاربردهای تابع در ایجاد مفاهیمی جدید، مهم و کاربردی در زبان برنامه‌نویسی پایتون می‌پردازد، مفاهیمی همچون  Generator ،Decorator‌ و lambda. مبحث تابع در پایتون با این درس به پایان نمی‌رسد و نکات باقی‌مانده در درس بعدی ارائه می‌شوند.



:emoji-size:`✔` سطح: متوسط

----


.. contents:: سرفصل‌ها
    :depth: 2

----


.. _python-decorator: 


Decorator
----------


دکوراتور (تزئین‌گر) یا همان **Decorator‌** ها [`PEP 318 <https://www.python.org/dev/peps/pep-0318//>`__] به توابعی گفته می‌شود که به منظور پوشش (wrap) توابع یا کلاس‌های دیگر پیاده‌سازی می‌شوند. Decorator‌ها در پایتون ابزار بسیار کاربردی و مفیدی هستند که به برنامه‌نویس این امکان را می‌دهند تا با کاهش حجم کدنویسی و بدون تغییر در بدنه توابع و کلاس‌های خود، رفتار و ویژگی‌های آن‌ها را گسترش دهد. در این بخش تمرکز بر روی **اعمال** Decorator‌ها به **توابع** است و Decorator‌ کلاس را در درس مربوط به کلاس‌ها بررسی خواهیم کرد.

برای پوشش یک تابع توسط Decorator‌ از سینتکسی مشابه ``decorator‌_name@`` در بالای بخش سرآیند استفاده می‌شود:

::

  def decorator_name(a_function):
      pass


  @decorator_name
  def function_name():
      print("Somthing!")


  function_name()

مفهومی که این سینتکس (``decorator‌_name`` + ``@``) در بالای بخش سرآیند یک تابع برای مفسر پایتون ایجاد می‌کند کاملا مشابه با سینتکس پایین است::

  wrapper = decorator_name(function_name)
  wrapper()

هر چیزی در پایتون یک شی است حتی مفاهیم پیچیده‌ای به مانند تابع؛ از درس پیش نیز به خاطر داریم که تابع در پایتون یک موجودیت **”first-class“** است که یعنی می‌توان تابع را مانند دیگر اشیا به صورت آرگومان به توابع دیگر ارسال نمود. نمونه کد بالا نیز نمایش ارسال یک تابع (``function_name``) به تابعی دیگر (``decorator‌_name``) است.


به مثال پایین توجه نمایید:

::

  >>> def decorator_name(func):
  ...     def wrapper():
  ...         print("Something is happening before the function is called.")
  ...         func()
  ...         print("Something is happening after the function is called.")
  ...     return wrapper
  ... 
  >>> 
  >>> @decorator_name
  ... def function_name():
  ...     print("Somthing!")
  ... 
  >>> 
  >>> function_name()
  Something is happening before the function is called.
  Somthing!
  Something is happening after the function is called.
  >>> 

نمونه کد بالا را می‌توان با ساختار ساده زیر نیز در نظر گرفت:

::

  >>> def decorator_name(func):
  ...     def wrapper():
  ...         print("Something is happening before the function is called.")
  ...         func()
  ...         print("Something is happening after the function is called.")
  ...     return wrapper
  ... 
  >>> 
  >>> def function_name():
  ...     print("Somthing!")
  ... 
  >>> 
  >>> wrapper = decorator_name(function_name)
  >>> wrapper()
  Something is happening before the function is called.
  Somthing!
  Something is happening after the function is called.
  >>> 

همانطور که با مقایسه دو نمونه کد بالا قابل مشاهده است، Decorator‌ها یک روپوش (wrapper) برای توابع و کلاس‌های ما بوجود می‌آورند. در هنگام فراخوانی تابع ``function_name`` مفسر پایتون متوجه decorator‌ آن شده است و به جای اجرا، یک نمونه شی از آن را به decorator‌ مشخص شده (``decorator‌_name``) ارسال می‌کند و یک شی جدید که در اینجا با تابع ``wrapper`` مشخص شده است را دریافت و اجرا می‌کند.

در مورد توابع دارای پارامتر نیز باید توجه داشت که در هنگام فراخوانی تابع مورد نظر و ارسال آرگومان به تابع، مفسر پایتون این آرگومان‌ها را به تابع ``wrapper`` از decorator‌ ارسال می‌کند::

  >>> def multiply_in_2(func):
  ...     def wrapper(*args):
  ...         return func(*args) * 2
  ...     return wrapper 
  ... 
  >>> 
  >>> @multiply_in_2
  ... def sum_two_numbers(a, b):
  ...     return a + b
  ... 
  >>> 
  >>> sum_two_numbers(2, 3)
  10

::

  >>> # normal
  >>>
  >>> def multiply_in_2(func):
  ...     def wrapper(*args):
  ...         return func(*args) * 2
  ...     return wrapper 
  ... 
  >>> 
  >>> def sum_two_numbers(a, b):
  ...     return a + b
  ... 
  >>> 
  >>> wrapper = multiply_in_2(sum_two_numbers)
  >>> wrapper(2, 3)
  10




می‌توان بیش از یک Decorator‌ به کلاس‌ها و توابع خود اعمال کرد که در این صورت ترتیب قرار گرفتن این Decorator‌ها برای مفسر پایتون دارای اهمیت است::

  @decorator_3
  @decorator_2
  @decorator_1
  def function_name():
      print("Somthing!")


  function_name()


::

  wrapper = decorator_3(decorator_2(decorator_1(function_name)))
  wrapper()


همچنین می‌توان به Decorator‌ها آرگومان نیز ارسال کرد::

  @decorator_name(params)
  def function_name():
      print("Somthing!")


  function_name()

در این حالت مفسر پایتون ابتدا آرگومان را به تابع Decorator‌ ارسال می‌کند و سپس حاصل را با آرگومان ورودی تابع مورد نظر فراخوانی می‌کند::

  temp_decorator = decorator_name(params)
  wrapper = temp_decorator(function_name)
  wrapper()

به نمونه کد پایین توجه نمایید::

  >>> def formatting(lowerscase=False):
  ...     def formatting_decorator(func):
  ...         def wrapper(text=''):
  ...             if lowerscase:
  ...                 func(text.lower())
  ...             else:
  ...                 func(text.upper())
  ...         return wrapper 
  ...     return formatting_decorator
  ... 
  >>> 
  >>> @formatting(lowerscase=True)
  ... def chaap(message):
  ...     print(message)
  ... 
  >>> 
  >>> chaap("I Love Python")
  i love python
  >>> 


**functools.wraps@**
~~~~~~~~~~~~~~~~~~~~~

در پایتون عنوانی مطرح است به نام **Higher-order functions** (توابع مرتبه بالاتر) و به توابعی گفته می‌شود که اعمالی را روی توابع دیگر انجام می‌دهند یا یک تابع جدید را به عنوان خروجی برمی‌گرداند. بر همین اساس یک ماژول به نام ``functools`` نیز در کتابخانه استاندارد پایتون قرار گرفته است که یک سری توابع کمکی و کاربردی برای این دست توابع ارائه می‌دهد [`اسناد پایتون <https://docs.python.org/3/library/functools.html>`__]. یکی از توابع داخل این ماژول ``wraps`` [`اسناد پایتون <https://docs.python.org/3/library/functools.html#functools.wraps>`__] می‌باشد.

**اما چرا معرفی این تابع در این بخش مهم است؟** وقتی ما از یک Decorator‌ استفاده می‌کنیم، اتفاقی که می‌افتد این است که یک تابع جدید جایگزین تابع اصلی ما می‌شود. به نمونه کدهای پایین توجه نمایید::

  >>> def func(x):
  ...     """does some math"""
  ...     return x + x * x
  ... 
  >>>
  >>> print(func.__name__)
  func
  >>> print(func.__doc__)
  does some math
  >>> 

::

  >>> def logged(func):
  ...     def with_logging(*args, **kwargs):
  ...         print(func.__name__ + " was called")
  ...         return func(*args, **kwargs)
  ...     return with_logging
  ... 
  >>> 
  >>> @logged
  ... def f(x):
  ...     """does some math"""
  ...     return x + x * x
  ... 
  >>> 
  >>> print(f.__name__)
  with_logging
  >>> print(f.__doc__)
  None
  >>>

:: 

  >>> # It is mean: f = logged(func)
  ... 
  >>> f = logged(func)
  >>> print(f.__name__)
  with_logging

در زمان استفاده Decorator‌ وقتی خواستیم نام تابع را چاپ کنیم ``(__print(f.__name`` نام تابع جدید (``with_logging``) چاپ شد و نه تابع اصلی (``f``).

استفاده از Decorator‌ همیشه به معنی از دست رفتن اطلاعات مربوط به تابع اصلی است که به منظور جلوگیری از این اتفاق و حفظ اطلاعات مربوط به تابع اصلی خود می‌توانیم از تابع ``wraps`` استفاده کنیم. این تابع در واقع خود یک Decorator‌ است که وظیفه آن کپی اطلاعات از تابعی که به عنوان آرگومان دریافت می‌کند به تابعی که به آن انتساب داده شده است::

  >>> from functools import wraps
  >>> 
  >>> def logged(func):
  ...     @wraps(func)
  ...     def with_logging(*args, **kwargs):
  ...         print(func.__name__ + " was called")
  ...         return func(*args, **kwargs)
  ...     return with_logging
  ... 
  >>> 
  >>> @logged
  ... def f(x):
  ...    """does some math"""
  ...    return x + x * x
  ... 
  >>> 
  >>> print(f.__name__)
  f
  >>> print(f.__doc__)
  does some math
  >>> 


لطفا به آخرین مثال از بحث Decorator‌ نیز توجه فرمایید. در این مثال زمان اجرای یک تابع را با استفاده از Decorator‌ها محاسبه خواهیم کرد [`منبع <https://realpython.com/primer-on-python-decorators/#a-few-real-world-examples>`__]::

  >>> import functools
  >>> import time
  >>> 
  >>> def timer(func):
  ...     """Print the runtime of the decorated function"""
  ...     @functools.wraps(func)
  ...     def wrapper_timer(*args, **kwargs):
  ...         start_time = time.perf_counter()   
  ...         value = func(*args, **kwargs)
  ...         end_time = time.perf_counter() 
  ...         run_time = end_time - start_time 
  ...         print(f"Finished {func.__name__!r} in {run_time:.4f} secs")
  ...         return value
  ...     return wrapper_timer
  ... 
  >>> 
  >>> @timer
  ... def waste_some_time(num_times):
  ...     result = 0
  ...     for _ in range(num_times):
  ...         for i in range(10000)
  ...             result += i**2
  ... 
  >>> 
  >>> waste_some_time(1)
  Finished 'waste_some_time' in 0.0072 secs
  >>> waste_some_time(999)
  Finished 'waste_some_time' in 2.6838 secs

در این مثال از تابع ``perf_counter`` [`اسناد پایتون <https://docs.python.org/3/library/time.html#time.perf_counter>`__] برای محاسبه فواصل زمانی (time intervals) استفاده شده که تنها از نسخه 3.3 به بعد در دسترس می‌باشد [`اطلاعات تکمیلی <https://stackoverflow.com/a/25787875/8434370>`__].

چنانچه درک کد دستور ``print`` در تابع ``wrapper_timer`` برایتان مبهم است به درس هفتم بخش **f-string** مراجعه نمایید [`درس هفتم f-string </lessons/l07-string-and-bytes-in-python.html#f-string>`__].


.. _python-generator: 

Generator
----------

ژنراتور (مولد) یا همان **Generator‌** ها [`PEP 255 <https://www.python.org/dev/peps/pep-0255/>`__] به توابعی گفته می‌شوند که به منظور ایجاد یک تابع با رفتاری مشابه اشیا ``iterator`` (تکرارکننده - درس نهم) پیاده‌سازی می‌گردند.

هنگام فراخوانی یک تابع معمولی، بدنه تابع اجرا می‌شود تا به یک دستور ``return`` برسد و خاتمه یابد ولی با فراخوانی یک تابع Generator‌، بدنه تابع اجرا نمی‌شود بلکه یک شی ``generator`` برگردانده خواهد شد که  می‌توان با استفاده از متد ``()__next__`` آن، مقادیر مورد انتظار خود را یکی پس از دیگری درخواست داد.

عملکرد Generator‌ به صورت **lazy** (کندرو) [`ویکی‌پدیا <https://en.wikipedia.org/wiki/Lazy_evaluation>`__] می‌باشد و داده‌ها را یکجا ذخیره نمی‌کند بلکه آنها را تنها در همان زمانی که درخواست می‌شوند، **تولید** (Generate) می‌کند. بنابراین در هنگام برخورد با مجموعه داده‌های بزرگ، Generator‌ها مدیریت حافظه کارآمدتری دارند و همچنین ما مجبور نیستیم پیش از استفاده از یک دنباله منتظر بمانیم تا تمام مقادیر آن تولید شوند!.

برای ایجاد یک تابع Generator تنها کافی است در یک تابع معمولی از یک یا چند دستور ``yield`` استفاده کنیم. اکنون مفسر پایتون در هنگام فراخوانی چنین تابعی یک شی ``generator`` برمی‌گرداند که توانایی تولید یک **دنباله** (Sequence) از مقادیر (یا شی) برای استفاده در کاربردهای تکرارپذیر را دارد.

سینتکس دستور ``yield`` شبیه دستور ``return`` است ولی با کاربردی متفاوت. این دستور در هر نقطه‌ای از بدنه تابع که باشد،  اجرای برنامه را  در آن نقطه متوقف می‌کند و  ما می‌توانیم با استفاده از متد ``()__next__`` مقدار **yield (حاصل) شده** را دریافت نماییم::


  >>> def a_generator_function():
  ...    for i in range(3):  # i: 0, 1, 2
  ...       yield i*i
  ...    return
  ... 
  >>> my_generator = a_generator_function()  # Create a generator
  >>> 
  >>> my_generator.__next__()  #  Use my_generator.next() in Python 2.x
  0
  >>> my_generator.__next__()
  1
  >>> my_generator.__next__()
  4
  >>> my_generator.__next__()
  Traceback (most recent call last):
    File "<stdin>", line 1, in <module>
  StopIteration
  >>> 

باید توجه داشت که پایان فرآیند تولید  تابع Generator توسط استثنا ``StopIteration`` گزارش می‌شود. البته در زمان استفاده از دستورهایی به مانند ``for`` این استثنا کنترل شده و حلقه پایان می‌پذیرد. نمونه کد قبل را به صورت زیر بازنویسی می‌کنیم::

  >>> def a_generator_function():
  ...    for i in range(3):  # i: 0, 1, 2
  ...       yield i*i
  ...    return
  ... 
  >>> 
  >>> for i in a_generator_function():
  ...     print(i)
  ... 
  0
  1
  4
  >>> 

به منظور درک بهتر عملکرد  تابع Generator‌، تصور کنید از شما خواسته شده است که یک تابع شخصی مشابه با تابع ``()range`` پایتون پیاده‌سازی نمایید. راهکار شما چه خواهد بود؟  ایجاد یک شی‌ای مانند لیست (list) یا توپِل خالی و پر کردن آن با استفاده از یک حلقه؟! این راهکار شاید برای ایجاد بازه‌های کوچک پاسخگو باشد ولی برای ایجاد یک بازه صد میلیونی آیا حافظه و زمان کافی در اختیار دارید؟. این مسئله را با استفاده از تابع Generator‌ به سادگی و درستی حل خواهیم کرد::

  >>> def my_range(stop):
  ...     number = 0
  ...     while number < stop:
  ...         yield number
  ...         number = number + 1
  ...     return
  ... 
  >>> 
  >>> for number in my_range(100000000):
  ...     print(number)



.. _python-generator-features: 

ویژگی‌های تابع Generator‌
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

* تابع Generator‌ شامل یک یا چند دستور ``yield`` می‌باشد.

* در زمان فراخوانی تابع Generator‌، تابع اجرا نمی‌شود ولی در عوض یک شی از نوع ``generator`` برای آن تابع برگردانده می‌شود.

* با استفاده از دستور ``yield`` می‌توانیم در هر نقطه‌ای از تابع Generator‌ که بخواهیم توقف ایجاد کنیم و مقدار  **yield (حاصل) شده** را با استفاده از متد ``()__next__`` دریافت نماییم. 

* با نخستین فراخوانی متد ``()__next__`` تابع اجرا می‌شود، تا زمانی که به یک دستور ``yield`` برسد. در این زمان  دستور ``yield`` یک نتیجه تولید می‌کند و اجرای تابع متوقف می‌شود. با فراخوانی مجدد  متد ``()__next__`` اجرای تابع از ادامه همان دستور ``yield`` سر گرفته می‌شود.

* معمولا نیازی به استفاده مستقیم از متد ``()__next__`` نمی‌شود و توابع Generator‌ از طریق دستورهایی به مانند ``for`` یا  توابعی به مانند ``()sum`` و... که توانایی دریافت یک **دنباله** (Sequence) را دارند، مورد استفاده قرار می‌گیرند.

* در پایان تولید توابع Generator‌ یک  استثنا ``StopIteration`` در نقطه توقف خود گزارش می‌دهند که می‌بایست درون برنامه کنترل شود.

* فراموش نکنیم که استفاده از دستور ``return`` در هر کجا از بدنه تابع باعث پایان یافتن اجرای تابع در آن نقطه می‌شود و توابع Generator‌ نیز از این امر مسثنا نیستند!.

* با فراخوانی متد ``close`` می‌توانید یک شی Generator‌ را خاموش کنید!. توجه داشته باشید که پس از فراخوانی این متد چنانچه باز هم درخواست ایجاد مقدار ارسال (``()__next__``) شود یک  استثنا ``StopIteration`` گزارش می‌گردد.



به یک نمونه کد دیگر نیز توجه نمایید::

  >>> def countdown(n):
  ...     print("Counting down from %d" % n)
  ...     while n > 0:
  ...        yield n
  ...        n -= 1
  ...     return
  ... 
  >>> 
  >>> countdown_generator = countdown(10)
  >>> 
  >>> countdown_generator.__next__()
  Counting down from 10
  10
  >>> countdown_generator.__next__()
  9
  >>> countdown_generator.__next__()
  8
  >>> countdown_generator.__next__()
  7
  >>> 
  >>> countdown_generator.close()
  >>> 
  >>> countdown_generator.__next__()
  Traceback (most recent call last):
    File "<stdin>", line 1, in <module>
  StopIteration
  >>> 


.. tip::
    شی Generator را می‌توان با استفاده از تابع ``()list`` به شی لیست تبدیل کرد::
     
       >>> countdown_list = list(countdown(10))
       Counting down from 10
       >>> 
       >>> countdown_list
       [10, 9, 8, 7, 6, 5, 4, 3, 2, 1]
       >>> 




.. _python-coroutine: 

در ادامه Coroutine :yield
------------------------------------

از نسخه پایتون 2.5 ویژگی‌های جدیدی به تابع Generator‌ افزوده شد [`PEP 342 <https://www.python.org/dev/peps/pep-0342/>`__]. اگر داخل یک تابع، دستور ``yield`` را در سمت راست یک عملگر انتساب ``=`` قرار دهیم آنگاه تابع مذکور رفتار متفاوتی از خود نشان می‌دهد که به آن در زبان برنامه‌نویسی پایتون **Coroutine** (کوروتین) گفته می‌شود. تصور کنید که اکنون می‌توانیم مقادیر دلخواه خود را به تابع Generator‌ ارسال کنیم!::

  >>> def receiver():
  ...     print("Ready to receive")
  ...     while True:
  ...         n = (yield)
  ...         print("Got %s" % n)
  ... 
  >>> 


  >>> receiver_generator = receiver()

  >>> receiver_generator.__next__() # python 3.x - In Python 2.x use .next()
  Ready to receive

  >>> receiver_generator.send('WooW!!')
  Got WooW!!

  >>> receiver_generator.send(1)
  Got 1

  >>> receiver_generator.send(':)')
  Got :)

چگونگی اجرای یک **Coroutine** همانند یک Generator‌ است ولی با این تفاوت که متد ``()send`` نیز برای ارسال مقدار به درون تابع در اختیار است.


با فراخوانی تابع Coroutine، بدنه اجرا نمی‌شود بلکه یک شی از نوع Generator‌ برگردانده می‌شود. متد ``()__next__`` اجرای برنامه را به نخستین ``yield`` می‌رساند، در این نقطه تابع در وضعیت تعلیق (Suspend) قرار می‌گیرد و آماده دریافت مقدار است. متد ``()send`` مقدار مورد نظر را به تابع ارسال می‌کند که این مقدار توسط عبارت ``(yield)`` در Coroutine دریافت می‌شود. پس از دریافت مقدار، اجرای Coroutine تا رسیدن به ``yield`` بعدی (در صورت وجود) یا انتهای بدنه تابع ادامه می‌یابد.

در بحث Coroutineها برای رهایی از فراخوانی متد ``()__next__`` می‌توان از Decorator‌ها استفاده کرد::


  >>> def coroutine(func):
  ...     def start(*args,**kwargs):
  ...         generator = func(*args,**kwargs)
  ...         generator.__next__()
  ...         return generator
  ...     return start
  ...   
  >>> 
  >>> @coroutine
  ... def receiver():
  ...     print("Ready to receive")
  ...     while True:
  ...         n = (yield)
  ...         print("Got %s" % n)
  ... 
  >>> 
  >>> receiver_generator = receiver()
  >>> receiver_generator.send('Hello World')  # Note : No initial .next()/.__next__() needed


یک Coroutine می‌تواند به دفعات نامحدود اجرا شود مگر اینکه اجرای آن توسط برنامه با فراخوانی متد ``()close`` یا به خودی خود با پایان خطوط اجرای تابع، پایان بپذیرد. 

چنانچه پس از پایان Coroutine، متد ``()send`` فراخوانی شود یک استثنا ``StopIteration`` رخ خواهد داد::

  >>> receiver_generator.close()
  >>> receiver_generator.send('value')
  Traceback (most recent call last):
    File "<stdin>", line 1, in <module>
  StopIteration


یک Coroutine می‌تواند همزمان با دریافت مقدار، خروجی نیز تولید و برگرداند::

  >>> def line_splitter(delimiter=None):
  ...     print("Ready to split")
  ...     result = None
  ...     while True:
  ...         line = yield result
  ...         result = line.split(delimiter)
  ... 
  >>> 
  >>> splitter = line_splitter(",")
  >>> 
  >>> splitter.__next__()  # python 3.x - In Python 2.x use .next()
  Ready to split
  >>> 
  >>> splitter.send("A,B,C")
  ['A', 'B', 'C']
  >>> 
  >>> splitter.send("100,200,300")
  ['100', '200', '300']
  >>> 

**چه اتفاقی افتاد؟!**

تابع ``line_splitter`` با مقدار ورودی ``","`` فراخوانی می‌شود. همانطور که می‌دانیم در این لحظه تنها اتفاقی که می‌افتد ایجاد یک نمونه شی از نوع Generator‌ خواهد بود (و هیچ یک از خطوط داخل بدنه تابع اجرا نخواهد شد). با فراخوانی متد ``()__splitter.__next`` بدنه تابع به اجرا درمیاید تا به نخستین ``yield`` برسد. یعنی عبارت ``"Ready to split"`` در خروجی چاپ، متغیر ``result`` با مقدار اولیه ``None`` تعریف و در نهایت با تایید شرط دستور ``while`` اجرا به سطر ``line = yield result`` می‌رسد. در این سطر بر اساس ارزیابی عبارت سمت راست عمل انتساب، مقدار متغیر  ``result`` که برابر ``None`` است به خارج از تابع برگردانده و سپس تابع در وضعیت تعلیق (Suspend) قرار می‌گیرد. ولی باید توجه داشت که هنوز عمل انتساب در این سطر به صورت کامل به انجام نرسیده است!. در ادامه با فراخوانی متد ``("splitter.send("A,B,C``، رشته ``"A,B,C"`` در ``yield`` قرار داده می‌شود و اجرای برنامه از حالت تعلیق خارج و ادامه می‌یابد. مقدار ``yield`` به ``line`` انتساب داده می‌شود و اجرای سطر ``line = yield result`` کامل می‌شود. در سطر بعد، رشته درون متغیر ``line`` بر اساس ``delimiter`` که در ابتدا با ``","`` مقداردهی شده بود تفکیک و به متغیر ``result`` انتساب داده می‌شود (مقدار متغیر ``result`` که تا پیش از این برابر ``None`` بوده است تغییر می‌کند). با پایان خطوط بدنه و تایید دوباره درستی شرط دستور ``while``، بدنه آن یکبار دیگر اجرا می‌شود تا از نو به ``yield`` برسد یعنی به سطر ``line = yield result``. اکنون در بار دوم اجرای حلقه بر خلاف بار نخست مقدار متغیر ``result`` برابر با ``None`` نبوده و عمل yield آن یا همان بازگرداندن آن در خروجی قابل مشاهده خواهد بود یعنی مقدار ``['A', 'B', 'C']`` که در بار نخست اجرای حلقه تولید شده بود، اکنون در خروجی به نمایش در خواهد آمد و سپس تابع بار دیگر در حالت تعلیق قرار می‌گیرد (تابع منتظر فراخوانی یکی از متدهای ``()send`` یا ``()__next__`` یا ``()close`` می‌ماند). روال کار با فراخوانی متد ``("splitter.send("100,200,300`` به همین صورت ادامه می‌یابد...

در مورد سطر ``line = yield result``، می‌دانیم که برای انجام عمل انتساب ابتدا لازم است مقدار عبارت سمت راست ارزیابی و سپس به سمت چپ انتساب داده شود. یعنی مفسر پایتون ابتدا ``yield result`` را اجرا می‌کند که حاصل آن بازگرداندن مقدار متغیر ``result`` (در بار نخست اجرای حلقه = ``None``) به خارج تابع خواهد بود و سپس عبارت ``line = yield`` که مقدار ارسالی از متد ``()send`` را به متغیر ``line`` انتساب می‌دهد.

|

مبحث Coroutine گسترده‌تر از سطحی است که در این درس می‌تواند بیان شود ولی در این لحظه برای دریافت مثال‌ها، کاربرد و جزییات بیشتر در موضوع Coroutine زبان برنامه‌نویسی پایتون، ارائه آقای David Beazley در کنفرانس PyCon'2009 می‌تواند مفید باشد.  

PDF: [`A Curious Course on Coroutines and Concurrency <https://www.dabeaz.com/coroutines/Coroutines.pdf>`__]

VIDEO: [`YouTube <https://www.youtube.com/watch?v=Z_OAlIhXziw>`__]



List Comprehensions
--------------------

**List Comprehensions** به عملیاتی گفته می‌شود که در طی آن می‌توان یک تابع را به تک تک اعضای یک نوع شی لیست (list) اعمال و نتیجه را در قالب یک نوع شی لیست جدید دریافت کرد [`PEP 202 <https://www.python.org/dev/peps/pep-0202/>`__]::

  >>> numbers = [1, 2, 3, 4, 5]
  >>> squares = [n * n for n in numbers]
  >>> 
  >>> squares
  [1, 4, 9, 16, 25]
  >>> 

نمونه کد بالا برابر است با::

  >>> numbers = [1, 2, 3, 4, 5]
  >>> squares = []
  >>> for n in numbers:
  ...     squares.append(n * n)
  ... 
  >>> 
  >>> squares
  [1, 4, 9, 16, 25]


سینتکس کلی List Comprehensions به صورت زیر است::

  [expression for item1 in iterable1 if condition1
      for item2 in iterable2 if condition2
      ...
      for itemN in iterableN if conditionN]

  # This syntax is roughly equivalent to the following code:

  s = []
  for item1 in iterable1:
      if condition1:
          for item2 in iterable2:
              if condition2:
              ...
                 for itemN in iterableN:
                     if conditionN: s.append(expression)


به مثال‌هایی دیگر در این زمینه توجه نمایید::

  >>> a = [-3,5,2,-10,7,8]
  >>> b = 'abc'

  >>> [2*s for s in a]
  [-6, 10, 4, -20, 14, 16]

  >>> [s for s in a if s >= 0]
  [5, 2, 7, 8]

  >>> [(x,y) for x in a for y in b if x > 0]
  [(5, 'a'), (5, 'b'), (5, 'c'), (2, 'a'), (2, 'b'), (2, 'c'), (7, 'a'), (7, 'b'), (7, 'c'), (8, 'a'), (8, 'b'), (8, 'c')]

  >>> import math
  >>> c = [(1,2), (3,4), (5,6)]
  >>> [math.sqrt(x*x+y*y) for x,y in c]
  [2.23606797749979, 5.0, 7.810249675906654]


توجه داشته باشید، چنانچه نتیجه اعمال List Comprehensions در هر نوبت شامل بیش از یک عضو باشد، می‌بایست مقادیر نتایج در داخل یک پرانتز قرار داده شوند (به صورت یک شی توپِل - tuple). 

مانند::

    [(x,y) for x in a for y in b if x > 0]

با توجه به این موضوع عبارت زیر از نظر مفسر پایتون نادرست می‌باشد::

  >>> [x,y for x in a for y in b]
    File "<stdin>", line 1
      [x,y for x in a for y in b]
             ^
  SyntaxError: invalid syntax
  >>> 

یک نکته مهم دیگر باقی‌مانده است. به نمونه کد پایین توجه نمایید::

  >>> x = 'before'
  >>> a = [x for x in (1, 2, 3)]
  >>> 
  >>> x
  'before'


اکنون List Comprehensions حوزه خود را دارد، در نتیجه مقدار متغیر خارج از آن بدون تغییر باقی می‌ماند. [`توضیحات بیشتر <https://stackoverflow.com/a/4199355/8434370>`__]



Generator Expressions
----------------------

عملکرد **Generator Expressions** مشابه **List Comprehensions** است ولی با خاصیت یک شی Generator و برای ایجاد آن کافی است به جای براکت ``[]`` در List Comprehensions از پرانتز ``()`` استفاده کنیم. [`PEP 289 <https://www.python.org/dev/peps/pep-0289/>`__]::

  >>> a = [1, 2, 3, 4]
  >>> b = (10*i for i in a)
  >>> 
  >>> 
  >>> b
  <generator object <genexpr> at 0x7f488703aca8>
  >>> 
  >>> b.__next__()  # python 3.x - In Python 2.x use .next()
  10
  >>> b.__next__()  # python 3.x - In Python 2.x use .next()
  20
  >>> 

درک تفاوت Generator Expressions و List Comprehensions بسیار مهم است. خروجی یک **List Comprehensions** دقیقا همان نتیجه انجام عملیات در قالب یک شی لیست است در حالی که خروجی یک **Generator Expressions** شی‌ای است که می‌داند چگونه نتایج را مرحله به مرحله تولید کند. درک این دست موضوعات نقش مهمی در بالا بردن کارایی (Performance) برنامه و مصرف حافظه (Memory) خواهد داشت.

با اجرای نمونه کد پایین؛ از میان تمام سطرهای داخل فایل The_Zen_of_Python.txt، سطرهایی که به صورت کامنت در زبان پایتون باشند چاپ می‌شوند:

.. code-block:: text
    :linenos:

    Beautiful is better than ugly.
    Explicit is better than implicit.
    Simple is better than complex.
    Complex is better than complicated.
    Flat is better than nested.
    Sparse is better than dense.
    Readability counts.
    Special cases aren't special enough to break the rules.
    Although practicality beats purity.
    Errors should never pass silently.
    Unless explicitly silenced.
    In the face of ambiguity, refuse the temptation to guess.
    There should be one-- and preferably only one --obvious way to do it.
    Although that way may not be obvious at first unless you're Dutch.
    Now is better than never.
    Although never is often better than *right* now.
    If the implementation is hard to explain, it's a bad idea.
    If the implementation is easy to explain, it may be a good idea.
    Namespaces are one honking great idea -- let's do more of those!
    ------------------------------------------------------------------
    # File Name: The_Zen_of_Python.txt
    # The Zen of Python
    # PEP 20: https://www.python.org/dev/peps/pep-0020

::

  >>> file = open("/home/saeid/Documents/The_Zen_of_Python.txt")
  >>> lines = (t.strip() for t in file) 
  >>> comments = (t for t in lines if t[0] == '#')
  >>> for c in comments:
  ...     print(c)
  ... 
  # File Name: The_Zen_of_Python.txt
  # The Zen of Python
  # PEP 20: https://www.python.org/dev/peps/pep-0020
  >>>

در سطر یکم، فایل The_Zen_of_Python.txt باز شده و در سطر دوم یک شی Generator برای دستیابی و strip کردن (حذف کاراکترهای خالی (space) احتمالی در ابتدا و انتهای متن سطر) آن‌ها به شیوه  **Generator Expressions** به دست آمده است. توجه داشته باشید که سطرهای فایل هنوز خوانده نشده‌اند و تنها امکان درخواست و پیمایش سطر به سطر فایل ایجاد شده است. در سطر سوم با ایجاد یک شی Generator دیگر (باز هم به شیوه **Generator Expressions**) امکان فیلتر سطرهای کامنت مانند در داخل فایل را به کمک شی ``lines`` مرحله قبل، به دست آورده‌ایم. ولی هنوز سطرهای فایل خوانده نشده‌اند چرا که هنوز درخواستی مبنی بر تولید به هیچ یک از دو شی Generator ایجاد شده (``lines`` و ``comments``) ارسال نشده است. تا اینکه بالاخره در سطر چهارم دستور حلقه ``for`` شی ``comments`` را به جریان می‌اندازد و این شی نیز بر اساس عملیات تعریف شده برای آن، شی  ``lines`` را به جریان در می‌آورد.

فایل The_Zen_of_Python.txt مورد استفاده در این مثال حجم بسیار کمی دارد ولی تاثیر به کار گرفتن **Generator Expressions** در این مثال را می‌توانید با استخراج کامنت‌های یک فایل چند گیگابایتی مشاهده نمایید!

.. tip::
    شی Generator ایجاد شده  به شیوه  **Generator Expressions** را نیز می‌توان با استفاده از تابع ``()list`` به شی لیست تبدیل کرد::
     
       >>> comment_list = list(comments)
       >>> comment_list
       ['# File Name: The_Zen_of_Python.txt', 
       '# The Zen of Python', 
       '# PEP 20: https://www.python.org/dev/peps/pep-0020']




.. _python-lambda: 

lambda و توابع ناشناس
---------------------------

در زبان برنامه‌نویسی پایتون توابع ناشناس (Anonymous functions) یا **Lambda functions** توابعی هستند که می‌توانند هر تعداد آرگومان داشته باشند ولی بدنه آن‌ها می‌بایست تنها شامل یک عبارت باشد. برای ساخت این دست توابع از کلمه کلیدی ``lambda`` استفاده می‌شود. الگوی ساختاری این نوع تابع به صورت زیر است::

  lambda args : expression

در این الگو ``args`` معرف هر تعداد آرگومان است که با استفاده از کاما (``,``) از یکدیگر جدا شده‌اند و ``expression`` بیانگر تنها یک عبارت پایتونی است که شامل دستوراتی همچون ``for`` یا ``while`` نمی‌شود.


به عنوان نمونه تابع پایین را در نظر بگیرید::

  >>> def a_function(x, y):
  ...     return x + y
  ... 
  >>>
  >>> a_function(2, 3)
  5

این تابع در فرم ناشناس به صورت زیر خواهد بود::

  >>> a_function = lambda x,y : x+y
  >>> a_function(2, 3)
  5

یا::

  >>> (lambda x,y: x+y)(2, 3)
  5


**کاربرد اصلی Lambda functions کجاست؟**

این دست توابع بیشتر در مواقعی که می‌خواهیم یک تابع کوتاه را به عنوان آرگومان به تابعی دیگر ارسال کنیم کاربرد دارند. 

برای نمونه از درس هشتم به یاد داریم که برای مرتب‌سازی اعضای یک شی لیست از متد ``()sort`` استفاده و بیان شد که متد ``()sort`` آرگومان اختیاری با نام ``key`` دارد که می‌توان با ارسال یک تابع تک آرگومانی به آن عمل دلخواهی را بر روی تک تک عضوهای لیست مورد نظر، پیش از مقایسه و مرتب‌سازی به انجام رساند (به عنوان مثال: تبدیل حروف بزرگ به کوچک)::

  >>> L = ['a', 'D', 'c', 'B', 'e', 'f', 'G', 'h']

  >>> L.sort()

  >>> L
  ['B', 'D', 'G', 'a', 'c', 'e', 'f', 'h']

  >>> L.sort(key=lambda n: n.lower())

  >>> L
  ['a', 'B', 'c', 'D', 'e', 'f', 'G', 'h']
  >>> 




|

----

:emoji-size:`😊` امیدوارم مفید بوده باشه




