.. role:: emoji-size

.. meta::
   :description: پایتون به پارسی - کتاب آنلاین و آزاد آموزش زبان برنامه‌نویسی پایتون - درس بیست و دوم: شی گرایی (OOP) در پایتون: Type Hinting و دیتا کلاس (Data Class)


.. _lesson-22: 

درس ۲۲: شی گرایی (OOP) در پایتون: Type Hinting و دیتا کلاس (Data Class)
===================================================================================================

.. figure:: /_static/pages/22-python-object-oriented-programming-type-hinting-data-class.jpg
    :align: center
    :alt: شی گرایی (OOP) در پایتون: Type Hinting و دیتا کلاس (Data Class)
    :class: page-image

    Photo by `Stefan Widua <https://unsplash.com/photos/vdds_nsH-FE>`__

این درس به عنوان آخرین بخش از دروس آموزش شی‌گرایی در زبان برنامه‌نویسی پایتون به شرح یک ویژگی جدید در این زبان با نام **دیتا کلاس (Data Class)** می‌پردازد. البته پیش از شروع لازم است با یک سینتکس جدید نیز در پایتون آشنا شویم، در این سینتکس ما نوع داده‌های خود را نیز به صراحت ذکر می‌کنیم، شیوه‌ای که Type Hints [`PEP 484 <https://www.python.org/dev/peps/pep-0484/>`__] خوانده می‌شود. هنگام ایجاد دیتا کلاس (Data Class) به دانش این سینتکس نیاز خواهیم داشت.


توجه داشته باشید که تمام مطالب این درس تنها از نسخه‌های 3.5 به بعد پایتون پشتیبانی می‌گردد (هر جایی که به نسخه‌ای بالاتر نیاز باشد، به صراحت ذکر می‌گردد).



:emoji-size:`✔` سطح: متوسط

----


.. contents:: سرفصل‌ها
    :depth: 2

----



Type Hinting
----------------------------

زبان برنامه‌نویسی پایتون همچنان یک زبان برنامه‌نویسی پویا (Dynamic) می‌باشد اما این زبان از نسخه 3.5 به بعد تلاش کرده در پاسخ به نیاز کامیونیتی و نیز کمک به توسعه و بهبود عملکرد ابزارهای شخص ثالث (3rd party) همچون Linterها، Type checkerها یا IDEها، استاندارد و نیز سینتکس امکان درج نوع را فراهم بیاورد. سینتکس این قابلیت در پایتون الهام گرفته از ابزار mypy [`وب‌سایت <http://mypy-lang.org>`__] که یک Static type checker است، می‌باشد. 

[`مطالعه بیشتر:‌ پرسش و پاسخ مرتبط در StackOverflow <https://stackoverflow.com/q/32557920>`__]

کد نویسی با Type Hinting یک امر اختیاری بوده که در واقع چیزی مشابه مستندسازی می‌باشد ولی باعث افزایش خوانایی کد می‌گردد. همچنین به شرط استفاده از ابزارهای استاندارد و بررسی کامل کد پیش از اجرا، می‌تواند از بروز برخی خطاهای runtime جلوگیری نماید.


استفاده از mypy
~~~~~~~~~~~~~~~~~~~~~~

نمونه کدهایی که در این بخش ارائه می‌شود، همگی قابلیت تست یا بررسی نوع را با ابزارهای  شخص ثالث استاندارد همچون mypy را دارد. برای استفاده از این ابزار می‌بایست ابتدا آن را نصب نمایید::

     $ pip3 install mypy


پس از نصب جهت بررسی کد، ابتدا لازم است کد خود را با استفاده از mypy کامپایل نمایید::

     $ mypy program.py


این دستور خطاهای احتمالی از عدم رعایت نوع مناسب در برنامه را پیدا و گزارش می‌دهد. پس از بررسی، debugging و رفع خطاهای احتمالی اکنون می‌توانید برنامه خود را اجرا نمایید::


     $ python3 program.py


Variable Annotations 
~~~~~~~~~~~~~~~~~~~~~~

سینتکس درج نوع برای متغییرها که در نسخه 3.6 پایتون ارائه گشته است [`PEP 526 <https://www.python.org/dev/peps/pep-0526/>`__]. بر اساس این سند، سینتکس تعریف یک متغیر به همراه نوع به صورت زیر خواهد بود::

    var: annotation

که در آن var نام متغیر و annotation نوع مورد نظر خواهد بود. همچنین چنانچه بخواهیم همزمان با اعلان نوع، یک مقدار اولیه نیز به متغیر خود انتساب دهیم::

    var: annotation = value


