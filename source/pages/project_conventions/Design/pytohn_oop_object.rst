===============================
Python OOP - Object
===============================

類別(class)與物件(Object)
---------------------------

何謂類別 (class)?
~~~~~~~~~~~~~~~~~
* 類別: 類別是一個模板，用來定義物件的屬性(Attributes)和方法(Methods)。它為物件提供了行為模式，決定物件該如何表現和操作。
* 定義一個類: 在Python中，使用class關鍵字來定義類別，每個類別可以包含屬性和方法，屬性是變數，而方法是函式(Function)，這些都屬於該類別。

舉例:
~~~~~

.. code-block:: python

    class Animal:  # 類別
        def __init__(self, name):
            self.name = name  # 屬性

        def speak(self):  # 方法
            print(f"{self.name} is making a sound.")

何謂物件 (object)?
~~~~~~~~~~~~~~~~~~
* 物件: 物件相當於類別的一個實例(instance)。換句話說，類別是一個定義，而物件是依據這個定義創建的具體實。每個物件擁有自己的數據(屬性)和行為(方法)。
* 定義一個物件: 當你定義了一個類別後，可以使用該類別來創建物件。每個物件是類別的一個具體實例，並且會有各自的屬性數據。

.. code-block:: python

    dog = Animal("Buddy")
    dog.speak()  # print: Buddy is making a sound.

.. note::
    * 類別是物件的模板，類別定義了物件的行為模式，而物件則是依據這個模板實例化出來的具體實體
    * 物件能夠使用類別中定義的屬性和方法來執行操作。
    * 類別本身也是一種物件：在Python中，類別本身也是一個特殊類型的物件，稱為"類別物件(class object)"。

初始化類別
--------------
* 當你創建一個類別實例時Python會自動呼叫__init__()方法來初始化該物件。這個方法可以接收參數，通常用來為物件設置初始值。

.. code-block:: python
        
    class Animal:
        def __init__(self, name, sound):
            self.name = name  # 初始化物件的 name 屬性
            self.sound = sound  # 初始化物件的 sound 屬性

        def speak(self):
            print(f"{self.name} says {self.sound}")

    # 創建 Animal 類別的實例
    dog = Animal("Dog", "Woof!")
    cat = Animal("Cat", "Meow")

    dog.speak()  # 輸出: Dog says Woof!
    cat.speak()  # 輸出: Cat says Meow

* 建立一個類別時，可以透過__init__()來指派類別中的屬性，但是，類別的定義其實不一定需要__init__()，若你並沒有要給予該物件預設值，是可以不用寫__init__。
* 關於self：__init__()中的self參數是指向物件本身的引用。當你創建一個物件時，Python會將這個物件的實例作為self傳遞給__init__()，這樣你可以在__init__()方法中定義該物件的屬性。
