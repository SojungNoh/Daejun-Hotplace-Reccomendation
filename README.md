# Daejun-Hotplace-Reccomendation

인공지능개론(CS470) 수업은 각 팀별로 인공지능 시스템을 구축해보는 프로젝트를 한 학기동안 진행하도록 한다.
학생들은 Computer Vision / Recommendation System / Reiforced Learning / Natural Language Processing 등으로 구성된 7개의 지정된 프로젝트를 진행하도록 되어있다.
원한다면 직접 별도의 프로젝트를 제안해도 된다 하여,우리 팀은 실용적으로 도움 될만한 것을 만들어보고자 했고, 그리하여 우리가 한 학기동안 목표로 잡은 것은:
"카이스트 학생들을 위한 대전 핫플레이스 추천시스템" 이다. 이 컨텐츠는 추천시스템을 직접 만들어 본 경험과, 추후 개선할 점을 정리해본다.    
    
    
# Needs & Goals
대전은 '노잼도시'라는 오명이 있을 정도로, 2030세대가 즐길만한 컨텐츠가 부족한 도시다. 카이스트는 도심에서 거리가 떨어져 있는 지역이기에, 카이스트 학생들은 대부분의 식사시간을 근처 대학가에서 보내곤 한다. 이들이 없는 시간을 쥐어짜내 외부의 장소를 방문할 경우, 한번 쓰는 시간을 양질의 장소에서 보낼 필요가 있다고 생각했다. 현재 이들이 사용하고 있는 블로그/평점사이트를 검색하는 것 이외에, 획기적으로 개개인에 최적화된 맛집 추천 시스템이 있다면, 더 시성비(시간 대비 성능)있게 시간을 보낼 수 있으리라 보았다.
따라서 목표는 카이스트학생들에게 최적화된 추천시스템을 만드는 것이다.  

