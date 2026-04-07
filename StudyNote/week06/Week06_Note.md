# Week 06 Note: Pandas 병합과 연결

## 1. 이 주제의 목적

6주차의 목표는 `Pandas`에서 여러 `DataFrame`을 하나로 다루는 두 가지 핵심 방식, 즉 `concat()`과 `merge()`를 정확히 구분하고 상황에 맞게 사용하는 것입니다.

이 주차에서 가장 중요한 질문은 문법이 아니라 아래입니다.

- 이 표들은 그냥 이어 붙이면 되는가?
- 아니면 공통 기준을 보고 매칭해야 하는가?
- 어떤 표를 기준으로 결과를 남겨야 하는가?

즉, 6주차는 단순 함수 사용법보다 데이터 관계를 읽는 훈련에 가깝습니다.

## 2. 왜 중요한가

실제 데이터는 보통 한 파일에 다 들어 있지 않습니다.

- 학생 정보표와 성적표가 따로 있을 수 있고
- 사용자 정보표와 구매 이력표가 분리되어 있을 수 있고
- 월별 데이터가 파일별로 나뉘어 있을 수 있습니다

이때 필요한 작업이 바로 병합과 연결입니다.

- 같은 구조의 표를 이어 붙이면 `concat()`
- 공통 열을 기준으로 서로 다른 정보를 결합하면 `merge()`

즉, 이 주제는 흩어진 데이터를 하나의 의미 있는 데이터셋으로 만드는 방법입니다.

## 3. 선수 개념

이번 주차를 보기 전에 아래 정도는 알고 있으면 좋습니다.

- 파이썬 `dict`, `list`
- `pd.DataFrame()` 기본 생성 방식
- 행과 열의 차이
- 결측치 `NaN` 의미

연결 포인트
- 실습 코드: [../../week06_Pandas_Merge_Concat.ipynb](../../week06_Pandas_Merge_Concat.ipynb)
- 이전 주차 구조 감각: [../week03/Week03_Note.md](../week03/Week03_Note.md)

## 4. 핵심 개념과 용어 해설

### 4-1. `DataFrame`

`DataFrame`은 Pandas의 기본 표 구조입니다.

```python
raw_data = {
    "subject_id": ["1", "2", "3", "4", "5"],
    "test_score": [51, 15, 15, 61, 16]
}

df_score = pd.DataFrame(raw_data, columns=["subject_id", "test_score"])
```

이 코드가 의미하는 것
- 파이썬 자료구조를 병합 가능한 표 구조로 바꾸는 단계입니다.

왜 필요한가
- `concat()`과 `merge()`는 `DataFrame`끼리 수행하는 연산이기 때문입니다.

### 4-2. 키 열(key column)

병합에서 가장 중요한 개념은 키 열입니다. 키 열은 서로 다른 표에서 같은 대상을 식별하는 기준입니다.

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

여기서 `subject_id`를 보면 떠올려야 하는 것
- 병합 기준점
- 같은 사람인지 판별하는 식별자
- 값이 어긋나면 병합 결과도 어긋난다

### 4-3. `concat()`

`concat()`은 표를 이어 붙이는 함수입니다.

```python
row_concat = pd.concat([df_class_a, df_class_b], axis=0, ignore_index=True)
```

핵심
- `axis=0`: 위아래 연결
- `axis=1`: 좌우 연결

왜 필요한가
- 같은 구조의 데이터가 여러 조각으로 나뉘어 있을 때 하나로 합치기 위해서입니다.

기억할 문장
- `concat()`은 "매칭"보다 "연결"입니다.

### 4-4. `merge()`

`merge()`는 공통 열을 기준으로 서로 다른 정보를 결합하는 함수입니다.

```python
inner_result = pd.merge(df_left, df_right, on="subject_id", how="inner")
```

왜 필요한가
- 이름표와 성적표처럼 서로 다른 속성을 가진 표를 하나로 맞춰야 하기 때문입니다.

기억할 문장
- `merge()`는 "이어 붙이기"가 아니라 "키 기준 매칭"입니다.

### 4-5. 조인 종류

#### `inner`

```python
pd.merge(df_left, df_right, on="subject_id", how="inner")
```

의미
- 양쪽에 모두 존재하는 키만 남김
- 교집합

#### `left`

```python
pd.merge(df_left, df_right, on="subject_id", how="left")
```

의미
- 왼쪽 표를 기준으로 모두 남김
- 오른쪽에 없으면 `NaN`

#### `right`

```python
pd.merge(df_left, df_right, on="subject_id", how="right")
```

의미
- 오른쪽 표를 기준으로 모두 남김

#### `outer`

```python
pd.merge(df_left, df_right, on="subject_id", how="outer")
```

의미
- 양쪽의 모든 키를 다 남김
- 합집합

시험장에서 기억할 문장
- `inner`: 둘 다 있는 것만
- `left`: 왼쪽 기준 보존
- `right`: 오른쪽 기준 보존
- `outer`: 전체 보존

