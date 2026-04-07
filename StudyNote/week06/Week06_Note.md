# Week 06 Note Map: Pandas 병합과 연결

이번 문서는 6주차 Pandas 병합과 연결 학습 노트의 허브입니다.
이번 주차는 여러 표를 한 번에 이어 붙이거나, 공통 열을 기준으로 결합하는 방법을 익히는 데 집중합니다.

기본 학습 동선
1. 실습 코드: [../../week06_Pandas_Merge_Concat.ipynb](../../week06_Pandas_Merge_Concat.ipynb)
2. DataFrame 생성과 키 열 이해: [notes/00_dataframe_and_keys.md](./notes/00_dataframe_and_keys.md)
3. `concat()`으로 연결하기: [notes/01_concat_basics.md](./notes/01_concat_basics.md)
4. `merge()`로 병합하기: [notes/02_merge_join_types.md](./notes/02_merge_join_types.md)
5. 자주 쓰는 옵션과 실수 방지: [notes/03_merge_options_and_cautions.md](./notes/03_merge_options_and_cautions.md)
6. 실습 흐름 요약: [notes/04_practice_walkthrough.md](./notes/04_practice_walkthrough.md)

주차 구성
- 실습 코드: [../../week06_Pandas_Merge_Concat.ipynb](../../week06_Pandas_Merge_Concat.ipynb)
- 이미지 자산: [assets](./assets)
- 파트별 노트: [notes](./notes)

실습 코드와 노트 매핑
- 노트북 `0 ~ 1`번 섹션: [00_dataframe_and_keys.md](./notes/00_dataframe_and_keys.md)
- 노트북 `2 ~ 3`번 섹션: [01_concat_basics.md](./notes/01_concat_basics.md)
- 노트북 `4 ~ 7`번 섹션: [02_merge_join_types.md](./notes/02_merge_join_types.md)
- 노트북 `8 ~ 9`번 섹션: [03_merge_options_and_cautions.md](./notes/03_merge_options_and_cautions.md)
- 노트북 `10`번 섹션: [04_practice_walkthrough.md](./notes/04_practice_walkthrough.md)

학습 목표
- `pd.DataFrame()`으로 병합 실습용 표를 직접 만들 수 있다.
- `pd.concat()`의 행 방향, 열 방향 연결 차이를 설명할 수 있다.
- `merge()`의 `inner`, `left`, `right`, `outer` 차이를 설명할 수 있다.
- 병합 기준 열인 키(`key`)의 의미를 이해할 수 있다.
- 같은 이름의 열이 겹칠 때 `suffixes`를 사용할 수 있다.
- 병합 결과를 `indicator=True`로 확인할 수 있다.
- `left_on`, `right_on`으로 열 이름이 달라도 병합할 수 있다.

학습 메모
- 수업 예시에서 `DataFrame(raw_data, columns=['subject_id', 'test_scor'])`처럼 보이는 코드는 보통 `test_score` 오타를 포함한 예시인 경우가 있습니다.
- 실제 실습에서는 열 이름을 정확히 맞추는 것이 중요합니다. `subject_id` 같은 공통 열이 병합의 기준점이 됩니다.
