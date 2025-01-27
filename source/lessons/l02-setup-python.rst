.. role:: emoji-size

.. meta::
   :description: پایتون به پارسی - کتاب آنلاین و آزاد آموزش زبان برنامه‌نویسی پایتون - درس دوم: نصب و راه‌اندازی پایتون
   :keywords: پایتون, آموزش برنامه نویسی, آموزش پایتون, نصب پایتون در ویندوز, نصب پایتون در لینوکس, سیستم مدیریت بسته پایتون, pip, راه اندازی پایتون, دانلود پایتون, آموزش pip


.. _lesson-02: 

درس ۰۲: نصب و راه‌اندازی پایتون
================================

.. figure:: /_static/pages/02-python-setup.jpg
    :align: center
    :alt: نصب و راه‌اندازی پایتون
    :class: page-image

    Photo by `Bermix Studio <https://unsplash.com/photos/8tQ7rBFgPu8>`__


در این درس به چگونگی نصب و راه‌اندازی محیط اجرای زبان برنامه‌نویسی پایتون در دو سیستم عامل ویندوز و گنولینوکس پرداخته و در پایان نیز توضیح مختصری از «سیستم مدیریت بسته» پایتون ارایه شده است.

:emoji-size:`✔` سطح: پایه

----

.. contents:: سرفصل‌ها
    :depth: 2

----

.. _download: 

دانلود
--------
برای ترجمه و اجرای سورس کد ایجاد شده به زبان پایتون لازم است «بسته نصبی پایتون» (همان CPython یا اگر ساده بگوییم: پایتون) که شامل مفسر، کتابخانه استاندارد، برنامه `IDLE <http://en.wikipedia.org/wiki/IDLE_%28Python%29>`_ (ویرایشگر پیش‌فرض پایتون) و... است را دانلود و بر روی سیستم عامل نصب نماییم.

.. note::
    پایتون (Python) نام یک زبان برنامه‌نویسی است. به این معنی که ساختاری برای بیان ایده‌ها یا فرمان‌های برنامه‌نویس به ماشین می‌باشد. برای فهماندن فرمان‌های پایتونی به ماشین، مانند هر زبان سطح بالای دیگری نیاز به یک «پردازنده‌ زبان» (Language Processor) است. برنامه‌ CPython پردازنده استاندارد زبان پایتون است که از آن به صورت ساده با همان نام Python یاد می‌شود.

در هر سیستم عاملی ممکن است پایتون از پیش نصب باشد (خصوصا در گنولینوکس) که در صورت رضایت از نسخه‌ موجود، دیگر لزومی به دانلود و نصب آن نخواهد بود؛ برای آگاهی یافتن از این موضوع می‌توانید دستور ``python --version`` را در خط فرمان سیستم عامل وارد نمایید؛ البته در ویندوز به دلیل اینکه عملکرد این دستور وابسته به دستکاری متغیر Path می‌باشد، بهتر است از مسیر Control Panel >‌ Programs and Features اقدام نمایید.

.. tip::
    `Path <http://en.wikipedia.org/wiki/PATH_(variable)>`_ یکی از «متغیر‌های محیطی» (`Environment Variables <http://en.wikipedia.org/wiki/Environment_variable>`_) سیستم عامل است. این متغیر حاوی فهرست دایرکتوری‌هایی می‌باشد که سیستم عامل در آن‌ها به دنبال یک فایل اجرایی هم نام با دستور وارد شده در خط فرمان می‌گردد.


هم اکنون این بسته  از `صفحه‌ دانلود آن <http://www.python.org/downloads>`_، متناسب با نوع سیستم عامل و معماری پردازنده قابل دانلود است (اندازه:‌ تقریبا ۲۵ مگابایت) که برای نصب در ویندوز به شکل یک `فایل نصبی <http://www.python.org/downloads/windows/>`_ (با قالب msi) و متناسب با دو معماری 32 (x86) و 64 (AMD64 ،EM64T ،x64 ،x86-64) بیتی منتشر می‌گردد و در صورت نیاز برای نصب آن در گنو‌لینوکس می‌بایست `سورس کد آن <http://www.python.org/downloads/source/>`_ (که به زبان C است) را دانلود نمایید.

.. note::
    امکان نصب نسخه‌های متفاوت پایتون در کنار یکدیگر وجود دارد.

