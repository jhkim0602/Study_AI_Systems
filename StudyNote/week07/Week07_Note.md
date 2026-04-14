# Week 07 Note: Matplotlib 히스토그램과 Seaborn 기초

## 1. 이 주제의 목적

7주차의 목표는 숫자 데이터와 관계형 데이터를 그래프로 읽는 기본 감각을 잡는 것입니다.  
이번 주차는 두 단계로 누적해서 진행합니다.

1. `matplotlib`로 히스토그램을 그리고 분포를 읽는다.
2. 같은 흐름을 `seaborn`으로 확장해서 더 분석 친화적으로 시각화한다.

즉, 7주차는 단순히 그래프를 "그리는 법"만 배우는 주차가 아닙니다.  
어떤 상황에서 어떤 그래프를 써야 하는지 판단하는 주차입니다.

이번 주차가 끝나면 아래 질문에 답할 수 있어야 합니다.

- 숫자 데이터가 어떤 구간에 몰려 있는지 어떻게 보는가
- 시간에 따라 값이 어떻게 변하는지 어떻게 보는가
- 두 수치형 변수 사이의 관계는 어떤 그래프로 보는가
- `matplotlib`와 `seaborn`은 어떤 관계인가

## 2. 왜 중요한가

데이터 분석에서는 표를 숫자로만 보는 것보다, 그래프로 바꾸어 읽는 순간 이해가 빨라집니다.

- 평균만 보면 분포 모양을 놓칠 수 있습니다
- 분포만 보면 시간 흐름을 놓칠 수 있습니다
- 시간 흐름만 보면 변수 사이 관계를 놓칠 수 있습니다

그래서 이번 주차에서는 아래 세 가지를 구분해야 합니다.

- `hist()` / `histplot()`: 분포
- `lineplot()`: 시간 흐름, 추세
- `scatterplot()`: 두 변수 사이 관계

이 구분이 잡혀야 이후 데이터 분석, 머신러닝 전처리, 결과 해석도 자연스럽게 이어집니다.

## 3. 선수 개념

이번 주차를 보기 전에 아래 정도는 이해하고 있으면 좋습니다.

- `NumPy` 배열과 난수 생성
- `Pandas DataFrame`과 `Series`
- `matplotlib`의 `figure`, `subplot`, `subplots`, `plot`
- `Pandas`의 `sort_values()`

연결 포인트
- 배열과 난수 기초: [../week03/Week03_Note.md](../week03/Week03_Note.md)
- `Pandas`와 `matplotlib` 기본 복습: [../week06/Week06_Note.md](../week06/Week06_Note.md)
- 실습 코드: [../../week07_Matplotlib_Seaborn.ipynb](../../week07_Matplotlib_Seaborn.ipynb)
- 실습 데이터: [data/fmri.csv](./data/fmri.csv)

## 4. 핵심 개념과 용어 해설

### 4-1. 히스토그램 전에 먼저 보는 기초 함수

히스토그램은 분포를 한눈에 보여 주는 도구입니다.  
하지만 그래프를 그리기 전에 숫자 데이터 상태를 먼저 점검하는 습관이 중요합니다.

> **참고 시각 자료: 정렬에서 히스토그램으로 가는 흐름**
> ![Distribution Workflow](./assets/week07_distribution_workflow.svg)

핵심 흐름
- 먼저 정렬해서 값의 순서를 본다
- 최솟값과 최댓값으로 범위를 본다
- 평균과 중앙값으로 중심을 본다
- 표준편차와 사분위수로 퍼짐을 본다
- 그 다음에 히스토그램으로 전체 분포 모양을 본다

#### `np.sort()`

```python
sorted_data = np.sort(data)
```

의미
- 데이터를 작은 값부터 큰 값까지 정렬합니다.

왜 필요한가
- 값의 흐름을 직접 볼 수 있습니다.
- 앞쪽과 뒤쪽 끝값을 통해 이상치 감각도 얻을 수 있습니다.

#### `np.min()`, `np.max()`

```python
data_min = np.min(data)
data_max = np.max(data)
```

