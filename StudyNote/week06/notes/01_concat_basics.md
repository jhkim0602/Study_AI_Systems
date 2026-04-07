# 01. `concat()`으로 연결하기

`concat()`은 표를 위아래로 붙이거나, 옆으로 붙이는 함수입니다.
핵심은 "기준 열을 맞춰 합친다"가 아니라 "그냥 이어 붙인다"에 가깝습니다.

연결 실습
- [../../../../week06_Pandas_Merge_Concat.ipynb](../../../../week06_Pandas_Merge_Concat.ipynb)

## 1. 행 방향 연결

```python
df_class_a = pd.DataFrame({
    "subject_id": ["1", "2"],
    "test_score": [80, 90]
})

df_class_b = pd.DataFrame({
    "subject_id": ["3", "4"],
    "test_score": [75, 88]
})

row_concat = pd.concat([df_class_a, df_class_b], axis=0, ignore_index=True)
print(row_concat)
```

해석
- `axis=0`은 위아래로 붙입니다.
- 같은 열 이름을 가진 표끼리 연결할 때 가장 자연스럽습니다.
- `ignore_index=True`를 쓰면 인덱스를 0부터 다시 정리합니다.

## 2. 열 방향 연결

```python
df_name = pd.DataFrame({
    "first_name": ["Alex", "Amy", "Allen"]
})

df_score = pd.DataFrame({
    "test_score": [51, 15, 15]
})

col_concat = pd.concat([df_name, df_score], axis=1)
print(col_concat)
```

해석
- `axis=1`은 좌우로 붙입니다.
- 이 경우 행의 순서가 같은 데이터라는 가정이 필요합니다.
- 순서가 어긋나 있으면 잘못 붙을 수 있습니다.

## 3. `concat()`과 `merge()` 차이

- `concat()`은 단순 연결에 가깝다.
- `merge()`는 키 열을 기준으로 맞춰서 결합한다.

쉽게 보면
- 출석부 두 장을 그냥 이어 붙이면 `concat()`
- 학생번호를 기준으로 이름표와 성적표를 맞춰 합치면 `merge()`

## 4. `concat()`에서 `NaN`이 생기는 이유

열 구성이 다르면 비어 있는 자리는 `NaN`이 들어갑니다.

```python
df_one = pd.DataFrame({"subject_id": ["1", "2"], "math": [90, 80]})
df_two = pd.DataFrame({"subject_id": ["3", "4"], "english": [85, 95]})

mixed_concat = pd.concat([df_one, df_two], ignore_index=True)
print(mixed_concat)
```

이유
- 첫 번째 표에는 `english` 열이 없음
- 두 번째 표에는 `math` 열이 없음
- 그래서 빈 칸을 `NaN`으로 채운다

정리
- `concat()`은 구조가 같을수록 다루기 쉽습니다.
- 구조가 다르면 결측치가 생길 수 있다는 점까지 함께 봐야 합니다.
