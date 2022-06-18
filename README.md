# Daejun-Hotplace-Reccomendation

인공지능개론(CS470) 수업은 각 팀별로 인공지능 시스템을 구축해보는 프로젝트를 한 학기동안 진행하도록 한다.
학생들은 Computer Vision / Recommendation System / Reiforced Learning / Natural Language Processing 등으로 구성된 7개의 지정된 프로젝트를 진행하도록 되어있다.
원한다면 직접 별도의 프로젝트를 제안해도 된다 하여, 우리 팀은 실용적으로 도움 될만한 것을 만들어보고자 했고, 그리하여 우리가 한 학기동안 목표로 잡은 것은:
"카이스트 학생들을 위한 대전 핫플레이스 추천시스템" 이다. 이 컨텐츠는 추천시스템을 직접 만들어 본 경험과, 추후 개선할 점을 정리해본다.


## Contents
1. Data Gathering
2. Data Preprocessing
3. Modeling
4. Recommendation
5. Evaluation 

## 1. Data Gathering
초기의 목표가 '카이스트'학생들을 위한 핫플레이스 추천시스템이었지만, 프로젝트 초기에는 수백명의 설문조사 데이터를 수집할 자신이 없었기 때문에 웹크롤링을 이용하는 방법을 선택했다(어쩌면 여기서부터 우리의 실수가 시작된 것일지도). 그 이유는 자료를 크롤링 할 수 있는 평점 사이트가 여러개가 있고, 보통 사람이라면 웹사이트마다 같은 사용자 아이디를 사용하지 않을까 하는 생각에서였다. 그렇게 KakaoMap / Diningcode / Siksin / Mangoplate 4개 웹사이트에서 상위 100개 맛집의 속성정보와, 맛집별로 15명의 평점을 수집했다.
