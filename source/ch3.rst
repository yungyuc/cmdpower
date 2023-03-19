*****************************
3. 使用指令列做文字處理
*****************************

3.1 ``sed`` 
================
``sed`` 是 **stream editor** 的縮寫，意即對資料流做編輯。
除此之外，``sed`` 強大的功能在於能自動化地對文字做插入、刪除等行為。

常見的用法是對檔案內容作文字匹配，接著將配對出來的文字做處理，
最後可以將修改後的結果輸出至檔案，或是將匹配的結果輸出至螢幕。

接下來會介紹幾個常見的使用方式，若需要可以使用 ``sed --help`` 來查看完整的指令。

.. highlight:: bash

``sed`` 的使用格式::

     sed [OPTION]... {script-only-if-no-other-script} [input-file]...

|

輸出文字
*********************
``sed`` 預設會將處理過後的檔案全部印出來。我們可以使用 ``-n`` 這個選項來關閉預設輸出行為，
並自訂輸出特定範圍來輸出資料。使用命令 ``p`` (print, 印出) 自訂輸出範圍格式: ``'行數p'``。

.. highlight:: bash
   :linenothreshold: 1

輸出指定範圍的內容::

     $sed '3p' file.txt #即使設定輸出範圍，缺少 -n 仍會印出所有 file.txt 的內容
     $sed -n '3p' file.txt #印出 file.txt 第 3 行的內容
     $sed -n '1,3p' file.txt #印出 file.txt 第 3 行的內容

|

搜尋並取代文字
**********************
.. highlight:: none

假設 ``input.txt.`` 的內容如下::

     Hello world. Hello world.
     This an apple.
     Hello world.

使用命令 ``s`` 對 ``input.txt`` 做文字取代。使用方式: ``'s/樣板/替換文字/[標籤]'``。
運作的方式很簡單， ``s`` 命令會搜尋輸入資料內的符合 "樣板" 的字串，並將其取代為 "替換文字"。
除此之外，還可以使用 "標籤" 來做其他的文字處理。

.. highlight:: bash
     :linenothreshold: 1

基本使用方式 ::

     $sed 's/Hello/world/' input.txt #將 hello 取代為 world
     world world. Hello world.
     This an apple.
     world world.

使用標籤 ``g`` ::

     $sed 's/Hello/world/g' input.txt #將 hello 取代為 world
     world world. world world.
     This an apple.
     world world.

標籤 ``g`` 意味者全部的、所有的 (global) 。當我們加上標籤 ``g`` 後，將會取代 **所有** 匹配成功的字串。 

|

同時使用多個指令
*****************
當我們想要同時使用多個指令對輸入資料做文字處理，可以使用 ``-e``、 ``;`` 將指令做區隔。
或是將所有需要的指令寫成一個腳本 (script)，並使用 ``-f``  來讀取指令。

假如我們想要同時對 ``input.txt`` 作刪除和替換文字。
刪除第一行，並將 ``an`` 取代為 ``AN``。

1. 使用 ``-e`` ::

     $sed -e '1d' -e 's/an/AN/' input1.txt
     This AN apple.
     Hello world.

2. 使用 ``;`` ::

     $sed '1d ; s/an/AN/' input1.txt
     This AN apple.
     Hello world.



3.2 ``cut`` 
================

3.3 ``awk`` 
================

3.4 ``jq`` 
================
``jq`` 可以想成是用 ``sed`` 來處理 json 檔案。 

3.5 ``gron`` 
================