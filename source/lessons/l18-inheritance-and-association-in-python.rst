.. role:: emoji-size

.. meta::
   :description: پایتون به پارسی - کتاب آنلاین و آزاد آموزش زبان برنامه‌نویسی پایتون - درس هجدهم: شی گرایی (OOP) در پایتون: وراثت (Inheritance)، Association و Mixin


.. _lesson-18: 

درس ۱۸: شی گرایی (OOP) در پایتون: وراثت (Inheritance)، Association و Mixin
========================================================================================================

.. figure:: /_static/pages/18-python-object-oriented-programming-inheritance-mro-mixin.jpg
    :align: center
    :alt: شی گرایی (OOP) در پایتون: وراثت (Inheritance)، Association و Mixin
    :class: page-image

    Photo by `Vidar Nordli-Mathisen <https://unsplash.com/photos/s-vhziQHngM>`__

  
این درس در ادامه درس پیش می‌باشد و به بررسی رابطه بین کلاس‌ها و اشیا می‌پردازد. در درس پنجم مقدمه‌ای از این روابط صحبت شده است و این درس  به صورت کامل دو رابطه **IS-A** یا Inheritance و **HAS-A** یا Association در مفهموم شی گرایی و چگونگی پیاده‌سازی آن‌ها در زبان برنامه‌نویسی پایتون را شرح می‌دهد.

در این درس همچنین به شرح **وراثت چندگانه (Multiple Inheritance)**، **Method Resolution Order** و کلاس‌های **Mixin** در زبان برنامه‌نویسی پایتون خواهیم پرداخت.


:emoji-size:`✔` سطح: متوسط

----


.. contents:: سرفصل‌ها
    :depth: 2

----


وراثت (Inheritance)
----------------------------------

وراثت به معنی امکانی است که یک کلاس بتواند صفات و رفتارهای یک کلاس دیگر را نیز به همراه خود داشته باشد. پیاده‌سازی وراثت در پایتون حداقل به دو کلاس نیاز دارد:

* **base class** یا **superclass**: کلاس اصلی یا کلاسی می‌خواهیم کلاس یا کلاس‌های دیگری آن را به ارث ببرند و صفات و رفتارهای آن به دیگر کلاس(ها) سرایت پیدا کند.
* **derived class** یا **subclass**: کلاس یا کلاس‌هایی که از superclass ارث‌بری خواهند داشت.

.. image:: /_static/lessons/l18-python-oop-inheritance.jpg
    :align: center

تصویر بالا یک نمونه ساده از ساختار وراثت را نمایش می‌دهد. در برنامه ما قرار است یک کلاس گنجشک (Sparrow) و سگ (Dog) ایجاد گردد، از آنجا که برخی از رفتارهای این دو کلاس یکسان است مانند راه رفتن (Walk) یا نفس کشیدن (Breathe)، یک superclass کلاس برای آن‌ها با نام Animal ایجاد می‌کنیم که شامل صفات و رفتارهای مشترک دو کلاس نام برده باشد - پیاده‌سازی پایتونی تصویر بالا به صورت نمونه کد زیر خواهد بود:

.. code-block:: python
    :linenos:

    class Animal:

        def walk(self):
          print(f'{self.__class__.__name__}: walking...')
    
        def breathe(self):
          print(f'{self.__class__.__name__}: breathing...')
    
    
    class Sparrow(Animal):
    
        def fly(self):
          print(f'{self.__class__.__name__}: flying...')
    
    
    class Dog(Animal):
    
        def run(self):
          print(f'{self.__class__.__name__}: running...')
    
    
    sparrow = Sparrow()
    dog = Dog()
    
    sparrow.walk()
    sparrow.breathe()
    sparrow.fly()

    print('-' * 30)

    dog.walk()
    dog.breathe()
    dog.run()

::

    Sparrow: walking...
    Sparrow: breathing...
    Sparrow: flying...
    ------------------------------
    Dog: walking...
    Dog: breathing...
    Dog: running...

.. tip:: 

  همانطور که از نمونه کد بالا مشاهده می‌شود، زمانی که یک شی subclass، متد superclass خود را فراخوانی می‌کند، مقدار ``self`` در متد superclass برابر با شی فراخوانی کننده متد یعنی همان subclass خواهد بود. 

به صورت پیش‌فرض هر شی پایتون حاوی  Attributeها و متدهایی است که فهرست آن‌ها با استفاده از تابع ``dir`` [`اسناد پایتون <https://docs.python.org/3/library/functions.html#dir>`__] قابل مشاهده خواهد بود. با این توضیح صفت ``__self.__class``  حاوی کلاس شی می‌باشد و ``__self.__class__.__name`` نیز نام کلاس شی را در بر دارد - *این موضوع در درس‌های پیش نیز مطرح شده بود*::

    >>> class Sample:
    ...     def imethod(self):
    ...         print(dir(self))
    ...         print()
    ...         print(self.__class__)
    ... 
    >>> 
    >>> sample = Sample()
    >>> sample.imethod()
    ['__class__', '__delattr__', '__dict__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__le__', '__lt__', '__module__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', '__weakref__', 'imethod']

    <class '__main__.Sample'>
    >>> 

با این حال، برخی اشیا پایتون حاوی  Attributeهایی هستند که ممکن است توسط تابع ``dir``  نمایش داده نشود. از این  Attributeها به عنوان Special Attributes یاد می‌شود [`اسناد پایتون <https://docs.python.org/3/library/stdtypes.html#special-attributes>`__]. برای مثال صفت ``__definition.__name`` بسته به نوع definition، حاوی نام کلاس، تابع، متد یا غیره می‌باشد.

