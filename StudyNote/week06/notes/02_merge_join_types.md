# 02. `merge()`와 조인 종류

`merge()`는 Pandas에서 가장 중요한 데이터 결합 함수 중 하나입니다.
공통 열을 기준으로 두 표를 맞춰 붙입니다.

연결 실습
- [../week06_Pandas_Merge_Concat.ipynb](../week06_Pandas_Merge_Concat.ipynb)

## 1. 기본 형태

```python
merged = pd.merge(df_left, df_right, on="subject_id")
print(merged)
```

기본값
- `on="subject_id"`: 어떤 열을 기준으로 합칠지 지정
- `how="inner"`가 기본값

## 2. `inner` merge

```python
pd.merge(df_left, df_right, on="subject_id", how="inner")
```

의미
- 양쪽 표에 모두 있는 키만 남긴다.
- 교집합 개념으로 이해하면 쉽다.

예시 해석
- 왼쪽에 `1, 2, 3, 4`
- 오른쪽에 `1, 2, 3, 5`
- 결과는 `1, 2, 3`

## 3. `left` merge

```python
pd.merge(df_left, df_right, on="subject_id", how="left")
```

의미
- 왼쪽 표를 기준으로 모두 남긴다.
- 오른쪽에 없는 값은 `NaN`

예시 해석
- 왼쪽의 `4`는 오른쪽에 없으므로 점수 자리에 `NaN`이 들어간다.

## 4. `right` merge

```python
pd.merge(df_left, df_right, on="subject_id", how="right")
```

의미
- 오른쪽 표를 기준으로 모두 남긴다.
- 왼쪽에 없는 값은 `NaN`

예시 해석
- 오른쪽의 `5`는 왼쪽에 없으므로 이름 자리에 `NaN`이 들어간다.

## 5. `outer` merge

```python
pd.merge(df_left, df_right, on="subject_id", how="outer")
```

의미
- 양쪽 표의 모든 키를 다 남긴다.
- 합집합 개념으로 보면 쉽다.

예시 해석
- `1, 2, 3, 4, 5`가 모두 결과에 포함된다.
- 없는 값은 각 위치에 `NaN`으로 채워진다.

## 6. 그림 없이 이해하는 빠른 기억법

- `inner`: 둘 다 있는 것만
- `left`: 왼쪽은 무조건 살림
- `right`: 오른쪽은 무조건 살림
- `outer`: 전부 살림

## 7. 왜 병합 결과 행 수가 예상과 다를까?

아래 상황이면 결과가 늘어날 수 있습니다.

```python
df_dup = pd.DataFrame({
    "subject_id": ["1", "1", "2"],
    "quiz_score": [10, 20, 30]
})
```

이 경우 `subject_id='1'`이 두 번 있으므로, 같은 키와 여러 번 매칭될 수 있습니다.

정리
- 조인 종류를 외우는 것도 중요하지만,
- 더 중요한 것은 어떤 표를 기준으로 남길지 결정하는 것입니다.
