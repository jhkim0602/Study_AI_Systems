# 00. 대표 배열 연산 함수와 `axis`

이 문서는 4주차 내적 학습에 들어가기 전에, NumPy에서 자주 쓰는 대표 연산 함수와 `axis` 개념을 정리합니다. 내적도 결국 배열 원소를 곱하고 더하는 계산이므로, 배열 연산을 먼저 익혀 두면 뒤 내용이 훨씬 자연스럽습니다.

연결 실습
- [../week04_Vector_DotProduct.ipynb](../week04_Vector_DotProduct.ipynb)
- 복습 링크: [Week03 배열 속성 노트](../../week03/notes/01_array_creation_and_properties.md)

## 1. 대표 함수

벡터와 행렬을 다룰 때 자주 쓰는 함수는 아래 정도입니다.

- `np.sum()` : 합계
- `np.mean()` : 평균
- `np.max()` : 최댓값
- `np.min()` : 최솟값

```python
ops_matrix = np.array([[1, 2, 3], [4, 5, 6]])

print(np.sum(ops_matrix))
print(np.mean(ops_matrix))
print(np.max(ops_matrix))
print(np.min(ops_matrix))
```

## 2. 원소별 배열 연산

NumPy 배열끼리의 `+`, `-`, `*`는 기본적으로 같은 위치의 원소끼리 계산합니다.

![배열 원소별 연산](/Users/junghwan/Study_AI_Systems/StudyNote/week04/assets/week04-array-elementwise.svg)

```python
arr_x = np.array([1, 2, 3])
arr_y = np.array([4, 5, 6])

print(arr_x + arr_y)  # [5 7 9]
print(arr_x * arr_y)  # [ 4 10 18]
```

해석
- `1 + 4`, `2 + 5`, `3 + 6`처럼 같은 위치끼리 더한다.
- `1 * 4`, `2 * 5`, `3 * 6`처럼 같은 위치끼리 곱한다.
- 내적은 여기서 한 단계 더 나아가, 곱한 결과를 다시 모두 더하는 계산이다.

## 3. 비교연산

배열끼리 크기를 비교하거나, 특정 값과 비교하면 결과는 `True`, `False`로 이루어진 불리언 배열로 나온다.

![배열 비교연산](/Users/junghwan/Study_AI_Systems/StudyNote/week04/assets/week04-array-comparison.svg)

```python
compare_arr = np.array([1, 3, 5, 7])

print(compare_arr > 3)    # [False False  True  True]
print(compare_arr == 5)   # [False False  True False]
print(compare_arr % 2 == 1)  # [ True  True  True  True]
```

해석
- 각 위치의 원소가 조건을 만족하면 `True`, 아니면 `False`가 된다.
- NumPy는 비교 결과도 배열로 돌려주기 때문에 여러 값을 한 번에 검사할 수 있다.
- 이 결과는 나중에 조건 필터링의 기준으로 바로 사용할 수 있다.

## 4. `axis`란?

2차원 배열에서는 전체를 한 번에 계산할 수도 있고, 행 기준 또는 열 기준으로 따로 계산할 수도 있습니다.

![axis와 합계 계산](/Users/junghwan/Study_AI_Systems/StudyNote/week04/assets/week04-axis-sum.svg)

```python
print(np.sum(ops_matrix, axis=1))
print(np.sum(ops_matrix, axis=0))
```

해석
- `axis=0` : 열 기준 계산
- `axis=1` : 행 기준 계산

## 5. `sum` 예시

```python
ops_matrix = np.array([[1, 2, 3], [4, 5, 6]])

print(np.sum(ops_matrix))          # 전체 합계
print(np.sum(ops_matrix, axis=0))  # 열별 합계
print(np.sum(ops_matrix, axis=1))  # 행별 합계
```

결과 해석
- 전체 합계: `21`
- 열별 합계: `[5 7 9]`
- 행별 합계: `[6 15]`

## 6. `mean`도 같은 방식으로 본다

`mean` 역시 전체 평균, 열별 평균, 행별 평균으로 나눠서 볼 수 있습니다.

```python
print(np.mean(ops_matrix))          # 전체 평균
print(np.mean(ops_matrix, axis=0))  # 열별 평균
print(np.mean(ops_matrix, axis=1))  # 행별 평균
```

해석
- 전체 평균: 모든 값을 합쳐서 평균을 낸다.
- `axis=0`: 각 열의 평균을 구한다.
- `axis=1`: 각 행의 평균을 구한다.

## 7. 왜 4주차에 이 내용이 필요한가?

내적도 결국 배열 내부 연산의 한 형태입니다.
그래서 내적으로 들어가기 전에
- 배열 전체를 계산하는 함수
- 행과 열을 나눠 계산하는 방법
- 같은 위치 원소끼리 계산하는 방식
- 비교 결과를 불리언 배열로 보는 방식

을 먼저 익혀 두면 훨씬 이해가 쉽습니다.
