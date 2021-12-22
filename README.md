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