همان‌طور که بیان شد subclass‌ها به Attributeهای superclass کلاس خود نیز دسترسی دارند، به نمونه کدی دیگر نیز توجه نمایید:

.. code-block:: python
    :linenos:

    class SuperClass:
        super_class_attr = {'one':1, 'two':2}
    
        def __init__(self, param_1):
            self.super_instance_attr = param_1
    

    class SubClass(SuperClass):
        sub_class_attr = {'six':6, 'seven':7}
    
        def __init__(self, param_1, param_2):
            super().__init__(param_1)
            self.sub_instance_attr = param_2

        def sub_instance_method(self):
            print('Called: sub_instance_method')
            print(self.super_instance_attr)
            print(self.sub_instance_attr)
    
        @classmethod
        def sub_class_method(cls):
            print('Called: sub_class_method')
            print(cls.super_class_attr)
            print(cls.sub_class_attr)
    

    sub = SubClass('param_1', 'param_2')
    
    print(sub.super_instance_attr)
    print(sub.sub_instance_attr)
    print('-' * 30)
    print(SubClass.super_class_attr)
    print(SubClass.sub_class_attr)
    print('-' * 30)
    sub.sub_instance_method()
    print('-' * 30)
    SubClass.sub_class_method()

::

    param_1
    param_2
    ------------------------------
    {'one': 1, 'two': 2}
    {'six': 6, 'seven': 7}
    ------------------------------
    Called: sub_instance_method
    param_1
    param_2
    ------------------------------
    Called: sub_class_method
    {'one': 1, 'two': 2}
    {'six': 6, 'seven': 7}


.. tip:: 

  از درس پیش مفهوم سازنده (Constructor) در شی گرایی را بیاد داریم. چنانچه در superclass متدهای سازنده (``__new__`` و  ``__init__``) پیاده‌سازی شده باشند، می‌بایست این متدها در subclass‌ها نیز پیاده‌سازی شوند، نیازی نیست که سرآیند تعریف این دو متد با superclass یکسان باشد ولی می‌بایست مقادیر مورد نیاز متد superclass فراهم شود. برای این کار لازم است داخل متد subclassها به superclass دسترسی داشه باشیم، تابع ``super`` [`اسناد پایتون <https://docs.python.org/3/library/functions.html#super>`__] این امکان را فراهم می‌کند.

خروجی  تابع ``super`` [`اسناد پایتون <https://docs.python.org/3/library/functions.html#super>`__] شی است که نقش واسط را بین دو کلاس subclass و superclass دارد. نمونه کد زیر چگونگی فراخوانی انواع متدهای superclass را از subclass نمایش می‌دهد:


.. code-block:: python
    :linenos:

    class SuperClass:
    
        def super_instance_method(self):
            print('Called: super_instance_method')
            print(self)
    
        @classmethod
        def super_class_method(cls):
            print('Called: super_class_method')
            print(cls)

        @staticmethod
        def super_static_method():
            print('Called: super_static_method')
    

    class SubClass(SuperClass):
    
        def sub_instance_method(self):
            super().super_instance_method()
            super().super_class_method()
            SuperClass.super_static_method()
    
        @classmethod
        def sub_class_method(cls):
            super().super_class_method()
            SuperClass.super_static_method()

        @staticmethod
        def sub_static_method():
            SuperClass.super_static_method()
    

    sub = SubClass()
    
    sub.sub_instance_method()
    print('-' * 30)
    SubClass.sub_class_method()
    print('-' * 30)
    SubClass.sub_static_method()

::

    Called: super_instance_method
    <__main__.SubClass object at 0x7f9c77052898>
    Called: super_class_method
    <class '__main__.SubClass'>
    Called: super_static_method
    ------------------------------
    Called: super_class_method
    <class '__main__.SubClass'>
    Called: super_static_method
    ------------------------------
    Called: super_static_method

می‌دانیم که مفسر پایتون به صورت خودکار اطلاعات مربوط به شی فراخوانی کننده یک Instance Method را فراهم می‌آورد. زمانی که یک Instance Method از subclass فراخوانی می‌شود، تابع ``super`` می‌تواند آن شی و از طریق آن شی نیز به کلاس دسترسی داشته باشد بنابراین از داخل Instance Method کلاس subclass می‌توان به واسطه تابع ``super`` به هر دو نوع Instance Methodها و Class Methodهای superclass دسترسی پیدا کرد، چرا که تابع ``super`` می‌تواند مقادیر ``self``  و ``cls`` را به منظور فراخوانی متدهای متناظر superclass به دست آورد.

همچنین می‌دانیم که در فراخوانی Class Method، تنها اطلاعات مربوط به کلاس فراهم است و نه شی. زمانی که یک Class Method از subclass فراخوانی می‌شود، تابع ``super`` می‌تواند به کلاس مرتبط دسترسی داشته باشد بنابراین از داخل Class Method کلاس subclass تنها می‌توان به واسطه تابع ``super`` به Class Methodهای superclass دسترسی پیدا کرد، چرا که تابع ``super`` تنها می‌تواند مقدار ``cls`` را به منظور فراخوانی متدهای متناظر superclass به دست آورد.

در زمان فراخوانی Static Method نیز می‌دانیم که مفسر پایتون هیچ اطلاعاتی از شی و کلاس مرتبط را فراهم نمی‌آورد، بنابراین فراخوانی این متد با استفاده از تابع ``super`` انجام نمی‌پذیرد. در صورت نیاز به فراخوانی Static Methodهای کلاس superclass در کلاس subclass، همواره می‌توانید از نام کلاس superclass بهره بگیرید.


