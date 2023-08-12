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
1. mask_rle to polygon : yolov8-segmentation 모델을 사용하기 위해서 mask_rle 형식의 label을 normalize된 polygon 형태로 변환하는 알고리즘  
 1) rle_decode 함수를 통해서 사진의 mask label을 얻는다. (이때 mask를 transpose해야된다.)
 2) mask label에서 상, 하, 좌, 우의 값이 모두 1인 값을 제외한 point만 저장한다. (즉 건물의 모서리만 저장)
 3) connect_coordinate 함수를 통해서 x, y좌표의 차이가 1 이하인 point들을 하나의 건물로 묶는다.
 4) 2 ~ 3까지의 과정을 한 사진에서 건물의 수 만큼 반복한 후에, 각 point를 normalize시키고 txt로 저장한다.
 5) 1 ~ 4를 사진마다 반복한다.

2. polygon sorting : mask_rle to polygon에서의 label을 더 깔끔하게 분류하기 위한 알고리즘, 건물 모서리의 점이 polygon을 깔끔하지 않게 만든다는 것을 파악한 후에 이를 해결하기 위해 작성  
 1) mask_rle to polygon의 알고리즘으로 생성된 txt파일을 읽어온다.
 2) create_new 함수를 통해서 건물의 point중 하나를 선정하고 그 point와 거리가 가장 가까운 건물의 point를 채워나가면서 polygon을 정렬한다.
 3) 정렬된 polygon에서 순서대로 세 개의 point를 뽑아서 두개의 vector를 만든다. (ex vector1 = (x2 - x1, y2 - y1), vector2 = (x3 - x2, y3 - y2)) 
 4) x좌표 변화량과 y좌표 변화량 중에 하나가 같다면 그대로 남겨두고, 아니라면(모서리) 리스트에서 제외한다.
 5) 리스트를 txt로 저장한다.
 5) 1~4를 txt파일 마다 반복한다.

![슬라이드1](https://github.com/Blusooyeon/swAI2023/assets/122274660/d9aa6e99-4d80-4f7e-bdee-2927d494fd25)

3. train,valid dataset 이미지 증강 : albumentation 라이브러리의 x 축 대칭, y축 대칭, transpose 를 통해서 data의 개수를 증가시켰다.  
