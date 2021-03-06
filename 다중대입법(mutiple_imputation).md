# 다중대입법(Mutiple Imputation)
- 다중대입법은 시뮬레이션을 여러번 반복해서 결측값을 채워 넣는 방법
- MI 방법에서는 missing data가 있는 데이터셋이 있을 때 simulation을 통해서 누락된 데이터를 채운 complete dataset을 3~10개를 만듬
- missing data를 <b>몬테카를로 방법</b>을 이용해서 채움  
- 각각의 dataset에 대해 표준적인 통계방법을 이용하여 그 결과를 바탕으로 통계 결과 및 신뢰 구간을 제공해 줌

## 몬테카를로 방법
- 난수를 이용하여 함수의 값을 확률적으로 계산하는 알고리즘
- 방법은 다양하지만, 다음과 같음
  - 가능한 입력의 도메인을 정의함
  - 도메인에 대한 확률 분포에서 임의로 입력을 생성
  - 입력에 대한 결정론적인 계산 수행
  - 결과를 집계
- 한 예시로 원의 넓이를 구하는 방법을 생각할 수 있음
- 반지름이 1인 원과 1인 사각형이 있다고 할때, 해당 도메인 안에 무수히 많은 점을 그려  
  원 안에 포함된 점의 개수를 셀 수 있으면 원의 넓이를 근사하게 계산할 수 있음 

## mice 패키지 작동 방법 분석
- 먼저 누락된 자료가 있는 dataFrame을 `mice()`함수를 적용하게 되면 누락된 값은 dataFrame에 있는 다른 모든 변수를 사용하여 값을 예측하여 채워 넣음
- 이렇게 누락된 자료가 채워진 dataset을 3개~10개정도 만듬(default로는 5개 만듬)
- 대입 과정에는 random 구성 성분이 있기 때문에 dataset마다 조금씩 다름
- 각각의 완성된 데이터셋에 대해 `with()`함수를 사용해서 통계모형을 순서대로 적용함
- 마지막으로 `pool()`함수를 적용하여 이들의 각각 분석 결과를 하나로 합침
- 최종모형의 표준오차와 p값은 누락된 자료와 다중 대입에 의한 불확실성을 정확하게 반영함
~~~r
require(mice) 
imp <- mice(mydata,m)     # dataset을 m개 만든다. 디폴트는 5 
fit <- with(imp,analysis) # analysis는 lm(), glm(), gam(), nbrm() 등이다  
                          # fit에는 m개의 분석결과가 들어간다 
pooled <- pool(fit)       # pooled 는 m개의 분석결과의 평균이다               
summary(pooled)
~~~

## mice 패키지 사용 예시



## 용어 정리
- 몬테카를로 방법
  - 난수를 이용하여 함수의 값을 확률적으로 계산하는 알고리즘
  