.. note:: 

  این برنامه‌نویس است که تصمیم می‌گیرد یک کلاس چگونه طراحی شود. اینکه کدام متد باید از کدام نوع باشد مسئله‌ای است که برنامه‌نویس باید در زمان طراحی کلاس خود به آن فکر کند و از امکانات زبان برنامه‌نویسی پایتون به درستی در جهت بهتر و راحت‌تر به انجام رساندن مسئله خود بهره بگیرد.


.. tip:: 

  هر شی از یک کلاس علاوه بر اینکه از نوع آن کلاس محسوب می‌شود، از نوع superclass نیز به حساب می‌آید. در واقع یک شی نوع subclass، نوع superclass را نیز به ارث می‌برد::

       >>> class SuperClass:
       ...     pass
       ... 
       >>> class SubClass(SuperClass):
       ...     pass
       ... 
       >>> sub = SubClass()
       >>> 
       >>> isinstance(sub, SubClass)
       True
       >>> isinstance(sub, SuperClass)
       True
       >>> isinstance(sub, object)
       True

  در واقع این نمایش رابطه **IS-A**  می‌باشد. توجه داشته باشید که این رابطه از پایین به بالا می‌باشد و برعکس آن صادق نیست. برای نمونه، مثال نخست را بیاد آورید. گنجشک (Sparrow) یک  Animal است ولی  Animal لزوما گنجشک نیست!

  تمام کلاس‌های پایتون به صورت ضمنی از کلاس ``object`` ارث‌بری دارند.

  



وراثت چندگانه (Multiple Inheritance)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

پایتون جزو معدود زبان‌های برنامه‌نویسی مدرنی است که از وراثت چندگانه پشتیبانی می‌کند، چیزی که در زبانی همچون Java نیز وجود ندارد. در واقع پیاده‌سازی وراثت چندگانه چالش‌هایی به همراه دارد، همانند Diamond Problem که در Java ترجیح داده شده است که از وراثت چندگانه پرهیز کند و نبود آن را با پیاده‌سازی مفهومی همچون Interface پوشش دهد [`ویکی‌پدیا <https://en.wikipedia.org/wiki/Interface_(Java)>`__]. 

فراموش نکنیم در پیاده‌سازی شی گرایی می‌بایست بنابر نیاز برنامه کدهای خود را به کوچک‌ترین واحدهای ممکن تقسیم کنیم و اینکه یک شی بتواند صفات و رفتارهای چندین کلاس را به همراه خود داشته باشد یک نیاز اساسی در شی گرایی است. این الزام فلسفه سادگی پایتون است که مانع از آن می‌شود تا مفاهیمی موازی درکنار هم ایجاد شوند - همانند Class و Interface - وراثت چندگانه راه حل ساده و منطقی زبان برنامه‌نویسی پایتون برای حل این مشکل است و این امکان را می‌دهد که یک کلاس بتواند بیش از یک superclass داشته باشد:
::

    >>> class SuperClassA:
    ...     pass
    ... 
    >>> class SuperClassB:
    ...     pass
    ... 
    >>> class SuperClassC:
    ...     pass
    ... 
    >>> class SubClass(SuperClassA, SuperClassB, SuperClassC):
    ...     pass
    ... 
    >>> sub = SubClass()
    >>> 
    >>> isinstance(sub, SubClass)
    True
    >>> isinstance(sub, SuperClassA)
    True
    >>> isinstance(sub, SuperClassB)
    True
    >>> isinstance(sub, SuperClassC)
    True    
    >>> isinstance(sub, object)
    True

نمونه کد بالا نمایش ساختار وراثت چندگانه در پایتون است که در آن کلاس SubClass به ترتیب از سه کلاس SuperClassA و SuperClassB و SuperClassC  ارث‌بری دارد. 

اکنون مهم‌ترین چالش چگونگی دسترسی به متدهای هر یک از این superclassها می‌باشد. تاکنون برای دسترسی به متدهای superclass از تابع  ``super``  استفاده می‌کردیم ولی حالا که صحبت از چندین superclass است، مثلا مقدارهی متد ``__init__`` (که در تمام superclassها با همین نام وجود دارد) توسط این تابع چگونه می‌تواند انجام شود؟ چگونه باید به پایتون بگوییم آرگومان‌هایی را که می‌خواهیم دقیقا به متد خاصی از superclass مورد نظر ارسال کند؟ البته نگران نباشید، پایتون مشکلی نخواهد داشت. در ادامه، حالات مختلف حل این مسئله را بررسی خواهیم کرد.

**شیوه یکم:** خیلی ساده، می‌توانیم اصلا از تابع ``super`` استفاده نکنیم و متدهای هر superclass را مستقیم با نام خودش فراخوانی کنیم که البته در این روش لازم است به ازای تمام پارامترهای متد superclass آرگومان متناظر را ارسال نماییم، از جمله برای ``self``:


.. code-block:: python
    :linenos:

    class SuperClassA:
        def __init__(self, param_0, param_3):  
            print('Called: SuperClassA.__init__()')
            self.param_0 = param_0
            self.param_3 = param_3
    
    
    class SuperClassB:
        def __init__(self, param_1):  
            print('Called: SuperClassB.__init__()')
            self.param_1 = param_1
    
    class SuperClassC:
        def __init__(self, param_2):  
            print('Called: SuperClassC.__init__()')
            self.param_2 = param_2
    
    
    class SubClass(SuperClassA, SuperClassB, SuperClassC):
        def __init__(self, param_0, param_1, param_2, param_3, param_4):  
            SuperClassA.__init__(self, param_0, param_3)
            SuperClassB.__init__(self, param_1)
            SuperClassC.__init__(self, param_2)
            self.param_4 = param_4
    
    
    sub = SubClass(0, 1, 2, 3, 4)
    
    print('param_0: ', sub.param_0)
    print('param_1: ', sub.param_1)
    print('param_2: ', sub.param_2)
    print('param_3: ', sub.param_3)
    print('param_4: ', sub.param_4)

::

    Called: SuperClassA.__init__()
    Called: SuperClassB.__init__()
    Called: SuperClassC.__init__()
    param_0:  0
    param_1:  1
    param_2:  2
    param_3:  3
    param_4:  4


    


**شیوه دوم:** رفتار تابع ``super`` را عمیق‌تر بشناسیم و درست از آن بهره بگیریم، برای این منظور می‌بایست شیوه پیمایش  superclassها و جستجو برای متد در تابع ``super`` پایتون را بشناسیم، این شیوه با نام  **Method Resolution Order** یا به اختصار **MRO** خوانده می‌شود.

**Method Resolution Order** ، همانطوری که از نام آن نیز مشخص است، **MRO** ترتیبی که می‌بایست بر اساس آن متدها جستجو شوند را پیدا می‌کند. پایتون برای این منظور از الگوریتم C3 linearization بهره گرفته است [`ویکی‌پدیا <https://en.wikipedia.org/wiki/C3_linearization>`__] (البته از نسخه 2.3 به بعد) [`اسناد پایتون <https://www.python.org/download/releases/2.3/mro>`__]. 

هر کلاس پایتون یک Special Attribute به اسم ``__mro__`` دارد که حاوی یک توپِل از ترتیب کلاس‌هایی است که پایتون بر اساس آن به دنبال یک متد می‌گردد [`اسناد پایتون <https://docs.python.org/3/library/stdtypes.html#class.__mro__>`__]، در واقع این مقدار حاصل تلاش MRO بر اساس محاسبه الگوریتم C3 linearization برای آن کلاس خواهد بود. برای مثال این مقدار برای کلاس ``SubClass`` ما برابر است با::


   >>> SubClass.__mro__
   (<class '__main__.SubClass'>, <class '__main__.SuperClassA'>, <class '__main__.SuperClassB'>, <class '__main__.SuperClassC'>, <class 'object'>)

همانطور که مقدار ``__mro__``  برای کلاس ``SubClass``  مشخص کرده است، پایتون برای جستجوی یک متد ابتدا داخل خود کلاس SubClass را بررسی و سپس شروع به پیمایش  superclassهای آن با ترتیب  SuperClassA و بعد SuperClassB و بعد SuperClassC می‌کند. آخرین کلاس همواره کلاس object می‌باشد، این کلاسی است که تمام کلاس‌های پایتون به صورت ضمنی و پیش‌فرض از آن ارث‌بری دارند و در یک سلسله مراتب وراثت بالاترین سطح وراثت می‌باشد. اکنون بر اساس این آگاهی می‌توانیم به شیوه زیر عمل کنیم:

.. code-block:: python
    :linenos:

    class SuperClassA:
        def __init__(self, param_0, param_3, *args):  
            print('Called: SuperClassA.__init__()')
            super().__init__(*args)
            self.param_0 = param_0
            self.param_3 = param_3
    
    
    class SuperClassB:
        def __init__(self, param_1, *args):  
            print('Called: SuperClassB.__init__()')
            super().__init__(*args)
            self.param_1 = param_1
    
    class SuperClassC:
        def __init__(self, param_2, *args): 
            print('Called: SuperClassC.__init__()')
            super().__init__(*args)
            self.param_2 = param_2
    
    
    class SubClass(SuperClassA, SuperClassB, SuperClassC):
        def __init__(self, param_0, param_1, param_2, param_3, param_4):  
            super().__init__(param_0, param_3, param_1, param_2)
            self.param_4 = param_4
    
    
    sub = SubClass(0, 1, 2, 3, 4)

همانطور که در نمونه کد بالا مشخص است متد SubClass تنها شامل یکبار فراخوانی تابع ``super`` است و از طرفی هم تمام متدهای متناظر در superclassهای آن نیز شامل فراخوانی تابع ``super`` هستند.

با آگاهی از حاصل MRO و  ترتیب پیمایش superclassها، متد مورد نظر  (در اینجا: ``__init__``) را هنگام فراخوانی ``super`` مقداردهی می‌کنیم. یعنی ارسال آرگومان‌ها را به ترتیبی قرار می‌دهیم که ابتدا قرار است متد متناظر در کلاس SuperClassA پیدا، فراخوانی و پارامترهای آن مقداردهی شود، سپس SuperClassB و در نهایت SuperClassC. (سطر ۲۴)

در این شیوه می‌بایست هر یک از متدهای متناظر در superclassها با متد مورد نظر ما در SubClass، نیز شامل فراخوانی تابع ``super`` باشند. چرا پایتون با اولین نتیجه موفق از یافتن متد، پیمایش را متوقف می‌کند ولی ما می‌خواهیم دیگر متدهای متناظر باقی‌مانده نیز فراخوانی شوند. در نتیجه با فراخوانی مجدد ``super`` این روند را دوباره به اجرا در می‌آوریم.

