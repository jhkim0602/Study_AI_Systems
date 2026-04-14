# 03. 자주 쓰는 옵션과 실수 방지

병합은 기본 문법만 알아도 시작할 수 있지만, 실제로는 몇 가지 옵션을 함께 알아야 훨씬 덜 헷갈립니다.

연결 실습
- [../week06_Pandas_Merge_Concat.ipynb](../week06_Pandas_Merge_Concat.ipynb)

## 1. 열 이름이 다를 때

```python
df_left = pd.DataFrame({
    "subject_id": ["1", "2", "3"],
    "first_name": ["Alex", "Amy", "Allen"]
})

df_attendance = pd.DataFrame({
    "id": ["1", "2", "4"],
    "attendance": ["Y", "N", "Y"]
})

pd.merge(df_left, df_attendance, left_on="subject_id", right_on="id", how="left")
```

의미
- 왼쪽 기준 열 이름과 오른쪽 기준 열 이름이 다를 때 사용한다.
- `on=` 대신 `left_on=`, `right_on=`을 쓴다.

## 2. 같은 이름의 일반 열이 겹칠 때

```python
midterm = pd.DataFrame({
    "subject_id": ["1", "2"],
    "test_score": [80, 90]
})

final = pd.DataFrame({
    "subject_id": ["1", "2"],
    "test_score": [85, 95]
})

pd.merge(midterm, final, on="subject_id", suffixes=("_mid", "_final"))
```

의미
- 공통 키 외에 같은 이름의 열이 있으면 자동으로 이름 충돌이 난다.
- 이때 `suffixes`로 구분 이름을 붙인다.

## 3. 병합 결과 확인하기

```python
checked = pd.merge(
    df_left,
    df_attendance,
    left_on="subject_id",
    right_on="id",
    how="outer",
    indicator=True
)
print(checked)
```

의미
- `_merge` 열이 새로 생긴다.
- `left_only`, `right_only`, `both` 값으로 어느 쪽에서 왔는지 확인할 수 있다.

## 4. 실전에서 꼭 확인할 것

- 병합 기준 열에 공백이 섞여 있지 않은가
- 자료형이 같은가
- 중복 키가 있는가
- `NaN`이 생긴 행은 왜 생겼는가

## 5. 실수 방지용 점검 코드

```python
print(df_left.dtypes)
print(df_attendance.dtypes)
print(df_left["subject_id"].duplicated().sum())
print(df_attendance["id"].duplicated().sum())
```

정리
- 병합 오류의 대부분은 문법보다 데이터 상태 문제에서 생깁니다.
- 열 이름, 자료형, 중복 여부를 먼저 확인하면 훨씬 안정적으로 병합할 수 있습니다.