::


    >>> a: int
    >>> a
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
    NameError: name 'a' is not defined
    >>> a = 10
    >>> a
    10

توجه داشته باشید، تا قبل از عمل انتساب هنوز متغیری ایجاد نشده است، چرا که این نام نمی‌داند باید به چه مقداری در حافظه اشاره داشته باشد.

::

    >>> item: int
    >>> for item in [1, 3, 9]:
    ...     print(item)
    ... 
    1
    3
    9



::

    >>> a: int = 5
    >>> a
    5
    >>> type(a)
    <class 'int'>


**اسکریپت زیر را در نظر بگیرید:**


.. code-block:: python
    :linenos:

    # sample.py

    a: int

    a = 'python'

    print(type(a))
    print(a)

چنانچه اسکریپت فوق را با پایتون اجرا نماییم- اسکریپت فوق بدون هیچ خطایی اجرا می‌گردد::

    $ python3 sample.py                                                                     
    <class 'str'>
    python

ولی اگر اسکریپت فوق را با mypy تست نماییم::

    $ mypy sample.py
    sample.py:5: error: Incompatible types in assignment (expression has type "str", variable has type "int")
    Found 1 error in 1 file (checked 1 source file)

یک خطا گزارش می‌گردد (بر روی سطر ۵)، چرا که نوع متغییر ``a`` برابر ``int`` مشخص شده است ولی با یک مقدار از نوع ``str`` مقداردهی شده است.


Function Annotations 
~~~~~~~~~~~~~~~~~~~~~~

سند [`PEP 3107 <https://www.python.org/dev/peps/pep-3107/>`__] به ارائه سینتکس مربوط به اعلام نوع در تعریف پارامترها و نیز نوع مقدار خروجی در پایتون می‌پردازد::

    def func(arg: arg_type, optarg: arg_type = default) -> return_type:
        ...

::

    >>> a: int = 7

    >>> def square_area(x:int=2) -> int:
    ...     return x * x
    ... 
    >>> square_area()
    4
    >>> square_area(5)
    25


::

    >>> square_area.__annotations__
    {'x': <class 'int'>, 'return': <class 'int'>}

    >>> __annotations__
    {'a': <class 'int'>}

با استفاده از یک attribute ویژه در پایتون به نام ``__annotations__`` می‌توان در زمان runtime به مشخصات و نوع تعریف شده در یک شی تابع، کلاس یا ماژول دسترسی پیدا کرد.

*توجه داشته باشید منظور annotations در پایتون عباراتی هستند که با سینتکس خاص معرفی شده توسط Variable Annotations و... ایجاد می‌شوند.*

برای توابعی که فاقد دستور ``return`` هستند، نوع خروجی می‌بایست به صورت ``None <-`` تعریف گردد. چرا که حتی توابع فاقد ``return`` نیز به صورت ضمنی شامل دستور ``return None`` هستند::


    >>> def print_item(x:str='') -> None:
    ...     print(x)



سینتکس annotation برای پارامترهایی ``kwargs**`` و ``args*`` به صورت زیر می‌باشد::

    >>> def print_all(*args:str, **kwargs:str) -> None:
    ...     print('args:', args)
    ...     print('kwargs:', kwargs)
    ... 
    >>> 
    >>> print_all('s', ('a', 'e'))
    args: ('s', ('a', 'e'))
    kwargs: {}
    >>> print_all('d', 'c', param='pppp')
    args: ('d', 'c')
    kwargs: {'param': 'pppp'}

در این مواقع نیز می‌بایست نوع با دقت مشخص گردد.



Class Annotations 
~~~~~~~~~~~~~~~~~~~~~~

بر اساس مطالب ارائه شده تا این لحظه می‌توان ساختار یک کلاس را به صورت زیر در نظر گرفت:


.. code-block:: python
    :linenos:

    from typing import ClassVar

    class Sample:

        a: str = 'a_data'
        b: ClassVar[str] = "b_data"

        x: int

        def __init__(self, x: int, y:int=8) -> None:
            self.x = x
            self.y = y

کلاس ``Sample`` شامل دو Class Attribute با نام‌های ``a`` و ``b`` - همچنین دو Instance Attribute به نام‌های  ``x`` و ``y`` می‌باشد. به دو شیوه تعریف هر کدام در مثال بالا توجه نمایید.

