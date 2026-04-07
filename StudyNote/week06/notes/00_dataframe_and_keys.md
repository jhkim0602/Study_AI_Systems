# 00. DataFrame 생성과 키 열 이해

이 문서는 병합과 연결을 배우기 전에, 실습에 자주 등장하는 `DataFrame` 생성 방식과 키 열의 의미를 먼저 정리합니다.

연결 실습
- [../../../../week06_Pandas_Merge_Concat.ipynb](../../../../week06_Pandas_Merge_Concat.ipynb)

## 1. 수업에서 자주 보는 생성 방식

교수님 수업에서는 보통 아래처럼 딕셔너리 데이터나 리스트 데이터를 `DataFrame`으로 바꾸는 예제가 먼저 나옵니다.

```python
import pandas as pd

raw_data = {
    "subject_id": ["1", "2", "3", "4", "5"],
    "test_score": [51, 15, 15, 61, 16]
}

df = pd.DataFrame(raw_data, columns=["subject_id", "test_score"])
print(df)
```

핵심
- `raw_data`는 아직 표가 아니라 파이썬 자료구조입니다.
- `pd.DataFrame()`을 써서 표 형태로 바꿉니다.
- `columns=[...]`를 쓰면 열 순서를 명확하게 고정할 수 있습니다.

## 2. 키 열이란?

병합에서 가장 중요한 열은 보통 `subject_id` 같은 공통 식별자입니다.

```python
df_left = pd.DataFrame({
    "subject_id": ["1", "2", "3", "4"],
    "first_name": ["Alex", "Amy", "Allen", "Alice"]
})

df_right = pd.DataFrame({
    "subject_id": ["1", "2", "3", "5"],
    "test_score": [51, 15, 15, 61]
})
```

여기서 `subject_id`가 바로 키 열입니다.

의미
- 왼쪽 표의 누구와 오른쪽 표의 누구를 연결할지 알려준다.
- 같은 `subject_id`를 가진 행끼리 한 사람의 정보라고 보고 결합한다.
- 키가 맞지 않으면 일부 데이터는 빠지거나 `NaN`이 생긴다.

## 3. 왜 키 열을 먼저 봐야 하나?

병합이 헷갈리는 가장 큰 이유는 문법보다 데이터 관계를 먼저 보지 않기 때문입니다.

먼저 확인할 것
- 두 표에 공통 열이 있는가?
- 그 열의 이름이 같은가?
- 값의 형식이 같은가? 예: 문자열 `"1"` 과 정수 `1`
- 중복 키가 있는가?

## 4. 실수 포인트

- 열 이름 오타: `test_scor`, `subjet_id` 같은 오타
- 자료형 불일치: 왼쪽은 정수, 오른쪽은 문자열
- 키 중복: 한쪽에 같은 `subject_id`가 여러 번 있으면 결과 행 수가 늘어날 수 있다

정리
- 병합은 표 두 개를 합치는 문법이지만,
- 실제로는 "같은 대상을 찾는 작업"입니다.
- 그래서 병합 전에 키 열을 먼저 확인하는 습관이 가장 중요합니다.