의미
- 데이터의 최솟값과 최댓값을 구합니다.

왜 필요한가
- 값이 어느 범위에 있는지 알 수 있습니다.
- 히스토그램 x축 해석의 기준이 됩니다.

#### `np.mean()`, `np.median()`

```python
data_mean = np.mean(data)
data_median = np.median(data)
```

의미
- `mean`: 산술평균
- `median`: 중앙값

왜 필요한가
- 데이터 중심이 어디쯤인지 알 수 있습니다.
- 이상치가 있으면 평균과 중앙값 차이도 의미 있는 신호가 됩니다.

#### `np.std()`

```python
data_std = np.std(data)
```

의미
- 평균 주변으로 데이터가 얼마나 퍼져 있는지를 보여 줍니다.

#### `np.percentile()`

```python
q1, q2, q3 = np.percentile(data, [25, 50, 75])
```

의미
- 25%, 50%, 75% 지점을 구합니다.
- `q2`는 중앙값과 같습니다.

#### `Series.sort_values()`

```python
df["score"].sort_values()
```

왜 필요한가
- 실제 분석에서는 `NumPy` 배열보다 `Pandas` 열을 직접 다루는 경우가 많기 때문입니다.

정리하면
- `sort`: 값의 순서
- `min/max`: 범위
- `mean/median`: 중심
- `std/percentile`: 퍼짐
- `hist`: 분포 모양

### 4-2. 히스토그램이란?

히스토그램은 연속형 숫자 데이터를 여러 구간(`bin`)으로 나누고, 각 구간에 값이 몇 개 들어 있는지 막대 높이로 보여 주는 그래프입니다.

> **참고 시각 자료: 히스토그램의 기본 구조**
> ![Histogram Concept](./assets/week07_histogram_concept.svg)

핵심
- x축: 값의 구간
- y축: 각 구간의 개수 또는 비율

왜 중요한가
- 숫자 데이터가 어디에 몰려 있는지 빠르게 볼 수 있기 때문입니다.

### 4-3. `plt.hist()`

히스토그램의 가장 기본 함수는 `plt.hist()`입니다.

```python
plt.hist(data)
plt.show()
```

조금 더 명시적으로 쓰면 아래와 같습니다.

```python
plt.hist(data, bins=10, color="skyblue", edgecolor="black")
plt.show()
```

이 코드가 의미하는 것
- `data`: 원본 숫자 데이터
- `bins=10`: 구간을 10개로 나눔
- `color`: 막대 채우기 색
- `edgecolor`: 막대 경계선 색

### 4-4. `bins`

히스토그램에서 가장 중요한 개념은 `bins`입니다.

```python
plt.hist(data, bins=5)
plt.hist(data, bins=30)
```

같은 데이터라도 `bins` 값에 따라 그래프 모양이 크게 달라집니다.

> **참고 시각 자료: bin 개수 차이**
> ![Bins Effect](./assets/week07_bins_effect.svg)

기억할 것
- `bins`가 너무 적으면 너무 거칠게 보입니다
- `bins`가 너무 많으면 잡음이 심해 보일 수 있습니다

### 4-5. count, density, alpha, cumulative

#### count와 density

기본 히스토그램은 각 구간의 개수(`count`)를 보여 줍니다.

```python
plt.hist(data, bins=20)
```

반면 `density=True`를 주면 정규화된 분포 형태를 봅니다.

```python
plt.hist(data, bins=20, density=True)
```

> **참고 시각 자료: count vs density**
> ![Count vs Density](./assets/week07_count_vs_density.svg)

왜 중요한가
- 샘플 수가 다른 집단 비교에서는 `density`가 더 공정할 수 있기 때문입니다.

#### `alpha`

```python
plt.hist(data_a, bins=20, alpha=0.5, label="A")
plt.hist(data_b, bins=20, alpha=0.5, label="B")
plt.legend()
```

의미
- `alpha`는 투명도입니다.

왜 필요한가
- 그래프를 겹쳐 그릴 때 하나가 다른 하나를 완전히 가리지 않게 하기 위해서입니다.

