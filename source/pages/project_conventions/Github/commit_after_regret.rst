=============================================
Git Flow - 延伸操作2. Commit 後 後悔了
=============================================

很多時候我們會不小心連續修改了好幾個檔案才想到之前的功能忘記 Commit ，或是一懶惰，就懶得修改玩一個功能就馬上 Commit ，這時候很常發生不小心只 Commit 了一小部份，或是漏掉了一些檔案應該要加在同一支 Commit 上，這時候該怎麼重新 Commit 呢?

延伸操作2. Commit 後 後悔了
-----------------------------

情境1.  Commit 後 後悔了，但我還沒 Push
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

假設我們 Commit 了一支 feature_1 ，但我發現目錄底下還有檔案忘記加了。

.. image:: /assets/Github/commit_after_regret-1.jpg

解決方法
~~~~~~~~~

1. 到你的上一支 Commit 右鍵，點選 "Reset current branch to this commit"。

.. image:: /assets/Github/commit_after_regret-2.jpg


2. 選擇 "Soft" 或 "Mixed" ，這樣就會回到你上一支 Commit ，並且你的修改還是會保存在你的目錄中。

.. image:: /assets/Github/commit_after_regret-3.jpg

.. hint::
    模式說明:

    Mixed: 預設模式，回到所選取分支並將暫存檔案丟棄，但不會修改到本地端的檔案。
    (白話文: 你的檔案不會被丟棄，但你需要重新將變更加到暫存內。)

    Soft: 回到所選取的分支，並且暫存區的檔案不會被丟棄，也不會修改到本地端的檔案。
    (白話文: 你會在暫存區看到你剛剛 add 的檔案，不需要重新選取。)

    Hard: 回到你所選分支，並且丟目錄與暫存區的檔案。
    (白話文: 回到這支Commit預設狀態，你的所有變更將會被刪除)


如果只是要整理 Commit 內容，請你選取 Soft ，切記，一但選擇 Herd ，你的程式碼會被刪除。

3. 選取 Soft 後，History 上看不到你剛剛的 Commit 。

.. image:: /assets/Github/commit_after_regret-4.jpg

並且可以在 File Status 中看到，原本 Commit 的檔案還在暫存中，此時你就可重新整理 Commit 了!

.. image:: /assets/Github/commit_after_regret-5.jpg


情境2.  Commit 後 後悔了，而且我已經 Push 上去 Remote repository
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. image:: /assets/Github/commit_after_regret-6.jpg

假設我們已經 Commit 並且推送到遠端了，通常不太建議在去修改，因為這有可能會影響到其他同樣在線的開發者，不過，如果只有你自己開發，你想要修改，也可以! 但注意，這會將你原本 Push 到遠端的 Commit 覆蓋掉!


解決方法
~~~~~~~~~~~~~

1. 到你的上一支 Commit 右鍵，點選 "Reset current branch to this commit"，選擇 Soft 或 Mixed ，回到上一次提交狀態。

.. image:: /assets/Github/commit_after_regret-7.jpg

2. 重新 Commit 你新的一次修改，如下圖，此時你的分支會從上一次 Commit 分叉出去

.. image:: /assets/Github/commit_after_regret-8.jpg

3. Push 時勾選 Force Push ，這樣就會將你的 Commit 覆蓋掉遠端的 Commit

.. image:: /assets/Github/commit_after_regret-9.jpg

.. hint::
    SourceTree 上開啟 Force Push 功能的方法:

    1. 點選最上方選單列的 "Tools"，選擇 "Options"。
    2. 點選 "Git"。
    3. 勾選 "Enable Force Push"。

4. 這樣就完成了! 可以看到原本的 "feat: feature_1" 被覆蓋掉了。

.. image:: /assets/Github/commit_after_regret-10.jpg
