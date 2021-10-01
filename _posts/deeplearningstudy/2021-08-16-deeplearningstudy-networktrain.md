---
title:  "[딥러닝스터디] 4. 신경망 학습"
excerpt: "손실함수"
date: 2021-08-16 09:30:00
categories:
  - 딥러닝스터디
  - 딥러닝

tags:
  - 딥러닝스터디
  - 신경망
---

신경망 학습이란 훈련 데이터로부터 가중치 매개변수의 최적값을 자동으로 획득하는 것을 뜻한다.
신경망이 학습할 수 있도록 해주는 지표인 손실함수가 있다. 손실 함수의 결과값을 작게 만드는 가중치 매개변수를 찾는 것이 **학습의 목표**이다.

# 기계학습 두가지 접근법

1. 이미지에서 특징을 추출하고 그 특징의 패턴을 기계학습 기술로 학습하는 방법
특징은 입력 데이터에서 본질적인 데이터(중요한 데이터)를 정확하게 추출할 수 있도록 설계된 변환기를 가리킨다. 이미지의 특징은 보통 벡터로 기술하고, 컴퓨터 비전 분야에서는 SIFT, SURF, HOG 등의 특징을 많이 사용한다. 이런 특징을 사용해 이미지 데이터를 벡터로 변환하고, 변환된 벡터를 가지고 지도 학습 방식의 대표 분류 기법인 SVM, KNN 등으로 학습할 수 있다.
모아진 데이터로부터 규칙을 찾아내는건 기계가 담당한다. 다만 이미지를 벡터로 변환할 때 사용하는 특징은 여전히 사람이 설계한다.

2. 신경망(딥러닝) 방식
사람이 개입하지 않고, 신경망은 이미지를 있는 그대로 학습한다. 신경망은 이미지에 포함된 중요한 특징까지도 기계가 스스로 학습한다.
딥러닝을 종단간 기계학습(end-to-end machine learning)이라고도 한다. 종단간은 처음부터 끝까지라는 의미로, 데이터에서 목표한 결과를 사람의 개입없이 얻는다는 의미이다.
즉, 신경망은 모든 문제를 주어진 데이터 그대로를 입력 데이터로 활용해 end-to-end로 학습할 수 있다.

<br>
<br>

# 훈련데이터와 시험데이터

기계학습 문제는 데이터를 훈련데이터(training data)와 시험데이터(test data)로 나눠 학습과 실험을 수행하는 것이 일반적이다.

우선 훈련 데이터만 사용해 학습하면서 최적의 매개변수를 찾는다.
그 후 시험 데이터를 사용해 훈련된 모델의 실력을 평가한다.

이렇게 훈련, 시험 데이터로 나누는 이유는 범용능력을 제대로 평가하기 위함이다.

**범용능력**은 아직 보지 못한 데이터(훈련 데이터에 포함되지 않는 데이터)로도 문제를 올바르게 풀어내는 능력이다. 범용능력을 획득하는 것이 기계학습의 최종 목표이다.

한 데이터셋에만 지나치게 최적화된 상태를 **오버피팅**(overfitting)이라고 한다.

<br>
<br>

# 손실 함수

신경망 학습에서는 현재의 상태를 지표로 표현한다. 이 지표를 기준으로 최적의 매개변수 값을 탐색한다. 신경망 학습에서 사용하는 지표는 손실함수(loss function)이라고 한다. 이 손실 함수는 임의의 함수를 사용할 수도 있지만 일반적으로 **평균 제곱 오차**와 **엔트로피 오차**를 사용한다.

### 1. 평균 제곱 오차

가장 많이 쓰이는 손실함수는 평균 제곱 오차(mean squared error, MSE)이다.

```python
def mean_squared_error(y, t):
    return 0.5 * np.sum((y-t)**2)
```
각 원소의 출력(추정 값)과 정답 레이블(참 값)의 차를 제곱한 후, 그 총합을 구한다.

