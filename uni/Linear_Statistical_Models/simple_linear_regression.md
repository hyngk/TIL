# Simple Linear Regression

## Simple Linear Regression이란?
하나의 설명변수 X를 이용하여 하나의 반응변수 Y의 평균적인 변화를 직선으로 설명하거나 예측하는 방법이다
- Y: 반응변수(response variable), 종속변수(dependent variable)
- X: 설명변수(explanatory variable), 독립변수(independent variable), 예측변수(predictor)

e.g.\
-공부시간 X로 시험점수 Y를 설명\
-광고비 X로 판매량 Y를 예측\
-경력 X로 연봉 Y를 설명
$$
Y=\beta_0+\beta_1X+\varepsilon
$$

## Covariance
두 변수가 함께 어떻게 변하는지를 나타내는 값 (방향)\
선형관계만을 측정 (X,Y가 비선형적인 방식으로 관여되있을 수 있음)
### 기본 아이디어
1. 각 관측값이 평균에서 얼마나 떨어져 있는지를 봄 (편차)
$$
y_i-\bar{y},\ x_i-\bar{x}
$$
2. 두 편차의 곱을 생각
$$
(y_i-\bar{y})(x_i-\bar{x})
$$
- X와 Y가 동시에 평균보다 크거나 동시에 작으면 곱이 양수
- 하나는 평균보다 크고 다른 하나는 작으면 곱이 음수

### 표본 공분산
$$
\operatorname{cov}(X,Y)
=
\operatorname{cov}(Y,X)
=
\frac{1}{n-1}
\sum_{i=1}^{n}
(x_i-\bar{x})(y_i-\bar{y})
$$
- $x_i, y_i$: $i$번째 관측값 $(i=1,\ldots,n)$
- $\bar{x}, \bar{y}$: 각각 $X, Y$의 표본평균 ($\mu_X, \mu_Y$: 각각 $X, Y$의 모집단평균)
- $n$: 관측값의 개수
- $n-1$: 자유도
### 부호 해석
부호는 Y와 X사이 선형 관계의 방향을 나타냄\
if cov(X,Y)>0, a positive relationship between Y and X\
if cov(X,Y)<0, a negative relationship between Y and X\
if cov(X,Y)=0, no linear relationship between Y and X

But, 공분산의 크기를 보고 관계의 강도를 비교하면 안됨.\
공분산은 측정 단위가 바뀌면 값도 변하기 때문\
e.g.\
-키, cm/m -> 실제 관계는 같지만 공분산의 값은 달라짐\
-소득을 원 단위로 측정하다가 만 원 단위로 바꾸면 공분산 값도 바뀜

=> 관계의 강도를 비교할 땐 단위의 영향을 제거한 상관계수(correlation coefficient)를 사용
## Correlation coefficient
공분산의 단위 문제를 해결하기 위해 X와 Y를 표준화 한 뒤 공분산을 계산한 것\
관계의 방향과 강도 모두 나타냄\
측정단위가 바뀌어도 변하지 않음\
선형관계만을 측정 (X,Y가 비선형적인 방식으로 관여되있을 수 있음)

### standardize the data
$$
z_{y,i}=\frac{y_i-\bar{y}}{s_y}
$$

$$
z_{x,i}=\frac{x_i-\bar{x}}{s_x}
$$
- Z has mean=0 and sd=1
- $s_y$: the sample standard deviation of Y
- $s_x$: the sample standard deviation of X
$$
s_y
=
\sqrt{
\frac{1}{n-1}
\sum_{i=1}^{n}
(y_i-\bar{y})^2
}
$$
$$
s_x
=
\sqrt{
\frac{1}{n-1}
\sum_{i=1}^{n}
(x_i-\bar{x})^2
}
$$
### standardized X and Y
$$
\operatorname{cor}(Y,X)
=
\frac{1}{n-1}
\sum_{i=1}^{n}
\left(
\frac{y_i-\bar{y}}{s_y}
\right)
\left(
\frac{x_i-\bar{x}}{s_x}
\right)
=
\frac{cov(Y,X)}{s_ys_x}
$$
$$
=r_{XY}
=
\frac{1}{n-1}
\sum_{i=1}^{n}
z_{x,i}z_{y,i}
$$
$
-1 \le cor(Y,X) \le 1
$
### 해석
- 부호는 방향 나타냄
- -1 또는 1에 가까울수록 Y와 X사이의 선형관계가 더 강해짐
- if cor(Y,X)=0, no linear relationship between Y and X
### 특징
- 대칭적
$$Cor(X,Y)=Cor(Y,X)$$
X와 Y의 역할이 구분되지 않음
- 인과관계를 의미하지 않음

상관계수가 높다고 해서 X가 Y의 원인이라는 뜻은 아님

## Simple Linear Regression Model
$$
Y=\beta_0+\beta_1X+\varepsilon
$$
- $\beta_0,\beta_1$: parameter
- $\varepsilon$: error

