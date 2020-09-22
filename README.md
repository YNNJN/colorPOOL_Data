# colorPOOL_Data

> 빅데이터 기반의 배색 추천 시스템 구현 프로젝트  
>
> 웹사이트에의 적용은 [colorPOOL](https://github.com/Locker-SSAFY/colorPOOL)에서 확인할 수 있습니다  

<br>

<br>

## Step 1. 데이터 전처리

> 수집한 Kuler 데이터의 전처리를 진행함. 자세히는 데이터를 양자화하고, 표본 색채를 설정하여, 조화 관계를 예측하기 위한 기준 데이터인 `기준 행렬`을 생성함  

<br>

### 0. 개요

> 1. 데이터 변환  
> 2. 데이터 양자화  
> 3. 기준 행렬 K 생성  

<br>

#### New :  

- feat : [데이터 로드 & 상위 데이터 추출](https://github.com/YNNJN/colorPool_Data/commit/ce40fad93eb3c46fa00a21818c8005314b1dccf7) 
- feat : [데이터 변환 (RGB -> XYZ -> CIE Lab)](https://github.com/YNNJN/colorPool_Data/commit/5ed97ba295e7d76ad4ed09dc5c4ab4d31c07654e)
- feat : [컬러 데이터 양자화](https://github.com/YNNJN/colorPool_Data/commit/33f9384e5bcbbbc7aee005a8a0a5b97286acd7f9)
- feat : [데이터 양자화 간격 조정 (5 -> 3)](https://github.com/YNNJN/colorPool_Data/commit/37e909231b52c21702705d31a2ea52c98b860260)
- feat : [기준행렬 생성을 위한 데이터프레임 형태 변환](https://github.com/YNNJN/colorPool_Data/commit/3623766f613b2f36109927e3a9e0a5f6f93f8a58)
- feat : [표본 색채 & 배색 설정](https://github.com/YNNJN/colorPool_Data/commit/8df53a314e9dd253bed615d52d75d6771831fc19)
- feat : [기준 행렬 생성](https://github.com/YNNJN/colorPool_Data/commit/0e4350098ab2ace5ad5e29686bf90360c327ca51)

<br>

#### Bug fixes :  

- fix : [데이터프레임 형태 변환 시의 오류 수정](https://github.com/YNNJN/colorPool_Data/commit/887d97f048d032e11e5c597c33e802c26ac58809)

<br>

#### Issues :  

- [데이터 전처리 중 양자화 간격 이슈 🔮](./docs/issues/데이터%20전처리%20중%20양자화%20간격%20이슈%20🔮.md)

<br>

### 1. 데이터 변환

> RGB → XYZ → CIE L*a*b. 

- RGB → XYZ 단계에서 `colormath`의 라이브러리를 사용함  
- XYZ → CIE Lab 단계에서 `colour-science`의 라이브러리를 사용함  

<br>

### 2. 데이터 양자화

> Kuler에서 활용되는 색채들 중 서로 비슷한 색들을 근사하여 분석하기 위하여, 데이터를 양자화 함  
>
> 더불어 색차 표본간의 거리를 사람이 인식하는 색차와 균등하게 보정하기 위한 보정을 진행함  

- [인지적으로 균등한 색채 이론](http://cs.haifa.ac.il/hagit/courses/ist/Lectures/IST05_ColorLAB.pdf)을 참고하여 dE(색 공간 내 기하학적 거리를 의미)의 값을 5 정도로 갖는 값이 적당하다고 판단함  
- 이 값을 결정하는 변수인 dL, da, db 의 값을 추산한 결과로, 최종 양자화 간격으로 3을 결정함  
- 양자화 후 dEh 값을 역으로 계산하여 양자화 한 값의 신뢰도를 검증하는 단계를 거쳤음  

<br>

### 3. 기준 행렬 K 생성

> 조화 관계를 예측하기 위한 기준 데이터로서, 기준 행렬을 생성함  
>
> 즉 각 배색이 어떤 색채를 포함하는 지를 나타내는 색채-테마 관계 기준 행렬을 의미함  

- N X M 행렬로  
  - N : 표본 색채 (7234)
  - M : 테마 (3000)
  - value :
    - 표본 색채(행)가 테마(열)에 포함될 경우 해당 테마의 rating 값
    - 표본 색채(행)가 테마(열)에 포함될 경우 해당 테마의 rating 값

<br>