```python
>>> t = [0, 0, 1, 0, 0, 0, 0, 0, 0, 0]
>>> y = [0.1, 0.05, 0.6, 0.0, 0.05, 0.1, 0.0, 0.1, 0.0, 0.0]
>>> mean_squared_error(np.array(y), np.array(t))
0.09750000000031
>>>
>>> y = [0.1, 0.05, 0.1, 0.0, 0.05, 0.1, 0.0, 0.6, 0.0, 0.0]
>>> mean_squared_error(np.array(y), np.array(t))
0.59750000000003
```
위 코드를 보면 정답은 2이다. 이때 첫번째는 정답과 같이 2일 확률이 높다고 추정했고, 두번째는 7일 확률이 높다고 가정했다. 평균제곱오차를 보면 정답을 추정한 첫번째 케이스가 평균제곱오차 값이 더 작은 것을 알 수 있다.

### 2. 교차 엔트로피 오차

```python
def cross_entropy_error(y, t):
    delta = 1e-7
    return -np.sum(t*np.log(y + delta))
```
y와 t는 넘파이 배열이다. np.log를 계산할 때 아주 작은 값인 delta를 더했다. 이는 np.log함수에 0을 입력하면 마이너스 무한대를 뜻하는 -inf가 되어 더 이상 계산을 진행할 수 없기 때문이다. 그래서 아주 작은 값을 더해서 절대 0이 되지 않도록, 즉 마이너스 무한대가 발생하지 않도록 한 것이다.

```python
>>> t = [0, 0, 1, 0, 0, 0, 0, 0, 0, 0]
>>> y = [0.1, 0.05, 0.6, 0.0, 0.05, 0.1, 0.0, 0.1, 0.0, 0.0]
>>> cross_entropy_error(np.array(y), np.array(t))
0.51082545709933802
>>>
>>> y = [0.1, 0.05, 0.1, 0.0, 0.05, 0.1, 0.0, 0.6, 0.0, 0.0]
>>> cross_entropy_error(np.array(y), np.array(t))
2.30258409299145158
```
정답을 예측한 첫번째 경우가 교차 엔트로피 오차가 더 작은것을 알 수 있다.

### 3. 미니배치 학습

기계학습 문제는 훈련 데이터에 대한 손실 함수의 값을 구하고, 그 값을 최대한 줄여주는 매개변수를 찾아낸다. 이렇게 하려면 모든 훈련 데이터를 대상으로 손실 함수 값을 구해야 한다. 즉 훈련 데이터가 100개 있으면 그로부터 계산한 100개의 손실 함수 값들의 합을 지표로 삼는 것이다.

만약 N개의 훈련 데이터가 있다면 N으로 나누어 정규화해야 한다. 즉 **평균 손실 함수**를 구해야 한다. 이렇게 평균을 구해 사용하면 훈련 데이터 개수와 관계없이 언제든 통일된 지표를 얻을 수 있다.

그런데 MNIST 데이터셋은 훈련 데이터가 60000개다. 그래서 모든 데이터를 대상으로 손실 함수의 합을 구하려면 시간이 걸린다. 만약 빅데이터 수준이 되면 그 수는 수백만에서 수천만도 넘는다. 이 많은 데이터를 대상으로 일일이 손실 함수를 계산하는 것은 현실적이지 않다. 이런 경우 데이터 일부를 추려 전체의 **근사치**로 이용할 수  있다.

신경망 학습에서도 훈련 데이터로부터 일부만 골라 학습을 수행한다. 이 일부를 **미니배치**(mini-batch)라고 한다. 예로들어 mnist데이터셋의 60000장의 훈련 데이터 중에서 100장을 무작위로 뽑아 그 100장만을 대상으로 학습한다. 이러한 학습방법을 **미니배치 학습**이라고 한다.

