# Recommend-a-product-based-on-the-consumer-s-preference-by-using-E-commerce-Data
데이터마이닝 수업 시간에 한 팀프로젝트로 주로 Clustring과 연관분석을 맡음.
## 프로젝트 소개
* 데이터 분석을 통한 각 고객에 대한 세분화된 제품 카테고리 추천 시스템
* 매년 증가하는 온라인 쇼핑 소비자들을 위해서, 자동적으로 그들의 need를 파악하여 필요한 물품을 추천하는 서비스를 제안
* 모든 물품 중 소비자들이 필요로 하는 상위 K개의 상품을 추천하여 만족도와 그들의 시간을 절약해줌
* 브랜드 별 리뷰를 분석하여 긍적적/부정적 평가 확인하여 추천된 카테고리 중 긍정적인 평가 제품에 대해서만 추천함.
## 데이터 소개
* 약 67501979개를 포함한 2019년 11월 e-commerce data를 사용함
* 사용자 별 웹 사이트 방문, 구매 기록 등 multi-category에 대한 사람들의 행동 분석

![image](https://user-images.githubusercontent.com/67357059/147014505-262d2623-b01e-4858-b3ee-3a0fb84082ac.png)
## EDA
* 월 초, 월 말 보다는 중순에 방문자가 증가
![image](https://user-images.githubusercontent.com/67357059/147014576-fc18ddb9-a9ce-4979-a972-e02b8c70a04b.png)

* 금요일과 주말에 방문자가 증가

![image](https://user-images.githubusercontent.com/67357059/147014657-ac444ac9-5818-44f5-995c-2c924b34fce7.png)

* Smartphone이 가장 높은 비율을 가짐

![image](https://user-images.githubusercontent.com/67357059/147014761-da8c064d-eacd-4c1c-9bdb-a4878bea207d.png)

## 데이터 전처리

* Event 횟수가 100번 이상인 122720명의 유저 중 10%에 해당하는 명의 유저들 샘플링
( 그 결과 65,701,979의 데이터 중 2,313,346개의 데이터 샘플링 3.52%)
* 제품의 종류와 브랜드 중 Event 횟수가 최대 Event 횟수의 1%인 종류와 브랜드 제거 (그 결과 2,313,345개의 데이터 샘플 중 1,061,845개의 데이터가 남음)
* 결측치의 경우 이상치 처리 부분에서 모두 제거됨.
* 브랜드 가치에 따라 사용자가 구매하고자 하는 상품이 달라져 Brand 별 가치 평가 데이터 추가함

![image](https://user-images.githubusercontent.com/67357059/147015070-cfdc7b7d-5149-4514-8e91-77cb09a5536a.png)

* Category Code 속성을 사용하기 위해 대분류, 중분류로 분할

![image](https://user-images.githubusercontent.com/67357059/147015116-5179e8dd-f40c-4bcb-a91a-aeb5e54ee5bc.png)

* EDA를 기반으로 낮과 밤 방문 여부, 요일별 방문 속성 추가, 구매 유무 정보 추가

![image](https://user-images.githubusercontent.com/67357059/147015210-32f932f6-d7f7-482d-995e-c00db4b824e5.png)

* 불필요한 속성 제거 (유저 아이디, 방문 시간 등)

![image](https://user-images.githubusercontent.com/67357059/147015254-3a7b3514-048e-41c4-98f1-f3cb9f38992c.png)

* 데이터 범주화를 위한 타입 변환(Label Encoding)

![image](https://user-images.githubusercontent.com/67357059/147015328-0738b369-c3d9-496b-963a-cbd947b560fd.png)

*Prodcuct_id로부터 제품 추출

![image](https://user-images.githubusercontent.com/67357059/147015356-78616b98-fe34-412f-ac6f-d52f5989a1a5.png)

* 제품의 종류와 브랜드 event 수를 이용한 인기 척도 추출

![image](https://user-images.githubusercontent.com/67357059/147015423-62fd917f-bc95-4978-9af1-5d73123b3313.png)

## Clustering

* 목적: 유저별 유형과 방문 기록을 기반하여 군집화를 통한 상품 추천
* 단계
  * weekday, day/night, event_type_cat 을 통해 유저별 개인 속성 군집을 생성 
  * 1번 과정을 통해 생성된 유저 별 유형 군집과 나머지 속성을 이용하여 2차 군집을 진행
  * 최종 군집을 통해 속성 별 상품 추천
* 첫번째 군집화를 통해 고객들의 유형 정보를 생성 (요일, 밤낮, 구매 유무를 이용해 유저별 특성 클러스터를 생성)
* TSNE 방법을 이용해 2차원으로 축호한 뒤 15개로 군집화 진행
 * Elbow Method를 사용해 SSE 값이 일정하게 유지되는 지점(K=15)를 군집 수로 선정
 * 군집화 시각화를 통해 벡터 공간 상 유저별 유형 분리 확인 (실루엣:0.753)

![image](https://user-images.githubusercontent.com/67357059/147016048-f6ba950f-ffda-4701-9a44-e77f34d744ba.png)

* 두번째 군집화를 통해 고객들에게 제품을 추천
 * 고객 유형 군집, 가격, 제품 종류, 브랜드, 제품의 종류, 브랭드 인기, 브랜드 평판을 이용해 새로운 군집 생성
* TSNE방법을 이용해 2차원으로 축소한 뒤 Elbow Method를 사용해 SSE값이 일정하게 유지되는 지(K=60)개로 군집화 진행

![image](https://user-images.githubusercontent.com/67357059/147016481-3969a521-0dd9-44f1-8a9e-455aac6c2f2f.png)

### Clustering 평가
* 내부지표
  * 실루엣 점수: 각 군집 간의 거리가 얼마나 효율적으로 분리되었는지 확인하는 척도
  * 0에서 1사이의 숫자로 표현되며 값이 1에 가까울 수록 군집이 효과적으로 분리됨을 의미
  * 제안 방법의 평균 실루엣 점수:0.5889
* 외부지표
  * SSE: 오차제곱합을 의미하며, 최적의 군집 수를 선택하는 지표
  * 제안 방법의 SSE: 0.00144 x 1e8
* 군집 별 케이스 분석
  * Cluster 2
    * Brand: Acer(0), Apple(20), Asus(30)
    * Main category: Computers
    * Sub_category: Notebook
    * 유저 성향: 금/낮/view
 
![image](https://user-images.githubusercontent.com/67357059/147016947-09471bd0-eee5-4a2f-8307-c95477af7dbe.png)

* 이외의 케이스 분석은 PPT참조바람

## 연관 분석
* 연관 분석을 통해 어떤 제품을 구매하는 구매자가 동시에 고려할 것으로 예측되는 제품을 찾아 광고 추천 시스템에 사용하고자 함
  * 연관 분석을 위해 각 고객에 대해 시간에 따라 구매하는 제품의 종류에 대한 trasaction들을 추출함

![image](https://user-images.githubusercontent.com/67357059/147017076-7f748e19-59e8-409f-8df3-09ad881bf0aa.png)

  * 최소지지도를 낮춰가며 흥미로운 결과를 찾아봄 (최종적으로 최소지지도는 0.1로 설정함)

![image](https://user-images.githubusercontent.com/67357059/147017147-d27640c3-7238-45a3-94bb-e0cc20ae0c44.png)

  * Lift=1로 설정한 후 연관 분석을 진행함

![image](https://user-images.githubusercontent.com/67357059/147017222-b50f0eed-9d7f-4561-ad3d-d733eae876ae.png)

  * 가정: 전자 시계를 구매하는 고객들은 헤드폰을 구매한다
  * 결과: Confidence 0.45, lift 1.7로 성립
  * 원인 파악: 헤드폰과 전자 시계는 운동을 위해 많이 사용함. 따라서 운동하는 사람들이 전자 시계와 헤드폰을 동시에 구매 고려할 것임
  * 결과 활용: 헤드폰이 있는 페이지에 전자 시계에 대한 광과를 진행함. 또는 전자 시계가 있는 웹페이지에 헤드폰에 대한 광고를 함
## 지도 학습을 이용한 상품 구매 예측
* XGBoost 알고리즘을 이용한 예측
  * 이전 모델에서의 실제 값과 예측 값의 오차를 Training 데이터에 투입하고 gradient를 이용하여 오류를 보완하는 방식
* XGBoost 알고리즘이란?
  * 약한 분류기를 세트로 묶어서 정확도를 예측하는 기법임
  * Greedy Algorithm을 이용해 분류기를 발견하고 분산 처리를 이용해 빠른 속도로 적합한 비중 파라미터를 찾는 알고리즘
  * 장점
    * 병렬 처리를 사용해 학습과 분류가 빠름
    * 다른 알고리즘과 연계하여 앙상블 학습이 가능함
* 모델의 목표: 사용자가 특정 제품을 구매할지 여부를 장바구니에 추가할 때 예측
* 결과: cart에 들어가기 위해서는 Weekday의 영향이 가장 컸으며 두번째로는 activiety_count의 영향이 컸음

![image](https://user-images.githubusercontent.com/67357059/147018038-98805432-9f5a-4893-b435-01dc4bc50842.png)

## 결과
* 클러스터링
  * 유저별 유형을 추출한 군집정보와 방문기록을 통해 군집 수행
  * 최종 군집 결과를 통해 사용자에게 필요한 제품을 추천
  * 결론
    * 클러스터링을 통해 실제 유저의 히스토리와 관련된 제품들을 추천해주는 것을 결과분석을 통해 확인함
* 연관분석
  * 클러스터링을 통해 추천한 제품과 연관성이 있는 제품을 사용자에게 추천
  * 결과
    * 연관분석을 통해 기존 데이터 관측을 통해 발견될 수 없었던 상품들 간의 연관성을 마이닝함
    * 사용자가 샀던 상품의 제품만 추천하는 것이 아니라 상품들 간의 추천도 가능함을 결과분석을 통해 확인함 
* 머신러닝 알고리즘을 통한 상품 구매 예측
  * 머신러닝 알고리즘을 통해 사용자가 히스토리를 기반으로 상품을 장바구니에 담을 때, 상품을 구매할지 여부를 예측
  * 결론
    * 사용자 히스토리를 기반으로 상품을 구매 예측 성능은 68%를 달성
    * Information gain 지표를 통해 구매를 할 때 중요하게 사용되는 사용자의 특징정보(요일, 이벤트 발생 횟수) 역시 확인가능
* 향후 연구
  * 데이터 측면:
    * 2019년 11월 웹 쇼핑 데이터 뿐만 아니라 다른 기간의 다양한 데이터에 적용시켜 계절에 따른 상품을 추천하는 기능 추가
    * 유저 별 속성이 많이 포함된 데이터를 사용하여 유저들을 더 세분화하여 제품을 추천
  * 모델 측면:
    * 유저 별로 더욱 세분화하여 각 유저 클러스터에 대해 연관분석을 진행한 후 유저 속성에 따른 어떤 제품과 연관된 제품 추천 시스템 구축
    * 구매 영향도를 바탕으로 feature 별로 weight를 다르게 주어 클러스터링과 연관분석 진행