Each observation\
$
y_i=\beta_0+\beta_1x_i+\varepsilon_i,\ i
$=1,2,...,n
## 상관계수와 회귀의 차이
| 구분 | 상관계수 | 회귀분석 |
|---|---|---|
| 목적 | 선형관계의 방향·강도 측정 | X로 Y 설명·예측 |
| 변수 역할 | X,Y가 대칭적 (둘 다 중요) | X와 Y의 역할이 다름 (Y가 더 중요) |
| 결과 | 단위 없는 하나의 수 | 절편과 기울기를 가진 식 |
| 인과관계 | 증명하지 못함 | 단독으로 인과관계를 증명하지 못함 |

단순회귀에서는 기울기의 부호와 상관계수의 부호가 같음\
: $\hat{\beta}_1$는 $Cov(Y,X),Cor(Y,X)$와 항상 같은 부호
$$
\hat{\beta}_1
=
\frac{\operatorname{cov}(Y,X)}
{\operatorname{Var}(X)}
=
\operatorname{cor}(Y,X)
\frac{s_y}{s_x}
$$
## Parameter Estimation
$Estimate\ parameter\ \beta_0\ and\ \beta_1$\
goal: 반응변수와 예측변수에 대한 산점도의 점들을 가장 잘 대표하는 line찾기
### LSM(Least Squares Method)
The parameters are estimated using the least squares method
$$
\min_{\beta_0,\beta_1}
\sum_{i=1}^{n}
(y_i-\hat{y}_i)^2
$$
#### fitted value
회귀직선으로 계산한 Y의 값
$$
\hat y_i=\hat\beta_0+\hat\beta_1x_i
$$
#### 잔차(residual)
실제 점과 회귀직선 사이의 수직거리(vertical distance)

즉, 실제값과 fitted value의 차이
$$
e_i=y_i-\hat y_i
$$
잔차의 합은 0\
$\therefore$ 잔차를 제곱해서 더한다

$
\sum_{i=1}^{n}e_i^2
$ : SSE (Sum of Squared Errors)

즉, 모든 점과 직선 사이의 수직거리 제곱의 합이 가장 작아지는 직선을 찾는다

즉, 최소제곱법은 SSE를 최소화하는 방법이다
## 회귀계수 공식
$$
\hat\beta_1
=
\frac{\sum(x_i-\bar x)(y_i-\bar y)}
{\sum(x_i-\bar x)^2}
=
\frac{\operatorname{Cov}(X,Y)}
{\operatorname{Var}(X)}
$$
$$
\hat\beta_0=\bar y-\hat\beta_1\bar x
$$
=> 최소제곱 회귀직선은 항상 평균점$
(\bar x,\bar y)
$을 지남

## 계산 예시
$$ 
X=(1,2,3),\qquad Y=(2,3,5)
$$
평균
$$
\bar x=2,\qquad \bar y=\frac{10}{3}
$$
편차의 곱
$$ 
\sum(x_i-\bar x)(y_i-\bar y)=3
$$
$$
\sum(x_i-\bar x)^2=2
$$
$
\therefore
\hat\beta_1=\frac{3}{2}=1.5
$

절편은

$$
\hat\beta_0=\frac{10}{3}-1.5(2)
=\frac13
$$
$$\therefore\hat Y=\frac13+1.5X
$$
해석: X가 1 증가할 때 Y의 평균적인 예측값은 약 1.5 증가한다
### fitted value 계산
$$
\hat y_1=\frac13+1.5(1)=1.833
$$
$$
\hat y_2=\frac13+1.5(2)=3.333
$$
$$
\hat y_3=\frac13+1.5(3)=4.833
$$
### residual 계산
$$
e_1=2-1.833=0.167
$$
$$
e_2=3-3.333=-0.333
$$
$$
e_3=5-4.833=0.167
$$
## 오차와 잔차의 차이
| 구분 | 오차(error) | 잔차(residual) |
|---|---|---|
| 기호 | $\epsilon_i$ | $e_i$ |
| 정의 | 실제 회귀직선과 관측값의 차이 | 추정된 회귀직선과 관측값의 차이 |
| 기준 | 모집단의 참된 직선 | 표본으로 적합한 직선 |
| 관측 가능 여부 | 직접 알 수 없음 | 계산 가능 |
$$
\epsilon_i=y_i-(\beta_0+\beta_1x_i)
$$
$$
e_i=y_i-(\hat\beta_0+\hat\beta_1x_i)
$$
## $\hat\beta_0$, $\hat\beta_1$이 얼마나 믿을 만한 추정값인가?
표본을 바꾸면 $\hat\beta_0$, $\hat\beta_1$도 달라짐. 따라서 추정량에는 변동이 있음
### 회귀계수의 분산
$\hat{\beta}$이 표본에 따라 얼마나 변하는지를 알아야하는데 그 퍼진 정도를 나타내는 것이 $Var(\hat{\beta})$
#### 기울기 추정량의 분산
$$
\operatorname{Var}(\hat{\beta}_1)
=
\frac{\sigma^2}
{\displaystyle\sum_{i=1}^{n}(x_i-\bar{x})^2}
$$
#### 절편 추정량의 분산
$$
\operatorname{Var}(\hat{\beta}_0)
=
\sigma^2
\left[
\frac{1}{n}
+
\frac{\bar{x}^{\,2}}
{\displaystyle\sum_{i=1}^{n}(x_i-\bar{x})^2}
\right]
$$
### 오차분산의 추정
실제 $\sigma^2$는 알 수 없기 때문에 잔차로 추정한다

