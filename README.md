# Daejun-Hotplace-Reccomendation

인공지능개론(CS470) 수업은 각 팀별로 인공지능 시스템을 구축해보는 프로젝트를 한 학기동안 진행하도록 한다.
학생들은 Computer Vision / Recommendation System / Reiforced Learning / Natural Language Processing 등으로 구성된 7개의 지정된 프로젝트를 진행하도록 되어있다.
원한다면 직접 별도의 프로젝트를 제안해도 된다 하여, 우리 팀은 실용적으로 도움 될만한 것을 만들어보고자 했고, 그리하여 우리가 한 학기동안 목표로 잡은 것은:
"카이스트 학생들을 위한 대전 핫플레이스 추천시스템" 이다. 이 컨텐츠는 추천시스템을 직접 만들어 본 경험과, 추후 개선할 점을 정리해본다.


## Contents
1. Data Gathering / Data Preprocessing
2. Modeling
3. Recommendation
4. Evaluation 
5. Discussion

## 1. Data Gathering / Data Preprocessing
초기의 목표가 '카이스트'학생들을 위한 핫플레이스 추천시스템이었지만, 프로젝트 초기에는 수백명의 설문조사 데이터를 수집할 자신이 없었기 때문에 웹크롤링을 이용하는 방법을 선택했다(어쩌면 여기서부터 우리의 실수가 시작된 것일지도). 그 이유는 자료를 크롤링 할 수 있는 평점 사이트가 여러개가 있고, 보통 사람이라면 웹사이트마다 같은 사용자 아이디를 사용하지 않을까 하는 생각에서였다. 그렇게 KakaoMap / Diningcode / Siksin / Mangoplate 4개 웹사이트에서 상위 100개 맛집의 속성정보와, 맛집별로 15명의 평점을 수집했다.

4개 웹사이트의 자료를 식당별로 매칭해본 결과, dataset이 지나치게 sparse하게 나오는 치명적인 문제가 발생했다. 우리 예상과 달리, 아이디가 겹치는 일은 거의 없었고, 어떤 웹사이트에서는 사용자 아이디를 암호화하기도 했기 때문에, 유의미한 데이터를 얻지 못했다. 따라서 가장 널리 사용되고 있는 KakaoMap의 데이터만 사용함으로써 사용자와 데이터의 일관성을 확보하기로 했다. 결론적으로 도출한 데이터는 다음 두가지다. 

- KakaoMap 상위 500개 맛집의 Item Information ⓐ
- KakaoMap 상위 500개 맛집의 15개 User-Ratings Dataset ⓑ

## 2. Modeling 
(1) Contents Based Recommendation
(2) Collaborative Filtering
(3) Graph-Based Recommendation
이렇게 세가지 모델을 활용해 추천을 하기로 했다. 전통적인 방식인 Contents Based Recommendation / Collaborative Filtering를 사용해 기본적인 성능을 파악해보고,
1번과 2번 모델에 쓰인 데이터셋을 둘 다 활용해 추천성능을 높여보기 위해 Graph-based Recommendation을 도입해보기로 했다. 프로젝트 흐름은 다음과 같다.

![Layout](https://user-images.githubusercontent.com/52244004/174427047-54bff9cb-c231-4c48-bd18-1efbff2cdae8.PNG)

## 3. Recommendation
최종적인 추천은 다음과 같이 이루어졌다. 결과를 도출하기는 했지만, 유저의 True Like와 겹치는 아이템이 모든 방식에서 거의 없는 것을 알 수 있다.
![Layout2](https://user-images.githubusercontent.com/52244004/174427326-cf954d69-ee1b-4c70-aa93-6fd8d5929017.PNG)

## 4. Evaluation
결과 해석을 하기 위해 도입한 매트릭은 MAP@K, NDCG다. 각각 적절한 결과의 랭킹이 높을수록 더 높은 값을 내는 방식이기 때문에, 추천시스템에 적합한 평가모델이다.
- MAP@K는 Precision을 기반으로 한 평가방식으로, 실제 추천에 유저의 like 아이템이 있었는지를 확인하는 Binary 방식이다.
- NDCG도 같은 방식이지만, Precision 대신 Relevancy를 사용한다. 추천된 아이템과 유저의 like 아이템간의 relevancy를 계산에 사용하는 non-binary 방식이다.

![Layout3](https://user-images.githubusercontent.com/52244004/174427660-3b441e37-50d0-475f-8b77-9b06fcce0610.PNG)
결과가 전반적으로 아주 처참하다.. 그럼에도 불구하고 의미를 부여하자면,

#### - Contents-Based Recommendation은 더 정교한 Hand engineering이 필요하다.
Random 추천만도 못한 결과를 보였다. 이렇게 된 이유는 ⓐ의 Feature가 적절하지 않기 때문이지 않을까 싶다. 더욱 정교한 feature map이 있어야 더 나은 추천을 할 수 있다.
#### - Collaborative Filtering / Graph-Based는 성능이 확보된 모델이다.
이런 최악의 상황에서도 랜덤 추천보다는 성능이 약 3배 낫기 때문이다.

## 5. Discussion
