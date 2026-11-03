각 픽셀이 무엇인가?
        → 이미지 세그멘테이션, mask
# 세그멘테이션
이미지를 픽셀 단위로 분류하는 문제

분류에서는 이미지 한 장에 대해 답 하나를 냄

객체 탐지에서는 박스 단위로 답함

세그멘테이션에서는 모든 픽셀에 답을 붙임\
이 픽셀은 고양이인가?\
이 픽셀은 배경인가?\
이 픽셀은 사람인가?

따라서 입력이 256×256 이미지라면 출력도 기본적으로 256×256 크기의 마스크가 됩니다.\
입력:  256 × 256 × 3\
출력:  256 × 256 × 클래스 수

pixel-wise classification(픽셀별 분류)
## 종류
### Semantic segmentation
같은 클래스의 객체를 구분x

예를 들어 얼룩말 2마리가 있으면 얼룩말 픽셀은 모두 같은 색
### Instance segmentaion
같은 클래스라도 객체를 각각 구분

얼룩말 1 → 마스크 A\
얼룩말 2 → 마스크 B

객체 탐지의 bounding box와 마스크를 결합한 문제라고 생각할 수 있습니다.
### Panoptic segmentation
Semantic segmentation과 Instance segmentation을 모두 수행

배경과 도로 → semantic 방식\
사람과 자동차 → instance 방식
# 마스크
각 픽셀이 어떤 클래스에 속하는지 나타내는 정답 지도
## 이진 세그멘테이션
클래스가 두 개

0=배경\
1=관심 객체
## 다중 클래스 세그멘테이션 
0=배경\
1=사람\
2=자동차\
3=나무

각 픽셀에 클래스 번호를 붙힘
# CNN만 사용해서 세그멘테이션할 수 없는 이유
CNN은 층을 지날수록 feature map의 크기를 줄입니다.
하지만 세그멘테이션은 출력이 원본 이미지와 같은 크기여야 합니다.

=> 세그멘테이션 모델은 다음 두 가지를 모두 해야함
1. 이미지를 줄이며 무엇이 있는지 파악
2. 다시 키우며 정확히 어디에 있는지 복원
# Encoder-Decoder
세그멘테이션 모델은 보통 Encoder-Decoder 구조를 사용
## Encoder
이미지를 점점 작게 만들면서 특징을 추출

큰 이미지
→ 작은 feature map
→ 더 작은 feature map
→ 의미가 풍부한 feature
## Decoder
작아진 feature map을 다시 키워서 출력 mask를 만듭니다.

작은 feature map
→ 업샘플링
→ 더 큰 feature map
→ 원본 크기 mask

### 업샘플링
작아진 feature map을 다시 크게 만드는 과정

대표적인 방법은 3가지
#### Nearest Neighbor
가장 가까운 픽셀 값을 그대로 복사

장점:
- 단순함
- 빠름
- 학습이 필요 없음

단점:
- 계단 현상이 생길 수 있음
- 새로운 정보를 만들지는 않음
#### Bilinear Interpolation
주변 픽셀들의 값을 거리 기반으로 평균내어 새 픽셀을 만듭니다.

Nearest보다 부드럽습니다.

장점:
- 결과가 부드러움
- 학습이 필요 없음

단점:
- 지나치게 흐려질 수 있음
- 새로운 의미 있는 특징을 학습하지는 않음
#### Transposed Convolution
학습 가능한 커널을 사용해 feature map을 키웁니다.

일반 convolution이 이미지를 줄이거나 특징을 추출한다면, transposed convolution은 반대 방향으로 feature를 확장하는 역할

작은 feature map
→ 학습된 커널
→ 큰 feature map

업샘플링에 사용되는 가중치도 학습된다.

따라서 모델은 어떤 방식으로 키워야 세그멘테이션 결과가 좋아지는지도 학습할 수 있습니다.

다만 kernel과 stride 설정이 좋지 않으면 checkerboard artifact라는 격자무늬 인공물이 생길 수 있습니다.

그래서 실제 모델에서는 다음 조합도 자주 사용합니다.\
Interpolation
→ Convolution
## 전체 구조
```
입력 이미지
    ↓
Encoder: 줄이면서 특징 추출
    ↓
Bottleneck: 가장 압축된 의미 표현
    ↓
Decoder: 키우면서 mask 복원
    ↓
픽셀별 클래스 mask
```
# U-Net
U자 모양으로 생겼기 때문에 U-Net이라고 부릅니다.