```python
import sys, os
sys.path,append(os.pardir)
import numpy as np
from dataset.mnist import load_mnist

(x_train, t_train), (x_test, t_test) = \
  load_mnist(normalize=True, one_hot_label=True)

print(x_train.shape) # (60000, 784)
print(t_train.shape) # (60000, 10)

train_size = x_train.shape[0]
batch_size = 10
batch_mask = np.random.choice(train_size, batch_size)
x_batch = x_train[batch_mask]
t_batch = t_train[batch_mask]
```

np.random.choice는 지정한 범위의 수 중에서 무작위로 원하는 개수만 꺼낼 수 있다. 

```python
>>> np.random.choice(60000, 10)
array([8013, 14666, 58210, 23832, 2546, 8107, 19410, 21411, 50001, 25465])
```

이 함수를 사용해 함수가 출력한 배열을 미니배치로 뽑아낼 데이터의 인덱스로 사용하면 된다. 손실함수도 이 미니배치로 계산한다.

### 4. (배치용) 교차 엔트로피 오차 구현

```python
def cross_entropy_error(y, t):
    if y.ndim == 1:
        t = t.reshape(1, t.size)
        y = y.reshape(1, y.size)

    batch_size = y.shape[0]
    return -np.sum(t*np.log(y)) / batch_size
```
y는 신경망의 출력, t는 정답 레이블이다. y가 1차원이라면, 즉 데이터 하나당 교차 엔트로피 오차를 구하는 경우 reshape함수로 데이터의 형상을 바꿔준다. 그리고 배치의 크기로 나눠 정규화하고 이미지 1장당 평균의 교차 엔트로피 오차를 계산한다.

```python
def cross_entropy_error(y, t):
    if y.ndim == 1:
        t = t.reshape(1, t.size)
        y = y.reshape(1, y.size)

    batch_size = y.shape[0]
    return -np.sum(np.log(y[np.arange(batch_size), t])) / batch_size
```
정답 레이블이 원-핫 인코딩이 아니라 '2'나 '7'등의 숫자 레이블로 주어졌을 때의 교차 엔트로피 오차는 위 코드와 같이 구현할 수 있다.

### 5. 손실함수 설정 이유

궁극적인 목표는 높은 정확도를 끌어내는 매개변수 값을 찾는 것이다. 정확도 대신 손실함수의 값이라는 우회적인 방법을 택하는 이유는 신경망 학습에서의 '미분' 역할 때문이다.

신경망 학습에서는 최적의 매개변수(가중치와 편향)를 탐색할 때 손실함수의 값을 가능한 한 작게 하는 매개변수 값을 찾는다. 이때 매개변수의 미분(기울기)을 계산하고, 그 미분 값을 단서로 매개변수의 값을 서서히 갱신하는 과정을 반복한다.

신경망을 학습할 때 정확도를 지표로 삼아서는 안된다. 정확도를 지표로 하면 매개변수의 미분이 대부분의 장소에서 0이 되기 때문이다.

<br>
<br>

# 경사법(경사 하강법)

신경망은 최적의 매개변수(가중치와 편향)를 학습 시 찾아낸다. 최적이란 손실함수가 최솟값이 될 때의 매개변수 값이다.
이때 기울기를 이용해 함수의 최솟값(또는 가능한 작은 값)을 찾으려는 것이 경사 하강법이다.

각 지점에서 함수의 값을 낮추는 방안을 제시하는 지표가 기울기이다.

경사법은 현위치에서 기울어진 방향으로 일정 거리만큼이동한다. 그 후 이동한 곳에서도 마찬가지로 기울기를 구하고, 또 그 기울어진 방향으로 나아가기를 반복한다. 이렇게 함수의 값을 점차 줄이는 것을 경사법(gradient method)이라 한다.

경사법은 기계학습을 최적화하는데 흔히 쓰이는 방법이다.

