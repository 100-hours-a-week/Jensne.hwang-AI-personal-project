# Jensne.hwang-AI-personal-project
Jensen Personal Project
[제목 없음](https://www.notion.so/1bb0c6fd68408039b58ce8c1d9614355?pvs=21)

## 데이터셋

[Fruits and Vegetables Image Recognition Dataset](https://www.kaggle.com/datasets/kritikseth/fruit-and-vegetable-image-recognition)


### 설명

야채와 과일이 혼합된 36개의 클래스로 구성된 데이터

| 항목 | 개수(per class) |
| --- | --- |
| Train | 100 |
| Test | 10 |
| Validation | 10 |

## 개요

주어진 야채 및 과일 이미지들이 있는 데이터셋을 바탕으로 분류모델을 생성하는것이 목표임

→ 하지만 데이터를 살펴보니 높은 해상도와 퀄리티를 지니므로 사전학습 모델을 사용하지 않고도 높은 성능을 도출할 수 있을 것으로 보임

따라서 베이스 CNN 모델부터 시작하여 추가적인 기법들을 도입하여 각 모델을 직접 생성하여 평가하고 사전학습 모델을 적용한 후 각 모델들을 비교하여 평가를 진행해보고 어느정도의 차이를 나타내는지 분석하고자 함

## 목표

채소 / 야채의 이진 분류 및 클래스 세부 분류 진행

## 1차 적용 구상

기본적인 모델을 사용하지만 이미지 처리와 관련하여 추가적으로 적용하면 효과가 있을것으로 보이는 기법들을 조사하여 적용해보고자 함

모델은 기본적인 CNN을 Base로 하여 사전학습 모델과의 성능 차이를 비교 (+ attention 적용)

- CNN (Base)
    
    이미지 분류 진행 모델
    
- Attention (지역 특징 추출)
    
    이미지 데이터의 컨텍스트 벡터를 추출하여 사용하기 위해 적용해보고자 함
    
    → 분류에 중요한 특징을 추출하기 위한 기법
    
- 사전 학슴 모델
    
    이미지 분류의 성능을 높이기 위한 방법(기존 학습 모델의 구조를 그대로 사용하여 성능 향상)
    

## 모델 구상

분류를 수행하기 위한 여러가지의 기법을 적용하여 모델을 구상 (성능 기준)

1. **only CNN**
    
    가장 기본적인 모델을 통해 어느정도의 성능이 나오는지 확인
    
2. **CNN + attention (지역적 특성 강화)**
    
    attention을 적용하였을 때 지역적 특성을 파악하여 성능 개선이 가능할 것으로 판단
    
    우려사항 : 이미지 별 중요 특징이 다를 것으로 보이는데 어텐션 메커니즘을 단순 적용하였을 때 재대로된 성능 개선이 일어날 것인가?
    
3. **Pre-Model + Fine Tune**
    
    사전 학습 모델을 사용하여 분류 진행
    
    → 레이어가 깊고 성능이 좋기에 어느정도 성능이 보장될 것으로 보임
    
    우려사항 : 이미지를 분류하기에는 과한 모델을 사용하여 자원 낭비일수도 있을 것으로 보임
    
4. **Pre-Model(feature extraction) + attention**
    
    사전학습 모델을 특징 추출기로 사용하여 어텐션을 적용
    
    → 위 2번 모델에서 이미지 자체에 attention을 적용한다면 이미지에서의 특징을 뽑겠지만 추출된 feature로부터 attention을 적용한다면 분류 자체에 맞는 성능개선이 가능할 것으로 보임
    
    우려사항 : 특징 추출 및 attention까지 적용하면 연산량이 많고 모델이 무거워짐
    

## 최종 결론

### 모델 결과 정리

| 모델 | 정확도(%) | 손실 | Learning Rate |
| --- | --- | --- | --- |
| CNN | 95.82 | 0.2348 | 0.0001 |
| CNN + attention | 94.71 | 0.2671 | 0.0001 |
| VGG16 | 95.82 | 0.1388 | 0.00001 |
| ResNet50 | 97.49 | 0.0924 | 0.0001 |
| VGG16 + attention | 96.66 | 0.1396 | 0.00001 |
| ResNet50 + attention | 96.94 | 0.1261 | 0.0001 |

### CNN vs Pretrained

기본 구조의 CNN과 사전학슴 모델의 차이를 살펴보면 확실히 사전학습 모델을 사용한 경우 정확도가 더 높거나 손실이 감소한 것을 알 수 있다

### Model vs Model + attention

단순 모델을 사용한 결과와 각 모델에 어텐션 블록을 추가하여 사용한 결과를 비교하였을 때 비교적 정확도가 낮고 손실이 큼을 알 수 있다.

이는 어텐션 블록을 통과한 특성벡터는 값의 범위가 바뀌고 사라지는 값 또한 존재하여 초기 학습속도가 느린 문제가 있고 또한 비교적 단순한 데이터이기에 기존 모델로도 충분한 성능이 잘 나온다는 이유가 복합적으로 작용한 것으로 보인다.

### 결론

이번 프로젝트는 야채와 과일 이미지를 활용해 여러 분류 모델들을 만들어 보는 프로젝트였다.

기본적인 주제는 사전학습 모델을 활용하는 것이었지만 데이터를 확인해본 결과 사전학습 모델의 사용이 꼭 필요한 것인가에 대한 의문이 들었고 기본 CNN모델을 활용하여 분류 모델을 만들었을 때 사전학습 모델과 어느정도의 성능차이가 존재할지 알아보고자 했다.

전체 과정을 통해 만든 모델은 6가지로 기본 CNN, VGG16, ResNet50과 각 모델에 attention기법을 적용한 모델들이었다.

적합한 비교를 위해 Learning Rate의 차이를 제외하고는 모두 동일한 하이퍼파라미터를 사용하여 비교를 진행하였다.

처음 예상은 사전학습 모델이 더 성능이 높고 attention까지 적용하면 더욱 성능이 좋을 것이라 생각하였다.

결론적으론 사전학습 모델이 성능이 높은것은 맞았지만 attention을 적용한 결과로는 더 성능이 떨어짐을 보였다.

이는 attention 블록을 통해 초기 학습속도가 느려지는 현상이 발생하고 데이터가 기본 모델로도 높은 분류 성능을 보일 만큼 복잡하고 어려운 데이터가 아니었기에 오히려 무거운 모델에서는 반대의 현상을 보인 것으로 보인다.

차후에는 다른 데이터를 사용하여 모델이 더욱 학습하기 어려운 상황에서 각각의 모델이 어떻게 변화하는지를 살펴보고 추가적으로 경량화 기법을 적용하여 생기는 변화 현상을 알아보고자 한다.

## 참고자료

https://junklee.tistory.com/111

https://ganghee-lee.tistory.com/43

https://lswook.tistory.com/105

https://blog.naver.com/winddori2002/222057978305
