====================================
Python OOP - 繼承(Inheritance)
====================================

繼承(Inheritance)
-----------------------

* 繼承: 指的是一個類別(子類)從另一個類別(父類)獲得它的屬性和方法的功能。
* 繼承允許子類別重用父類別的程式碼，無須重新實作，並且可以添加新功能或修改已存在的行為，從而促進程式的擴展和維護。

名詞說明
-----------------------

* 父類別(Person/Base class): 提供繼承給其他類別的類別，父類別定義了某些基本行為或屬性，供其他類別使用。
* 子類別(child/Derived class): 從父類別繼承屬性和方法的類別，子類別可以擴展或覆寫(override)父類別的功能，使其更符合子類別的需求。
* 單繼承：子類別只繼承自一個父類別。
* 多繼承：子類別同時繼承多個父類別。

簡單範例
-----------------------

.. code-block:: python

    # test_oop.py

    # 定義父類別
    class Animal:
        def __init__(self, name):
            self.name = name

        def speak(self):
            return f"{self.name} makes a sound"

    # 定義子類別，繼承自 Animal
    class Dog(Animal):
        pass

    # 創建物件
    dog = Dog("Buddy")

    print(dog.speak())  # Buddy makes a sound

* 透過初始化父類別，並賦予物件屬性一個值(self.name)。
* 子類Dog可直接使用父類別已存在的方法，避免重寫相同邏輯。

新增與複寫(override)
-----------------------

.. code-block:: python

    class Animal:
        def __init__(self, name):
            self.name = name

        def speak(self):
            return f"{self.name} makes a sound"

    # 定義子類別，繼承自 Animal
    class Dog(Animal):
        def speak(self):  # 複寫
            return f"{self.name} says Woof!"
        def play_games(self):  # 新增
            return f"{self.name} is very happy!"

    # 創建物件
    animal = Animal("Animal")
    dog = Dog("Buddy")

    print(dog.speak())  # says Woof!
    print(dog.play_games())  # Buddy is very happy!

    print(animal.speak())  # Animal makes a sound
    print(animal.play_games())  # AttributeError: 'Animal' object has no attribute 'play_games'

輸出結果
~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: 

    Buddy says Woof!
    Buddy is very happy!
    Animal makes a sound
    Traceback (most recent call last):
    File "/Users/xiu/Desktop/test_oop.py", line 23, in <module>
        print(animal.play_games())  # AttributeError: 'Animal' object has no attribute 'play_games'
    AttributeError: 'Animal' object has no attribute 'play_games'

* 透過複寫speak()方法，實現了不同的行為，可以看到子類的物件可以呼叫speak與新增的play_games方法，父類的物件也保有了原有的speak方法，但無法呼叫子類所新增的方法。

super()函式
-----------------------

* super(): 用於確保多重繼承時，父類別可以被正確初始化與呼叫。
* 若子類別同樣定義了__init__()方法來初始化物件，會取代原有父類的__init__()方法，因此就必須額外宣告一個super().__init__()來初始化父類。
* 多重繼承的應用，會涉及到Python的方法解析順序(method resolution order, MRO)，在多重繼承的情況下，super()確保了正確的父類別方法被呼叫，解析順序也會依照物件本身->物件的第一個父類(由左至右依序解析)->物件第一個父類的父類->…->物件第二個父類->物件第二個父類的父類->..，這在設計上必須多加留意。(下個小章節有提到)

super()應用範例
~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: python

    # 定義父類別
    class Animal:
        def __init__(self, name):
            self.name = name

        def speak(self):
            return f"{self.name} makes a sound"

    # 定義子類別，繼承自 Animal
    class Dog(Animal):
        def __init__(self, name, breed):
            # 使用 super() 呼叫父類別的 __init__ 方法來初始化 name
            super().__init__(name)
            self.breed = breed  # 新增一個屬性 breed
            
        def speak(self):  # 複寫 speak 方法
            return f"{self.name} says Woof!"
        
        def play_games(self):  # 新增一個 play_games 方法
            return f"{self.name} is very happy!"

    # 創建物件
    animal = Animal("Animal")
    dog = Dog("Buddy", "Golden Retriever")

    print(dog.speak())  # Buddy says Woof!
    print(dog.play_games())  # Buddy is very happy!
    print(f"{dog.name} is a {dog.breed}")  # Buddy is a Golden Retriever

    print(animal.speak())  # Animal makes a sound

輸出結果
~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: 

    Buddy says Woof!
    Buddy is very happy!
    Buddy is a Golden Retriever
    Animal makes a sound

