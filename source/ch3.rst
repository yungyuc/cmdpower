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

指令 ``cut`` 主要是用來做文字擷取，並將結果輸出。 ``cut`` 會針對每一行輸入作切分，來取的我們需要的文字。
常見的切分方式有 3 種。
     1. 使用位元組 (bytes)
     2. 使用字元 (characters)
     3. 使用區塊 (fields)


.. highlight::bash

``cut`` 的使用格式::

     cut OPTION... [FILE]...

從 ``cut`` 的定義格式來看，我們必須明確定義 OPTION。否則將會得到錯誤::
     
     $cut input.txt
     cut: you must specify a list of bytes, characters, or fields
     Try 'cut --help' for more information.

``-b`` 
************
``-b, --byte`` ，意指使用位元組 (byte) 來定義文字擷取的範圍。因為每一個字元 (character) 的長度是
1 個位元組，所以我們可以使用

.. highlight::bash

明確標示位元組的位置::

     $cut -b 1,2,3 input_cut.txt

使用連字符號 (``-``) 來標示位元組的範圍::

     $cut -b 1-3,6-9 input_cut.txt
     $cut -b 1- input_cut.txt
     $cut -b -3 input_cut.txt


``-d``
**********
``-d, --delimiter``，使用自訂的分隔符號 (delimiter) 來切分文字。可以和下一個 ``-f`` 搭配使用。

``-f``
***********
``-f, --fields``， ``-f`` 預設使用 TAB 當作分隔符號來切分輸入的字串，
並依據我們設定的數字，擷取特定範圍的欄位(field)。

.. highlight::bash

     $cut -f 2 input.txt
     $cut -d " " -f 1-2 input.txt #使用空白作為分隔符號，並輸出第 1~2 個欄位。


3.3 ``awk`` 
================

3.4 ``jq`` 
================
``jq`` 可以想成是用 ``sed`` 來處理 json 檔案。 

3.5 ``gron`` 
================