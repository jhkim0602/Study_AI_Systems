# 04. 실습 흐름 요약

이 문서는 6주차 실습 코드를 어떤 순서로 보면 좋은지 빠르게 정리합니다.

연결 실습
- [../week06_Pandas_Merge_Concat.ipynb](../week06_Pandas_Merge_Concat.ipynb)

## 1. 첫 단계: 표 직접 만들기

실습은 `raw_data`를 `DataFrame`으로 만드는 것부터 시작합니다.

```python
raw_data = {
    "subject_id": ["1", "2", "3", "4", "5"],
    "test_score": [51, 15, 15, 61, 16]
}
df_score = pd.DataFrame(raw_data, columns=["subject_id", "test_score"])
```

여기서 중요한 것은
- 열 이름을 분명히 적는 것
- 병합에 사용할 `subject_id`를 눈에 익히는 것

## 2. 두 번째: `concat()`으로 연결 감각 익히기

처음에는 병합보다 연결이 더 단순합니다.

- 위아래 연결: `axis=0`
- 좌우 연결: `axis=1`

이 단계에서는
- 인덱스가 어떻게 바뀌는지
- 열 구성이 다르면 왜 `NaN`이 생기는지

를 보면 됩니다.

## 3. 세 번째: `merge()`로 키 기준 결합

이제 `df_left`, `df_right`를 놓고 `subject_id` 기준으로 합칩니다.

순서 추천
1. `inner`
2. `left`
3. `right`
4. `outer`

이 순서가 좋은 이유
- 교집합부터 보면 가장 단순하다
- 그다음 어느 표를 기준으로 남기는지 비교하기 쉽다

## 4. 네 번째: 실제 수업에서 자주 나오는 변형

- 열 이름이 다를 때: `left_on`, `right_on`
- 같은 이름의 점수 열이 겹칠 때: `suffixes`
- 결과 출처를 보고 싶을 때: `indicator=True`

## 5. 마지막 체크

실습이 끝나면 아래 질문에 답할 수 있으면 됩니다.

- `concat()`과 `merge()`는 언제 다르게 쓰는가?
- `left merge`와 `outer merge` 차이는 무엇인가?
- 키 열이 맞지 않으면 왜 `NaN`이 생기는가?
- 열 이름이 다를 때는 어떤 옵션을 써야 하는가?

이 네 가지를 설명할 수 있으면 이번 주차 핵심은 잡은 것입니다.