#### `cumulative=True`

```python
plt.hist(data, bins=20, cumulative=True)
```

왜 필요한가
- 특정 값 이하가 얼마나 누적되는지 볼 수 있기 때문입니다.

### 4-6. `ax.hist()`

여러 축을 다룰 때는 `plt.hist()`보다 `ax.hist()`가 더 관리하기 쉽습니다.

```python
fig, ax = plt.subplots(figsize=(6, 4))
ax.hist(data, bins=20, color="steelblue", edgecolor="black")
ax.set_title("Histogram")
```

기억할 것
- `plt.hist()`: 빠른 단일 그래프
- `ax.hist()`: 객체 기반 제어

### 4-7. `matplotlib`에서 `seaborn`으로 왜 확장하는가

`matplotlib`만으로도 충분히 강력한 그래프를 그릴 수 있습니다.  
하지만 실제 분석에서는 아래 요구가 자주 생깁니다.

- `DataFrame` 열 이름으로 바로 그리고 싶다
- 기본 스타일이 보기 좋았으면 좋겠다
- 그룹 비교를 더 쉽게 하고 싶다

이때 `seaborn`이 자연스럽게 이어집니다.

### 4-8. `seaborn`은 무엇인가

`seaborn`은 `matplotlib` 위에서 동작하는 통계 시각화 라이브러리입니다.

> **참고 시각 자료: seaborn의 위치**
> ![Seaborn Role](./assets/week07_seaborn_role.svg)

이 말을 이해할 때 떠올려야 할 것
- 내부적으로는 `matplotlib`를 활용합니다
- 더 좋은 기본 스타일을 제공합니다
- `Pandas DataFrame`과 잘 연결됩니다

### 4-9. 기본 모듈 호출과 테마 설정

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

