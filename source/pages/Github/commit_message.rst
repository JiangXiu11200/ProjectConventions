===============================
Commit Message
===============================

Commit Message
================

為什麼Git Commit也要有規範?! Commit message重要嗎?
-------------------------------------------------------

當你的專案只有你自己一個人在開發，並且永遠不會有其他人時，你當然可以亂寫你的commit message，畢竟那是你自己的事情，不過你依然會遇到一個風險: 一個月後的你測試或開發到一半突然發現: oh! 這個bug好像之前遇過，來翻一下之前的控管紀錄好了..。阿! 慘了，我之前的紀錄到底在寫什麼鬼?!! 這問題到底在哪啊? 算了，重寫好了..

抑或是: Oh! 我這段程式到底在寫什麼? 這是三個月前寫的，奇怪，我怎麼會這樣寫呢? 來翻一下紀錄好了。阿! 慘了，我之前的紀錄到底在寫什麼鬼?!!

回到主題: Commit message重要嗎?
答案是: 肯定重要。

.. hint::
    Commit Message 最好兼俱 Why 及 What，讓日後進行維護的人員，得以更快進入狀況。

良好的Commit message可以讓日後維護更有效率，無論是你自己維護專案、多人一起開發，抑或是已經交接給其他人…，好的Commit可以讓任何參與專案的人員，都更快了解這些程式碼撰寫的原因。

Commit message的規範 - 約定式提交
-----------------------------------

`約定式提交 <https://www.conventionalcommits.org/en/v1.0.0/>`_ 是一個基於commit message的規範，這個規範的目的是讓commit message更具有可讀性，並且可以自動生成changelog。

約定式提交是一種輕量化的提交規則，利用一些簡單條件來建立明確的提交歷史。

1. 提交結構與說明

.. code-block:: bash

    <type>[optional scope]: <description>

    [optional body]

    [optional footer]

* type: 提交類型
    常用提交類型:
    * feat: 新增一個功能。
    * fix: 修正一個bug。
    * docs: 新增文件或註解。
    * style: 修改了程式的命名或編寫風格。(前提是不影響程式碼原有功能)
    * refactor: 重構程式碼。
    * test: 測試。
    * build: 建造了一個新的版本。(通常用於release new version)
    * chore: 其他不影響程式碼的變動。
* scope: 作用範圍
    * 作用範圍是可選的，用於描述commit影響的範圍。
* description: 提交描述。
    * 一個簡短的描述，描述這支Commit具體做了什麼事情。
* body: 提交內容
    * 提交的詳細內容，可以包含更多的資訊。 
* footer: 提交訊息。
    * 些重大改變或是其他標記，我們常用的是標記Issue訊息。

2. 提交範例

* 範例1. 修復Bug:

.. code-block:: bash

    fix(core):
    fix apply k8s error causes web data to fail to be deleted.

    (issue#13)

由此可知，這支Commit修復了'Core'這個Module的一個Bug，修復了因為k8s導致Web數據無法清除的問題，並且這問題被記錄於專案的issue#13。

`延伸閱讀: Creating issue <https://.....com/pages/Github/creating_issue>`_

* 範例2. 新增功能:

.. code-block:: bash

    feat(account):
    add an API name management page.

淺顯易懂，這支Commit對'account'這個Module新增了一個API name管理頁面。

* 範例3. 重構程式碼:

.. code-block:: bash

    refactor(core):
    refactor the code to make it more readable.

* 範例4. 測試:

.. code-block:: bash

    test (alert): add test for mail account
    adjust tests for alert_rule and alert

是否同樣淺顯易懂，看描述可知，這支Commit對'alert'這個Module新增了使用者信箱的測試，並且調整了測試內容。

總結
-----

1. 每次Commit都必須包含類型(type)、修改範圍(your_project_scope)、簡要描述(describe)，目前中心的專案，必須符合前三項，而內文(content)、與頁尾(footer)則依照你的需求，如何撰寫可以讓其他人一眼就看得懂你在推什麼。

2. 好的Commit message可以讓整體控管更具有結構性，讓協作、參與開發、未來交接，抑或是未來的你自己，有辦法知道，你得專案一路上到底做了什麼事情。

參考資料
--------

- `約定式提交 一種用於增加提交說明之人機可讀性意義的規範 <https://www.conventionalcommits.org/zh-hant/v1.0.0-beta.4/>`_
- `Git Commit Message 這樣寫會更好，替專案引入規範與範例 <https://wadehuanglearning.blogspot.com/2019/05/commit-commit-commit-why-what-commit.html>`_