### 4-6. 자주 쓰는 옵션

#### 열 이름이 다를 때

```python
pd.merge(df_left, df_attendance, left_on="subject_id", right_on="id", how="left")
```

왜 필요한가
- 실제 데이터에서는 같은 의미인데 열 이름이 다른 경우가 많기 때문입니다.

#### 같은 이름의 일반 열이 겹칠 때

```python
pd.merge(midterm, final, on="subject_id", suffixes=("_mid", "_final"))
```

왜 필요한가
- 병합 후 열 이름 충돌을 막고 의미를 보존하기 위해서입니다.

#### 병합 결과 출처 확인

```python
pd.merge(df_left, df_right, on="subject_id", how="outer", indicator=True)
```

왜 필요한가
- `left_only`, `right_only`, `both`로 어느 쪽에서 왔는지 검증할 수 있기 때문입니다.

## 5. 실습 파일과 핵심 흐름

관련 실습
- [../../week06_Pandas_Merge_Concat.ipynb](../../week06_Pandas_Merge_Concat.ipynb)

추천 실습 순서
1. `raw_data`를 `DataFrame`으로 만들기
2. `df_left`, `df_right` 준비하기
3. `concat(axis=0)`로 위아래 연결 보기
4. `concat(axis=1)`로 좌우 연결 보기
5. `inner`, `left`, `right`, `outer` 비교하기
6. `left_on`, `right_on`, `suffixes`, `indicator=True` 확인하기
7. 중복 키 사례 보기

실습 중 계속 확인할 질문
- 지금 이 표들은 누적 관계인가, 매칭 관계인가?
- 기준이 되는 키 열은 무엇인가?
- 병합 후 `NaN`이 생기면 왜 생겼는가?

## 6. 자주 하는 실수

### 실수 1. `concat()`과 `merge()`를 같은 것으로 생각함

올바른 방향
- `concat()`은 연결
- `merge()`는 키 기준 병합

### 실수 2. 키 열을 확인하지 않고 병합함

올바른 방향
- 공통 열 이름, 자료형, 중복 여부를 먼저 봐야 합니다.

### 실수 3. `NaN`을 함수 오류로 생각함

올바른 방향
- `NaN`은 종종 정상적인 병합 결과입니다.
- 어떤 키가 없었는지 먼저 봐야 합니다.

### 실수 4. `concat(axis=1)`를 병합처럼 사용함

올바른 방향
- 순서 기반 결합과 키 기반 결합은 다릅니다.
- 객체 기준 매칭이 필요하면 `merge()`가 우선입니다.

### 실수 5. 열 이름 오타를 놓침

올바른 방향
- 수업 예시에서 `test_scor` 같은 오타가 보이면 실제 코드는 반드시 확인해야 합니다.

## 7. 시험 대비 포인트

시험 직전에는 아래를 설명할 수 있어야 합니다.

- `concat()`과 `merge()` 차이
- 키 열이 왜 중요한가
- `inner`, `left`, `right`, `outer` 차이
- `NaN`이 생기는 이유
- `left_on`, `right_on`, `suffixes`, `indicator=True`의 목적

서술형 답안 구조 예시

> Pandas에서 여러 `DataFrame`을 합칠 때는 `concat()`과 `merge()`를 사용한다. `concat()`은 같은 구조의 데이터를 위아래 또는 좌우로 단순 연결할 때 사용하며, `merge()`는 `subject_id`와 같은 공통 키를 기준으로 서로 다른 정보를 결합할 때 사용한다. `merge()`에서는 `inner`, `left`, `right`, `outer` 조인을 선택할 수 있고, 기준 표에 없는 데이터는 `NaN`으로 나타날 수 있다. 실전에서는 병합 전 열 이름, 자료형, 중복 키를 먼저 점검하는 것이 중요하다.

## 8. 기존 문서와 연결 포인트

- 실습 코드: [../../week06_Pandas_Merge_Concat.ipynb](../../week06_Pandas_Merge_Concat.ipynb)
- 보충 문서: [notes/00_dataframe_and_keys.md](./notes/00_dataframe_and_keys.md)
- 보충 문서: [notes/01_concat_basics.md](./notes/01_concat_basics.md)
- 보충 문서: [notes/02_merge_join_types.md](./notes/02_merge_join_types.md)
- 보충 문서: [notes/03_merge_options_and_cautions.md](./notes/03_merge_options_and_cautions.md)
- 보충 문서: [notes/04_practice_walkthrough.md](./notes/04_practice_walkthrough.md)

## 9. 빠른 요약

- 6주차의 핵심은 `concat()`과 `merge()`를 구분하는 것입니다.
- `concat()`은 연결, `merge()`는 키 기준 병합입니다.
- 키 열, 자료형, 중복 여부를 먼저 봐야 합니다.
- 조인 종류는 어떤 표를 기준으로 남길지 결정하는 문제입니다.
