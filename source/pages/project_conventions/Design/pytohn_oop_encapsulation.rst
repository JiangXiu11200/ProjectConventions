====================================
Python OOP - 封裝(Encapsulation)
====================================

封裝(Encapsulation)
-----------------------

* 封裝: 指的是將資料(屬性，例如參數)和操作這些資料的方法(行為，例如Function)封裝在同一個類別中，並透過控制對這些資料的訪問來保護它們。
* 主要目的是提高系統的安全性和靈活性，並隱藏物件的內部實現細節，從而減少外部對其依賴。
* 簡單的說，Class就是一種封裝過程。
* Python透過 `__` 可以宣告成私有屬性或方法，用以宣告該屬性或方法僅能透過內部訪問。

範例
~~~~~

.. code-block:: python

    class Person:
        def __init__(self, name: str, age: int):
            self.__name = name  # 私有屬性
            self.__age = age  # 私有屬性

        def get_name(self):
            return self.__name

        def set_name(self, name: str):
            if isinstance(name, str):  # 確保輸入值是字串
                self.__name = name
            else:
                raise ValueError("Name must be a string")
        
        def get_age(self):
            return self.__age

        def set_age(self, age: int):
            if 0 < age < 120:  # 確保年齡在合理範圍內
                self.__age = age
            else:
                raise ValueError("Invalid age")

    person = Person("Leo", 25)
    print(person.__name)  # attribute error

輸出結果
~~~~~~~~~

.. code-block:: bash

    Traceback (most recent call last):
    File "/Users/xiu/Desktop/test.py", line 25, in <module>
        print(person.__name)  # Leo
    AttributeError: 'Person' object has no attribute '__name'

* 範例中， `__name` 和 `__age` 屬於私有屬性，僅能透過 `get_name()` 和 `set_name()` 等方法來訪問或修改；同理，若將function前加上 `__` ，也會報告 `AttributeError`。
* 這樣的設計保證了外部無法直接修改物件的屬性，使其成為私有屬性(private attribute)。

.. hint::

    注意: 若宣告單底線 `_` 同樣也是私有屬性，但這僅只於讓閱讀程式碼的人知道該屬性或方法屬於私有的，實際程式碼運行，仍然可以呼叫包含單底線的屬性或方法。

裝飾器(Decorator)
-----------------------

@staticmethod
~~~~~~~~~~~~~~
* 靜態函數，不屬於該物件的實例，無須件立物件即可直接調用，就像普通的Function一樣。
* 適合用於定義一些物件內的工具，好比說Get current time，你會一直使用到，但這個功能又只是該物件實例需要使用到的工具，不會存取物件內的屬性，就可以定義成靜態方法。

.. code-block:: python

    from datetime import datetime

    class TimeUtility:
        def __init__(self):
            pass

        @staticmethod  # 靜態方法
        def get_current_time():
            return datetime.now().strftime("%Y-%m-%d %H:%M:%S")

    # 使用靜態方法來獲取當前時間
    current_time = TimeUtility.get_current_time()
    print(f"Current Time: {current_time}")

* 在使用靜態方法上，無論是否有初始化這個物件，都是可以被呼叫使用。

.. code-block:: python

    from datetime import datetime

    class TimeUtility:
        def __init__(self):
            pass

        @staticmethod  # 靜態方法
        def get_current_time():
            return datetime.now().strftime("%Y-%m-%d %H:%M:%S")

    # 使用靜態方法來獲取當前時間
    current_time = TimeUtility.get_current_time()
    print(f"Current Time: {current_time}")

    # 使用實例方法來獲取當前時間
    current_time = TimeUtility()
    print(current_time.get_current_time())

* 但根據Python的設計慣例，一般建議用類別名稱來呼叫靜態方法，這樣可以表明該方法不依賴於實例的狀態。
* 這樣的做法雖然不是強制的，但更符合Python開發中常見的實踐方式，提升程式碼的可讀性和邏輯清晰度。

@classmethod
~~~~~~~~~~~~~~
* 利用@classmethod裝飾器定義的方法，其會有一個cls參數用來指向類內所使用到的類別變數。
* 所有的實例都可以訪問這個類別變數。
* 與static method一樣，無須建立物件即可調用。
* 其用意是可以區分層級。
* 可以簡單解讀成：類別內部的全域變數。

.. code-block:: python

    class Person:
        population = 0  # 類別變數，紀錄人數

        def __init__(self, name):
            self.name = name
            Person.population += 1  # 每次創建一個新的人時，增加人口數

        @classmethod
        def get_population(cls):
            return f"There are currently {cls.population} people."

        @classmethod
        def create_anonymous(cls):
            return cls("Anonymous")

    # 使用類別來呼叫 classmethod
    print(Person.get_population())  # There are currently 0 people.

    # 創建新實例，修改類別變數
    p1 = Person("John")
    print(Person.get_population())  # There are currently 1 people.

    # 使用 classmethod 創建匿名對象
    anonymous = Person.create_anonymous()
    print(anonymous.name)  # Anonymous
    print(Person.get_population())  # There are currently 2 people.

輸出結果
~~~~~~~~~

.. code-block:: bash

    There are currently 0 people.
    There are currently 1 people.
    Anonymous
    There are currently 2 people.

* 類別變數 population: 這是一個類別變數，所有實例都可訪與修改問這個變數。每當我們創建一個Person實例時，這個變數就會增加。
* get_population透過cls.來存取。
* create_anonymous這個方法利用cls來創建一個名稱為"Anonymous"的Person實例。

