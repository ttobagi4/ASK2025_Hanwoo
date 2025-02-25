# 🥇 한우 육질등급 및 품질 판정 자동화 알고리즘 구현 (ASK 2025)
<h3> Implementation of an Automated Algorithm for Determining Korean Beef Meat Quality <br><br>
저자 : 김민지 , 공저자 : 이규원, 이성재, 류병석, 김영균 <br><br>

<h2> ✔️ 전체 알고리즘 </h2> <br>

![image01](https://github.com/user-attachments/assets/0212c8c0-be86-4570-9485-a9b87b6254ba) <br>

<h3>
본 알고리즘은 Python 3.12.7, OpenCV 4.11.0 버전을 기반으로 구현되었으며, Numpy 및 os 라이브러리를 함께 활용 <br><br>
데이터셋은 AI 허브의 ‘축산물 품질(QC) 이미지’에서 한우 등급별 이미지 715장을 사용했으며, 이 중 15장은 알고리즘 구축에, 나머지 700장은 정확도 향상과 결과 산출에 활용</h3> <br><br>

<h2> 1. 이미지 전처리  </h2>
<h3>- 양방향 필터(Bilateral Filter)로 노이즈를 제거<br>
- R, G, B 채널에 각각 0.2126, 0.7152, 0.0722의 가중치를 적용한 가중 평균을 사용해 전체적인 밝기를 계산<br>
- 이미지의 선명도를 향상하기 위해 라플라시안 필터(Laplacian Filter)와 언샤프 마스크(Unsharp Mask)를 적용<br>
- OpenCV의 GrabCut 알고리즘을 활용해 배경을 제거하고 관심 영역(ROI)을 분리</h3><br>

![image02](https://github.com/user-attachments/assets/be1e766f-bc9f-4a5a-8122-7a3475727d13) <br><br>

<h2> 2. 근내지방도 계산 </h2>
<h3>- 양방향 필터(Bilateral Filter)로 노이즈를 제거<br>
- 이미지를 그레이스케일(Grayscale)로 변환한 후, 이진화(Binarization)를 적용하여 육질과 지방부 픽셀값을 각각 0과 255로 구분<br>
- 근내지방도(Marbling Score)는 지방 비율에 따라 1에서 9 사이의 정수로 결정</h3><br>

![image03](https://github.com/user-attachments/assets/e9627222-58c9-4230-87e2-63d1a03cb274) <br><br>

<h2> 3. 육질부 및 지방부 대표색 선정 </h2>
<h3>- CIE Lab 색 공간과 ΔE(CIE76) 색 차이 지표를 활용<br>
- 전처리된 RGB 이미지를 CIE Lab 색 공간으로 변환한 후, K-means 클러스터링을 적용하여 육질부와 지방부, 2개의 클러스터로 나눔 <br>
- 육색의 정상 범위는 논문의 <표 1>의 No. 2에서 6까지, 지방색은 No. 1에서 6으로 설정</h3><br>
  
![image04](https://github.com/user-attachments/assets/4a66792d-855b-4c8f-ba14-b78e675882f1) <br><br>

<h2> 4. 조직감 판정 </h2>
<h3>- CIE Lab 색 공간과 ΔE(CIE76) 색 차이 지표를 활용<br>
- 검은 배경은 제외하고 육질부와 지방부만 포함하는 단면 이미지를 HSV 색 공간으로 변환 <br>
- 데이터로 사용한 1++ 등급 한우 이미지 100장의 HSV 분석 결과를 기반으로 평균값 기준을 설정한 결과, H값 평균이 94 이하이고 S값 평균이 167 이상이면 조직감(보수력 및 탄력성)이 우수한 것으로 판단</h3><br>

<h2> 5. 육질등급 및 품질 판정 </h2>
<h3>- CIE Lab 색 공간과 ΔE(CIE76) 색 차이 지표를 활용<br>
- 모든 항목에 대한 분석이 완료되면 논문의 (그림 5)과 같은 결과가 출력 <br>
- 육질등급(intramuscular_fat)은 근내지방도에 따라 5가지로 구분
- 육색(meat_result)과 지방색(fat_result)은 정상(Acceptable) 또는 비정상(Unacceptable)으로 판정 
- 조직감은 H값 평균이 94 이하이고 S값 평균이 167 이상이면 '양호(Good)'으로, 두 조건 중 하나라도 충족하지 않으면 '불량(Poor)'으로 출력</h3><br>

![image05](https://github.com/user-attachments/assets/d9edba8e-46cc-4659-839f-94191504967e) <br><br>

<h2> 6. 연구 결과 </h2>
<h3>- CIE Lab 색 공간과 ΔE(CIE76) 색 차이 지표를 활용<br>
- 전체 육질등급 판정 정확도 평균은 79.7% <br>
- 1++에서 3등급까지의 판정 정확도는 각각 84.29%, 82.14%, 85.71%, 83.57%</h3><br>

![image06](https://github.com/user-attachments/assets/affc0578-16eb-41de-802d-3b4f59d0c2a7) <br><br>

  
