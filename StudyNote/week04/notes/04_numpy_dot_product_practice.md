# 04. NumPy 내적 실습

이 문서는 NumPy에서 내적을 실제로 계산하는 방법을 정리합니다.

연결 실습
- [../week04_Vector_DotProduct.ipynb](../week04_Vector_DotProduct.ipynb)

## 1. `.dot()` 사용

```python
a = np.array([1, 2, 3])
d = np.array([4, 5, 6])

print(a.dot(d))
```

## 2. `np.dot()` 사용

```python
print(np.dot(a, d))
```

## 3. `@` 연산자 사용

```python
print(a @ d)
```

세 방법은 같은 내적 결과를 계산합니다.

## 4. norm과 함께 보기

```python
print(np.linalg.norm(a))
```

`norm`은 벡터의 크기입니다.

학습 포인트
- `dot`은 방향 관련성
- `norm`은 벡터 크기
- 두 값을 같이 보면 벡터 해석이 더 잘 됩니다.
