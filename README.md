# SW중심대학 공동 AI 경진대회   

https://dacon.io/competitions/official/236092/overview/description    
위성 이미지의 건물 영역 분할(Image Segmentation)을 수행하는 AI모델을 개발   
2023.07.03 ~ 2023.07.28   

-------------------
### 시도해본 모델  

Unet  
mmdetection - faster-RCNN  
mmrotate - rtm-detection  
yolov8

-------------------
### 최종 모델 
yolov8-segmentation  

-------------------
### 데이터 전처리  
1. RLE인코딩된 데이터 -> mask  
2. mask -> polygon
![슬라이드1](https://github.com/Blusooyeon/swAI2023/assets/122274660/d9aa6e99-4d80-4f7e-bdee-2927d494fd25)

3. train,valid dataset 이미지 증강
