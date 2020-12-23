## Hybried Recommend System

#### 개요

  본 프로젝트에서는 Steam게임의 API와 Steam 유저의 프로필을 크롤링 하여 데이터를 수집하여   진행한다. 먼저 게임의 특성들을 종합하여 이를 기반으로 게임을 추천해주는 콘텐츠 기반 추천 서비스를 개발한다. 이후 Steam 유저들이 구매한 게임들을 바탕으로 Matrix를 구성하여 협업 필터링을 구현하여 게임 추천 서비스를 개발하고자 한다. 마지막으로 콘텐츠 기반 추천 서비스와 협업 필터링 기반 추천 서비스 간 최적의 가중치를 파악하고 이를 기반으로 하이브리드 추천 서비스를 개발하고자 한다.
  
---------------
  
#### 데이터 수집

1. 게임정보
  - 게임 고유 ID 수집
  
    https://steamspy.com/api
  - ID를 통한 게임 정보 수집
  
    http://store.steampowered.com/api/appdetails
  
2. 유저가 보유한 게임리스트
  - 17자리 유저 고유번호 수집 (Steam 리뷰 페이지 크롤링)
  
  - Steam API (GetOwnedGames (v0001)) 활용
  
    https://developer.valvesoftware.com/wiki/Steam_Web_API#Game_interfaces_and_methods
    
    -----------------
#### 데이터전처리

1. 게임 별 5개의 Attribute를 가진 데이터프레임 형성


2. 유저별 보유 게임리스트

    (1) Time_rating 형성
        플레이시간을 기준으로 time_rating 전환
      
      
    (2) Latent Factor 모델
        원데이터의 NaN값을 처리하기 위하여 Latent Factor[n(factor)=20] 임베딩 진행
      
 ---------------
      
#### 모델링

1. Content Based Filtering

   각 게임의 Description을 활용하여 그 게임의 설정과 플레이 방식을 유추하여 비슷한 게임을 추천하고자 하였다. 이를 위해 활용한 것은 TF-IDF 라이브러리이다. 분석 대상으로는 단어 2개를 기준으로 해당 게임의 특징을 도출하고자 하였다. 그리고 Vector화된 게임의 특징들을 Cosine Similarity 모델을 통해 유사도 검증을 진행하였다. 그리고 유사도가 95% 이상인 게임들 중 20개를 추천할 수 있도록 하였다.


2. Collaborative Filtering

   유저가 보유한 게임과 다른 유저가 보유한 게임을 비교하여 새로운 게임을 추천해주는 협업필터링이다. 협업필터링에 사용한 알고리즘은 Matrix Factorization이다. User-Item 데이터프레임을 기반으로 피어슨 상관계수를 활용하여 유사도를 검증한 후 게임을 추천하는 방식이다.



3. Hybrid Filtering

      (1) 하이브리드 필터링은 Content-Based-Filtering과 Collaborative-Filtering로 도출된 rating을 가중치를 넣어 게임을 추천하는 Parallelized hybridization을 진행한다. 추천 시스템에 있어 산업적 학계적 사례에 따라 Collaborative Filtering의 가중치(0.95)를 Content-Based-Filtering 보다 높은 가중치(0.90)를 부여한다.
   
   
      (2)또다른 하나의 하이브리드 모델은 CBF와 CLF 추천결과를 비교하여 산출하는 모델링이다. 먼저 CBF와 CLF 모두가 추천한 게임을 우선 추천 게임으로 선정하고 CBF와 CLF 각각 1~3위까지의 게임을 번갈아가면서 추천하게 된다.