متدها در کلاس از قوانین حاکم بر تابع در پایتون پیروی می‌کنند، در نتیجه متدهای متناظر در superclassها باید به گونه‌ای تعریف شده باشند که هر تعداد پارامتر را بپذیرند. برای این منظور در انتهای تعریف پارامترهای این متدها، یک پارامتر ``args*`` قرار داده‌ایم. این پارامتر، تمامی آرگومان‌های اضافی ارسال شده به آن تابع را در خود نگه‌داری می‌کند. در نتیجه برای ادامه روند فراخوانی متدهای نظیر باقی‌مانده، تنها کافی است این مقدار ارسال گردد. (تابع در پایتون - درس دوازدهم)



اگر شیوه ارسال آرگومان‌ها را به صورت **نام=مقدار** تغییر دهیم، ترتیب ارسال آرگومان‌ها از اهمیت می‌افتد و پیاده‌سازی آسان‌تر و کد خواناتر خواهد بود - با این روش چنانچه متدهای مورد نظر در superclasها پارامتر همنام نداشته باشند، حتی ترتیب MRO نیز دیگر اهمیت نخواهد داشت:

.. code-block:: python
    :linenos:

    class SuperClassA:
        def __init__(self, param_0, param_3, **kargs):  
            print('Called: SuperClassA.__init__()')
            super().__init__(**kargs)
            self.param_0 = param_0
            self.param_3 = param_3
    
    
    class SuperClassB:
        def __init__(self, param_1, **kargs):  
            print('Called: SuperClassB.__init__()')
            super().__init__(**kargs)
            self.param_1 = param_1
    
    class SuperClassC:
        def __init__(self, param_2, **kargs): 
            print('Called: SuperClassC.__init__()')
            super().__init__(**kargs)
            self.param_2 = param_2
    
    
    class SubClass(SuperClassA, SuperClassB, SuperClassC):
        def __init__(self, p0, p1, p2, p3, p4):  
            super().__init__(param_0=p0, param_1=p1, param_2=p2, param_3=p3)
            self.param_4 = p4
    
    
    sub = SubClass(0, 1, 2, 3, 4)


.. note:: 

  آنچه در مثال بررسی شد حالتی پیچیده از فراخوانی متد مهم ``__init__`` بود. همواره زمانی که از وراثت چندگانه بهره می‌برید، در زمان فراخوانی یک متد که در دو یا چند superclass مشترک است، می‌بایست به یکی از شیوه‌های ارائه شده عمل نمایید.



Method Resolution Order
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

در این بخش به شرح چگونگی عملکرد **Method Resolution Order** پایتون و محاسبه الگوریتم C3 linearization خواهیم پرداخت. توجه داشته باشید مطالعه این بخش الزامی نیست و در هر زمان شما با استفاده از ``__Class.__mro``  می‌توانید به مقصود دست پیدا کنید!

برای شروع لازم است قوانین زیر را در نظر داشته باشیم (توجه: در ادامه برای ساده‌سازی توضیحات از ذکر حضور کلاس ``object`` صرف‌نظر شده است!):

۱) حاصل الگوریتم C3 linearization برای یک کلاس که superclass ندارد برابر با همان کلاس خواهد بود::

       >>> class A: pass

       >>> A.__mro__
       ( <class '__main__.A'>, <class 'object'>)

۲) چنانچه کلاس مورد نظر تنها شامل یک سطح از سلسله مراتب وراثت می‌باشد، حاصل الگوریتم C3 linearization برای آن کلاس برابر است با لیستی از خود آن کلاس و  superclassهای آن کلاس به ترتیبی که قرار گرفته‌اند (از چپ به راست)::
    
       >>> class A: pass
       >>> class B: pass
       >>> class C(B, A): pass

       >>> C.__mro__
       (<class '__main__.C'>, <class '__main__.B'>, <class '__main__.A'>, <class 'object'>)


۳) محاسبه حاصل الگوریتم C3 linearization برای یک کلاس که بیش از یک سطح سلسله مراتب وراثت دارد کمی زحمت دارد! در حالت کلی این مقدار برابر است با: «لیستی تک عضوی شامل آن کلاس » ``+`` لیستی با اعضای منحصر به فرد که حاصل ادغام (merge) «نتیجه خطی شدن (linearization) تک تک superclassهای آن کلاس» و «لیستی از  superclassهای آن کلاس». عمل ادغام در اینجا علاوه بر اینکه تکرارپذیر می‌باشد نکاتی دارد که در ادامه ذکر خواهد شد .

اکنون برای پی بردن به چگونگی ایجاد حاصل ``__Class.__mro`` و درک عملکرد الگوریتم C3 linearization دو مثال معروف در این زمینه را بررسی خواهیم کرد. نخست ساختار الماس (Diamond):

.. image:: /_static/lessons/l18-python-mro-diamond.png
    :align: center

.. code-block:: python
    :linenos:

    class A: pass
    class B(A): pass
    class C(A): pass
    class D(B, C): pass

    print (D.__mro__)

::

    (<class '__main__.D'>, <class '__main__.B'>, <class '__main__.C'>, <class '__main__.A'>, <class 'object'>)

روند محاسبه الگوریتم C3 linearization برای کلاس ``D`` این مثال به صورت زیر می‌باشد:

.. code-block:: python
    :linenos:

    L(A) := [A]

    L(B) := [B] + merge(L(A), [A])
          = [B] + merge([A], [A])
          = [B, A]

    L(C) := [C] + merge(L(A), [A])
          = [C] + merge([A], [A])
          = [C, A]

    L(D) := [D] + merge(L(B), L(C), [B, C])
          = [D] + merge([B, A], [C, A], [B, C])
          = [D, B] + merge([A], [C, A], [C])
          = [D, B, C] + merge([A], [A], [])
          = [D, B, C, A]