sns.set_theme(style="whitegrid")
```

이 코드를 보면 바로 떠올려야 할 것
- `NumPy`: 수치 계산
- `Pandas`: 표 처리
- `matplotlib`: 화면과 축 제어
- `seaborn`: 분석 친화적 시각화

### 4-10. `fmri` 데이터 사용

이번 주차 후반에서는 `seaborn` 예제로 `fmri` 데이터를 사용합니다.  
실행 재현성을 위해 로컬 CSV로 함께 둡니다.

```python
fmri = pd.read_csv("StudyNote/week07/data/fmri.csv")
fmri.head()
```

주요 열
- `subject`: 실험 대상
- `timepoint`: 시간 축
- `event`: 자극 종류
- `region`: 뇌 영역
- `signal`: 측정 신호

왜 중요한가
- 시간 흐름과 그룹 비교가 함께 있는 실제 형태의 데이터이기 때문입니다.

### 4-11. `sns.histplot()`

`matplotlib`의 `plt.hist()`를 배웠다면, 그 다음은 `seaborn`의 `sns.histplot()`입니다.

```python
sns.histplot(data=df_scores, x="score", bins=15)
plt.show()
```

왜 필요한가
- `DataFrame` 열 이름으로 바로 시각화할 수 있기 때문입니다.
- 그룹 비교와 스타일 확장이 더 자연스럽기 때문입니다.

자주 보는 옵션
- `bins`: 구간 수
- `kde=True`: 부드러운 분포 곡선
- `hue`: 그룹별 색상 구분
- `stat="density"`: 개수 대신 밀도 기준

> **참고 시각 자료: `histplot` 핵심 옵션**
> ![Seaborn Histplot Options](./assets/week07_histplot_options.svg)

### 4-12. `sns.lineplot()`과 시간 흐름 데이터

히스토그램이 분포를 보는 도구라면, `lineplot`은 시간이나 순서에 따른 변화를 보는 도구입니다.

```python
sns.lineplot(data=fmri, x="timepoint", y="signal")
plt.show()
```

그룹 비교까지 하고 싶다면 아래처럼 확장합니다.

```python
sns.lineplot(
    data=fmri,
    x="timepoint",
    y="signal",
    hue="event",
    style="region"
)
plt.show()
```

이 코드가 의미하는 것
- `x="timepoint"`: 시간 축
- `y="signal"`: 측정값
- `hue="event"`: 자극 종류별 색 구분
- `style="region"`: 영역별 선 스타일 구분

> **참고 시각 자료: `fmri`에서 `lineplot` 읽기**
> ![Seaborn Lineplot FMRI](./assets/week07_fmri_lineplot.svg)

### 4-13. 산점도(scatterplot)란 무엇인가

산점도는 두 수치형 변수의 값을 x축과 y축에 두고, 각 관측값을 점 하나로 표현하는 그래프입니다.

쉽게 말하면
- 점 1개 = 데이터 1개
- x축 = 첫 번째 수치형 변수
- y축 = 두 번째 수치형 변수

입니다.

> **참고 시각 자료: 산점도의 기본 해석**
> ![Seaborn Scatterplot Concept](./assets/week07_scatterplot_concept.svg)

산점도를 보면 무엇을 읽어야 하는가
- 양의 관계: x가 커질수록 y도 커지는가
- 음의 관계: x가 커질수록 y가 작아지는가
- 군집: 점들이 그룹으로 나뉘는가
- 이상치: 다른 점들과 멀리 떨어진 점이 있는가

### 4-14. `sns.scatterplot()`

가장 기본적인 문법은 아래와 같습니다.

```python
sns.scatterplot(data=df_study, x="study_hours", y="score")
plt.show()
```

왜 필요한가
- 두 수치형 변수 사이의 관계를 시각적으로 확인할 수 있기 때문입니다.

자주 보는 옵션

#### `hue`

```python
sns.scatterplot(data=df_study, x="study_hours", y="score", hue="group")
```

역할
- 그룹별로 점 색을 다르게 합니다.

#### `style`

```python
sns.scatterplot(data=df_study, x="study_hours", y="score", hue="group", style="passed")
```

역할
- 점 모양을 다르게 합니다.

#### `size`

```python
sns.scatterplot(data=df_study, x="study_hours", y="score", size="sleep_hours")
```

역할
- 세 번째 수치 정보를 점 크기로 표현합니다.

기억할 것
- `hue`: 색
- `style`: 모양
- `size`: 크기

### 4-15. 언제 어떤 그래프를 쓰는가

아래 구분이 가장 중요합니다.

- `hist()` / `histplot()`: 분포
- `lineplot()`: 시간 흐름, 추세
- `scatterplot()`: 두 변수 관계
- `countplot()`: 범주형 개수 비교
- `boxplot()`: 중앙값, 사분위수, 이상치

## 5. 실습 파일과 핵심 흐름

관련 실습
- [../../week07_Matplotlib_Seaborn.ipynb](../../week07_Matplotlib_Seaborn.ipynb)
- [data/fmri.csv](./data/fmri.csv)

추천 실습 순서
1. `np.sort()`, `min/max`, `mean/median`, `std`, `percentile`로 데이터 상태 보기
2. `plt.hist()`로 기본 히스토그램 그리기
3. `bins`, `density`, `alpha`, `cumulative`를 바꿔 보기
4. `ax.hist()`와 `Series.sort_values()` 확인하기
5. `sns.set_theme()`로 `seaborn` 기본 스타일 적용하기
6. `sns.histplot()`으로 같은 분포를 다시 보기
7. `fmri.csv`를 읽고 `lineplot()`으로 시간 흐름 보기
8. `scatterplot()`으로 두 변수 사이 관계 보기
9. `hue`, `style`, `size`를 바꾸며 표현 범위를 넓혀 보기

실습 중 계속 확인할 질문
- 지금 보고 싶은 것은 분포인가?
- 시간 흐름인가?
- 두 변수 관계인가?
- `matplotlib`와 `seaborn` 중 어느 쪽이 더 자연스러운가?

## 6. 자주 하는 실수

### 실수 1. 히스토그램을 막대그래프와 같은 것으로 생각함

올바른 방향
- 히스토그램은 연속형 수치 데이터를 구간별로 집계하는 그래프입니다.
- 막대그래프는 이미 집계된 범주형 값을 표현합니다.

### 실수 2. `bins`를 아무 생각 없이 두고 해석함

올바른 방향
- `bins`를 바꾸면 같은 데이터도 다르게 보입니다.
- 해석 전에 구간 수가 적절한지 먼저 봐야 합니다.

### 실수 3. `density`와 `count`를 구분하지 않음

올바른 방향
- y축이 개수 기준인지 밀도 기준인지 먼저 확인해야 합니다.

### 실수 4. `seaborn`을 `matplotlib`와 완전히 다른 것으로 생각함

올바른 방향
- `seaborn`은 `matplotlib` 위에서 동작합니다.
- 둘은 끊어지는 것이 아니라 이어집니다.

### 실수 5. `lineplot`으로 범주 비교를 하려고 함

올바른 방향
- `lineplot`은 시간이나 순서가 있는 데이터에 더 적합합니다.

### 실수 6. 산점도에서 점 하나가 무엇을 뜻하는지 생각하지 않음

올바른 방향
- 산점도에서 점 1개는 관측값 1개입니다.
- 어떤 행(row)을 나타내는지 생각해야 해석이 정확합니다.

### 실수 7. `scatterplot`에 범주형 열을 억지로 넣음

올바른 방향
- `scatterplot`은 기본적으로 두 수치형 변수의 관계를 볼 때 가장 적합합니다.

## 7. 시험 대비 포인트

시험 직전에는 아래를 설명할 수 있어야 합니다.

- `np.sort()`, `min/max`, `mean/median`, `std`, `percentile`의 역할
- 히스토그램이 무엇인가
- `bins`, `density`, `alpha`, `cumulative`의 의미
- `plt.hist()`와 `ax.hist()` 차이
- `seaborn`이 무엇인가
- `sns.histplot()`, `sns.lineplot()`, `sns.scatterplot()` 기본 문법
- `hue`, `style`, `size`, `kde`, `stat` 의미
- `histplot`, `lineplot`, `scatterplot` 차이

서술형 답안 구조 예시

> 히스토그램은 연속형 수치 데이터를 여러 구간으로 나누고 각 구간의 개수나 비율을 막대로 나타내는 그래프이다. `matplotlib`에서는 `plt.hist()`나 `ax.hist()`를 사용하며, `bins`는 구간 개수를 정하는 핵심 옵션이다. 이후 `seaborn`으로 확장하면 `sns.histplot()`으로 같은 분포를 더 분석 친화적으로 그릴 수 있고, 시간 흐름은 `sns.lineplot()`, 두 수치형 변수 사이 관계는 `sns.scatterplot()`으로 표현한다. 이때 `hue`, `style`, `size`를 이용하면 그룹 정보와 추가 변수도 함께 표현할 수 있다.

## 8. 기존 문서와 연결 포인트

- 배열과 난수: [../week03/Week03_Note.md](../week03/Week03_Note.md)
- `Pandas`, `matplotlib` 기본: [../week06/Week06_Note.md](../week06/Week06_Note.md)
- 실습 코드: [../../week07_Matplotlib_Seaborn.ipynb](../../week07_Matplotlib_Seaborn.ipynb)
- 실습 데이터: [data/fmri.csv](./data/fmri.csv)

## 9. 빠른 요약

- 7주차는 `matplotlib` 히스토그램과 `seaborn` 기초를 한 흐름으로 누적해서 배우는 주차입니다.
- 먼저 `sort`, `min/max`, `mean/median`, `std`, `percentile`로 데이터를 점검합니다.
- 분포는 `hist()` / `histplot()`, 추세는 `lineplot()`, 관계는 `scatterplot()`으로 봅니다.
- `seaborn`은 `matplotlib` 위에서 동작하며, `DataFrame` 기반 시각화에 더 편리합니다.
- `hue`, `style`, `size`, `kde`, `stat`는 시험에서도 자주 나올 핵심 옵션입니다.