경사법은 최솟값을 찾느냐, 최댓값을  찾느냐에 따라 이름이 다르다. 전자를 경사 하강법(gradient descent method), 후자를 경사 상승법(gradient ascent method)이라고 한다. 손실 함수의 부호를 반전시키면 최솟값, 최댓값을 찾는 문제는 같은 것이라 하강이냐 상승이냐는 본질적으로 중요하지 않다. 다만 일반적으로 딥러닝 분야에서는 경사법은 **경사하강법**으로 등장할 때가 많다.

```python
def _numerical_gradient_no_batch(f, x):
    h = 1e-4 # 0.0001
    grad = np.zeros_like(x) # x와 형상이 같은 배열을 생성
    
    for idx in range(x.size):
        tmp_val = x[idx]
        
        # f(x+h) 계산
        x[idx] = float(tmp_val) + h
        fxh1 = f(x)
        
        # f(x-h) 계산
        x[idx] = tmp_val - h 
        fxh2 = f(x) 
        
        grad[idx] = (fxh1 - fxh2) / (2*h)
        x[idx] = tmp_val # 값 복원
        
    return grad

def gradient_descent(f, init_x, lr=0.01, step_num=100):
    x = init_x

    for i in range(step_num):
        grad = numerical_gradient(f, x)
        x -= lr * grad

    return x
```
인수 f는 최적화하려는 함수, init_x는 초깃값, lr은 학습률, step_num은 경사법에 따른 반복 횟수를 뜻한다. 함수의 기울기는 numerical_gradient(f,x)로 구하고, 그 기울기에 학습률을 곱한 값으로 갱신하는 처리를 step_num번 반복한다.

경사법에서는 갱신하는 양이 있는데 이것을 **학습률**(learning rate)라고 한다.
한번의 학습으로 얼마만큼 학습해야 할지, 즉 매개변수 값을 얼마나 갱신하느냐를 정하는 것이 학습률이다.

학습률 값은 0.01, 0.001 등 미리 특정 값으로 정해두어야 한다. 일반적으로 학습률 값이 너무 크거나 작으면 좋은장소를 찾아갈 수 없다. 신경망 학습에서는 보통 이 학습률 값을 변경하면서 올바르게 학습하고 있는지를 확인하면서 진행한다.


#### 하이퍼파라미터(hyper parameter, 초매개변수)

학습률 같은 매개변수를 하이퍼파라미터라고 한다. 이는 가중치와 편향 같은 신경망의 매개변수와는 성질이 다른 매개변수이다. 신경망의 가중치 매개변수는 훈련 데이터와 학습 알고리즘에 의해서 **자동**으로 획득되는 매개변수인 반면, 학습률 같은 하이퍼파라미터는 사람이 직접 설정해야하는 매개변수이다. 
일반적으로 하이퍼파라미터는 여러 후보 값 중에서 시험을 통해 가장 잘 학습하는 값을 찾는 과정을 거쳐야 한다.

<br>
<br>

# 신경망에서의 기울기

신경망 학습에서 기울기를 구해야 한다. 가중치 매개변수에 관한 손실함수의 기울기를 구해야 한다.

```python
import sys, os
sys.path.append(os.pardir)  # 부모 디렉터리의 파일을 가져올 수 있도록 설정
import numpy as np
from common.functions import softmax, cross_entropy_error
from common.gradient import numerical_gradient


class simpleNet:
    def __init__(self):
        self.W = np.random.randn(2,3) # 정규분포로 초기화

    def predict(self, x):
        return np.dot(x, self.W)

    def loss(self, x, t):
        z = self.predict(x)
        y = softmax(z)
        loss = cross_entropy_error(y, t)

        return loss
```

위 코드는 간단한 신경망을 예로 들어 실제로 기울기를 구하는 코드이다.

simpleNet 클래스는 형상이 2*3인 가중치 매개변수 하나를 인스턴스 변수로 갖는다. predict메소드는 예측을 수행하고, loss메소드는 손실 함수의 값을 구한다.
여기서 인수 x는 입력데이터, t는 정답 레이블이다.