$Var(\varepsilon_i)=\sigma^2$\
-> $\sigma$(오차의 모집단 표준편차)는 데이터가 회귀직선 주변에 얼마나 퍼져 있는지를 나타내는 값. 하지만 실제로는 알 수 없는 값이기 때문에 데이터로 추정해야 한다.
$$
\hat{\sigma}^{\,2}
=
\frac{\operatorname{SSE}}{n-2}
\ (
\operatorname{SSE}
=
\sum_{i=1}^{n}e_i^2
=
\sum_{i=1}^{n}(y_i-\hat{y}_i)^2)
$$
왜 n-2로 나눌까?\
단순선형회귀에서 표본으로 추정한 모수가 2개이기 때문
- $\beta_0$추정
- $\beta_1$추정

따라서 자유도(degrees of freedom)은 n-2이다
### 표준오차
Standard error, SE는 추정량의 표준편차를 추정한 값\
표본이 바뀔 때 추정값이 얼마나 흔들리는가를 나타내는 수치
$$
\operatorname{SE}(\hat{\beta}_1)
=
\frac{\hat{\sigma}}
{\sqrt{\displaystyle\sum_{i=1}^{n}(x_i-\bar{x})^2}}
$$

$$
\operatorname{SE}(\hat{\beta}_0)
=
\hat{\sigma}
\sqrt{
\frac{1}{n}
+
\frac{\bar{x}^{\,2}}
{\displaystyle\sum_{i=1}^{n}(x_i-\bar{x})^2}
}
$$
**표준오차가 작을수록 $\hat{\beta_0}$, $\hat{\beta_1}$이** 표본에 따라 크게 흔들리지 않고 **더 정밀함**

=> 다음 상황에서 작아짐
1. 오차의 변동 $\hat{\sigma}$가 작을 때
2. X의 관측값들이 넓게 퍼져 있을 때
3. 표본 크기 n이 충분히 클 때
#### 표준오차와 표준편차의 차이
| 구분 | 표준편차(SD, standard deviation) | 표준오차(SE, standard error) |
|---|---|---|
| 측정 대상 | 개별 데이터의 퍼짐 | 추정량의 불확실성 |
| 질문 | 데이터가 얼마나 흩어져 있는가? | 추정값이 얼마나 정확한가? |
| 사용 | 기술통계(descriptive statistics) | 추론통계(inferential statistics) |
| 표본 크기 영향 | 직접적인 영향이 작음 | 표본이 커지면 보통 작아짐 |
- **SD**: 하나의 표본 안에서 관측값들이 평균 주변에 얼마나 퍼져 있는지 나타내는 값
- **SE**: 여러 표본을 반복해서 뽑았을 때, 표본평균이나 회귀계수 같은 추정값이 얼마나 변하는지 나타내는 값

예를 들어 시험점수의 표준편차는 학생들 점수의 퍼짐을 나타냄\
반면 시험 평균의 표준오차는 표본을 반복해서 뽑았을 때 표본평균이 얼마나 달라지는지를 나타냄

회귀분석에서는 $\bar X$ 대신 $\hat{\beta}_0,\hat{\beta}_1$ 같은 회귀계수 추정값이 여러 표본에서 얼마나 달라지는지를 표준오차로 측정한다

## Hypothesis Testing
### Assumptions (선형회귀의 기본가정)
1. Linearity\
X와 Y의 평균적인 관계가 직선이어야 한다
$$
E(Y\mid X=x)=\beta_0+\beta_1x
$$
2. IID\
independent and identically distributed\
오차들이 서로 독립이고 같은 분산을 가져야 한다
$$
E(\epsilon_i)=0
$$
$$
\operatorname{Var}(\epsilon_i)=\sigma^2
$$
3. normality\
오차가 정규분포를 따른다고 가정한다
$$
\epsilon_i\sim N(0,\sigma^2)
$$

### 기울기에 대한 가설검정
#### 검정의 목적
설명변수 X가 반응변수 Y를 선형적으로 설명하는데 유용한지 확인
$$
H_0:\beta_1=0
$$
$$
H_1:\beta_1\neq0
$$
#### 귀무가설 $H_0$
$$
\beta_1=0
$$
X가 Y를 설명하는 데 선형적으로 유의한 도움이 없다
- 주의할 점: X랑 Y가 아무런 관계도 없다는 뜻이 아니라, **선형관계가 없다**는 뜻\
곡선 관계는 여전히 존재할 수 있음
### 검정통계량