@property
~~~~~~~~~~~~~~
* 透過宣告@property，可以將屬性封裝在類的內部。
* 可以將屬性封裝在類內部，使得對外部訪問的控制更加靈活，可防止不當修改屬性。
    * Getter (@property)
    * @變數名稱.setter
    * @變數名稱.deleter
* 若沒有宣告.setter或.deleter，其變數僅能訪問，無法修改或刪除，達到保護效果。

.. code-block:: python

    class Account:
        def __init__(self, account_name, initial_balance):
            self.account_name = account_name
            self._balance = initial_balance  # 使用單下劃線表示這是一個受保護的屬性

        @property
        def balance(self):
            """獲取當前餘額"""
            return self._balance

    account = Account('John', 100)

    print(f"John has $ {account.balance}")  # John has $ 100
    account.balance = 500  # AttributeError: can't set attribute

輸出結果
~~~~~~~~~

.. code-block::

    John has $ 100
    Traceback (most recent call last):
    File "/Users/xiu/Desktop/test.py", line 14, in <module>
        account.balance = 500  # AttributeError: can't set attribute
    AttributeError: can't set attribute

* 當透過@property裝飾器時，該變數會是受保護的變數，必須透過宣告setter或deleter才能修改。

.. code-block:: python

    class Account:
        def __init__(self, account_name, initial_balance):
            self.account_name = account_name
            self._balance = initial_balance  # 使用單下劃線表示這是一個受保護的屬性

        @property
        def balance(self):
            """獲取當前餘額"""
            return self._balance

        @balance.setter
        def balance(self, amount):
            """設置餘額，需確保餘額不能為負數"""
            if amount < 0:
                raise ValueError("餘額不能為負數")
            self._balance = amount

    account = Account('John', 100)
    print(f"John has $ {account.balance}")  # John has $ 100
    account.balance = 500  # AttributeError: can't set attribute
    print(f"John has $ {account.balance}")  # John has $ 500
    account.balance = -1000  # ValueError: 餘額不能為負數

輸出結果
~~~~~~~~~

.. code-block:: bash

    John has $ 100
    John has $ 500
    Traceback (most recent call last):
    File "/Users/xiu/Desktop/test.py", line 22, in <module>
        account.balance = -1000  # ValueError: 餘額不能為負數
    File "/Users/xiu/Desktop/test.py", line 15, in balance
        raise ValueError("餘額不能為負數")
    ValueError: 餘額不能為負數

* 透過.setter可以對該屬性進行修改，同時，也可定義其他邏輯來判斷修改方法是否正確，達到保護效果。

.. code-block:: python

    class Account:
        def __init__(self, account_name, initial_balance):
            self.account_name = account_name
            self._balance = initial_balance  # 使用單下劃線表示這是一個受保護的屬性

        @property
        def balance(self):
            """獲取當前餘額"""
            return self._balance

        @balance.setter
        def balance(self, amount):
            """設置餘額，需確保餘額不能為負數"""
            if amount < 0:
                raise ValueError("餘額不能為負數")
            self._balance = amount

        @balance.deleter
        def balance(self):
            """刪除餘額，這裡可以定義刪除的行為"""
            del self._balance

    account = Account('John', 100)
    print(f"John has $ {account.balance}")  # John has $ 100
    account.balance = 500  # AttributeError: can't set attribute
    print(f"John has $ {account.balance}")  # John has $ 500
    del account.balance
    print(f"John has $ {account.balance}")

輸出結果
~~~~~~~~~

.. code-block:: bash

    John has $ 100
    John has $ 500
    Traceback (most recent call last):
    File "/Users/xiu/Desktop/test.py", line 28, in <module>
        print(f"John has $ {account.balance}") 
    File "/Users/xiu/Desktop/test.py", line 9, in balance
        return self._balance
    AttributeError: 'Account' object has no attribute '_balance'

* 使用deleter可以刪除整個變數。
* 通常可以用在刪除陣列中的某個元素、dict的某個key-value，抑或是定義其他刪除行為，好比說定義不可刪除的判斷，以防意外刪除。
* 主要可以使用在釋放記憶體、不需要再使用到的屬性。

參考資料
-----------------------
* ChatGPT4o
* `物件導向程式設計（使用Python） <https://hackmd.io/@fgisc32ndxckeisc38th/OOP#1-%E9%A1%9E%E5%88%A5%EF%BC%88class%EF%BC%89%E8%88%87%E7%89%A9%E4%BB%B6%EF%BC%88object%EF%BC%89>`_
* `[Python]-關於物件導向程式設計 (Object-Oriented Programming, OOP) <https://medium.com/@leo122196/python-%E9%97%9C%E6%96%BC%E7%89%A9%E4%BB%B6%E5%B0%8E%E5%90%91%E7%A8%8B%E5%BC%8F%E8%A8%AD%E8%A8%88-object-oriented-programming-oop-b3ce7ae019f3#0176>`_
* `[Python]-封裝 (Encapsulation): 物件導向的三大特色之一 <https://medium.com/@leo122196/python-%E5%B0%81%E8%A3%9D-encapsulation-%E7%89%A9%E4%BB%B6%E5%B0%8E%E5%90%91%E7%9A%84%E4%B8%89%E5%A4%A7%E7%89%B9%E8%89%B2%E4%B9%8B%E4%B8%80-9196f8aa4ef6>`_
* `[Python Property 教學：保護變數資料的 Getter 與 Setter <https://haosquare.com/python-property/#Property_%E7%9A%84%E5%A5%BD%E8%99%95>`_
