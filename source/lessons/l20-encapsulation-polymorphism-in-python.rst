.. role:: emoji-size

.. meta::
   :description: پایتون به پارسی - کتاب آنلاین و آزاد آموزش زبان برنامه‌نویسی پایتون - درس بیستم: شی گرایی (OOP) در پایتون: Encapsulation و چندریختی (Polymorphism) (OOP)


.. _lesson-20:

درس ۲۰: شی گرایی (OOP) در پایتون: Encapsulation و چندریختی (Polymorphism)
===================================================================================================

.. figure:: /_static/pages/20-python-object-oriented-programming-polymorphism-encapsulation.jpg
    :align: center
    :alt: شی گرایی (OOP) در پایتون: Encapsulation و چندریختی (Polymorphism)
    :class: page-image

    Photo by `sanjiv nayak <https://unsplash.com/photos/yTR70oYHEQw>`__

این درس در ادامه دروس گذشته مرتبط با آموزش شی گرایی در زبان برنامه‌نویسی پایتون می‌باشد. تاکنون با دو تا از چهار مفهوم مهم در شی‌گرایی آشنا شده‌ایم: **وراثت (Inheritance)** - درس هجدهم و **انتزاع (Abstraction)** - درس نوزدهم. این درس به بررسی دو مورد باقی‌مانده، یعنی **کپسوله‌سازی (Encapsulation)** و **چندریختی (Polymorphism)** در زبان برنامه‌نویسی پایتون می‌پردازد.


:emoji-size:`✔` سطح: متوسط

----


.. contents:: سرفصل‌ها
    :depth: 2

----



کپسوله‌سازی (Encapsulation)
---------------------------------------------------------------

در مبحث شی گرایی به پنهان‌سازی اطلاعات درونی یک شی و محدود کردن دسترسی به آن‌ها از بیرون، **کپسوله‌سازی (Encapsulation)** گفته می‌شود - در واقع Encapsulation برابر است با **Information hiding**.

پیش از هر چیزی لازم است بدانیم که زبان برنامه‌نویسی پایتون از فلسفه‌ای پیروی می‌کند که در جمله «اینجا همه بزرگسال هستیم» "we are all consenting adults here" خلاصه می‌شود! بنابراین این زبان برخلاف زبان‌هایی مانند Java و ++C یک Encapsulation قوی (strong) در اختیار برنامه‌نویس قرار نمی‌دهد. پایتون به برنامه‌نویس اعتماد دارد و می‌گوید «اگر دوست داری در مکان‌های تاریک پرسه بزنی، من مطمئنم که دلیل خوبی داری و هیچ مشکلی ایجاد نمی‌کنی!»


.. tip:: 
  به صورت پیش‌فرض تمام اجزای داخلی یک کلاس، **public** هستند و از هر جایی خارج از کلاس مرتبط خود، قابل دستیابی می‌باشند.

.. tip:: 
  بر اساس یک قرارداد مابین برنامه‌نویسان پایتون،‌ چنانچه ابتدای نام Attributeها و Methodها با **یک کاراکتر خط زیرین** (``_``) آغاز شود، این مفهوم را با خود می‌رساند که «دست نزنید مگر داخل همان کلاس یا subclassهای آن». رعایت این قرارداد معادل سطح دسترسی **protected** در Java و ++C می‌باشد.

.. tip:: 
  
  زبان برنامه‌نویسی پایتون از تکنیک «دستکاری نام» یا **name mangling** [`ویکی‌پدیا <https://en.wikipedia.org/wiki/Name_mangling>`__] پشتیبانی می‌کند. به کمک این تکنیک و با قراردادن **دو کاراکتر خط زیرین** (``__``) در ابتدای نام هر یک از اجزای داخلی یک کلاس (Attributeها و Methodها)، می‌توان معادل سطح دسترسی **private** در Java و ++C را پیاده‌سازی کرد [`اسناد پایتون <https://docs.python.org/3/tutorial/classes.html#private-variables>`__].

به نمونه کد زیر توجه نمایید:

.. code-block:: python
    :linenos:

    class Student:

        def __init__(self, name, score=0):
            self.name = name
            self.__score = score
 
        def display(self):
            print('name:', self.name)
            print('score:', self.__score)


    student = Student('Saeid', 70)

    #accessing using method
    student.display()

    #accessing directly from outside
    print('-' * 10, 'Accessing directly from outside')
    print('name:', student.name)
    print('score:', student.__score)