* **سطر ۱:** حاصل خطی سازی (linearization) کلاس A یا همان L(A) برابر است با لیستی که تنها شامل همان کلاس A است چرا که کلاس A بدون superclass است.

* **سطر ۳:** حاصل خطی سازی (linearization) کلاس B یا همان L(B) برابر است با «لیستی که تنها شامل همان کلاس B» ``+`` ادغام «حاصل خطی سازی (linearization) تک تک superclassهای کلاس B - در اینجا: L(A)» و لیستی از superclassهای کلاس  B - در اینجا: [A]

* **سطر ۴:** حاصل L(A) جایگذاری شده است. 

* **سطر ۵:** حاصل ادغام چند لیست که تنها شامل یک کلاس می‌باشند برابر است با آن کلاس: ``[A] + [B] = [B,A]``

* **سطر ۷:** حاصل خطی سازی (linearization) کلاس C همانند کلاس  B می‌باشد.

* **سطر ۱۱:** حاصل خطی سازی (linearization) کلاس D یا همان L(D) برابر است با «لیستی که تنها شامل همان کلاس D» ``+`` ادغام «حاصل خطی سازی (linearization) تک تک superclassهای کلاس D با حفظ ترتیب از چپ به راست - در اینجا: L(B) , L(C)» و لیستی از superclassهای کلاس  D  با حفظ ترتیب از چپ به راست - در اینجا: [B,C]

* **سطر ۱۲:** حاصل L(B) و L(C) جایگذاری شده است. 

* **سطر ۱۳:** اکنون عملیات ادغام شامل بیش از یک کلاس است، در این شرایط عملیات ادغام و انتخاب یک کلاس مطلوب آنقدر تکرار می‌شود تا دیگر کلاسی باقی نماند. فرآیند انتخاب کلاس مطلوب به این صورت است که از چپ‌ترین کلاس موجود در چپ‌ترین لیست شروع می‌کنیم به انتخاب، این کلاس می‌بایست در باقی لیست‌ها در صورت وجود چپ‌ترین عضو باشد، در غیر این صورت چپ‌ترین کلاس موجود در لیست بعدی انتخاب و بررسی خواهد شد. چنانچه کلاس انتخاب شده شرایط را دارا باشد عمل ادغام برای آن کلاس صورت می‌پذیرد و داخل تمام لیست‌ها در صورت وجود نیز حذف می‌گردد. در اینجا: ابتدا کلاس B انتخاب می‌شود، این کلاس شرایط مطلوب بودن را دارا می‌باشد، در نتیجه عمل ادغام برای آن به انجام می‌رسد.

* **سطر ۱۴:** در ادامه عمل ادغام فرآیند خطی سازی برای کلاس D، این بار ابتدا کلاس A انتخاب می‌شود، این کلاس شرایط لازم را ندارد چرا که در جایگاهی از لیست دوم نیز حضور دارد که جایگاه نخست (چپ‌ترین) نیست. کلاس A رها می‌شود و به سراغ لیست دوم می‌رویم، نخستین عضو آن یعنی کلاس C شرایط لازم برای ادغام را دارد، در نتیجه در این مرحله عمل ادغام برای کلاس C به انجام می‌رسد.

* **سطر ۱۵:** حاصل ادغام چند لیست که تنها شامل یک کلاس می‌باشند برابر است با همان کلاس، در نتیجه کلاس A انتخاب و عمل ادغام برای آن به انجام می‌رسد.

عملیات با موفقیت به پایان رسید و ما به مقداری برابر با ``__D.__mro`` دست پیدا کردیم!

|

اکنون مثال پیچیده‌تری را بررسی می‌کنیم. برگرفته شده از [`اینجا <https://www.wikiwand.com/en/C3_linearization>`__]  و [`اینجا <https://www.python.org/download/releases/2.3/mro>`__] :

.. image:: /_static/lessons/l18-python-mro-example.png
    :align: center

.. code-block:: python
    :linenos:

    class O: pass
    
    class A(O): pass
    class B(O): pass
    class C(O): pass
    class D(O): pass
    class E(O): pass
    
    class K1(A,B,C): pass
    class K2(D,B,E): pass
    class K3(D,A): pass
    
    class Z(K1,K2,K3): pass
    
    print (Z.__mro__)

::

    (<class '__main__.Z'>, <class '__main__.K1'>, <class '__main__.K2'>, <class '__main__.K3'>, <class '__main__.D'>, <class '__main__.A'>, <class '__main__.B'>, <class '__main__.C'>, <class '__main__.E'>, <class '__main__.O'>, <class 'object'>)