نوع ``ClassVar`` یک wrapper برای نوع متغیرهای داخل کلاس می‌باشد که وظیفه آن برچسب زدن یک متغیر به عنوان Class Attribute می‌باشد. این wrapper از داخل ماژول ``typing`` در دسترس خواهد بود [`اسناد پایتون <https://docs.python.org/3/library/typing.html#typing.ClassVar>`__]. به منظور افزایش خوانایی بهتر است تمامی Class Attribute با استفاده از ``ClassVar`` نوع گذاری گردند.

به حاصل دستورات زیر در رابطه با کلاس ``Sample`` مثال قبل توجه نمایید:




.. code-block:: python
    :linenos:


    obj = Sample(5)

    print('\nSEC#01', '-' * 30)
    print('Class  Atrr:', dir(Sample))
    print('Object Atrr:', dir(obj))

    print('\nSEC#02', '-' * 30)
    print(Sample.__annotations__)
    print(obj.__annotations__)

    print('\nSEC#03', '-' * 30)
    print('Class  vars:', vars(Sample))
    print('Object vars:', vars(obj))

    print('\nSEC#04', '-' * 30)
    print('x:', obj.x)
    print('y:', obj.y)
    print('a:', Sample.a)
    print('b:', Sample.b)

    print('\nSEC#05', '-' * 30)

    obj.x = 10
    obj.y = 16
    Sample.a = "PYTHON"
    Sample.b = "LANGUAGE"
    print('x:', obj.x)
    print('y:', obj.y)
    print('a:', Sample.a)
    print('b:', Sample.b)


::
    
    SEC#01 ------------------------------
    Class  Atrr: ['__annotations__', '__class__', '__delattr__', '__dict__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__le__', '__lt__', '__module__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', '__weakref__', 'a', 'b']
    Object Atrr: ['__annotations__', '__class__', '__delattr__', '__dict__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__le__', '__lt__', '__module__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', '__weakref__', 'a', 'b', 'x', 'y']

    SEC#02 ------------------------------
    {'a': <class 'str'>, 'b': typing.ClassVar[str], 'x': <class 'int'>}
    {'a': <class 'str'>, 'b': typing.ClassVar[str], 'x': <class 'int'>}

    SEC#03 ------------------------------
    Class  vars: {'__module__': '__main__', '__annotations__': {'a': <class 'str'>, 'b': typing.ClassVar[str], 'x': <class 'int'>}, 'a': 'a_data', 'b': 'b_data', '__init__': <function Sample.__init__ at 0x7faae8f16bf8>, '__dict__': <attribute '__dict__' of 'Sample' objects>, '__weakref__': <attribute '__weakref__' of 'Sample' objects>, '__doc__': None}
    Object vars: {'x': 5, 'y': 8}


    SEC#04 ------------------------------
    x: 5
    y: 8
    a: a_data
    b: b_data

    SEC#05 ------------------------------
    x: 10
    y: 16
    a: PYTHON
    b: LANGUAGE


تابع ``vars`` تمام attributeهای شی دریافتی را در قالب یک شی دیکشنری برمی‌گرداند [`اسناد پایتون <https://docs.python.org/3/library/functions.html#vars>`__].



ماژول typing
~~~~~~~~~~~~~~~~~~~~~~

این ماژول از نسخه 3.5 با هدف فراهم آوردن پشتیبانی از Type Hinting در Runtime پایتون، افزوده شده است [`اسناد پایتون <https://docs.python.org/3/library/typing.html>`__]. 

برخی از مواردی که این ماژول در پشتیبانی از قابلیت Type Hints فراهم آورده است به شرح زیر است. جهت آشنایی بیشتر می‌توانید به صفحه اصلی مستندات مراجعه نمایید.


**-- معادل برخی از انواع --**

تاکنون فقط به ذکر نوع از انواع ساده‌ای همچون ``int`` و ``str`` پرداخته‌ایم، با این حال ذکر نوع برای نوع داده دیکشنری که شامل اعضایی به صورت کلید:مقدار بوده و هر عضو نیز می‌تواند از دو نوع مختلف باشد چگونه باید انجام شود؟ در پاسخ باید گفت که ماژول ``typing`` یک سری انواع معادل فراهم آورده است.


* ``Dict`` [`اسناد پایتون <https://docs.python.org/3/library/typing.html#typing.Dict>`__] معادل ``dict``

  ::

       >>> from typing import Dict

       >>> d: Dict[str, int] = {'a': 97, 'b': 98, 'c': 99, 'd': 100}

       >>> d
       {'a': 97, 'b': 98, 'c': 99, 'd': 100}
       >>> type(d)
       <class 'dict'>
       >>> isinstance(d, dict)
       True


  ::

       >>> d = {'a': 97, 'b': 98, 'c': 99, 'd': 100}