::

    name: Saeid
    score: 70
    ---------- Accessing directly from outside
    name: Saeid
    Traceback (most recent call last):
      File "sample.py", line 20, in <module>
        print('score:', student.__score)
    AttributeError: 'Student' object has no attribute '__score'

داده‌های private را در خارج از کلاس نمی‌توان مستقیم مورد دستیابی قرار داد و همانطور که از نمونه کد بالا مشاهده می‌شود دستیابی چنین عناصری در پایتون باعث بروز AttributeError می‌شود. اما گفته شد که پایتون Encapsulation قوی ندارد، چه اتفاقی افتاد؟

مفسر پایتون بر اساس تکنیک **name mangling**، نام تمام عناصری (Attributeها و Methodها) که تنها با **دو کاراکتر خط زیرین** شروع شده باشند (مانند ``spam__``) را به صورت زیر با افزودن نام کلاس به ابتدای آن تغییر می‌دهد::

    _classname__spam

بنابراین اگر برنامه‌نویسی به دنبال دستیابی نام موجود (``spam__``) در کلاس باشد، چیزی پیدا نخواهد کرد. با این کار پایتون به صورت نسبی امکان دور نگه‌داشتن عناصر را از حالت عمومی فراهم آورده است. برای مثال پیش داریم::

    >>> dir(student)
    ['_Student__score', '__class__', '__delattr__', '__dict__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__le__', '__lt__', '__module__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', '__weakref__', 'display', 'name']


متدهای Setter و Getter
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
 
در برنامه‌نویسی شی گرا چنانچه بخواهیم دسترسی به داده‌ای را به شدت محدود کنیم، به آن داده سطح دسترسی private را اعمال می‌کنیم. اما گاهی می‌خواهیم تنها روند دستیابی و تغییر برخی از داده‌ها را کنترل کنیم - دسترسی مجاز است ولی چگونگی آن مهم است - در این صورت علاوه بر تنظیم سطح دسترسی private به آن عناصر متدهایی را برای تغییر (به عنوان Setter) و دستیابی (به عنوان Getter) آن‌ها نیز می‌بایست ایجاد کنیم:

.. code-block:: python
    :linenos:

    class Student:

        def __init__(self, name, score=0):
            self.name = name
            self.__score = score

        def set_score(self, score):
            if isinstance(score, int) and  0 <= score <= 100:
                self.__score = score

        def get_score(self):
            return self.__score


    student = Student('Saeid', 70)
    student.set_score(99)
    student.set_score('100')
    student.set_score(-10)
    print(f'{student.name}, score:', student.get_score())


::

    Saeid, score: 99




چندریختی (Polymorphism)
---------------------------------------------------------------

چندریختی از کلمات یونانی Poly (زیاد) و Morphism (ریخت) گرفته شده است و در برنامه‌نویسی شی گرا به این معنی است که از یک نام یکسان متد برای انواع مختلف می‌توان استفاده کرد.

در مبحث برنامه‌نویسی شی گرا به شیوه‌های زیر می‌توان چندریختی (Polymorphism) را پیاده‌سازی کرد:

* Method Overriding
* Method Overloading
* Operator Overloading

در ادامه به بررسی و پیاده‌سازی هر مورد در زبان برنامه‌نویسی پایتون خواهیم پرداخت.


Method Overriding
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

این نوع از چندریختی در هنگام پیاده‌سازی وراثت (Inheritance - درس هجدهم) قابل استفاده است و تا کنون نیز بارها از آن بهره گرفتیم!.

در واقع به پیاده‌سازی دوباره یک متد از کلاس **superclass** در کلاس **subclass** را **Method Overriding** می‌گویند. در این مواقع متد superclass در زیر سایه متد هم نام در subclass قرار می‌گیرد و هنگام فراخوانی متد توسط اشیای کلاس subclass، این متد subclass است که فراخوانی می‌گردد:

.. code-block:: python
    :linenos:

    class Animal:

        def breathe(self):
          print('Animal, breathing...')

        def walk(self):
          print('Animal, walking...')
    
    
    class Dog(Animal):

        def walk(self):
          print('Dog, walking...')


    dog = Dog()
    dog.breathe()
    dog.walk()