.. code-block:: python
    :linenos:

    L(O)  := [O]                                                # the linearization of O is trivially the singleton list [O], because O has no parents
    
    L(A)  := [A] + merge(L(O), [O])                             # the linearization of A is A plus the merge of its parents' linearizations with the list of parents...
           = [A] + merge([O], [O])
           = [A, O]                                             # ...which simply prepends A to its single parent's linearization
    
    L(B)  := [B, O]                                             # linearizations of B, C, D and E are computed similar to that of A
    L(C)  := [C, O]
    L(D)  := [D, O]
    L(E)  := [E, O]
    
    L(K1) := [K1] + merge(L(A), L(B), L(C), [A, B, C])          # first, find the linearizations of K1's parents, L(A), L(B), and L(C), and merge them with the parent list [A, B, C]
           = [K1] + merge([A, O], [B, O], [C, O], [A, B, C])    # class A is a good candidate for the first merge step, because it only appears as the head of the first and last lists
           = [K1, A] + merge([O], [B, O], [C, O], [B, C])       # class O is not a good candidate for the next merge step, because it also appears in the tails of list 2 and 3; but class B is a good candidate
           = [K1, A, B] + merge([O], [O], [C, O], [C])          # class C is a good candidate; class O still appears in the tail of list 3
           = [K1, A, B, C] + merge([O], [O], [O])               # finally, class O is a valid candidate, which also exhausts all remaining lists
           = [K1, A, B, C, O]
    
    L(K2) := [K2] + merge(L(D), L(B), L(E), [D, B, E])
           = [K2] + merge([D, O], [B, O], [E, O], [D, B, E])    # select D
           = [K2, D] + merge([O], [B, O], [E, O], [B, E])       # fail O, select B
           = [K2, D, B] + merge([O], [O], [E, O], [E])          # fail O, select E
           = [K2, D, B, E] + merge([O], [O], [O])               # select O
           = [K2, D, B, E, O]
    
    L(K3) := [K3] + merge(L(D), L(A), [D, A])
           = [K3] + merge([D, O], [A, O], [D, A])               # select D
           = [K3, D] + merge([O], [A, O], [A])                  # fail O, select A
           = [K3, D, A] + merge([O], [O])                       # select O
           = [K3, D, A, O]
    
    L(Z)  := [Z] + merge(L(K1), L(K2), L(K3), [K1, K2, K3])
           = [Z] + merge([K1, A, B, C, O], [K2, D, B, E, O], [K3, D, A, O], [K1, K2, K3])    # select K1
           = [Z, K1] + merge([A, B, C, O], [K2, D, B, E, O], [K3, D, A, O], [K2, K3])        # fail A, select K2
           = [Z, K1, K2] + merge([A, B, C, O], [D, B, E, O], [K3, D, A, O], [K3])            # fail A, fail D, select K3
           = [Z, K1, K2, K3] + merge([A, B, C, O], [D, B, E, O], [D, A, O])                  # fail A, select D
           = [Z, K1, K2, K3, D] + merge([A, B, C, O], [B, E, O], [A, O])                     # select A
           = [Z, K1, K2, K3, D, A] + merge([B, C, O], [B, E, O], [O])                        # select B
           = [Z, K1, K2, K3, D, A, B] + merge([C, O], [E, O], [O])                           # select C
           = [Z, K1, K2, K3, D, A, B, C] + merge([O], [E, O], [O])                           # fail O, select E
           = [Z, K1, K2, K3, D, A, B, C, E] + merge([O], [O], [O])                           # select O
           = [Z, K1, K2, K3, D, A, B, C, E, O]                                               # done



  
انجمن (Association)
----------------------------------

وراثت مفهوم پرکاربردی از شی‌گرایی است ولی همانطور که مشاهده خواهید کرد، همیشه تمام روابط را نمی‌توان اینگونه تعریف کرد. با تعریف و چگونگی پیاده‌سازی رابطه **IS-A** در شی گرایی پایتون آشنا شده‌ایم، اکنون می‌بایست با مفهوم رابطه **HAS-A** در شی گرایی آشنا شویم. 


رابطه **HAS-A** زمانی پیش می‌آید که یک شی حاوی یک یا چند شی دیگر باشد. در این رابطه برخلاف آنچه در وراثت (**IS-A**) شاهد آن بودیم، یک شی از طریق یکی شدن با دیگران گسترش پیدا نمی‌کند - بلکه با مالک شدن اشیای دیگر گسترش می‌یابد که در شی گرایی با عنوان **Association** شناخته می‌شود و بر اساس شدت مالکیت، Association بر دو نوع قابل تقسیم است:

* Composition
* Aggregation


توجه داشته باشید که پیاده‌سازی این نوع رابطه (HAS-A) هیچ نکته خاص پایتونی ندارد، تنها تعریف Attribute برای شی است. آنچه مورد تاکید است مفهوم این موارد در برنامه‌نویسی شی گراست که درک آن‌ها خالی از لطف نمی‌باشد. برنامه‌نویس باید بتواند بر اساس مسئله، کلاس‌های خود و روابط بین آن‌ها را به بهترین شکل ممکن طراحی کند، دانستن این موارد به این امر کمک خواهند کرد.


Composition
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

در **Composition** یک شی بخشی از شی دیگر خواهد بود به صورتی که در حالت تکی مفهومی در برنامه نخواهد داشت و تنها جزیی از شی دیگر بودن است که به آن در برنامه مفهوم می‌بخشد. برای مثال رابطه  شی بازو (Arm) و  پا (Leg) با شی انسان (Human) از این نوع است. شی Arm تنها در داخل شی Human مفهوم و کاربرد دارد. در واقع شی Arm یا Leg تنها برای استفاده در شی Human ایجاد گردیده‌اند و با از بین رفتن شی Human، شی Arm و  Leg نیز از بین می‌روند. 

.. code-block:: python
    :linenos:	
    
    class Arm:
    	pass
    	
    class Leg:
    	pass

    class Human:
    	def __init__(self):
    	    self.arm = Arm()
    	    self.leg = Leg()
    
    human = Human()

از لحاظ منطقی اگر نگاه کنیم، شی Human برای داشتن قابلیت‌های بازو (Arm) و  پا (Leg)، نباید از کلاس‌های مربوط به آن‌ها ارث‌بری داشته باشد. چراکه نمی‌توانیم بگوییم یک Human، یک Arm است (IS-A) ولی می‌توانیم بگوییم که یک Human، یک Arm دارد (HAS-A).
    	

