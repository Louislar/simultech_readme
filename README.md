# simultech程式使用方法

## 目錄
[TOC]

## 所有檔案介紹
- ptrans_predict.py: 主要程式
- Train_data_new.csv: 預設輸入csv檔案

## 程式使用
:::warning
:zap: 注意: 請先自行在ptrans_predict.py的同資料夾下，創好一個名為model_ptrans的資料夾，以用來儲存訓練好的model
:::
1. 程式運行: 

    需使用command line運行程式

    輸入指令: python ptrans_predict.py <預設csv檔案位置> <訓練天數>

    範例指令: python ptrans_predict.py Train_data_new.csv 49

    範例指令說明: 
    1. Train_data_new.csv與ptrans_predict.py位於同一資料夾底下
    1. 我想取49天作為訓練的資料大小

2. 切換task做訓練/預測: 
    
    需於下圖中的判斷式(程式第238行)切換使用的task function
    
    ![image](https://raw.githubusercontent.com/Louislar/simultech_readme/master/simultech_main_function.png "changing place")
    
    圖中使用的task即為The index date prediction problem


## 程式輸出
1. 共同輸出

    + 任何一個task執行完後都會輸出一個model_best_all.hdf5在model_ptrans資料夾底下，預設為只儲存最好的model，若要將訓練過程中的model都儲存，只需要將程式碼中的第217行解除註解即可(如下圖)。
    
    ![image](https://raw.githubusercontent.com/Louislar/simultech_readme/master/simultech_save_multiModel.png "save multimodel")
    
    + 任何一個task執行完後都會在cmd上印出預測後的結果，即為使用測試資料預測的正確性，如下圖(以p~trans~ prediction problem(n=49)為例)
    
    ![image](https://raw.githubusercontent.com/Louislar/simultech_readme/master/simultech_predict_result.png "predict result")
    
    1. Original accuracy: 是原始預測的準確率
    2. Adjusted accuracy_1: 是把與正確level相鄰1個level的數值也當作正確數值並乘上0.75，然後加入正確數值數量當中計算正確率
    3.  Adjusted accuracy_1: 是把與正確level相鄰1個level的數值也當作正確數值並乘上1，然後加入正確數值數量當中計算正確率
    4.  neighbor predicted: 預測的數值與正確數值差一個level的個數
    5.  Correctly predict: 預測各個level正確的數量
    6.  Predict Neighbor: 預測到各個正確level正負一個level的數量
    7.  Wrong Predict: 預測該level錯誤的數量
    
    
    
3. 特別輸出
    + The index date prediction problem: 
        
        此task會額外輸出一個data_set.csv，csv當中第二欄之後會記錄用於訓練的n天的每一天個別感染人數，而第一欄為正確解答m，也就是流感初始發生於用於訓練的這n天的m周之前。
        
    + The peak date problem、The peak value problem

        這兩種task會額外輸出一個data_set_peakpredict.csv，csv當中第二欄以後會記錄用於訓練的n天的每一天個別感染人數，第一欄紀錄的是本列資料的正確答案，若為The peak date problem就是紀錄哪一天感染人數最多;若是The peak value problem就是紀錄最多感染人數的數量。

## 預設輸入csv檔的格式以及訓練資料大小
預設輸入的csv檔案為Train_data_new.csv

- csv內部格式說明如下: 

    每一列為一筆資料(tuple)，每一筆資料都包含366個欄位(attribute)

    第一個欄位為ptrans的數值，單位為1/1000

    例如: csv中為75，實際ptrans數值為0.0075

    而剩下的365個欄位就是一年當中，每一天的感染人數
    
- 訓練資料、驗證資料、測試資料拆分說明
    
    資料總數量為49065筆
    
    先將20%分出來做測試資料(test data)
    
    剩下80%再分成80%的訓練資料(train data)以及20%的驗證資料(validation data)

## 5個Task的名稱
總共有五個task，於論文內的名稱如下
- The p~trans~ prediction problem
- The peak date problem
- The peak value problem
- next day problem
- The index date prediction problem

## 5個task對應的程式內部function
論文內的名稱與python程式內的函數對應

|論文中的task名稱|程式中的function名稱|
|:------------:|:----------------:|
|<span class="text-nowrap"> **The p~trans~ prediction problem**</span>|<span class="text-nowrap"> **_read_data_ptranslevel()**</span>|
|<span class="text-nowrap"> **The peak date problem**</span>|<span class="text-nowrap"> **_read_data_peakdate()**</span>|
|<span class="text-nowrap"> **The peak value problem**</span>|<span class="text-nowrap"> **_read_data_peakvalue()**</span>|
|<span class="text-nowrap"> **next day problem**</span>|<span class="text-nowrap"> **_read_data_nextday()**</span>|
|<span class="text-nowrap"> **The index date prediction problem**</span>|<span class="text-nowrap"> **_read_data_indexdate()**</span>|

## 5個task function對訓練資料的取樣方法
|task名稱|訓練資料取樣方式|
|:------------:|:----------------:|
|<span class="text-nowrap"> **The p~trans~ prediction problem**</span>|<span class="text-nowrap"> **前n天的每一天感染人數**</span>|
|<span class="text-nowrap"> **The peak date problem**</span>|<span class="text-nowrap"> **前n天的每一天感染人數**</span>|
|<span class="text-nowrap"> **The peak value problem**</span>|<span class="text-nowrap"> **前n天的每一天感染人數**</span>|
|<span class="text-nowrap"> **next day problem**</span>|<span class="text-nowrap"> **前n天的每兩天感染人數的增加量**</span>|
|<span class="text-nowrap"> **The index date prediction problem**</span>|<span class="text-nowrap"> **隨機的連續n天**</span>|

* n可以透過command line的第二個參數做調整
* 以範例指令為例的話，n就是49，也就是連續49天

## 5個task function的目標
5個task個別會給予n天的數據當作訓練資料，接著利用深度學習預測出各種數值

|task名稱|預測目標|
|:------------:|:----------------:|
|<span class="text-nowrap"> **The p~trans~ prediction problem**</span>|<span class="text-nowrap"> **預測ptrans是多少(csv中的第一個欄位)**</span>|
|<span class="text-nowrap"> **The peak date problem**</span>|<span class="text-nowrap"> **預測感染人數最多的是哪一天**</span>|
|<span class="text-nowrap"> **The peak value problem**</span>|<span class="text-nowrap"> **預測感染人數最多的數量**</span>|
|<span class="text-nowrap"> **next day problem**</span>|<span class="text-nowrap"> **預測給定的連續天數的下一天的感染人數**</span>|
|<span class="text-nowrap"> **The index date prediction problem**</span>|<span class="text-nowrap"> **預測感染開始發生是哪一天(csv預設都是第一天)**</span>|