* ``List`` [`اسناد پایتون <https://docs.python.org/3/library/typing.html#typing.List>`__] معادل ``list``

  ::

       >>> from typing import List

       >>> L: List[int] = [97, 98, 99, 100]

       >>> L
       [97, 98, 99, 100]
       >>> type(L)
       <class 'list'>
       >>> isinstance(L, list)
       True


  ::

       >>> L = [97, 98, 99, 100]




* ``Set`` [`اسناد پایتون <https://docs.python.org/3/library/typing.html#typing.Set>`__] معادل ``set``

  ::

       >>> from typing import Set

       >>> s: Set[str] = {'a', 'b', 'c', 'd'}

       >>> s
       {'d', 'c', 'a', 'b'}
       >>> type(s)
       <class 'set'>
       >>> isinstance(s, set)
       True

  ::

       >>> s = {'a', 'b', 'c', 'd'}


|



**-- NewType --**

با استفاده از این تابع می‌توان یک نوع جدید یا در واقع یک Wrapper شخصی برای انواع موجود ایجاد نماییم [`اسناد پایتون <https://docs.python.org/3/library/typing.html#newtype>`__].


سینتکس ``NewType('UserId', int)`` یک نوع جدید با نام ``UserId`` بر اساس نوع اصلی ``int`` ایجاد می‌کند. توجه داشته باشید که نوع جدید تنها از نظر ظاهر برای ابزارهای type checker متفاوت بوده ولی در پایتون همان ماهیت نوع اصلی را خواهد داشت:

::

       >>> from typing import NewType

       >>> UserId = NewType('UserId', int)

       >>> some_id = UserId(524313)

       >>> some_id
       524313
       >>> type(some_id)
       <class 'int'>
       >>> isinstance(some_id, int)
       True

::

      >>> def get_user_name(user_id: UserId) -> str:
      ...      if user_id == 1633:
      ...          return 'saeid'
      ...      else:
      ...          return ''
      ... 
      >>> saeid_id = UserId(1633)
      >>> get_user_name(saeid_id)
      'saeid'

|

**-- Any --**

یک نوع خاص که به معنی هر نوعی می‌باشد، در واقع ``Any`` هر نوعی می‌تواند باشد [`اسناد پایتون <https://docs.python.org/3/library/typing.html#the-any-type>`__]. دو قطعه کد زیر از نظر ابزارهای type checker کاملا مشابه یکدیگر هستند:

::

        >>> def func(param):
        ...     return param
        ... 
        >>> 

::

        >>> from typing import Any
        >>> def func(param: Any) -> Any:
        ...     return param
        ... 
        >>> func(4)
        4
        >>> func('py')
        'py'
        >>> func([0, 1, 2])
        [0, 1, 2]


|

**-- Callable --**

یک نوع خاص دیگر برای شرح نوع یک شی Callable (درس هفدهم) به مانند توابع می‌باشد [`اسناد پایتون <https://docs.python.org/3/library/typing.html#callable>`__]. ساختار این نوع به صورت زیر است:

::

        Callable[[Arg1Type, Arg2Type,...], ReturnType]


به مثال زیر توجه نمایید:

.. code-block:: python
    :linenos:

    from typing import Any, Callable

    class Response:

        def __init__(self, code:int, message:str, result:Any) -> None:
            self.code = code
            self.message = message
            self.result = result


    def success_handler(result:Any) -> None:
        pass


    def error_handler(code:int, message:str) -> None:
        pass


    def async_query(on_success: Callable[[Any], None],
                    on_error: Callable[[int, str], None]) -> Response:
        pass


    async_query(success_handler, error_handler)




Data Classes
----------------------------

از **نسخه 3.7 پایتون** یک ویژگی جالب به پایتون اضافه گردید. دیتا کلاس **(Data Class)** [`PEP 557 <https://www.python.org/dev/peps/pep-0557>`__]، در واقع سینتکسی ساده‌سازی شده برای ایجاد کلاس‌هایی می‌باشد که معمولا تنها حاوی Instance Attribute هستند. این نوع کلاس با استفاده از دکوراتور ``dataclass@`` از ماژول ``dataclasses`` ایجاد می‌گردد [`اسناد پایتون <https://docs.python.org/3/library/dataclasses.html>`__]. برای مثال کلاس زیر را در نظر بگیرید:


.. code-block:: python
    :linenos:

    from dataclasses import dataclass

    @dataclass
    class Student:
        name: str
        score: int

    student = Student('Saeid', 70)
    print(student)
    print('-' * 30)
    print(student.name)
    print(student.score)
    print('-' * 30)
    print(Student('Saeid', 70) == Student('Saeid', 70))

::

    Student(name='Saeid', score=70)
    ------------------------------
    Saeid
    70
    ------------------------------
    True


در این نوع کلاس برای تعریف Attributeها از سینتکس  Variable Annotations [`PEP 526 <https://www.python.org/dev/peps/pep-0526/>`__] استفاده می‌شود. 

باید توجه داشت که طبق سند PEP 484 پیروی از اصول Type Hints در پایتون اجباری نبوده، نیست و نخواهد شد. ولی Data Class یک استثناست و در آن حتما می‌بایست Attributeها به شیوه شرح داده شده، تعریف گردند و به آن‌ها فیلدهای (field) دیتا کلاس گفته می‌شود.

از آنجا که این نوع کلاس برای ایجاد یک کاربرد عمومی از کلاس‌ها توسعه یافته (نگهداری اطلاعات)، بنابراین بسیاری از عملیات‌ها در آن خودکارسازی شده تا پیاده‌سازی این کلاس ساده‌تر از هر کلاس دیگری باشد. برای مثال نیازی به پیاده‌سازی متد ``__init__`` نیست و این متد به صورت خودکار برای کلاس ما ایجاد می‌گردد (به لطف Type Hinting!). اکنون اگر بخواهیم دیتاکلاس مثال قبل را به صورت عادی پیاده‌سازی کنیم:


.. code-block:: python
    :linenos:

    class Student:

        def __init__(self, name, score):
            self.name = name
            self.score = score


    student = Student('Saeid', 70)
    print(student)
    print('-' * 30)
    print(student.name)
    print(student.score)
    print('-' * 30)
    print(Student('Saeid', 70) == Student('Saeid', 70))

::

    <__main__.Student object at 0x7f922a311518>
    ------------------------------
    Saeid
    70
    ------------------------------
    False


با مقایسه این دو خروجی، مشاهده می‌شود که مقدار چاپ شی (سطر ۹) و نیز حاصل مقایسه دو شی (سطر ۱۴) با مقادیر یکسان، متفاوت است. دلیل نیز پیشتر بیان شد،‌ تعدادی متد خاص همانند ``__init__`` برای دیتا کلاس‌ها به صورت خودکار تولید می‌شوند که با پیاده‌سازی پیش‌فرض متفاوت‌ بوده و بر نوع کاربرد این کلاس‌ها و راحتی استفاده تمرکز شده است. این پیاده‌سازی را می‌توان به صورت زیر نمایش داد:



.. code-block:: python
    :linenos:

    class Student:

        def __init__(self, name, score):
            self.name = name
            self.score = score

        def __str__(self):
            return (f'{self.__class__.__name__}'
                    f'(name={self.name!r}, score={self.score!r})')

        def __eq__(self, other):
            return (self.name, self.score) == (other.name, other.score)


    student = Student('Saeid', 70)
    print(student)
    print('-' * 30)
    print(student.name)
    print(student.score)
    print('-' * 30)
    print(Student('Saeid', 70) == Student('Saeid', 70))

::

    Student(name='Saeid', score=70)
    ------------------------------
    Saeid
    70
    ------------------------------
    True

از دروس پیش با متد ``__eq__`` آشنا هستیم، متد ``__str__`` [`اسناد پایتون <https://docs.python.org/3/reference/datamodel.html#object.__str__>`__] نیز یکی دیگر از متدهای خاص پایتون می‌باشد و هنگامی که یک شی می‌خواهد به نوع str تبدیل گردد، به صورت خودکار فراخوانی می‌گردد (**تبدیل به نوع رشته - درس هفتم**)، به صورت مشابه متد ``__repr__`` [`اسناد پایتون <https://docs.python.org/3/reference/datamodel.html#object.__repr__>`__] نیز قابل پیاده سازی است.


بهتر است مقداردهی اولیه اشیای دیتاکلاس‌ها را به روش **نام=مقدار** انجام دهید (هنگام نمونه‌سازی)، در غیر این صورت اگر ترتیب تعریف فیلدها در کلاس را از بالا به پایین در نظر بگیریم، آنگاه ترتیب قرار گرفتن پارامترها در متد ``__init__`` که قرار است تولید شود، با حفظ ترتیب، از چپ به راست خواهند بود.

