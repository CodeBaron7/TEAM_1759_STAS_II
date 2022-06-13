# 肺腺癌病理切片影像之腫瘤氣道擴散偵測競賽 II：運用影像分割作法於切割STAS輪廓



程式架構說明於報告 (貳、程式碼) 小節內

以下為重現實驗教學

   下載程式碼
```
git clone https://github.com/CodeBaron7/TEAM_1759_STAS_II.git
```

## 資料準備

1. 至官方Download Dataset下載SEG_Train_Datasets、Private_Image
2. 下載預先轉換為Mask圖像的Train_Masks : https://drive.google.com/file/d/1EZZcXG09Fk5XDALJapJRfatVtHiQbGj2/view?usp=sharing
3. 下載Train1與Train2已訓練完成的模型 : https://drive.google.com/file/d/1phcAQ1sSoFUQ4JhGbVQFZNSXE_mzLqv-/view?usp=sharing

準備好後將資料照著下方檔案結構，擺放資料，請注意檔名與路徑需一致。
### 檔案結構:
```
.
└── TEAM_1759_STAS_II
      ├─code
      |   ├─Train1.ipynb       # 第一次訓練
      |   ├─Train2.ipynb       # 第二次訓練
      |   └─Inference.ipynb  
      ├─Dataset
      |   ├─Private_Image      # From Private_Image.zip
      |   ├─Train_Images       # From SEG_Train_Dataset.zip
      |   └─Train_masks        # From https://drive.google.com/file/d/1EZZcXG09Fk5XDALJapJRfatVtHiQbGj2/view?usp=sharing
      ├─result
      |   ├─submit_images      # Inference後提交圖片存放處
      |   ├─Train1_models      # From https://drive.google.com/file/d/1phcAQ1sSoFUQ4JhGbVQFZNSXE_mzLqv-/view?usp=sharing
      |   └─Train2_models      # From https://drive.google.com/file/d/1phcAQ1sSoFUQ4JhGbVQFZNSXE_mzLqv-/view?usp=sharing
      ├─requirements.txt           
      └─README.md  
```
注意: Dataset 與result 內各資料夾僅有一層，例如Private_Image打開即為Private_00000000.jpg ~ Private_00000183.jpg

## 環境配置:

   以下2種方式可2擇1選用:

### 1.執行於TWCC CCS cm.super
      
   容器映像檔pytorch-22.02-py3
  
   至requirements.txt目錄下安裝其餘函式庫
```sh
pip install -r requirements.txt
```
### 2.於Local端執行

   硬體要求 : 

   建立虛擬環境並進入
```
Python -m venv stas
cd stas\Scripts
activate
```
   更新pip與setuptools
```sh
python -m pip install --upgrade pip
pip install --upgrade setuptools
```
   依cuda版本安裝對應pytorch函式庫
```sh
pip3 install torch torchvision torchaudio --extra-index-url https://download.pytorch.org/whl/cu113
```
   至requirements.txt目錄下安裝其餘函式庫
```sh
pip install -r requirements.txt
```

## Train
   至STAS\code路徑下輸入  
```sh
ipython
```
### 第1次訓練
   執行Train1.ipynb
```sh
%run Train1.ipynb
```
   訓練後模型存放於result/Train1_models
### 第二次訓練
注意: 第二次訓練時 result/Train1_models資料夾內要有第一次訓練之模型

可自行訓練或是至https://drive.google.com/file/d/1phcAQ1sSoFUQ4JhGbVQFZNSXE_mzLqv-/view?usp=sharing 下載
   
   執行Train2.ipynb
```sh
%run Train2.ipynb
```
訓練後模型存放於result/Train2_models
## Inference
注意:使用result/Train2_models內best_retrain_Fold00、01、02.bin模型做Ensemble

   至STAS/code路徑下輸入  
```sh
ipython
```
   執行Inference.ipynb
```sh
%run Inference.ipynb
```
Inference後圖片存放於result/submit_images
