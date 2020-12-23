## Hybried Recommend System

####개요
  본 프로젝트에서는 Steam게임의 API와 Steam 유저의 프로필을 크롤링 하여 데이터를 수집하여   진행한다. 먼저 게임의 특성들을 종합하여 이를 기반으로 게임을 추천해주는 콘텐츠 기반 추천 서비스를 개발한다. 이후 Steam 유저들이 구매한 게임들을 바탕으로 Matrix를 구성하여 협업 필터링을 구현하여 게임 추천 서비스를 개발하고자 한다. 마지막으로 콘텐츠 기반 추천 서비스와 협업 필터링 기반 추천 서비스 간 최적의 가중치를 파악하고 이를 기반으로 하이브리드 추천 서비스를 개발하고자 한다.
  
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
    
#### 데이터전처리

1. 게임 별 5개의 Attribute를 가진 데이터프레임 형성


2. 유저별 보유 게임리스트
  (1) Time_rating 형성
      플레이시간을 기준으로 time_rating 전환
      
  (2) Latent Factor 모델
      원데이터의 NaN값을 처리하기 위하여 Latent Factor[n(factor)=20] 임베딩 진행
      
#### 모델링