::

    Animal, breathing...
    Dog, walking...

در این نمونه کد، کلاس Dog از کلاس Animal ارث‌بری دارد و متد ``walk`` از کلاس Animal را Override کرده است. همانطور که از خروجی مشاهده می‌شود، برخلاف متد ``breathe``، هنگام فراخوانی متد ``walk`` توسط شی Dog، متد باز‌پیاده‌سازی شده موجود در این کلاس فراخوانی می‌شود.

.. tip:: 

  همان‌طور که پیش‌تر نیز انجام می‌دادیم، چنانچه تمایل به فراخوانی متد متناظر در superclass را داشته باشیم، می‌توانیم از تابع ``super`` استفاده کنیم.

.. tip:: 

  اتفاقی که در بحث انتزاع (Abstraction) و ارث‌بری از کلاس‌های Abstract شاهد بودیم نیز در واقع پیروی از همین مبحث بوده و با این تفاوت که Method Overriding اجباری می‌بود.

.. tip:: 

  در زبان برنامه‌نویسی پایتون تنها این نام متدهاست که در Method Overriding نقش دارد و تعداد پارامترهای تعریف شده در هر متد مهم نمی‌باشد. بنابراین متد همنام موجود در subclass می‌تواند پارامترهای متفاوتی نسبت superclass داشته باشد. البته تغییر در پارامترهای متد باز‌پیاده‌سازی شده چیزی نیست که بخواهیم آن را پیشنهاد بدهیم (به خصوص در بحث پیاده‌سازی متدهای Abstract) چرا که یکی از پیامدهای آن شکسته شدن اصل Liskov Substitution Principle [`ویکی‌پدیا <https://en.wikipedia.org/wiki/Liskov_substitution_principle>`__] می‌شود.



Method Overloading
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

این نوع از چندریختی به امکان کنارهم قرار گرفتن چندین متد همنام ولی با پارامترهای متفاوت (از نظر تعداد یا نوع) در کنار هم می‌باشد. یک شی می‌تواند با ارسال آرگومان‌های متفاوت و فراخوانی یک نام یکسان از متد، کارهای متفاوتی را به انجام برساند.

همانطور که در قسمت پیش نیز اشاره شد، در زبان برنامه‌نویسی پایتون تعداد و نوع پارامترهای تعریف شده برای یک تابع یا متد، هیچ ارتباطی با هویت آن متد ندارد و یک متد تنها با نام آن شناسایی می‌شود. **بنابراین Method Overloading در پایتون پشتیبانی نمی‌شود** و چنانچه چندین متد یا تابع همنام با پارامترهای متفاوت در یک کلاس یا ماژول در کنار هم باشند، خطایی رخ نمی‌دهد ولی باید توجه داشته باشید که متد یا تابع آخر، تمام موارد پیش از خود را در زیر سایه خواهد گرفت:

.. code-block:: python
    :linenos:

    class Animal:

        def breathe(self):
            print('breathing...')

        def walk(self):
            print('walking...')

        def walk(self, time=30):
            print(f'{time} minutes, walking...')

        def walk(self, minutes=30, seconds=59):
            print(f'{minutes} minutes and {seconds} seconds, walking...')


    animal = Animal()
    animal.walk()


::

    30 minutes and 59 seconds, walking...


همان‌طور که از خروجی نمونه کد بالا مشاهده می‌شود، با فراخوانی متد ``walk`` توسط شی Animal، از میان سه متد تعریف شده، این آخرین متد است که اجرا می‌گردد.


Operator Overloading
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

گونه‌ای از مفهوم چندریختی که در زبان برنامه‌نویسی پایتون پشتیبانی می‌شود، Operator Overloading می‌باشد که به انجام عملیات متفاوت با استفاده یک عملگر (Operator - درس ششم) یکسان اشاره دارد. برای مثال عملگر ``+`` هنگامی که به همراه دو شی ``int`` قرار بگیرد عمل جمع ریاضی (arithmetic addition) را بین آن دو به انجام می‌رساند ولی زمانی که با دو شی ``str`` قرار بگیرد، مقدار آن دو شی رشته را به یکدیگر پیوند می‌دهد (concatenate)::

    >>> a = 3
    >>> b = 'string'

    >>> a + a
    6
    >>> b + b
    'stringstring'


