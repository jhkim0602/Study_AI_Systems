# Week 03 Note: NumPy 기초

이번 주차에서는 `NumPy`의 가장 기초적인 배열 개념을 학습합니다.
실습 코드는 루트의 [../../week03_NumPy.ipynb](../../week03_NumPy.ipynb)에 있고, 이 문서는 그 내용을 그림과 함께 정리한 학습 노트입니다.

## 학습 목표

- NumPy 배열이 무엇인지 이해한다.
- `shape`, `ndim`, `size`, `dtype`의 의미를 익힌다.
- 기본 인덱싱과 슬라이싱을 실습한다.
- `sum`, `mean`, `max`, `min`, `reshape`를 사용해 본다.

## 1. 1차원 배열과 2차원 배열

NumPy 배열을 공부할 때 가장 먼저 봐야 할 것은 배열의 모양(`shape`)과 차원 수입니다.

![NumPy 배열 구조](./assets/numpy-array-structure.svg)

핵심 정리
- `shape=(5,)` 는 값 5개를 가진 1차원 배열입니다.
- `shape=(2, 3)` 는 2행 3열 구조의 2차원 배열입니다.
- `ndim` 은 차원 수입니다.
- `size` 는 전체 원소 개수입니다.

예시

```python
import numpy as np

arr = np.array([1, 2, 3, 4, 5])
arr2 = np.array([[1, 2, 3], [4, 5, 6]])

print(arr.shape)   # (5,)
print(arr2.shape)  # (2, 3)
print(arr2.ndim)   # 2
```

## 2. 인덱싱과 슬라이싱

배열에서는 원하는 위치의 값 하나를 꺼내거나, 일정 구간을 잘라서 확인할 수 있습니다.

![NumPy 인덱싱과 슬라이싱](./assets/numpy-indexing-slicing.svg)

예시

```python
import numpy as np

arr = np.array([10, 20, 30, 40, 50])

print(arr[0])    # 10
print(arr[2])    # 30
print(arr[-1])   # 50
print(arr[1:4])  # [20 30 40]
```

## 3. reshape

`reshape()`는 배열 안의 값은 그대로 유지하면서, 배열의 모양만 바꾸는 함수입니다.

![NumPy reshape](./assets/numpy-reshape.svg)

예시

```python
import numpy as np

arr = np.array([1, 2, 3, 4, 5, 6])
arr2 = arr.reshape(2, 3)

print(arr.shape)   # (6,)
print(arr2.shape)  # (2, 3)
print(arr2)
```

## 4. 자주 쓰는 기초 함수

기초 실습에서는 아래 함수들만 먼저 익혀도 충분합니다.

```python
import numpy as np

arr = np.array([1, 2, 3, 4, 5])

print(np.sum(arr))   # 15
print(np.mean(arr))  # 3.0
print(np.max(arr))   # 5
print(np.min(arr))   # 1
```

## 학습 방법

추천 순서
1. [../../week03_NumPy.ipynb](../../week03_NumPy.ipynb)을 엽니다.
2. 이 문서의 그림을 보며 배열 구조를 이해합니다.
3. 노트북 셀을 위에서부터 직접 실행합니다.
4. 숫자를 바꾸면서 `shape`와 출력 결과가 어떻게 달라지는지 확인합니다.