به همین دلیل می‌بایست در ترتیب قرارگرفتن فیلدهایی که دارای مقدار پیش‌فرض هستند دقت کرد و آن‌ها را جزو فیلد‌های انتهایی درنظر گرفت. چرا که تعریف متد ``__init__`` با خطا مواجه می‌گردد. از تعریف توابع به یاد داریم، پارامتر با مقدار پیش‌فرض نمی‌تواند پیش از پارامتر بدون مقدار پیش‌فرض قرار بگیرد! برای مثال سینتکس تعریف تابع زیر اشتباه می‌باشد::

        def func (a, b, name='s', d):
                 ^
    SyntaxError: non-default argument follows default argument


.. _data-class-type-hinting: 

Type Hinting
~~~~~~~~~~~~~~~~~~~~~~

تنها این Attributeهای یک دیتا کلاس است که می‌بایست بر اساس قوانین سینتکس Type Hinting نوشته شوند. در این بین برای درج Class Attributeها نیز می‌بایست حتما از ``ClassVar`` استفاده گردد، در غیر این صورت آن Attribute در حکم Instance Attribute خواهد بود.


متد ``__post_init__``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

دیتا کلاس‌ها همچنین می‌توانند شامل متد نیز باشند، چگونگی تعریف متد در دیتا کلاس تفاوتی با دیگر کلاس‌ها ندارد. 

از طرفی می‌دانیم که متد ``__init__`` یک دیتا کلاس به صورت خودکار ایجاد می‌گردد و مرحله initialize شی از دستان ما خارج شده است. با این حال چنانچه اگر کلاس  شامل متدی با نام ``__post_init__`` باشد، این متد پس از ``__init__`` به صورت خودکار فراخوانی می‌گردد:

.. code-block:: python
    :linenos:

    from dataclasses import dataclass

    @dataclass
    class Student:
        name: str
        score: int

        def __post_init__(self):
            print("__post_init__ got called:", self)
            if self.name == 'Saeed':
                self.name =  'Saeid'


    student = Student('Saeed', 70)
    print(student)

::

    __post_init__ got called: Student(name='Saeed', score=70)
    Student(name='Saeid', score=70)



از طریق ماژول ``dataclasses`` یک annotation type جدید با نام ``InitVar`` در دسترس است. چنانچه در تعریف هر یک از Attributeها کلاس از این نوع استفاده کنیم، آن Attribute به عنوان پارامتر به متد ``__post_init__`` ارسال می‌گردد. باید توجه داشت که این نوع Attributeها به عنوان **Init-only variables** شناخته می‌شوند [`اسناد پایتون <https://docs.python.org/3/library/dataclasses.html#init-only-variables>`__] و مفسر پایتون آن‌ها را صرفا به ``__post_init__`` ارسال می‌کند و **جزو فیلدهای دیتا کلاس قرار نمی‌دهد**:


.. code-block:: python
    :linenos:

    from dataclasses import dataclass, InitVar

    @dataclass
    class Student:
        name: InitVar[str]
        score: int

        def __post_init__(self, name):
            if name == 'Saeid':
                self.score =  100


    student = Student('Saeid', 70)
    print(student)
    print('-' * 30)
    print(student.name)


::

    Student(score=100)
    ------------------------------
    Traceback (most recent call last):
      File "sample.py", line 16, in <module>
        print(student.name)
    AttributeError: 'Student' object has no attribute 'name'


تابع ``field`` و ``fields``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

تابع ``fields`` از ماژول ``dataclasses`` یک شی از دیتا کلاس یا خود دیتا کلاس را از ورودی دریافت و یک توپِل حاوی تمام فیلد‌های آن بر می‌گرداند [`اسناد پایتون <https://docs.python.org/3/library/dataclasses.html#dataclasses.fields>`__]:

.. code-block:: python
    :linenos:

    from dataclasses import dataclass, InitVar, fields

    @dataclass
    class Student:
        name: str
        score: int = 70
        age: InitVar[int] = 18


    obj = Student('saeid', 90, 20)
    print(obj)
    print(fields(obj))

::

   Student(name='saeid', score=90)
   (Field(name='name',type=<class 'str'>,default=<dataclasses._MISSING_TYPE object at 0x7f7e5c68cd68>,default_factory=<dataclasses._MISSING_TYPE object at 0x7f7e5c68cd68>,init=True,repr=True,hash=None,compare=True,metadata=mappingproxy({}),_field_type=_FIELD), Field(name='score',type=<class 'int'>,default=70,default_factory=<dataclasses._MISSING_TYPE object at 0x7f7e5c68cd68>,init=True,repr=True,hash=None,compare=True,metadata=mappingproxy({}),_field_type=_FIELD))