U-Net의 가장 중요한 아이디어는 skip connection입니다.

Encoder에서 만들어진 feature map을 Decoder의 같은 해상도 단계로 전달합니다.

- encoder의 깊은 층은 '이미지 안에 무엇이 있는가'를 잘 앎. 하지만 pooling 때문에 '정확한 경계가 어디인가'에 대한 정보가 약해짐
- encoder의 얕은 층은 공간 해상도가 높아서 '경계,윤곽,정확한 위치'에 대한 정보를 잘 가지고 있음

=> U-Net은 '깊은 층의 의미 정보
+
얕은 층의 위치 정보'를 합침
## U-Net의 skip connection은 ResNet과 다름
ResNet: H(x) = F(x) + x

U-Net : feature map을 채널 방향으로 이어붙임
```
Encoder feature
+
Decoder feature
→ Concatenation
```
e.g.\
Encoder feature: [64, 64, 64]\
Decoder feature: [64, 64, 64]\
=> [128, 64, 64]

즉, U-Net에서는 skip connection이 channel 수를 증가시킵니다.

그 후 convolution을 적용해서 다시 필요한 channel 수로 정리
## 전체 흐름 예시
```
입력: 256×256×3

Encoder
256×256×3
→ 128×128×64
→ 64×64×128
→ 32×32×256

Bottleneck
32×32×256

Decoder
32×32×256
→ 64×64×128
→ skip connection
→ 64×64×256

→ 128×128×64
→ skip connection
→ 128×128×128

→ 256×256×32
→ skip connection
→ 256×256×64

최종 출력
256×256×클래스 수
```
## 최종 출력
### 이진 세그멘테이션
출력 크기: H×W×1
### 다중 클래스 세그멘테이션
출력 크기: H×W×클래스 수

각 픽셀에서 가장 높은 클래스를 선택하면 최종 mask가 됨
## Loss
픽셀별 손실 계산
### Cross Entropy Loss
다중 클래스 세그멘테이션에서 사용할 수 있습니다.

각 픽셀별로 모델이 예측한 클래스vs정답클래스 를 비교

이미지가 256×256이라면 총 65,536개의 픽셀에 대해 손실을 계산한 뒤 평균냅니다.
### Binary Cross Entropy
이진 세그멘테이션에서 사용할 수 있습니다.

각 픽셀이:
배경인가?
객체인가?
를 판단
### Dice Loss
Dice 기반 손실

#### Dice Score
$$
Dice=
\frac{2|P\cap G|}
{|P|+|G|}
$$
- P: 예측 mask
- G: 정답 mask
- $P\cap G$: 두 mask가 겹치는 영역

Dice가 1이면 완전히 일치하고, 0이면 전혀 겹치지 않습니다.

Dice Loss는 보통:
$$
DiceLoss=1-Dice
$$
로 정의

전체 Loss
= BCE 또는 Cross Entropy + Dice Loss

Cross Entropy는 픽셀별 분류를 안정적으로 학습시키고, Dice Loss는 전체 mask의 겹침을 개선
# 세그멘테이션의 IoU
bounding box가 아니라 mask끼리 비교
$$
IoU=
\frac{|P\cap G|}
{|P\cup G|}
$$
예측 mask와 정답 mask의 교집합
÷
예측 mask와 정답 mask의 합집합

세그멘테이션에서 Dice와 IoU의 관계는 다음과 같습니다.
$$
Dice=\frac{2IoU}{1+IoU}
$$
둘 다 mask가 얼마나 겹치는지 평가합니다.

일반적으로:
- Dice score
- IoU
- pixel accuracy

를 함께 확인
# 최종 정리
```
입력 이미지
    ↓
Encoder
CNN으로 특징 추출
    ↓
작아진 feature map
무엇이 있는지 파악
    ↓
Decoder
업샘플링으로 공간 크기 복원
    ↓
Skip connection
Encoder의 위치·경계 정보 결합
    ↓
최종 mask
각 픽셀의 클래스 예측
    ↓
BCE / Cross Entropy / Dice Loss
픽셀별 정답과 비교
    ↓
Dice / IoU
mask 품질 평가
```