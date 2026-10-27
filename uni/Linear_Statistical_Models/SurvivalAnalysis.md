# 생존분석
사건의 발생과 **발생까지 걸린 시간**을 설명하고 예측
- 시간이 우리의 관심사
- 설명 + 예측

사건(의학)==실패failure(신뢰성)

하나의 연구에서 여러 개의 사건이 발생하는 경우는 다루지 않음 - 하나의 사건만이 일어난다고 가정

logit or probit 모형: Y가 0아니면 1일 때 취하는 모형 (사건의 발생만을 고려)\
생존분석: 사건의 발생 여부+발생까지 걸린시간을 모형 내에서 동시 분석\
time to evnets
- continuous time
- discrete time

## data
1. 각 개체마다 사건이 시작되는 시간과 끝나는 시간이 다름\
-> 생존시간이 정규분포를 따른다고 가정할 수 없음
2. 연구기간 내 follow up을 실패할 수 있음\
중도절단(censored)관측(중도절단과 사건발생은 서로 관련없음)

censoring데이터를 다루는게 중요한 문제

### Type of censored data
- uncensored: end time 이전에 사건 발생
- right cnesored: end time까지 사전 발생하지 않음 (신뢰성 분석에서 대부분)
- left cnesored: 시작하는 시점이 불분명한 것 
- interval censored
## 주요 용어
T: 특정 사건까지의 발생시간(확률변수)\
T>0인 모든 숫자가 가능

t: T가 가질 수 있는 특정 값

d: 중도절단 or 실패
- 0: 중도절단
- 1: 사건발생(실패)
### 주요 함수
기초 통계량
1. survival function: Time-Survival probability
2. hazard function
3. cumulative hazard function
4. mean residual life
