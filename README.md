# simultech程式使用方法

## 所有檔案介紹
- ptrans_predict.py: 主要程式
- Train_data_new.csv: 預設輸入csv檔案

## 程式使用
需使用command line運行程式

輸入指令: python ptrans_predict.py <預設csv檔案位置> <訓練天數>

範例指令: python ptrans_predict.py Train_data_new.csv 49

範例指令說明: 
1. Train_data_new.csv與ptrans_predict.py位於同一資料夾底下
1. 我想取49天作為訓練的資料大小


## 5個Task的名稱
總共有五個task
- prediction problem
- The peak date problem
- The peak value problem
- next day problem
- The index date prediction problem

## 5個task對應的程式內部function
論文內的名稱與python程式內的函數對應

|論文中的task名稱|程式中的function名稱|
|:------------:|:----------------:|
|<span class="text-nowrap"> **prediction problem**</span>|<span class="text-nowrap"> **_read_data_ptranslevel()**</span>|
|<span class="text-nowrap"> **The peak date problem**</span>|<span class="text-nowrap"> **_read_data_peakdate()**</span>|
|<span class="text-nowrap"> **The peak value problem**</span>|<span class="text-nowrap"> **_read_data_peakvalue()**</span>|
|<span class="text-nowrap"> **next day problem**</span>|<span class="text-nowrap"> **_read_data_nextday()**</span>|
|<span class="text-nowrap"> **The index date prediction problem**</span>|<span class="text-nowrap"> **_read_data_indexdate()**</span>|

## 5個task function對訓練資料的取樣方法
|task名稱|訓練資料取樣方式|
|:------------:|:----------------:|
|<span class="text-nowrap"> **prediction problem**</span>|<span class="text-nowrap"> **前n天**</span>|
|<span class="text-nowrap"> **The peak date problem**</span>|<span class="text-nowrap"> **前n天**</span>|
|<span class="text-nowrap"> **The peak value problem**</span>|<span class="text-nowrap"> **前n天**</span>|
|<span class="text-nowrap"> **next day problem**</span>|<span class="text-nowrap"> **前n天**</span>|
|<span class="text-nowrap"> **The index date prediction problem**</span>|<span class="text-nowrap"> **隨機的連續n天**</span>|

* n可以透過command line的第二個參數做調整
* 以範例指令為例的話，n就是49，也就是連續49天

## 5個task function的目標
5個task個別會給予n天的數據當作訓練資料，接著利用深度學習預測出各種數值
|task名稱|預測目標|
|:------------:|:----------------:|
|<span class="text-nowrap"> **prediction problem**</span>|<span class="text-nowrap"> **預測ptrans是多少(csv中的第一個欄位)**</span>|
|<span class="text-nowrap"> **The peak date problem**</span>|<span class="text-nowrap"> **預測感染人數最多的是哪一天**</span>|
|<span class="text-nowrap"> **The peak value problem**</span>|<span class="text-nowrap"> **預測感的染人數最多的數量**</span>|
|<span class="text-nowrap"> **next day problem**</span>|<span class="text-nowrap"> **預測給定的連續天數的下一天的感染人數**</span>|
|<span class="text-nowrap"> **The index date prediction problem**</span>|<span class="text-nowrap"> **預測感染開始發生是哪一天(csv預設都是第一天)**</span>|

## 預設輸入csv檔的格式
預設輸入的csv檔案為Train_data_new.csv

csv內部格式說明如下: 

每一列為一筆資料(tuple)，每一筆資料都包含366個欄位(attribute)

第一個欄位為ptrans的數值，單位為1/1000

例如: csv中為75，實際ptrans數值為0.0075

而剩下的365個欄位就是一年當中，每一天的感染人數