پیش‌تر گفتیم، Attributeهای داخل یک دیتا کلاس فیلد (Field) خوانده می‌شوند. خروجی بالا نمایش ساختار یک شی Field از دیتا کلاس می‌باشد [`اسناد پایتون <https://docs.python.org/3/library/dataclasses.html#dataclasses.field>`__]. در واقع متغیرهایی که داخل دیتا کلاس با سنتکس Variable Annotations تعریف می‌شوند، به صورت خودکار به فیلد (Field) تبدیل می‌شوند. فیلدها می‌توانند حاوی مقدار پیش‌فرض باشند (همانند فیلد ``score``). برای کاستن از حجم functionality داخل یک دیتا کلاس، ماژول ``dataclasses`` پایتون شامل تابعی است با نام ``field`` که توانایی و انعطاف زیادی در فراهم آوردن مقدار پیش‌فرض برای فیلدهای تعریف شده ایجاد می‌کند. 

یک شی فیلد شامل پارامترهایی است که از طریق تابع ``field`` قابل تنظیم هستند، البته به جز دو پارامتر زیر که از تعریف Variable Annotations استنباط می‌شوند:


* ``name``: نام فیلد

* ``type``: نوع (type) فیلد

**تعریف مقدار پیش‌فرض برای یک فیلد با استفاده از تابع** ``field``::

    field(*, default=MISSING, default_factory=MISSING, repr=True, hash=None, init=True, compare=True, metadata=None)

* توجه:‌ همانطور که از مبحث Keyword-Only Arguments از درس دوازدهم به یاد داریم، فراخوانی این تابع تنها با استفاده از ارسال آرگومان به صورت نام=مقدار مجاز خواهید بود.


* ``default``:  مقدار پیش‌فرض فیلد، در صورت عدم نیاز می‌بایست با مقدار ویژه ``MISSING`` مقداردهی گردد.

* ``default_factory``: یک موجودیت callable بدون آرگومان را دریافت می‌کند و در زمانی که به مقدار پیش‌فرض برای فیلد نیاز باشد، فراخوانی می‌گردد. در صورت عدم نیاز می‌بایست با مقدار ویژه ``MISSING`` مقداردهی گردد. به بیانی دیگر می‌توان با استفاده از این پارامتر،‌ یک تابع به فیلد اختصاص داد که مقدار یا مقادیر پیش‌فرضی را برای فیلد مورد نظر تولید نماید. 

* توجه: در هر فیلد تنها یکی از دو پارامتر ``default`` یا ``default_factory`` می‌تواند حاوی مقداری غیر از ``MISSING`` باشد.


* ``repr``, ``init``, ``compare``, ``hash``: در صورتی که هر کدام از این پارامتر‌ها برابر با مقدار ``True`` (پیش‌فرض) تنظیم گردند، فیلد مربوطه به متدهای ایجاد شده متناظر با هر پارامتر ارسال خواهد شد::

      repr    -->> __repr__ __str__
      init    -->> __init__
      compare -->> __eq__ __ne__ __lt__ __le__ __gt__ __ge__
      hash    -->> __hash__


* توجه چنانچه مقدار ``compare`` برابر ``True`` تنظیم گردد (حالت پیش‌فرض)،‌ مقدار ``hash`` می‌بایست ``None`` (و نه ``False``) باشد، چرا که عملیات مقایسه دو شی دیگر به مقدار hash وابسته نبوده و از طریق متدهای تولید شده (__eq__ و غیره) انجام خواهد شد. 


* ``metadata``: می‌توان اطلاعات اضافی و دلخواه پیرامون فیلد را در قالب یک شی دیکشنری به این پارامتر ارسال کرد.



 به نمونه کد زیر توجه نمایید:

.. code-block:: python
    :linenos:

    from dataclasses import dataclass, field, fields
    from typing import List


    def get_default_books():
        return []


    @dataclass
    class Book:
        id: int 
        name: str = field(compare=False)   


    @dataclass
    class Author:
        id: int 
        name: str = field(compare=False, metadata={'coding': 'UTF-8'})   
        books: List[Book] = field(default_factory=get_default_books, compare=False)



    author = Author(id=1, name='Saeid')
    print(author)
    print(fields(author))


