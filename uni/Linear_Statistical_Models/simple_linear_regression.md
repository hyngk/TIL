# Simple Linear Regression
## Simple Linear Regression이란?
A method that studies the linear relationship between a single response variable(Y) and a single predictor variable(X) and expresses that relationship mathematically as a straight-line model with an intercept and a slope\
(하나의 반응변수와 하나의 예측변수 사이의 선형적 관계를 연구하고, 그 관계를 절편과 기울기를 가진 직선 모형으로 수식화하는 방법)
## Covariance
두 변수가 함께 어떻게 변하는지를 나타내는 값\
선형관계만을 측정 (X,Y가 비선형적인 방식으로 관여되있을 수 있음)
### Sample covariance
$$
s_{XY}
=
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

-> Y와 X사이 선형 관계의 방향을 나타냄\
if cov(X,Y)>0, a positive relationship between Y and X\
if cov(X,Y)<0, a negative relationship between Y and X\
if cov(X,Y)=0, no linear relationship between Y and X

But, 공분산은 측정 단위가 바뀌면 값도 변하기 때문에 (e.g. 키, cm/m -> 실제 관계는 같지만 공분산의 값은 달라짐), 관계의 강도를 나타내진 않는다.

=> 관계의 강도를 비교할 땐 단위의 영향을 제거한 correlation coefficient를 사용
## Correlation coefficient
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
- 부호는 방향 나타냄
- -1 또는 1에 가까울수록 Y와 X사이의 선형관계가 더 강해짐
- if cor(Y,X)=0, no linear relationship between Y and X

## Simple Linear Regression Model
$$
Y=\beta_0+\beta_1X+\varepsilon
$$
- $\beta_0,\beta_1$: parameter
- $\varepsilon$: error term

Each observation\
$
y_i=\beta_0+\beta_1x_i+\varepsilon_i,\ i
$=1,2,...,n

회귀 분석과 상관 분석의 차이
: 상관계수는 Cor(X,Y)=Cor(Y,X)를 만족시키기 때문에 X와 Y는 상관분석에서 둘 다 중요하다.
but, 회귀분석에서 X는 X자체가 아니라 Y를 얼마나 잘 설명하는지가 중요하기 때문에 Y가 더 중요하다.
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
$$
y_i-\hat{y}_i=\varepsilon_i=y_i-\beta_0-\beta_1x_i,\ i=1,2,...,n
$$
Estimated $\beta_0$ and $\beta_1$
$$
\hat{\beta}_0
=
\bar{y}
-
\hat{\beta}_1\bar{x}
$$

$$
\hat{\beta}_1
=
\frac{
\displaystyle\sum_{i=1}^{n}
(y_i-\bar{y})(x_i-\bar{x})
}{
\displaystyle\sum_{i=1}^{n}
(x_i-\bar{x})^2
}
$$
### $\hat\beta_0$, $\hat\beta_1$이 얼마나 믿을 만한 추정값인가?
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

$\beta_0$, $\beta_1$은 모집단의 실제 값이고, $\hat{\beta}_0$, $\hat{\beta}_1$은 표본으로 계산한 추정값.\
표본을 새로 뽑을 때마다 추정값은 달라짐\
=> $\hat{\beta}$이 표본에 따라 얼마나 변하는지를 알아야하는데 그 퍼진 정도를 나타내는 것이 $Var(\hat{\beta})$
$$
\operatorname{Var}(\hat{\beta}_1)
=
\frac{\sigma^2}
{\displaystyle\sum_{i=1}^{n}(x_i-\bar{x})^2}
$$

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
#### Standard error
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
=>표준오차가 작을수록 $\hat{\beta_0}$, $\hat{\beta_1}$이 표본에 따라 크게 흔들리지 않고 더 정밀함
#### 표준오차와 표준편차

- **표준편차(SD)**: 하나의 표본 안에서 관측값들이 평균 주변에 얼마나 퍼져 있는지 나타내는 값
- **표준오차(SE)**: 여러 표본을 반복해서 뽑았을 때, 표본평균이나 회귀계수 같은 추정값이 얼마나 변하는지 나타내는 값

```text
표본 A:  ●  ● ●    ●
표본 B:    ● ●  ●
표본 C:  ●   ● ● ●
         └──┘
      한 샘플의 표본 내부의 퍼짐
       = 표준편차(SD)

       x̄_A      x̄_B      x̄_C
         ●        ●        ●
         └────────┴────────┘
       표본 간 추정값의 퍼짐
       = 표준오차(SE)
```

회귀분석에서는 $\bar X$ 대신 $\hat{\beta}_0,\hat{\beta}_1$ 같은 회귀계수 추정값이 여러 표본에서 얼마나 달라지는지를 표준오차로 측정한다

### Fitted value와 residual

$$
\hat{y}_i
=
\hat{\beta}_0+\hat{\beta}_1x_i
$$

- $y_i$: 실제 관측값
- $\hat{y}_i$: fitted value, 회귀직선이 예측한 값
- $e_i$: residual, 실제값과 fitted value의 차이

$$
e_i=y_i-\hat{y}_i
$$
- The sum of all residuals is zero

$\hat{\beta}_1$는 $Cov(Y,X),Cor(Y,X)$와 항상 같은 부호
$$
\hat{\beta}_1
=
\frac{\operatorname{cov}(Y,X)}
{\operatorname{Var}(X)}
=
\operatorname{cor}(Y,X)
\frac{s_y}{s_x}
$$

## Hypothesis Testing