* Dog類的__init__()方法中，使用super().init(name)呼叫父類別Animal的__init__()方法，這樣就能正確初始化name屬性。
* Dog類別除了繼承父類別Animal的屬性外，還透過__init__()新增了一個breed屬性。

多重繼承
-----------------------

* 多重繼承顧名思義，一個類別同時繼承多個父類別，讓子類別可以繼承多個父類別的屬性和方法。
* 這會讓程式碼看起來複雜些，尤其是又有多層的時候。

多重繼承與MRO範例
~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: python

    class AAA:
        def test(self):
            print("from class A.")

    class BBB:
        def test(self):
            print("from class B.")

    class CCC(AAA):
        def test(self):
            print("from class C.")

    class DDD(BBB):
        def test(self):
            print("from class D.")

    class EEE(DDD, CCC):
        def test(self):
            print("from class E.")

    object = EEE()
    object.test()
    print(object.__class__.mro())

輸出結果
~~~~~~~~~~~~~~~~~~~~~~~

.. code-block::

    from class E.
    [<class '__main__.EEE'>, <class '__main__.DDD'>, <class '__main__.BBB'>, <class '__main__.CCC'>, <class '__main__.AAA'>, <class 'object'>]

* 由於EEE也有包含了test的方法。
* 若將EEE還有DDD的方法都移除呢?

.. code-block:: python

    class AAA:
        def test(self):
            print("from class A.")

    class BBB:
        def test(self):
            print("from class B.")

    class CCC(AAA):
        def test(self):
            print("from class C.")

    class DDD(BBB):
        def yoyo(self):  # 隨意改名稱
            print("from class D.")

    class EEE(DDD, CCC):
        def yoyo(self):  # 隨意改名稱
            print("from class E.")

    object = EEE()
    object.test()
    print(object.__class__.mro())

輸出結果
~~~~~~~~~~~~~~~~~~~~~~~

.. code-block::

    from class B.
    [<class '__main__.EEE'>, <class '__main__.DDD'>, <class '__main__.BBB'>, <class '__main__.CCC'>, <class '__main__.AAA'>, <class 'object'>]

* 解析順序如上一章節提到的MRO: 依照物件本身 -> 物件的第一個父類(由左至右) -> 物件第一個父類的父類 -> … -> 物件第二個父類 -> 物件第二個父類的父類 -> …
* 這在設計上需要多加留意，並且這樣的設計有機會讓程式碼的可讀性降低。

參考資料
-----------------------
* ChatGPT4o
* `物件導向程式設計（使用Python） <https://hackmd.io/@fgisc32ndxckeisc38th/OOP#1-%E9%A1%9E%E5%88%A5%EF%BC%88class%EF%BC%89%E8%88%87%E7%89%A9%E4%BB%B6%EF%BC%88object%EF%BC%89>`_
* `[Python]-關於物件導向程式設計 (Object-Oriented Programming, OOP) <https://medium.com/@leo122196/python-%E9%97%9C%E6%96%BC%E7%89%A9%E4%BB%B6%E5%B0%8E%E5%90%91%E7%A8%8B%E5%BC%8F%E8%A8%AD%E8%A8%88-object-oriented-programming-oop-b3ce7ae019f3#0176>`_
* `[Python]-封裝 (Encapsulation): 物件導向的三大特色之一 <https://medium.com/@leo122196/python-%E5%B0%81%E8%A3%9D-encapsulation-%E7%89%A9%E4%BB%B6%E5%B0%8E%E5%90%91%E7%9A%84%E4%B8%89%E5%A4%A7%E7%89%B9%E8%89%B2%E4%B9%8B%E4%B8%80-9196f8aa4ef6>`_
* `[Python Property 教學：保護變數資料的 Getter 與 Setter <https://haosquare.com/python-property/#Property_%E7%9A%84%E5%A5%BD%E8%99%95>`_
* `[Python]-繼承 (Inheritance): 物件導向的三大特色之一 <https://medium.com/@leo122196/python-%E7%B9%BC%E6%89%BF-inheritance-%E7%89%A9%E4%BB%B6%E5%B0%8E%E5%90%91%E7%9A%84%E4%B8%89%E5%A4%A7%E7%89%B9%E8%89%B2%E4%B9%8B%E4%B8%80-433891ad8423>`_
* `[Python - super() 函式與 MRO 詳解] <https://myapollo.com.tw/blog/python-super-mro/>`_