::

    Author(id=1, name='Saeid', books=[])
    (Field(name='id',type=<class 'int'>,default=<dataclasses._MISSING_TYPE object at 0x7f5e66a58e48>,default_factory=<dataclasses._MISSING_TYPE object at 0x7f5e66a58e48>,init=True,repr=True,hash=None,compare=True,metadata=mappingproxy({}),_field_type=_FIELD), Field(name='name',type=<class 'str'>,default=<dataclasses._MISSING_TYPE object at 0x7f5e66a58e48>,default_factory=<dataclasses._MISSING_TYPE object at 0x7f5e66a58e48>,init=True,repr=True,hash=None,compare=False,metadata=mappingproxy({'coding': 'UTF-8'}),_field_type=_FIELD), Field(name='books',type=typing.List[__main__.Book],default=<dataclasses._MISSING_TYPE object at 0x7f5e66a58e48>,default_factory=<function get_default_books at 0x7f5e66bcb1e0>,init=True,repr=True,hash=None,compare=False,metadata=mappingproxy({}),_field_type=_FIELD))


Immutable Data Classes
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

دکوراتور ``dataclass@`` چندین پارامتر با مقدار پیش‌فرض دارد که به شرح زیر می‌باشند [`اسناد پایتون <https://docs.python.org/3/library/dataclasses.html#dataclasses.dataclass>`__]::

    @dataclass(init=True, repr=True, eq=True, order=False, unsafe_hash=False, frozen=False)
    class Sample:
       ...


* ``init``: اگر ``True`` باشد، متد ``__init__`` تولید می‌شود.

* ``repr``: اگر ``True`` باشد، متد ``__repr__`` تولید می‌شود.

* ``order``: اگر ``True`` باشد، متدهای ``__gt__`` ،``__le__`` ،``__lt__`` و ``__ge__`` تولید می‌شوند.

* ``unsafe_hash``: اگر ``False`` باشد، آنگاه بر اساس مقادیر ``eq`` ،``init`` و ``frozen`` و شرایط موجود یک متد ``__hash__`` مناسب تولید می‌شود.


**frozen**

چنانچه این پارامتر برابر ``True`` تنظیم گردد، دیتا کلاس Immutable (غیرقابل تغییر) خواهد شد و دیگر نمی‌توان مقدار هیچکدام از فیلدهای اشیای آن را پس از نمونه‌سازی تغییر داد، این رفتار در موارد بسیاری می‌تواند مفید باشد:



.. code-block:: python
    :linenos:

    from dataclasses import dataclass

    @dataclass(frozen=True)
    class Position:
        name: str
        lon: float = 0.0
        lat: float = 0.0

    pos = Position('Tehran', 35.6, 51.5)

    print(pos.name)
    print('-' * 30)
    pos.name = 'Qazvin'

::

    Tehran
    ------------------------------
    Traceback (most recent call last):
      File "sample.py", line 13, in <module>
        pos.name = 'Qazvin'
      File "<string>", line 3, in __setattr__
    dataclasses.FrozenInstanceError: cannot assign to field 'name'


.. _data-class-inheritance: 

وراثت (Inheritance)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

دیتا کلاس‌ها می‌توانند از یکدیگر ارث‌بری داشته باشند:


.. code-block:: python
    :linenos:

    from dataclasses import dataclass


    @dataclass
    class Person:
        name: str


    @dataclass
    class Friend(Person):
        city: str

        def say_hi(self):
            print(f'Hi {self.name}')


    f = Friend(city='Tehran', name='Armin')
    f.say_hi()

    f = Friend('Tehran', 'Armin')
    f.say_hi()

::

    Hi Armin
    Hi Tehran


بهتر است مقداردهی اولیه اشیای دیتاکلاس‌ها را به روش **نام=مقدار** انجام دهید، در غیر این صورت باید بدانید در هنگام ارث‌بری ابتدا فیلدهای supperclass مقداردهی می‌شوند! در نتیجه می‌توان تعریف متد ``__init__`` برای کلاس ``Friend`` را برابر با تعریف زیر فرض کرد::


    def __init__(self, name, city):

به همین دلیل نیز اگر یکی از فیلدهای supperclass دارای مقدار پیش‌فرض باشد، می‌بایست فیلدهای subclass نیز دارای مقدار پیش‌فرض باشند. چرا که تعریف متد ``__init__`` با خطا مواجه می‌گردد. از تعریف توابع به یاد داریم، پارامتر با مقدار پیش‌فرض نمی‌تواند پیش از پارامتر بدون مقدار پیش‌فرض قرار بگیرد!


|

----

:emoji-size:`😊` امیدوارم مفید بوده باشه