معمولا در پیاده‌سازی Composition، اشیای مورد نیاز یک شی در داخل آن شی ایجاد می‌گردند. چرا که این اشیا در بیرون از کلاس مورد نظر کاربردی نخواهند داشت و می‌بایست با از بین رفتن شی مالک (در اینجا: Human)، آن‌ها نیز از بین بروند.


Aggregation
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

در تعریف **Aggregation** یک شی بخشی از شی دیگر می‌شود ولی به صورت مستقل نیز می‌تواند در برنامه حضور داشته باشد و طول عمر (life cycle) آن‌ها وابسته به یکدیگر نیست. برای مثال رابطه دانش‌آموز (Student) و مدرسه (School) می‌تواند از این نوع در نظر گرفته شود، وقتی School تعطیل می‌شود - Student هنوز وجود دارد. 


.. code-block:: python
    :linenos:	
    
    class Student:
    	pass

    class School:
    	def __init__(self, students):
    		self.students = students
    
    students = [Student(), Student(), Student()]
    
    school = School(students)


معمولا در پیاده‌سازی Aggregation، اشیای مورد نیاز یک شی در زمان نمونه‌سازی به آن ارسال می‌گردند. چرا که این اشیا در بیرون از کلاس موجودیت‌های مستقلی هستند و طول عمر (life cycle) آن‌ها وابسته به شی مالک (در اینجا: School) نیست.



Mixin
----------------------------------

همواره در میان صحبت از شی گرایی، کلاس و وراثت از مفهومی با نام **Mixin** نیز یاد می‌شود [`ویکی‌پدیا <https://en.wikipedia.org/wiki/Mixin>`__]. Mixin نوعی استفاده از مفهوم کلاس و وراثت می‌باشد ولی با هدفی دیگر، Mixin کلاسی است که معمولا تنها شامل متد بوده و با هدف گسترش عملکردهای (functionality) دیگر کلاس‌ها توسعه می‌یابد.

پشتیبانی وراثت چندگانه در زبان برنامه‌نویسی پایتون، پیاده‌سازی Mixin را بسیار ساده کرده است. Mixin مجموعه‌ای از functionalityهاست که هر کلاسی که با آن functionalityها نیاز داشته باشد، می‌تواند از آن Mixin ارث‌بری داشته باشد. البته باید توجه داشت که منطق پیاده‌سازی Mixin ایجاد رابطه IS-A نمی‌باشد و هدف تنها گسترش functionality است، حتی اگر ظاهر کار چنین نباشد!

زمانی را تصور کنید که قصد دارید یک عملکرد یا متد یا functionality جدید را به مجموعه‌ای از کلاس‌های خود اضافه نمایید. در این شرایط چه کار باید کرد؟ این functionality را به بالاترین supperclass هر سلسله مراتب از کلاس‌های خود اضافه کنیم،‌ در این صورت علاه‌بر اینکه کلاس‌های پایه خود را به آسانی دستکاری کرده‌اید، یک کد یکسان را نیز چندین بار تکرار کرده‌اید که بر خلاف یکی از مهم‌ترین اصول برنامه نویسی است (DRY - Don't Repeat Yourself) [`ویکی‌پدیا <https://en.wikipedia.org/wiki/Don%27t_repeat_yourself>`__]. پاسخ این مشکلات ایجاد Mixin است.

توجه داشته باشید که Mixin یک الگو طراحی است و نه یک ابزار، بنابراین پیاده‌سازی آن در زبان برنامه‌نویسی پایتون همانند ایجاد هر کلاس دیگری است منتها مرسوم است که به منظور تفکیک این دست از کلاس‌ها، در انتهای نام آن‌ها Mixin ذکر می‌گردد. چرا که Mixin کلاس‌هایی هستند که نباید از آن‌ها شی ایجاد شود و تنها دلیل موجودیت آن‌ها گسترش functionality است:


.. code-block:: python
    :linenos:	

    class Vehicle:

        def travel(self):
            pass


    class Car(Vehicle):
        pass

    class Boat(Vehicle):
        pass

    class Plane(Vehicle):
        pass


نمونه کد بالا نمایش کلاس مربوط به سه نوع وسیله نقلیه (Vehicle) می‌باشد: خودرو (Car)، قایق (Boat) و هواپیما (Plane)، اکنون می‌خواهیم قابلیت پخش رادیو را به دو کلاس Car و Boat اضافه نماییم. اگر این functionality را به کلاس پایه (Vehicle) اضافه کنیم، در این صورت کلاس Plane نیز ناخواسته به این عملکرد دسترسی پیدا خواهد کرد که نه درست است و نه مطلوب. از طرفی نیز قرار دادن این عملکرد به صورت جداگانه در هر دو کلاس Car و Boat برخلاف DRY می‌باشد. برای این مسئله از الگو Mixin استفاده خواهیم کرد:


.. code-block:: python
    :linenos:	

    class RadioMixin:
        def __init__(self):
            self.radio = Radio()

        def play_on_station(self, station):
            self.radio.set_station(station)
            self.radio.play_song()


    class Car(Vehicle, RadioMixin):
        def __init__(self):
            RadioMixin.__init__(self)

    class Boat(Vehicle, RadioMixin):
        def __init__(self):
            RadioMixin.__init__(self)



|

----

:emoji-size:`😊` امیدوارم مفید بوده باشه




