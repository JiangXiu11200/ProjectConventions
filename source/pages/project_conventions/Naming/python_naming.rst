==========================================
Python Naming
==========================================

參數命名規則
----------------

1. Packages
~~~~~~~~~~~~~
* 字母全部小寫。
* 多個單字以底線（"_"）分隔。
* 最好堅持以一個單字為命名。

.. code-block:: python

    # packages example: 
    from your_packages import your_modules
    from oam_core import config
    from rest_framework import status

2. Modules
~~~~~~~~~~~~~
* 字母全部小寫。
* 多個單字以底線（"_"）分隔。
* 最好堅持以一個單字為命名。

.. code-block:: python

    # modules example:
    from your_packages import your_modules
    from oam_system import oam_system_config

3. Classes
~~~~~~~~~~~~~
* 每個字母開頭均大寫，如 ``ActivityLogMiddleware``。
* 異常類別以 "Error" 結尾。

.. code-block:: python

    # classes example:
    class ActivityLogMiddleware:
        pass

    class ValueError(Exception):
        pass

4. Global (module-level) Variables
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
* 字母全部小寫。
* 多個單字以底線（"_"）分隔。

.. code-block:: python

    # global variables example:
    global_variable = 42  # global variable

    def my_function():
        a = 40  # local variable
        ...

5. Instance Variables
~~~~~~~~~~~~~~~~~~~~~~~~~~
* 字母全部小寫。
* 多個單字以底線（"_"）分隔。
* 非公共（Non-public）變數以底線（"_"）開頭。
* 名稱修飾（Name Mangling）以雙底線（"__"）開頭。

.. code-block:: python

    # instance variables example:
    class MyClass:
        def __init__(self):
            self.instance_variable = 1
            self.__private_variable = 2
            self.__variable_with_name_mangling = 3

6. Methods
~~~~~~~~~~~~~
* 字母全部小寫。
* 多個單字以底線（"_"）分隔。
* 非公共（Non-public）方法則以底線（"_"）開頭。
* 名稱修飾（Name Mangling）以雙底線（"__"）開頭。

.. code-block:: python

    # methods example:
    class MyClass:
        def my_method(self):
            pass

        def _my_private_method(self):
            pass

        def __my_private_method_with_name_mangling(self):
            pass

7. Method Arguments
~~~~~~~~~~~~~~~~~~~~~~~~
* Instance methods 以 ``self`` 開頭。
* Class methods 以 ``cls`` 開頭。

.. code-block:: python

    # method arguments example:
    class MyClass:
        def __init__(self):
            ...

        def my_function(self, my_args):
            pass

        @classmethod
        def my_class_method(cls, cls_arg):
            pass

        def video_capture(self, video_queue):
            ...

8. Functions
~~~~~~~~~~~~~~
* 字母全部小寫。
* 多個單字以底線（"_"）分隔。

.. code-block:: python

    # functions example:
    def my_function():
        pass

    def video_capture():
        ...

9. Constants
~~~~~~~~~~~~~~~
* 字母全部大寫。
* 多個單字以底線（"_"）分隔。

.. code-block:: python

    # constants example:
    VIDEO_EXTENSION = ".mp4"
    FOUR_CC = "mp4v"
    LOG_EXTENSION = ".json"
    MAX_RETRIES = 5

10. Interface
~~~~~~~~~~~~~~~~
* 命名名稱以 "I" 開頭。

.. code-block:: python

    # interface example:
    class IMyInterface:
        pass

參考資料
----------
* `Python Naming Conventions <https://visualgit.readthedocs.io/en/latest/pages/naming_convention.html>`_
* `Python Coding Style <https://medium.com/@austinchang5116/python-coding-style-e2d8a3704e20>`_
* `Python naming conventions for interfaces and abstract classes? <https://stackoverflow.com/questions/10723839/python-naming-conventions-for-interfaces-and-abstract-classes>`_
* `PEP 8 – Style Guide for Python Code <https://peps.python.org/pep-0008/>`_