در هنگام آخرین ویرایش  این درس نسخه‌  3.11.2 جدیدترین نسخه‌ منتشر یافته‌ پایتون است. برای دسترسی به جدید‌ترین ویژگی‌ها، پیشنهاد می‌شود همیشه جدیدترین نسخه‌ موجود از پایتون را دانلود نمایید.

**با توجه به اینکه تغییر ملموسی در شیوه نصب پایتون ایجاد نگردیده، از بروزرسانی تصاویر و دستورات صرف نظر گردیده است.**


.. _setup-on-windows: 

نصب در ویندوز
---------------
درست به مانند هر برنامه‌ دیگری در ویندوز، نصب به راحتی تنها با چند بار کلیک بر روی دکمه‌ Next به پایان می‌رسد. پیشنهاد می‌شود مسیر پیش‌فرض نصب (مثلا برای نصب نسخه‌ 3.4.2:‌ ``C:\Python34``) را تغییر ندهید.  قابل ذکر است که در نسخه‌های جدیدتر این مسیر به داخل فولدر Program Files انتقال یافته است، برای مثال برای نسخه 3.11.2 مسیر پیش‌فرض نصب ``C:\Program Files\Python311`` خواهد بود.

در هنگام نصب نسخه‌‌ای که قصد دارید از آن به صورت نسخه‌ پیش‌فرض پایتون خود استفاده نمایید، به این نکته توجه داشته باشید که در مرحله‌ سفارشی‌سازی (Customize) گزینه‌ افزودن خودکار مسیر مفسر پایتون به متغیر Path ویندوز را فعال نمایید (همانند تصویر پایین). در این صورت با وارد کردن دستور ``python`` در خط فرمان ویندوز، مفسر پایتون (این نسخه) فراخوانی می‌شود. برای شروع، با وارد کردن دستور ``python –V`` یا ``python --version`` می‌توانید از نسخه‌ پایتون نصب شده آگاهی یابید:

.. image:: /_static/lessons/l02-install-python-on-windows.png
    :align: center

.. code::

    > python --version
    python 3.4.2

در صورت تنظیم نبودن متغیر Path می‌بایست از طریق پنجره خط فرمان و دستور ``cd`` به مسیر نصب مفسر پایتون وارد شوید:


.. code::

    > python --version
    'python' is not recognized as an internal or external command, operable program or batch file.

    > cd C:\Python34\

    > python --version
    python 3.4.2

البته امکان دستکاری Path در هر زمانی وجود دارد:

مسیر Control Panel > System > Advanced system settings > Advanced را طی کرده و سپس با کلیک بر روی Environment Variables پنجره‌ جدیدی باز می‌گردد که در قسمت System variables آن Path را پیدا و انتخاب نمایید. بر روی Edit در پایین همان پنجره کلیک کرده و عبارت زیر (برای نسخه 3.4) را به ابتدای متن موجود در قسمت Variable value پنجره‌ جدید وارد و سپس بر روی دکمه‌ OK کلیک نمایید. :)

::

        C:\Python34;C:\Python34\Scripts;


.. image:: /_static/lessons/l02-add-path-on-windows.png
    :align: center

.. caution::
    در ویندوز از کاراکتر نقطه‌ ویرگول (سمی‌کالن Semicolon) یا ``;`` برای جدا‌سازی مسیر دایرکتوری‌ها در متغیر path استفاده می‌گردد. ``C:\Python34`` از عبارت یاد شده، مشخص کننده‌‌ مسیر مفسر پایتون (python.exe) است و با توجه به افزوده شدن pip (سیستم مدیریت بسته‌‌ پایتون) به بسته نصبی پایتون از نسخه‌ 3.4 به بعد، ``C:\Python34\Scripts`` نیز به منظور ایجاد امکان دسترسی و فراخوانی آن (pip.exe یا pip3.exe یا pip3.4.exe - فرقی ندارند) افزوده می‌شود.

اگر بخواهیم بر روی یک ویندوز از هر دو شاخه پایتون نسخه‌ای یا حتی چند نسخه‌ متفاوت از هر یک به مانند نسخه‌های 2.6، 2.7، 3.3 و 3.4 را نصب نماییم؛ یک راه خوب برای فراخوانی مفسر پایتونِ هر یک از آن‌ها در خط فرمان ویندوز، استفاده از راه‌انداز پایتون (Python Launcher) می‌باشد. در این راهکار نیازی به دستکاری متغیر Path نیست و از دستور ``py`` به جای دستور ``python`` استفاده می‌شود؛ به این صورت که با دستور ``py`` یا ``py -2`` مفسر آخرین نسخه‌ موجود از شاخه 2x پایتون و با دستور ``py -3`` نیز مفسر آخرین نسخه‌ موجود از شاخه 3x پایتون فراخوانی می‌گردد. برای فراخوانی مفسر دیگر نسخه‌ها هم می‌بایست نسخه‌ آن‌ها به صراحت ذکر گردد (مانند: ``py -3.3`` و ``py -2.6``):

