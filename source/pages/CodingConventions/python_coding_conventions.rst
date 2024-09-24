==========================================
PEP 8 - Style Guide for Python Code
==========================================

本文參考Python PEP8所提出的風格指南與Google Python Style Guide

前言
-----

Lab大多的專案都是以Python進行開發，尤其大多剛進來中心的同學都是先學Python，無論是第一組開發影像處理演算法或是第二組開發Web server、Socket等等。而在撰寫程式時，不知道你們有沒有發現一些怪怪的地方，像是: 程式碼排版、縮排、參數命名、註解寫法..等等，這些平常我們在'寫作'的風格，其實是有一系列規範的，其目的是在於使開發者可以寫出一系列可讀性高且風格一致的程式。

編排這些程式碼如同寫作，寫作需要了解讀句規則，寫程式也該清楚程式書寫風格，才得以在好的基礎下精進自我。

Python官方有一個編碼規範的風格指南: `PEP 8` (Python Enhancement Proposal8)，上面有許多關於Python的用法建議。

**讓我們開始吧!**

推薦工具
----------

你可以直接安裝排版工具: `Black Formatter` 這是一款強大的Python自動排版工具，它可以幫助你的程式碼風格統一，並且更符合PEP8的coding style。

.. tip::
    設定程式碼長度，預設是79個字元，建議可以改成120~150。

**安裝Black Formatter的步驟:**

1. 設定畫面：
.. image:: /assets/CodingConventions/black_formatter.png

2. 在設定中新增``--line-length=150``
.. image:: /assets/CodingConventions/length=150.png

PEP8程式碼編排規則
---------------------

Basic
-----

* 每行長度不超過79個字元
* 每行縮排4個空格
* 斷行字首與首行括號對齊
* 縮排使用空格而非Tab
* 判斷式或函式過長，使用反斜線```\```斷行

1. import
---------

* import library時，import整個packages或modules，不單獨import types、classes或functions

.. code-block:: python

    import x  # import整個package或module
    from x import y  # 從package或module import特定modules
    from x import y as z  # 用法:

    # 1. 有兩個同名的模組 y 需要導入
    # 2. y 與目前模組中的頂級名稱衝突
    # 3. y 與公共 API 中常用的參數名稱衝突（例如 features）
    # 4. y 的名稱太長或太通用，不方便使用。

**import錯誤範例**

.. code-block:: python

    import a, b
    import os, sys

2. 註解 (Comments):
-------------------

1. Inline Comments: ``#`` 後空一格，註解與程式碼至少空兩隔
2. Documentation Strings

    1. def之後才會出現
    2. 結尾的"""會獨立一行

.. code-block:: python

    def add_numbers(a: int, b: int) -> int:
        """This is an example of adding two numbers.

        Args:
            a: first number
            b: second number

        Returns:
            The result of adding two numbers
        """
        return a + b

3. 縮排 (Code Lay-out)
----------------------

**正確範例**

* 每層縮排4個空格
* 斷行字首與首行括號對齊

.. code-block:: python

    foo = long_function_name(var_one, var_two,
                            var_three, var_four)

看得出var_three與var_one是對齊的吧!

.. code-block:: python

    def long_function_name(
            var_one, var_two, var_three,
            var_four):
        print(var_one)

首行沒有參數，次行垂直縮排，並且要比其他程式碼再加一層縮排。

**錯誤範例**

.. code-block:: python

    foo = long_function_name(var_one, var_two,
        var_three, var_four)

首行有參數。

.. code-block:: python

    def long_function_name(
        var_one, var_two, var_three,
        var_four):
        print(var_one)

次行對齊不能與其他程式碼在同一行，須再加一層縮排。

4. 多行括號
------------

括號結束符與最後行的首字符對齊

.. code-block:: python

    my_list = [
        1, 2, 3,
        4, 5, 6,
        ]

括號結束符與首行的首字符對齊

.. code-block:: python

    my_list = [
        1, 2, 3,
        4, 5, 6,
    ]

5. 斷行
-------

判斷式或函式過長，使用反斜線 ``\`` 斷行

.. code-block:: python

    with open('/path/to/some/file/you/want/to/read') as file_1, \
        open('/path/to/some/file/being/written', 'w') as file_2:
        file_2.write(file_1.read())

6. 斷行時的操作符號
-------------------

操作符號對齊

.. code-block:: python

    income = (gross_wages
                + taxable_interest
                + (dividends - qualified_dividends)
                - ira_deduction
                - student_loan_interest)

7. 表達式、語句中的空格
------------------------

* ()、[]、{}等括號內不要有多餘空格
* 逗號、分號等符號前不用空格
* 括號內的運算符不用空格
* 調用函式時，key的括號前不用空格

**正確範例**

.. code-block:: python

    spam(ham[1], {eggs: 2})  # ()、[]、{}等括號內不要有多餘空格
    if x == 4: print(x, y)  # 逗號、分號等符號前不用空格
    matrix[: -1]  # 括號內的運算符不用空格
    ham[lower+offset : upper+offset]  # 括號內的運算符不用空格
    settings["rtsp"]  # 調用函式時，key的括號前不用空格

**錯誤範例**

.. code-block:: python

    spam( ham[ 1 ], { eggs: 2 } )
    if x == 4 : print(x , y)
    matrix[ : -1]
    ham[lower + offset : upper + offset]
    settings ["rtsp"]

內容尚在建置中...
-------------------

參考資料
----------

- `Python: PEP 8 程式碼風格指南 <https://adora-xu.com/2024/03/01/python-pep8-style-guide-for-python-code/>`_
- `PEP8 Python 編碼規範手冊 <https://cflin.com/wordpress/603/pep8-python%E7%B7%A8%E7%A2%BC%E8%A6%8F%E7%AF%84%E6%89%8B%E5%86%8A/>`_
- `PEP8 (Python Enhancement Proposal) <https://peps.python.org/pep-0000/>`_
- `Google Python Style Guide <https://google.github.io/styleguide/pyguide.html>`_
- `如何進行 Code Review? <https://enginebai.medium.com/code-review-guidelines-b76a859c377c>`_
- `Google Python 風格指南 - 中文版 <https://tw-google-styleguide.readthedocs.io/en/latest/google-python-styleguide/>`_