# Medical-Image-Captioning-with-BERT

[基於深度學習BERT語言模型之醫療影像報告生成系統](https://drive.google.com/file/d/1BNrEtlaVPoNLgjFw4i5U_ig6kUeeNTFK/view?usp=sharing)

Biomedical Image Report Generation System Based on Deep Learning BERT Language Model.

#運行環境

Windows 7

CPU:Intel i7-9700 3.00 GHz

GPU:GTX 2080 Ti

python3.6

Tensorflow-GPU 1.13

keras 2.3.1

bert4keras 0.7.9

OpenCv 4.2.0

tqdm

Numpy

Pandas

Matplotlib



#前處理

<img src="https://github.com/Rainman5042/Medical-Image-Captioning-with-BERT/blob/main/x-ray-chest.JPG?raw=true" width=60%>

IU X-Ray資料庫 (India University X Ray Dataset) 總共有3,955筆醫生所診斷的X光胸腔醫療影像報告，每筆報告中會附上一或兩張X光胸腔醫療影像，共7,470張影像。

資料庫由[kaggle](https://www.kaggle.com/raddar/chest-xrays-indiana-university)上所取得，影像已從DICOM轉成PNG，並將像素值轉換成0~255，並將影像使用黑色填補成正方圖形，再Resize成224x224，避免因縮放所造成的影像變形。

影像文字報告使用 Impression 以及 Finding 欄位做為主要的訓練資料，刪除額外標點符號，以及敏感個人資訊後，將文字報告字數大於15的資料作為訓練資料，共6990筆影像及文字。

#模型架構


<img src="https://github.com/Rainman5042/Medical-Image-Captioning-with-BERT/blob/main/BERT%20model.JPG?raw=true" width=100%>

# 如何運行

[下載模型權重以及X-Ray資料庫]()解壓縮到與X_RAY_kaggle_final_ver.ipynb同個資料夾中，執行X_RAY_kaggle_final_ver.ipynb。

## 訓練資料

count15.csv:為前處理過的文字資料。

count15_train.csv:亂數過後選取的6490筆訓練資料

count15_test.csv:亂數過後選取的500筆測試資料


## 訓練模型

可以選擇使用ResNet或是VGG作為CNN的主要模型

train_model = True使用選定的CNN模型重新訓練

train_model = False讀取模型權重並輸出預測結果

```
# VGG or ResNet
CNN_model = "VGG"

# 是否重新訓練，或是載入訓練好的模型
train_model = False
```

使用 GTX 2080 Ti 訓練時間約為11小時。

# 預測結果

可重複運行 ``just_show()`` 由測試資料中隨機選取5張影像輸出預測結果。

<img src="https://github.com/Rainman5042/Medical-Image-Captioning-with-BERT/blob/main/output.png?raw=true" width=100%>

```
image_id: 3268_IM-1551-1001.dcm.png
predict:
no acute cardiopulmonary process. the cardiomediastinal silhouette is within normal limits for size and contour. the lungs are normally inflated without evidence of focal airspace disease, pleural effusion, or pneumothorax. no acute bone abnormality.

references:
no acute cardiopulmonary abnormalities.the heart is normal in size and contour. there is no mediastinal widening. the lungs are clear bilaterally. no large pleural effusion or pneumothorax. the are intact.

bleu_1: 0.8091872791519434
```

運行 ``Evaluate_test(test_data)`` 預測並輸出500筆測試資料的平均評分結果(約45分鐘)。

```
{'rouge-1': 0.9224294936495717,
 'rouge-2': 0.6250400215426029,
 'rouge-L': 0.8715249880797927,
 'bleu_1': 0.6173024862047155,
 'bleu_2': 0.5454511397067838,
 'bleu_3': 0.48376701609267975,
 'bleu_4': 0.4289000705949729}
```