.. code::

    > py --version
    2.7.9
    > py -2 --version
    2.7.9
    > py -2.6 --version
    2.6.6
    > py -3 --version
    3.4.2
    > py -3.3 --version
    3.3.5

.. _setup-on-linux: 

نصب در گنولینوکس
------------------
پایتون معمولا در توزیع‌های گنولینوکس از پیش نصب می‌باشد (بر روی برخی نیز از هر دو شاخه آن نسخه‌ایی نصب است؛ به مانند: Ubuntu و Fedora). برای اطمینان کافی است دستورات ``python2 --version`` (برای نسخه 2x) و ``python3 --version`` (برای نسخه 3x) را در خط فرمان سیستم عامل وارد نمایید؛ به عنوان نمونه وضعیت نسخه‌های پایتون در Ubuntu 14.04 به صورت پایین است:

.. code::

    user> python --version
    python 2.7.6
    
    user> python2 --version
    python 2.7.6
    
    user> python3 --version
    python 3.4.0

.. note::
    هم اکنون نسخه‌ 3x، نسخه‌ پیش‌فرض پایتون در اکثر توزیع‌های گنولینوکس است، بنابراین دستور ``python --version`` نیز موجب فراخوانی مفسر پایتون نسخه‌ 3x و نمایش نسخه‌ آن می‌شود.

    `Arch Linux <https://www.archlinux.org/>`_ نخستین توزیع از گنولینوکس است که نسخه‌ 3x را به عنوان نسخه پیش‌فرض پایتون خود قرار داده است.

    در دستورات یاد شده به جای ``version--`` می‌توان از ``V-`` (حرف v بزرگ انگلیسی) نیز استفاده نمود.

اکنون با فرض اینکه توزیع مورد استفاده‌‌، از پیش فاقد نسخه‌ 3x بوده یا اینکه نسخه‌ نصب شده آنقدر قدیمی است که می‌بایست آن را ارتقا (Upgrade) داد؛ پس از دانلود سورس کد نسخه‌ جدید (در این زمان فایل: Python-3.4.2.tar.xz) به صورت زیر عمل خواهیم کرد (نصب نسخه‌ 2x نیز به همین شکل است):

نخست می‌بایست تمام بسته‌های پیش‌نیاز در سیستم عامل نصب گردند. برای این منظور می‌توانید دستورات پایین را (متناسب با نوع توزیع خود) در خط فرمان سیستم عامل وارد نمایید:

در *Fedora*:

.. code::

    user> sudo dnf update
    user> sudo dnf install make automake autoconf pkgconfig glibc-devel gcc gcc-c++ bzip2 bzip2-devel tar tcl tcl-devel tix tix-devel tk tk-devel zlib-devel ncurses-devel sqlite-devel openssl-devel openssl readline-devel gdbm-devel db4-devel expat-devel libGL-devel libffi-devel gmp-devel valgrind-devel systemtap-sdt-devel xz-devel libX11-devel findutils libpcap-devel

در *Ubuntu*:

.. code::

    user> sudo apt-get update
    user> sudo apt-get install build-essential
    user> sudo apt-get install make automake autoconf pkg-config libc6-dev gcc g++ bzip2 libbz2-dev tar tcl tcl-dev tix-dev tk tk-dev zlib1g-dev libncursesw5-dev libsqlite3-dev libssl-dev openssl libreadline-dev libgdbm-dev db4.8-util libexpat1-dev libgl-dev libffi-dev libgmp3-dev valgrind systemtap-sdt-dev xz-utils libX11-dev findutils libpcap-dev

پس از اطمینان از نصب بسته‌های پیش‌نیاز به مسیری که سورس کد پایتون (پس از دانلود) در آن قرار دارد رفته (در اینجا: دایرکتوری Downloads) و فایل سورس کد را از حالت فشرده خارج نمایید. حاصل کار یک دایرکتوری جدید با نامی مشابه Python-3.4.2 است که با استفاده از دستور ``ls`` قابل مشاهده است؛ اکنون می‌بایست از طریق خط فرمان و دستور ``cd`` وارد مسیر آن شوید.