![캡처0](https://user-images.githubusercontent.com/52244004/174429991-addf0403-b422-4969-b5bf-7b626aa315fb.PNG)
    
    

# Contents
  1. Data Gathering / Data Preprocessing
  2. Modeling
  3. Recommendation
  4. Evaluation 
  5. Discussion    

![캡처](https://user-images.githubusercontent.com/52244004/174429997-9c00634e-8321-4e44-a254-6da41b6da210.PNG)
 
    
    
### Packages
- [ml_metrics](https://pypi.org/project/ml_metrics/)
- [scipy](https://scipy.org/)
- [sklearn](https://scikit-learn.org/stable/)
- [implicit](https://implicit.readthedocs.io/en/latest/)
- [torch](https://pytorch.org/)
- [torch_geometric](https://pytorch-geometric.readthedocs.io/en/latest/)
- [numpy](https://numpy.org/)
- [pandas](https://pandas.pydata.org/)
- [tqdm](https://tqdm.github.io/)
    
    
### 1. Data Gathering / Data Preprocessing
  초기의 목표가 '카이스트'학생들을 위한 핫플레이스 추천시스템이었지만, 프로젝트 초기에는 수백명의 설문조사 데이터를 수집할 자신이 없었기 때문에 웹크롤링을 이용하는 방법을 선택했다(어쩌면 여기서부터 우리의 실수가 시작된 것일지도). 그 이유는 자료를 크롤링 할 수 있는 평점 사이트가 여러개가 있고, 보통 사람이라면 웹사이트마다 같은 사용자 아이디를 사용하지 않을까 하는 생각에서였다. 그렇게 KakaoMap / Diningcode / Siksin / Mangoplate 4개 웹사이트에서 상위 100개 맛집의 속성정보와, 맛집별로 15명의 평점을 수집했다.
  4개 웹사이트의 자료를 식당별로 매칭해본 결과, dataset이 지나치게 sparse하게 나오는 치명적인 문제가 발생했다. 우리 예상과 달리, 아이디가 겹치는 일은 거의 없었고, 어떤 웹사이트에서는 사용자 아이디를 암호화하기도 했기 때문에, 유의미한 데이터를 얻지 못했다. 따라서 가장 널리 사용되고 있는 KakaoMap의 데이터만 사용함으로써 사용자와 데이터의 일관성을 확보하기로 했다. 결론적으로 도출한 데이터는 다음 두가지다. 
- KakaoMap 상위 500개 맛집의 Item Information ⓐ  
- KakaoMap 상위 500개 맛집의 15개 User-Ratings Dataset ⓑ     

![캡처2](https://user-images.githubusercontent.com/52244004/174430010-27c21944-cd34-4f43-ab8f-cb9eac9f11c7.PNG)

    
    
## 2. Modeling 
**(1) Contents Based Recommendation (cosine similiarity)**    
**(2) Collaborative Filtering (alternating least squares)**     
**(3) Graph-Based Recommendation (Light GCN)**     
  이렇게 세가지 모델을 활용해 추천을 하기로 했다. 전통적인 방식인 Contents Based Recommendation / Collaborative Filtering를 사용해 기본적인 성능을 파악해보고,
1번과 2번 모델에 쓰인 데이터셋을 둘 다 활용해 추천성능을 높여보기 위해 Graph-based Recommendation을 도입해보기로 했다. 프로젝트 흐름은 다음과 같다.     

![캡처3](https://user-images.githubusercontent.com/52244004/174430017-8ee474ce-f90e-466e-8898-657ef220311f.PNG)

    
    
## 3. Recommendation
  최종적인 추천은 다음과 같이 이루어졌다. 결과를 도출하기는 했지만, 유저의 True Like와 겹치는 아이템이 모든 방식에서 거의 없는 것을 알 수 있다.   

![캡처5](https://user-images.githubusercontent.com/52244004/174430157-f31c5c89-72f7-4176-b706-eed2b1afd21c.PNG)

      
      
## 4. Evaluation
  결과 해석을 하기 위해 도입한 매트릭은 MAP@K, NDCG다. 각각 적절한 결과의 랭킹이 높을수록 더 높은 값을 내는 방식이기 때문에, 추천시스템에 적합한 평가모델이다. MAP@K는 Precision을 기반으로 한 평가방식으로, 실제 추천에 유저의 like 아이템이 있었는지를 확인하는 Binary 방식이다. NDCG도 같은 방식이지만, Precision 대신 Relevancy를 사용한다. 추천된 아이템과 유저의 like 아이템간의 relevancy를 계산에 사용하는 non-binary 방식이다. 결과가 이렇게 나오긴 했지만, 무지성으로 이 matrix를 도입하는건 문제가 있을 수 있다. 왜냐하면 우리는 무지성으로 위 매트릭스를 반영했는데, 이 방식에서의 전제는; *유저 A가 좋아한다고 한 아이템을 정확히 맞추는* 방식이기 때문이다. 추천시스템이라면 *유저 A가 아직 경험해보지는 못했지만, 좋아할만한 아이템을 제시하는* 방식이어야한다고 생각한다.  
 
![캡처6](https://user-images.githubusercontent.com/52244004/174430031-0d54d671-f468-417f-bad4-ae005b8c7936.PNG)

아무튼 결과가 전반적으로 아주 처참하다.. 그럼에도 불구하고 의미를 부여하자면,    
- **Contents-Based Recommendation은 더 정교한 Hand engineering이 필요하다**      
   Random 추천만도 못한 결과를 보였다. 이렇게 된 이유는 ⓐ의 Feature가 적절하지 않기 때문이지 않을까 싶다. 더욱 정교한 feature map이 있어야 더 나은 추천을 할 수 있다.     
- **Collaborative Filtering / Graph-Based는 성능이 확보된 모델이다** 
   이런 최악의 상황에서도 랜덤 추천보다는 성능이 약 3배 낫기 때문이다.         
      
      
## 5. Discussion
  전반적으로 낮은 성능이 낮은 이유를 파악하기 위해, 추천시스템의 성능을 평가할 때 벤치마크가 되는 Movielens-1M의 데이터셋과 우리의 데이터셋을 비교해봤다.   
  
![캡처7](https://user-images.githubusercontent.com/52244004/174430040-b35929c6-58c5-4141-bec6-eb563aa53eac.PNG)   
    
Movielens-1M 데이터는 모든 사용자들이 적어도 20개 아이템에 대한 평가를 내린 반면, 우리 데이터는 고작 5% 밖에 안되는 사람들만이 적어도 5개 아이템을 평가했다.
이는 Data Sparsity를 비교해봄으로써 알 수 있는 부분이다. 본 프로젝트를 통해 유의미한 추천을 하기 위해서는 다음과 같은 부분을 개선해야하지 않을까 한다.    

**(1) 교내의 지인들을 동원해 User-survey를 진행했어야**    
  임의의 온라인 유저 데이터를 크롤링 할 것이 아니라, 처음부터 귀찮음을 감수하고 서베이를 통해 우리가 원하는 데이터를 정확하게 얻었어야 한다.ㅠㅠ
다양한 스펙트럼의 가게(Item)를 적어도 20개 정도 평가해달라고 부탁했다면, 처음부터 Sparse 하지 않은 데이터로 실험을 진행할 수 있었을 것이다.    

**(2) Item들의 평점/리뷰수/위치/카테고리만 Item Information ⓐ에 반영할 것이 아니라, 분위기 tag도 포함했어야**    
  Contents-base의 평가가 처참했던 것은, 정량적인 데이터들만 인풋으로 넣었기 때문이다. 맛집 평가 전용 웹사이트인 Siksin과 Mangoplate는 각 가게별 분위기 Tag도 함께 제공한다.
조금만 시간을 들여서 이 부분을 합쳐서 진행했더라면 보다 정교한 ⓐ 데이터셋을 얻었을 것이다.    

**(3) 이를 바탕으로 유저의 실제 Like와 relevancy가 높은 아이템을 추천해야, 실제 Like 아이템이 아니라**    
  현재의 추천은, 유저가 좋다고 평가한 것을 정확히 맞추는 방식이다. 하지만 위에 초록 글씨에도 적었지만, 이것은 전제가 틀렸다. 추천된 아이템과 유저가 좋아한 아이템간의 relevancy를 계산하는 매트릭을 별도로 만들었어야 정확한 평가가 이루어진다고 생각한다.      
      
      
  여러모로 한 학기동안 처음만난 멤버들과 패기 넘치는 텀프로젝트를 진행했다. 추천시스템을 직접 구현해보는 게 처음이기도 했고, 우리 모두 장님이 코끼리 만지듯 진행했지만, 프로젝트 실패의 원인을 규명할 수 있었던것만으로도 의미있는 실험이었다고 생각한다. Thanks everyone for the hard work! @ YTY, KJH, KM 