زیان برنامه‌نویسی پایتون این قابلیت را در اختیار برنامه‌نویس قرار می‌دهد که بتواند عملیات مورد نظر خود را برای اشیای خود در هنگام مواجه با عملگرها فراهم آورد. این کار با استفاده از پیاده‌سازی برخی متدهای خاص ممکن می‌شود و در ادامه به معرفی متدهای معادل چند عملگر پایتون می‌پردازیم. توجه داشته باشید که تعداد این متدهای بسیار بیشتر از این‌ها بوده و در ازای تمام عملگرهای ممکن، یک متد نظیر قابل پیاده‌سازی می‌باشد، برای مطالعه بیشتر می‌توانید به مستندات پایتون مراجعه نمایید:

.. container:: table-ltr

	===================  ===================================================================
	Binary Operators     Magic Metods
	===================  ===================================================================
	``+``                ``__add__(self, other)`` [`اسناد پایتون <https://docs.python.org/3/reference/datamodel.html#object.__add__>`__]
	``-``                ``__sub__(self, other)`` [`اسناد پایتون <https://docs.python.org/3/reference/datamodel.html#object.__sub__>`__]
	``*``                ``__mul__(self, other)`` [`اسناد پایتون <https://docs.python.org/3/reference/datamodel.html#object.__mul__>`__]
	``/``                ``__truediv__(self, other)`` [`اسناد پایتون <https://docs.python.org/3/reference/datamodel.html#object.__truediv__>`__]
	``//``               ``__floordiv__(self, other)`` [`اسناد پایتون <https://docs.python.org/3/reference/datamodel.html#object.__floordiv__>`__]
	``٪``                ``__mod__(self, other)`` [`اسناد پایتون <https://docs.python.org/3/reference/datamodel.html#object.__mod__>`__]
	``**``               ``__pow__(self, other)`` [`اسناد پایتون <https://docs.python.org/3/reference/datamodel.html#object.__pow__>`__]
	===================  ===================================================================

|

.. container:: table-ltr

	======================  ===================================================================
	Comparison Operators    Magic Metods
	======================  ===================================================================
	``<``                   ``__lt__(self, other)`` [`اسناد پایتون <https://docs.python.org/3/reference/datamodel.html#object.__lt__>`__]
	``>``                   ``__gt__(self, other)`` [`اسناد پایتون <https://docs.python.org/3/reference/datamodel.html#object.__gt__>`__]
	``<=``                  ``__le__(self, other)`` [`اسناد پایتون <https://docs.python.org/3/reference/datamodel.html#object.__le__>`__]
	``>=``                  ``__ge__(self, other)`` [`اسناد پایتون <https://docs.python.org/3/reference/datamodel.html#object.__ge__>`__]
	``==``                  ``__eq__(self, other)`` [`اسناد پایتون <https://docs.python.org/3/reference/datamodel.html#object.__eq__>`__]
	``!=``                  ``__ne__(self, other)`` [`اسناد پایتون <https://docs.python.org/3/reference/datamodel.html#object.__ne__>`__]
	======================  ===================================================================

|

|

به عنوان نمونه یک کلاس Number جدید می‌سازیم و عملگر ``+`` در آن پیاده‌سازی می‌کنیم:


.. code-block:: python
    :linenos:

    class Number:

        def __init__(self, number): 
            self.number = number 
   
        # adding two objects  
        def __add__(self, other_number): 
            return self.number + other_number.number 


    a = Number(5)
    b = Number(7)

    result = a + b

    print(f'{a.number } + {b.number } =', result)


::

    5 + 7 = 12


به عنوان مثالی دیگر، شخصی‌سازی سنجش برابر بودن دو شی:


.. code-block:: python
    :linenos:

    class Student:

        def __init__(self, name, score=0):
            self.name = name
            self.score = score

        def __eq__(self, other_student):
           return self.score == other_student.score


    a = Student('Saeid', 75)
    b = Student('Babak', 75)

    print(a == b)

::

    True

|

----

:emoji-size:`😊` امیدوارم مفید بوده باشه