.. code::

    user> cd Downloads/
    user> ls
    Python-3.4.2.tar.xz
    
    user> tar xf Python-3.4.2.tar.xz

    user> ls
    Python-3.4.2  Python-3.4.2.tar.xz
    
    user> cd Python-3.4.2/

در پایان دستورات پایین را به ترتیب وارد نمایید:

.. code::

    user> sudo ./configure
    user> sudo make
    user> sudo make install

اگر پیش از این نسخه‌ای از شاخه 3x پایتون نصب باشد (مانند 3.3)، دستور سطر سوم موجب جایگزین شدن نسخه‌ جدید (3.4) با آن می‌شود؛ این دستور در مواقع ارتقا نسخه پایتون مفید است. چنانچه قصد دارید همچنان به نسخه‌ پیشین نیز دسترسی داشته باشید، دستور ``make altinstall`` را جایگزین ``make install`` نمایید. به عنوان نمونه وضعیت نسخه‌ 3x پایتون، در زمان‌ قبل و بعد از نصب نسخه‌ جدید به همراه مسیر نصب آن در Fedora 20 آورده شده است. به تفاوت عملکرد دو دستور ``make altinstall`` و ``make install`` توجه نمایید:


گرفتن نسخه‌های از پیش نصب:

.. code::
    
    user> python --version
    Python 2.7.5
    
    user> python2 --version
    Python 2.7.5
    
    user> python3 --version
    Python 3.3.2

گرفتن مسیر و نسخه‌ پایتون 3x پس از نصب نسخه 3.4.2 با استفاده از دستور ``make altinstall`` :

.. code::
    
    user> which python3
    /usr/bin/python3
    
    user> which python3.4
    /usr/local/bin/python3.4
    
    user> python3 --version
    Python 3.3.2
    
    user> python3.4 --version
    Python 3.4.2

گرفتن مسیر و نسخه‌ پایتون 3x پس از نصب نسخه 3.4.2 با استفاده از دستور ``make install`` :

.. code::
    
    user> which python3
    /usr/local/bin/python3
    
    user> which python3.4
    /usr/local/bin/python3.4
    
    user> python3 --version
    Python 3.4.2
    
    user> python3.4 --version
    Python 3.4.2

در روشی دیگر برای نصب کردن چندین نسخه‌ متفاوت از یک شاخه پایتون، می‌توان هر یک را در محل مشخصی از دیسک نصب نمود. برای این منظور می‌بایست از دستوراتی مشابه پایین استفاده نمایید:

.. code::

    user> sudo ./configure --prefix=/opt/python3.4
    user> sudo make
    user> sudo make install


.. caution::
    عبارت ``opt/python3.4/`` در سطر یکم، مشخص کننده‌‌ محل نصب پایتون است که به دلخواه خود کاربر تعیین می‌گردد.

در صورت استفاده از این روش، مفسر پایتون نسخه نصب شده را می‌توان مشابه هر یک از دو دستور زیر (با ذکر مسیر نصب - در اینجا: opt/python3.4/)‌ فراخوانی نمود:

.. code::

    user> /opt/python3.4/bin/python3 --version
    Python 3.4.2
    user> /opt/python3.4/bin/python3.4 --version
    Python 3.4.2

برای راحتی در فراخوانی می‌توانید نشانی دایرکتوری مفسر پایتون را به متغیر محیطی Path سیستم عامل اضافه نمایید. برای این کار فایل پنهان bashrc. (البته چنانچه از پوسته پیش‌فرض یعنی bash استفاده می‌کنید) موجود در دایرکتوری home (مسیر ~) را توسط یک ویرایشگر متن، باز نموده و عبارتی مشابه پایین را در آن وارد و سپس تغییر ایجاد شده را ذخیره (Save) نمایید:

.. code::

    export PATH=$PATH:/opt/python3.4/bin 

اکنون برای فراخوانی پایتون نصب شده دیگر نیازی به وارد کردن مسیر آن نمی‌باشد ولی به خاطر داشته باشید به دلیل وجود نسخه 3x ای که از پیش نصب بوده (در اینجا: 3.3.2) لازم است نسخه جدید را با ذکر صریح نسخه فراخوانی نمایید:


.. code::

    user> python3 --version
    Python 3.3.2
    user> python3.4 --version
    Python 3.4.2

