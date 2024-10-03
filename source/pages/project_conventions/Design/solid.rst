===============================
SOLID 原則
===============================

SOLID 原則
---------------------

- S: 單一職責原則(Single Responsibility Principle, SRP)
- O: 開放/封閉原則(Open/Closed Principle, OCP)
- L: 里氏替換原則(Liskov's Substitution Principle, LSP)
- I: 介面分離原則(Interface Segregation Principle, ISP)
- D: 依賴反轉原則(Dependency Inversion Principle, DIP)

1. 單一職責原則 (SRP)
---------------------

**一個元件（通常是Class）只能有一個職責，每一個元件應該只有一個改變的理由。也就是說，類別應該只負責一個功能或任務，避免多個職責交錯在一起。**

舉例
~~~~~
假設我們有一個類別 User，它不僅負責使用者的資料處理，還負責將使用者資訊儲存到資料庫中。

不正確範例，不符合SRP
~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: python

    class User:
        def __init__(self, name, email):
            self.name = name
            self.email = email

        # 處理使用者資料
        def display_user_info(self):
            print(f"Name: {self.name}, Email: {self.email}")

        # 將使用者資料儲存到資料庫
        def save_to_db(self):
            # 假設這裡有資料庫儲存的邏輯
            print(f"Saving {self.name}'s info to the database")

* 這裡 User 類別負責了兩個職責：
    * 處理和顯示使用者資料。
    * 將使用者資料儲存到資料庫中。
* 如果將來資料庫邏輯改變了，我們就需要修改 User 類別的部分程式碼，這違反了單一職責原則，因為這個類別有兩個不同的責任。

正確範例，符合SRP
~~~~~~~~~~~~~~~~~~~~~~~~~

我們可以將這兩個職責分開，讓User類別專注於使用者資料，而將儲存的邏輯交給專門負責資料儲存的類別。

.. code-block:: python

    # 負責使用者資料處理的類別
    class User:
        def __init__(self, name, email):
            self.name = name
            self.email = email

        # 處理使用者資料
        def display_user_info(self):
            print(f"Name: {self.name}, Email: {self.email}")

    # 負責資料庫操作的類別
    class UserRepository:
        def save(self, user: User):
            # 假設這裡有資料庫儲存的邏輯
            print(f"Saving {user.name}'s info to the database")

    # 使用
    user = User("Alice", "alice@example.com")
    user_repo = UserRepository()

    user.display_user_info()    # 處理資料
    user_repo.save(user)        # 儲存資料

* 這樣的設計符合單一職責原則，每個類別都有明確的職責：
    * User 只負責與使用者資料相關的邏輯，例如儲存名稱、電子郵件和顯示資訊。
    * UserRepository 專門負責資料庫相關的操作，例如儲存使用者資料。
* 若符合SRP，假設需要修改資料庫邏輯時，只有UserRepository需要修改，而不會影響User類別，使得程式更易於維護和擴展。

.. note::
    * 為符合SRP，設計每個Module、Class或Function時，可以用測試的角度來看，倘若我這樣寫程式碼，寫單元測試的時候，是否會有其他依賴項?

2. 開放封閉原則 (OCP)
---------------------

**軟體實體(Module、Class、Function) 應該對擴展開放，對修改封閉。意思是當新增功能時，不應該修改現有的程式碼，而應透過擴展來實現。**

舉例
~~~~~
假設我們有一個付款系統，最初只支援信用卡付款。如果要增加新的支付方式，比如 PayPal，這個類別需要被修改。

不正確範例，不符合OCP
~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: python

    class PaymentProcessor:
        def process_payment(self, payment_type, amount):
            if payment_type == "credit_card":
                self.process_credit_card_payment(amount)
            elif payment_type == "paypal":
                self.process_paypal_payment(amount)
            else:
                raise ValueError("Unsupported payment type")

        def process_credit_card_payment(self, amount):
            print(f"Processing credit card payment of {amount}")

        def process_paypal_payment(self, amount):
            print(f"Processing PayPal payment of {amount}")

    # 使用
    processor = PaymentProcessor()
    processor.process_payment("credit_card", 100)
    processor.process_payment("paypal", 150)

* 在這個範例中，每次我們新增一個支付方式，都需要修改PaymentProcessor類別中的process_payment方法，這違反了 OCP，*因為我們不得不修改既有的程式碼來實現擴展*。

正確範例，符合OCP
~~~~~~~~~~~~~~~~~~~~

.. code-block:: python

    from abc import ABC, abstractmethod

    # 抽象支付處理類別
    class PaymentMethod(ABC):
        @abstractmethod
        def process(self, amount):
            pass

    # 信用卡支付實現
    class CreditCardPayment(PaymentMethod):
        def process(self, amount):
            print(f"Processing credit card payment of {amount}")

    # PayPal 支付實現
    class PayPalPayment(PaymentMethod):
        def process(self, amount):
            print(f"Processing PayPal payment of {amount}")

    # 高階處理類別
    class PaymentProcessor:
        def __init__(self, payment_method: PaymentMethod):
            self.payment_method = payment_method

        def process_payment(self, amount):
            self.payment_method.process(amount)

    # 使用
    credit_card_processor = PaymentProcessor(CreditCardPayment())
    credit_card_processor.process_payment(100)

    paypal_processor = PaymentProcessor(PayPalPayment())
    paypal_processor.process_payment(150)

* 可以利用多型（Polymorphism） 和抽象類別(abstractmethod) 來實現開放封閉原則，讓每個支付方式有自己的類別，並透過擴展新增新的支付方式，而不修改既有的程式碼。
    * PaymentProcessor 類別不再需要知道具體的支付方式細節。相反，支付方式被封裝在不同的子類別中（如CreditCardPayment和PayPalPayment）。
    * 當需要新增支付方式時，比如Apple Pay，只需創建一個新的類別來實現PaymentMethod，而不需要修改現有的PaymentProcessor類別。
* 這樣的設計符合了**對擴展開放** (可以透過新增新的支付方式類別來擴展系統功能)與**對修改封閉** (現有的類別如PaymentProcessor無需任何修改，避免影響既有功能的穩定性)

.. note::
    * 為了符合OCP，設計Module、Class、Function時，需要考慮到未來該功能遇到擴充需求時，是否會需要改動到原來的程式碼?
    * 這也會與Unittest有關，當新增新功能時，執行舊有的unittest，就可以看出是否改動到了原本的程式碼。
    * 在設計時，考慮使用抽象類別或介面，以便於未來擴展。這樣可以透過擴展現有的類別來添加新功能，而不必修改現有的程式碼。
    * 儘量使用合成而非繼承來實現擴展，這樣能夠保持更好的靈活性和降低耦合度。可以將不同的功能模組化，並在需要時進行組合。

3. 里氏替換原則 (LSP)
---------------------

**子類別應該可以替換父類別，並且系統行為不會改變。也就是說，繼承的類別應能完全替代基類，而不會破壞程式邏輯。**

舉例
~~~~~
Bird(鳥類)是一個父類別，我們可以設計一個方法fly()來讓鳥類飛行。但是，如果有一個子類別Ostrich(鴕鳥)，它不具備飛行的能力，這時我們把Ostrich當作Bird來處理時，就會出現問題，違反LSP。

不正確範例，不符合LSP
~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: python

    # 父類：鳥類
    class Bird:
        def fly(self):
            print("Bird is flying")

    # 子類：鴕鳥
    class Ostrich(Bird):
        def fly(self):
            raise Exception("Ostriches can't fly")

    # 使用 Bird 類別的地方
    def make_bird_fly(bird: Bird):
        bird.fly()

    # 測試
    sparrow = Bird()
    make_bird_fly(sparrow)  # 正常，麻雀可以飛

    ostrich = Ostrich()
    make_bird_fly(ostrich)  # 錯誤，鴕鳥不能飛，但我們仍然試圖讓它飛

* Ostrich是Bird的子類，但是它覆寫了fly()方法，並引發了一個異常，因為鴕鳥不能飛。如果我們試圖使用鴕鳥來調用fly()，程式會產生錯誤。這違反了里氏替換原則，因為我們無法用子類Ostrich替換父類Bird，而不影響程式的正常運行。
    * 當我們寫 make_bird_fly 方法時，我們預期所有的 Bird 都能夠飛行。但是鴕鳥不能飛行，這打破了這一假設。
    * 子類Ostrich不能支持Bird類別中的飛行行為，則Ostrich不應該是Bird的子類，因為這會導致子類無法滿足父類的行為定義。

正確範例，符合LSP
~~~~~~~~~~~~~~~~~~~~

.. code-block:: python

    # 抽象的鳥類基類
    class Bird:
        def move(self):
            pass

    # 會飛的鳥類
    class FlyingBird(Bird):
        def fly(self):
            print("FlyingBird is flying")

    # 不會飛的鳥類
    class NonFlyingBird(Bird):
        def move(self):
            print("NonFlyingBird is walking")

    # 子類：麻雀，可以飛
    class Sparrow(FlyingBird):
        pass

    # 子類：鴕鳥，不能飛，但可以跑
    class Ostrich(NonFlyingBird):
        pass

    # 測試
    def make_bird_move(bird: Bird):
        bird.move()

    # 測試麻雀，可以飛行
    sparrow = Sparrow()
    make_bird_move(sparrow)  # Output: "FlyingBird is flying"

    # 測試鴕鳥，不能飛行，但可以跑
    ostrich = Ostrich()
    make_bird_move(ostrich)  # Output: "NonFlyingBird is walking"

* 在這個改進的例子中，我們將鳥類分成了會飛的FlyingBird和不會飛的NonFlyingBird。這樣，Sparrow可以繼承自FlyingBird，而Ostrich繼承自NonFlyingBird，從而正確表達了不同鳥類的行為差異。
* make_bird_move不再依賴於所有鳥都能飛行，而是根據具體的行為進行分類。
* 符合LSP: 由此案例，我們依然可以在父類Bird的範疇內處理不同子類別的行為，每個子類別都能在它所應該具備的行為上替換父類，而不會破壞程式邏輯，也不會因此出現異常。

.. hint::
    里氏替換原則要求*子類必須能夠替代父類而不影響程式的正確運行*。如果子類引入了與父類不同的行為，會導致程式的行為異常，這就違反了 LSP。

.. note::
    * 為了符合LSP，開發前必須確定好每個子類別所繼承的父類別，並確保子類別能夠正常使用父類別的方法和屬性。
    * 確保子類別的行為和父類別保持一致，這樣在需要用到父類別的地方（例如其他代碼或函數）時，子類別也能正常運作，不會出現意外錯誤。
    * 設計或測試子類別時，應該遵循父類別的「約定」，例如方法需要接受的參數、返回的結果等，這樣才能讓使用者對子類別的行為有正確的預期。

4. 介面分離原則(ISP)
---------------------

**客戶端不應該被迫依賴它們不需要的介面。也就是說，應將大的介面拆分成多個專門的介面，以確保類別只需實作與自己相關的介面方法。**

舉例
~~~~~
假設我們有一個基於圖形設計的系統，我們設計了一個Shape介面，所有形狀都應該實現這個介面。但是我們把所有可能的操作（如畫圖、計算面積、計算體積等）都放在一個介面中，這會導致某些類別必須實現它們不需要的功能。

不正確範例，不符合ISP
~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: python

    # 不符合 ISP 的設計
    class Shape:
        def draw(self):
            pass

        def calculate_area(self):
            pass

        def calculate_volume(self):
            pass

    # 圓形類
    class Circle(Shape):
        def draw(self):
            print("Drawing a circle")

        def calculate_area(self):
            return 3.14 * 5 * 5  # 假設半徑是5

        def calculate_volume(self):
            raise NotImplementedError("Circle has no volume")  # 圓形不應該計算體積

    # 立方體類
    class Cube(Shape):
        def draw(self):
            print("Drawing a cube")

        def calculate_area(self):
            return 6 * 10 * 10  # 假設邊長是10

        def calculate_volume(self):
            return 10 * 10 * 10  # 假設邊長是10

* 在這個設計中，Shape介面包含了所有形狀的操作，包括draw()、calculate_area()和calculate_volume()。然而，對於像Circle這樣的形狀，它並不需要calculate_volume()方法，因為圓形沒有體積。這迫使Circle類別去實現一個它不需要的功能，並在實現時引發異常。這違反了介面分離原則。

正確範例，符合ISP
~~~~~~~~~~~~~~~~~~~~

.. code-block:: python

    # 更小的專注介面
    class Drawable:
        def draw(self):
            pass

    class AreaShape:
        def calculate_area(self):
            pass

    class VolumeShape:
        def calculate_volume(self):
            pass

    # 圓形類：只需要繪製和計算面積
    class Circle(Drawable, AreaShape):
        def draw(self):
            print("Drawing a circle")

        def calculate_area(self):
            return 3.14 * 5 * 5  # 假設半徑是5

    # 立方體類：需要繪製、計算面積和計算體積
    class Cube(Drawable, AreaShape, VolumeShape):
        def draw(self):
            print("Drawing a cube")

        def calculate_area(self):
            return 6 * 10 * 10  # 假設邊長是10

        def calculate_volume(self):
            return 10 * 10 * 10  # 假設邊長是10

* 為了符合ISP，我們可以將Shape介面分解成更小、更專注的介面，例如一個負責平面形狀的介面 AreaShape和一個負責立體形狀的介面VolumeShape，這樣每個類別只需實現它們實際需要的介面。
* 上述範例中，我們將大介面Shape分解成了更小的介面，分別處理繪製(Drawable)、計算面積(AreaShape)和計算體積(VolumeShape)的行為。
* Circle 只需要實現Drawable和AreaShape，而Cube則實現所有三個介面，這樣每個類別只需關注它應該實現的功能，避免了無用功能的耦合。

.. note::
    * 為符合ISP，在設計介面時，應該將其保持簡潔，確保每個介面只包含客戶端所需的方法，避免將不相關的方法集中在同一介面中。
    * 跟SRP有些類似，但這邊是在描述介面的單一化。
    * 避免設計一個大介面，以防未來系統會需要實現一堆不必要的介面。
    * 測試時，確保每個類別正確實現了其所依賴的介面，並能在不影響其他類別的情況下進行獨立測試。
    * 簡單明瞭的介面才是最好的介面。這樣可以讓使用這些介面的類別更加專注，避免在不需要的功能上浪費時間和資源。

5. 依賴反轉原則 (DIP)
---------------------

**高階模組不應依賴低階模組，兩者都應依賴抽象。也就是說，系統應依賴於抽象而不是具體的實現，以提高彈性和可維護性。**

舉例
~~~~~
假設我們有一個通知系統，發送通知可能是透過電子郵件或簡訊的方式。高階模組直接依賴低階模組的具體實現。

不正確範例，不符合DIP
~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: python

    # 具體的 Email 通知類別
    class EmailNotifier:
        def send(self, message):
            print(f"Sending Email: {message}")

    # 高階模組
    class NotificationService:
        def __init__(self):
            self.notifier = EmailNotifier()  # 依賴具體的 EmailNotifier

        def notify(self, message):
            self.notifier.send(message)

    # 使用高階模組
    service = NotificationService()
    service.notify("Hello World!")

* 在這個範例中，NotificationService 直接依賴 EmailNotifier。如果將來我們想改用 SMS 通知，必須修改 NotificationService 內的程式碼，這就違反了依賴反轉原則。

正確範例，符合DIP
~~~~~~~~~~~~~~~~~~~~
透過引入抽象來解耦高階模組和低階模組，使它們都依賴於介面（或抽象類別），而不是具體實現。

.. code-block:: python

    # 抽象的 Notifier 介面
    from abc import ABC, abstractmethod

    class Notifier(ABC):
        @abstractmethod
        def send(self, message):
            pass

    # 具體的 Email 通知實現
    class EmailNotifier(Notifier):
        def send(self, message):
            print(f"Sending Email: {message}")

    # 具體的 SMS 通知實現
    class SMSNotifier(Notifier):
        def send(self, message):
            print(f"Sending SMS: {message}")

    # 高階模組依賴 Notifier 介面，而非具體的實現
    class NotificationService:
        def __init__(self, notifier: Notifier):
            self.notifier = notifier  # 依賴抽象的 Notifier 介面

        def notify(self, message):
            self.notifier.send(message)

    # 使用高階模組，並可以靈活地指定具體的通知方式
    email_service = NotificationService(EmailNotifier())
    sms_service = NotificationService(SMSNotifier())

    email_service.notify("Hello via Email!")
    sms_service.notify("Hello via SMS!")

.. hint::
    * 高階模組: 泛指那些包含業務邏輯、規則或流程的部分。這些模組不應該關心具體的實現細節。
    * 低階模組: 泛指那些實現具體功能的部分，如資料庫操作、API呼叫等。

* 高階模組 NotificationService 不再直接依賴於 EmailNotifier 或 SMSNotifier，而是依賴於 Notifier 這個抽象介面。這樣的設計符合 DIP。
* 低階模組 EmailNotifier 和 SMSNotifier 各自實作了 Notifier 介面，這意味著它們可以被替換，而不會影響 NotificationService 的邏輯。
* 如果將來想新增一個新的通知方式（比如 Telegram），只需創建一個實作 Notifier 介面的類別，並傳遞給 NotificationService，而不需要修改現有的程式碼。

.. note::
    * 為了符合DIP，低層模組的函數和功能應該透過介面來進行定義，這樣高層模組不會直接依賴於具體的低層模組。
    * 這樣的設計不僅能降低模組之間的耦合度，還能提高系統的可維護性和可測試性。
    * 可以輕鬆替換或模擬低層模組而不影響高層模組的運作。
    * 進行單元測試時，可以使用模擬（mock）物件或替代的實現來測試高層模組，這樣能確保高層邏輯的正確性，而不必依賴於低層的具體實現，這也使得測試更加獨立和可靠。

總結
---------------------

.. list-table::
    :widths: 20 50 30
    :header-rows: 1
    :class: centered-first-column

    * - 項目
      - 概念
      - 筆記

    * - 單一職責原則（SRP)
      - 強調每個類別應該只有一個變更的理由，即一個類別只應該負責一件事情
      - 類別內部的自律

    * - 開放封閉原則（OCP)
      - 強調擴展是開放的，修改既有程式碼是封閉的
      - 未來的擴展與維護

    * - 里氏替換原則（LSP)
      - 強調子類必須可以替換父類而不會改變系統的正確性。
      - 類別之間的契約

    * - 介面分離原則（ISP)
      - 強調介面需保持專一性，避免設計過於臃腫的大介面
      - 確保類別只實現必要的功能

    * - 依賴反轉原則（DIP)
      - 強調高層模組不應依賴低層模組，兩者應依賴於抽象
      - 降低模組間的耦合度

參考資料
---------------------
* 範例程式碼: ChatGPT4o
* `最佳實務系列：Python中的SOLID原則 <https://www.cnblogs.com/jeff-ideas/p/14646351.html>`_
* `如何在Python裡應用SOLID原則 <https://aju.space/2016/06/17/use-S-O-L-I-D-in-python.html>`_