.. note::
    به صورت کلی برای فراخوانی پایتون نسخه 3x از یکی از دستورات ``python3.4``، ``python3``، ``python`` یا ``python3.x`` که x بیانگر بخش جزئی نسخه پایتون می‌باشد و برای نسخه 2x نیز از دستورات ``python2.7`` ،``python2`` یا ``python2.x`` استفاده می‌گردد. در این راستا چنانچه پایتون در مسیری خاص نصب گردد لازم است مسیر آن به متغیر Path اضافه شود. برای فراخوانی pip و IDLE هر نسخه نیز از همین رویه پیروی می‌شود.


.. _python-pip: 

سیستم مدیریت بسته
-------------------
`pip <http://pip.pypa.io/en/stable/>`_ (پِپ) سیستم مدیریت بسته‌‌ پایتون است. pip ابزاری است مبتنی بر خط فرمان که از آن برای نصب، حذف، بروز رسانی و در کل مدیریت بسته‌های (یا کتابخانه‌ها‌ی شخص ثالث) پایتون استفاده می‌گردد. برنامه‌نویس پس از یافتن بسته‌ مورد نیاز خود در PyPI یا وب‌سایت‌ها و سرویس‌های دیگری به مانند github.com و bitbucket.com می‌تواند به وسیله دستور pip در خط فرمان، اقدام به نصب آن در پایتون نماید.

.. tip::
    `PyPI <https://pypi.org/>`_ (پای‌پِ) یا مخزن بسته‌های پایتون (Python Package Index) محلی است که بسیاری از کتابخانه‌ها یا برنامه‌های شخص ثالث پایتون در آن نگه داری می‌شود. کاربران پایتون می‌توانند از طریق PyPI پروژه (یا بسته) خود را منتشر یا اقدام به جستجو و  دانلود بسته‌های مورد نیاز خود نمایند.
    

آشنایی با مخازنی همچون PyPI و استفاده از pip در توسعه پروژه‌های پایتونی اهمیت بالایی دارد. برای مثال فرض نمایید در پروژه خود می‌بایست تاریخ را با فرمت جلالی نمایش دهید. راه‌حل ابتدایی، توسعه کدها یا ماژولی برای تبدیل تاریخ میلادی (پیش‌فرض در پایتون) به جلالی توسط خودتان می‌باشد. راه‌حل دیگر اما جستجو برای یافتن  کتابخانه‌ یا ماژول‌هایی است که پیش‌تر توسط دیگران توسعه یافته و در مخازنی همانند PyPI منتشر یافته است. در این شرایط برای دسترسی به این کتابخانه‌ها‌ی شخص ثالث تنها کافی است با استفاده از pip آن‌ها را مجموعه کتابخانه‌های داخل رایانه خود اضافه نمایید.

pip از زمان انتشار نسخه‌ 3.4 به بسته‌ نصبی پایتون افزوده شده است و به همراه آن نصب می‌شود ولی در صورت نیاز به pip برای نسخه‌های قدیمی‌تر، می‌بایست آن را به صورت جداگانه‌ نصب نمایید.

.. note::
    نسخه 2.7.9 پایتون پس از نسخه 3.4 منتشر شده است؛ بنابراین با نصب این نسخه و نسخه‌های جدیدتر آن از شاخه 2x پایتون نیز pip در دسترس خواهد بود.


برای نصب pip لازم است تا فایل `get-pip.py <http://bootstrap.pypa.io/get-pip.py>`_ را دانلود نمایید. 

سپس به وسیله‌ دستوری مشابه ``python get-pip.py`` در خط فرمان، با سطح کاربری Administrator (در ویندوز) یا root (در گنولینوکس) می‌توانید اقدام به نصب pip نمایید. فراموش نشود، در زمان نصب نیاز به اتصال اینترنت می‌باشد.


.. note::
    منظور از ``python`` در دستور ``python get-pip.py``، فراخوانی مفسر پایتون نسخه‌ایست که قصد داریم pip را در آن نصب کنیم.

برای نمونه؛ با فرض دانلود بودن ``get-pip.py`` و قرار داشتن آن در دایرکتوری Downloads سیستم عامل،‌ برای نصب pip در نسخه 3x (مثلا قدیمی!) پایتون به صورت پایین عمل می‌نماییم:

در *گنولینوکس*:

.. code::

    user> cd Downloads/
    user> sudo python3 get-pip.py
    
    [...]
    Successfully installed [...]
    
    user> pip3 --version
    pip 7.0.1 [...]


در *ویندوز*:

.. code::

    > cd Downloads\
    > python get-pip.py
    
    [...]
    Successfully installed [...]
    
    > pip --version
    pip 7.0.1 [...]

*توجه داشته باشید که پیش از این، محل نصب پایتون نسخه 3x به ترتیبی که گفته شد به متغیر Path ویندوز افزوده بودیم و cmd نیز به صورت Administrator اجرا شده است.*

کار با pip بسیار آسان است. به عنوان نمونه برای نصب `Bottle <http://bottlepy.org/>`_ که یک وب فریم‌ورک (Web Framework) برای پایتون است از دستور ``pip install bottle`` استفاده می‌گردد. با وارد کردن این دستور، Bottle در PyPI (به عنوان مخزن پیش‌فرض pip) جستجو می‌شود و پس از یافتن، ابتدا دانلود، سپس نصب و به دایرکتوری site-packages پایتون افزوده می‌شود. 

در ادامه برخی از دستورات رایج pip آورده شده است. برای کسب دانش بیشتر از چگونگی استفاده‌ pip می‌توانید به `اسناد آن <http://pip.pypa.io/en/stable/>`_ مراجعه نمایید.


* نصب آخرین نسخه از یک بسته::
    
    # pip install [package name]
    
    root> pip install SomePackage

* نصب یک نسخه خاص از یک بسته:: 
    
    # pip install [package name]==[version]
    
    root> pip install SomePackage==1.0.4

* حذف یک بسته::
    
    # pip uninstall [package name]
    
    root> pip uninstall SomePackage

* بروز رسانی یک بسته::
    
    # pip install --upgrade [package name]
    
    root> pip install --upgrade SomePackage

  برای بروز رسانی خود pip نیز از همین الگو استفاده می‌شود: ``pip install --upgrade pip``

  البته در ویندوز می‌بایست از دستور زیر استفاده نمایید::

          python -m pip install -U pip

  به جای ``upgrade--`` می توانید از ``U-`` نیز استفاده نمایید.

|


* گرفتن فهرست تمام بسته‌های نصب شده:

  ::
    
      user> pip list

* گرفتن فهرست تمام بسته‌هایی که می‌بایست بروز رسانی شوند::
    
    user> pip list --outdated

* مشاهده جزییات یک بسته نصب شده::
    
    # pip show [package name]
    
    user> pip show SomePackage


* نصب تمام بسته‌هایی که درون یک فایل متنی به مانند requirements.txt مشخص شده است (`فایل نمونه <https://pip.pypa.io/en/stable/reference/requirements-file-format/#requirements-file-format>`__)::
    
    root> pip install -r requirements.txt
   


.. _python-pip-user: 
   
user--
~~~~~~~~

ماژول pip به صورت پیش‌فرض تمامی بسته‌های دریافتی را در مسیری قرار می‌دهد که در کل رایانه (تمامی کاربران) قابل دسترس باشد. این روش نصب و مدیریت بسته به صورت حرفه‌ای  پیشنهاد نمی‌شود، چرا که در بلند مدت و در هنگام توسعه برنامه‌های گوناگون، برنامه‌نویس را دچار مشکل خواهد کرد. علاوه بر این، هر نصب بسته نیاز به دسترسی root (دستور sudo) یا Administrator خواهد داشت که مشکلات خاص خود را به همراه دارد.


بهترین راه حل یا شیوه مدیریت پروژه در پایتون، ایجاد محیط مجازی (Virtual Environment) به ازای هر پروژه می‌باشد. در این حالت برای هر پروژه یک محیط پایتونی کاملا ایزوله و مستقل ایجاد می‌گردد. بنابراین ماژول pip هر بسته مورد نیاز در هر پروژه را تنها در همان پروژه قرار می‌دهد. چگونگی ایجاد محیط مجازی در پایتون توسط درس آینده بررسی خواهد شد.


شیوه دیگر استفاده از ``user--`` در میان دستور ماژول pip می‌باشد. این یک روش ساده برای پرهیز از نیاز به دسترسی root (دستور sudo) یا Administrator می‌باشد. در این شرایط ماژول pip هر بسته مورد نیاز را در محیط کاربری، کاربر جاری نگهداری می‌کند::

    user> pip install --user bottle

|

----

:emoji-size:`😊` امیدوارم مفید بوده باشه


