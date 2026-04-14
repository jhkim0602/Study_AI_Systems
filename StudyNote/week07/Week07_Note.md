# Week 07 Note: Matplotlib 히스토그램

## 1. 이 주제의 목적

7주차의 목표는 `matplotlib`에서 히스토그램(`hist`)을 제대로 이해하고, 숫자 데이터의 분포를 읽는 방법을 익히는 것입니다. 여기서 끝내지 않고, 히스토그램을 그리기 전에 숫자 데이터를 작은 값에서 큰 값으로 정렬하고, 범위와 중심, 퍼짐 정도를 먼저 점검하는 기초 함수들도 함께 누적해서 정리합니다.

히스토그램은 처음 보면 단순 막대그래프처럼 보일 수 있습니다.  
하지만 실제로는 아래를 확인하기 위한 도구입니다.

- 값이 어느 구간에 많이 몰려 있는가
- 데이터가 넓게 퍼져 있는가, 좁게 모여 있는가
- 한쪽으로 치우친 분포인가
- 이상치가 보이는가
- 두 집단의 분포가 어떻게 다른가

즉, 7주차는 "숫자 데이터를 한눈에 읽는 그래프"를 배우는 주차입니다.

## 2. 왜 중요한가

데이터 분석에서 평균 하나만 보는 것은 부족합니다.

- 평균이 같아도 분포는 다를 수 있습니다
- 이상치가 있으면 평균만으로는 전체 모양을 놓칠 수 있습니다
- 모델 입력 데이터가 어떤 범위와 형태를 가지는지 먼저 확인해야 합니다

이때 히스토그램은 가장 빠르게 분포를 확인할 수 있는 기본 도구입니다.

쉽게 말하면
- `plot()`은 추세
- `scatter()`는 관계
- `hist()`는 분포

를 보는 도구입니다.

## 3. 선수 개념

이번 주차를 보기 전에 아래 정도는 이해하고 있으면 좋습니다.

- `NumPy` 배열과 난수 생성
- `np.random.normal()`, `np.random.uniform()`
- `matplotlib`의 `figure`, `subplot`, `subplots`, `plot`
- `Pandas`의 `Series`, 수치형 열

연결 포인트
- 배열과 난수 기초: [../week03/Week03_Note.md](../week03/Week03_Note.md)
- `matplotlib` 기본 문법 복습: [../week06/Week06_Note.md](../week06/Week06_Note.md)
- 실습 코드: [../../week07_Matplotlib_Histogram.ipynb](../../week07_Matplotlib_Histogram.ipynb)

## 4. 핵심 개념과 용어 해설

### 4-1. 히스토그램 전에 먼저 보는 기초 함수

히스토그램은 분포를 한눈에 보여 주는 도구입니다.  
하지만 그래프를 그리기 전에 숫자 데이터의 기본 상태를 먼저 보는 습관이 중요합니다.

> **참고 시각 자료: 정렬에서 히스토그램으로 가는 흐름**
> ![Distribution Workflow](./assets/week07_distribution_workflow.svg)

핵심 흐름
- 먼저 정렬해서 값의 흐름을 본다
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
- 값의 전체 흐름을 직접 볼 수 있습니다.
- 이상치가 뒤쪽이나 앞쪽에 몰려 있는지도 빠르게 확인할 수 있습니다.

시험이나 설명에서 말할 수 있어야 할 것
- `np.sort()`는 분포를 "그리기 전"에 데이터를 정렬해 보는 가장 기본적인 점검입니다.
- 다만 정렬만으로는 구간별 빈도를 알 수 없으므로 히스토그램이 추가로 필요합니다.

#### `np.min()`, `np.max()`

```python
data_min = np.min(data)
data_max = np.max(data)
```

의미
- 데이터의 최솟값과 최댓값을 구합니다.

왜 필요한가
- 히스토그램의 x축 범위를 해석할 때 기준이 됩니다.
- 데이터가 어느 구간에 존재하는지 바로 알 수 있습니다.

#### `np.mean()`, `np.median()`

```python
data_mean = np.mean(data)
data_median = np.median(data)
```

의미
- `mean`: 산술평균
- `median`: 중앙값

왜 필요한가
- 데이터의 중심이 어디쯤인지 파악할 수 있습니다.
- 이상치가 큰 경우 평균과 중앙값 차이도 의미 있는 신호가 됩니다.

기억할 것
- 평균은 이상치에 민감합니다.
- 중앙값은 상대적으로 이상치 영향을 덜 받습니다.

#### `np.std()`

```python
data_std = np.std(data)
```

의미
- 평균 주변으로 데이터가 얼마나 퍼져 있는지를 수치로 보여 줍니다.

왜 필요한가
- 히스토그램에서 봉우리가 넓게 퍼진 분포인지, 좁게 몰린 분포인지 숫자로도 확인할 수 있기 때문입니다.

#### `np.percentile()`

```python
q1, q2, q3 = np.percentile(data, [25, 50, 75])
```

의미
- 25%, 50%, 75% 지점을 구합니다.
- `q2`는 중앙값과 같습니다.

왜 필요한가
- 데이터의 중간 50%가 어느 범위에 있는지 확인할 수 있습니다.
- 이후 `boxplot`이나 분포 비교로 넘어갈 때도 연결됩니다.

#### `Series.sort_values()`

`Pandas` 열을 다룰 때는 아래처럼 볼 수 있습니다.

```python
df["score"].sort_values()
```

왜 필요한가
- 실제 분석에서는 `NumPy` 배열보다 `DataFrame` 열을 직접 다루는 경우가 많기 때문입니다.

정리하면
- `sort`는 값의 순서
- `min/max`는 범위
- `mean/median`은 중심
- `std/percentile`은 퍼짐
- `hist()`는 분포 모양

이 다섯 가지가 같이 가야 숫자 데이터를 제대로 읽을 수 있습니다.

### 4-2. 히스토그램이란?

히스토그램은 연속형 숫자 데이터를 여러 구간(`bin`)으로 나눈 뒤, 각 구간에 값이 몇 개 들어 있는지 막대 높이로 보여 주는 그래프입니다.

> **참고 시각 자료: 히스토그램의 기본 구조**
> ![Histogram Concept](./assets/week07_histogram_concept.svg)

예를 들어 시험 점수 데이터가 있을 때,
- `0~10`
- `10~20`
- `20~30`

처럼 구간을 나누고 각 구간에 학생 수가 몇 명인지 세는 방식입니다.

핵심
- x축: 값의 구간
- y축: 각 구간의 개수 또는 비율

### 4-3. `plt.hist()`

히스토그램의 가장 기본 함수는 `plt.hist()`입니다.

```python
plt.hist(data)
plt.show()
```

조금 더 명시적으로 쓰면 아래와 같습니다.

```python
plt.hist(data, bins=10, color='skyblue', edgecolor='black')
plt.show()
```

이 코드가 의미하는 것
- `data`: 원본 숫자 데이터
- `bins=10`: 구간을 10개로 나눔
- `color`: 막대 채우기 색
- `edgecolor`: 막대 경계선 색

왜 필요한가
- 숫자 데이터 분포를 가장 빠르게 확인할 수 있기 때문입니다.

### 4-4. `bins`

히스토그램에서 가장 중요한 개념은 `bins`입니다.

`bins`는 데이터를 몇 개 구간으로 나눌지 정합니다.

```python
plt.hist(data, bins=5)
plt.hist(data, bins=30)
```

같은 데이터라도 `bins` 값에 따라 그래프 모양이 많이 달라집니다.

> **참고 시각 자료: bin 개수 차이**
> ![Bins Effect](./assets/week07_bins_effect.svg)

기억할 것
- `bins`가 너무 적으면 너무 거칠게 보입니다
- `bins`가 너무 많으면 잡음이 심해 보일 수 있습니다

즉, 히스토그램은 "그리기"보다 "어떻게 자를 것인가"가 중요합니다.

### 4-5. count와 density

기본 히스토그램은 각 구간의 개수(`count`)를 보여 줍니다.

```python
plt.hist(data, bins=20)
```

반면 `density=True`를 주면 확률밀도 형태로 정규화합니다.

```python
plt.hist(data, bins=20, density=True)
```

> **참고 시각 자료: count vs density**
> ![Count vs Density](./assets/week07_count_vs_density.svg)

차이
- count: 실제 개수 중심
- density: 서로 다른 크기의 데이터셋 비교에 유리

왜 중요한가
- 샘플 수가 다른 두 집단을 비교할 때는 `density=True`가 더 공정할 수 있기 때문입니다.

### 4-6. `alpha`

두 분포를 한 그래프에 겹쳐 그릴 때는 `alpha`를 많이 사용합니다.

```python
plt.hist(data_a, bins=20, alpha=0.5, label='A')
plt.hist(data_b, bins=20, alpha=0.5, label='B')
plt.legend()
```

`alpha`는 투명도입니다.

왜 필요한가
- 그래프를 겹쳐 그릴 때 하나가 다른 하나를 완전히 가리지 않게 하기 위해서입니다.

### 4-7. `cumulative=True`

누적 히스토그램은 구간별 누적 개수를 보여 줍니다.

```python
plt.hist(data, bins=20, cumulative=True)
```

왜 필요한가
- "이 값 이하가 전체의 얼마나 되는가"를 빠르게 보고 싶을 때 유용하기 때문입니다.

### 4-8. `ax.hist()`

여러 축을 다룰 때는 `plt.hist()`보다 `ax.hist()`가 더 관리하기 쉽습니다.

```python
fig, ax = plt.subplots(figsize=(6, 4))
ax.hist(data, bins=20, color='steelblue', edgecolor='black')
ax.set_title('Histogram')
```

기억할 것
- `plt.hist()`는 빠른 단일 그래프
- `ax.hist()`는 객체 기반 제어

### 4-9. 히스토그램과 막대그래프 차이

초심자가 가장 자주 헷갈리는 부분입니다.

- `bar()`: 이미 집계된 범주 데이터
- `hist()`: 아직 집계되지 않은 연속형 수치 데이터를 구간별로 자동 집계

예를 들어
- `bar()`: 학과별 학생 수
- `hist()`: 학생들의 키 분포

라고 생각하면 쉽습니다.

## 5. 실습 파일과 핵심 흐름

관련 실습
- [../../week07_Matplotlib_Histogram.ipynb](../../week07_Matplotlib_Histogram.ipynb)

추천 실습 순서
1. `np.random.normal()`로 샘플 데이터 만들기
2. `np.sort()`, `min/max`, `mean/median`, `std`, `percentile`로 데이터 상태 보기
3. `plt.hist()`의 가장 기본형 실행하기
4. `bins`를 바꿔서 모양 차이 보기
5. `color`, `edgecolor`, `alpha` 넣기
6. 두 분포를 겹쳐서 비교하기
7. `density=True`와 `cumulative=True` 보기
8. `ax.hist()`로 축 기반 그래프 그리기
9. 실제 데이터 열에 적용하기

실습 중 계속 확인할 질문
- 지금 보고 싶은 것은 개수인가, 비율인가?
- `bins`를 바꾸면 해석이 어떻게 달라지는가?
- 두 집단을 비교할 때 count가 맞는가, density가 맞는가?
- 이 그래프는 bar가 더 적절한가, hist가 더 적절한가?

## 6. 자주 하는 실수

### 실수 1. 히스토그램을 막대그래프와 같은 것으로 생각함

올바른 방향
- 히스토그램은 연속형 수치 데이터를 구간별로 집계하는 그래프입니다.
- 막대그래프는 이미 집계된 범주형 값을 표현합니다.

### 실수 2. `bins`를 아무 생각 없이 기본값으로만 둠

올바른 방향
- `bins`에 따라 분포 해석이 크게 달라집니다.
- 적당한 구간 수를 비교해 보며 읽어야 합니다.

### 실수 3. 두 집단 비교인데 데이터 개수 차이를 무시함

올바른 방향
- 샘플 수가 크게 다르면 `density=True`도 같이 검토해야 합니다.

### 실수 4. 겹쳐 그리면서 `alpha`를 쓰지 않음

올바른 방향
- `alpha`를 조정해야 그래프가 서로 가리지 않습니다.

### 실수 5. 히스토그램만 보고 평균을 단정함

올바른 방향
- 히스토그램은 분포 모양을 보여 주는 도구입니다.
- 평균, 표준편차, 이상치와 함께 해석해야 더 정확합니다.

### 실수 6. `hist()`에 문자열이나 범주형 데이터를 그대로 넣음

올바른 방향
- 히스토그램은 기본적으로 연속형 수치 데이터에 적합합니다.
- 범주형이면 `value_counts()` 후 `bar()`가 더 적합할 수 있습니다.

## 7. 시험 대비 포인트

시험 직전에는 아래를 설명할 수 있어야 합니다.

- 히스토그램이 무엇인가
- 히스토그램을 그리기 전에 어떤 기초 함수로 데이터를 점검할 수 있는가
- `bins`가 왜 중요한가
- `count`와 `density` 차이
- `alpha`가 왜 필요한가
- `cumulative=True`는 어떤 상황에서 쓰는가
- `bar()`와 `hist()` 차이
- `plt.hist()`와 `ax.hist()` 차이

서술형 답안 구조 예시

> 히스토그램은 연속형 수치 데이터를 여러 구간으로 나누고 각 구간에 값이 몇 개 들어 있는지 막대로 나타내는 그래프이다. `matplotlib`에서는 `plt.hist()` 또는 `ax.hist()`로 그릴 수 있으며, `bins`는 구간 개수를 정하는 핵심 옵션이다. 기본 히스토그램은 개수 기준이지만 `density=True`를 사용하면 정규화된 분포 형태로 비교할 수 있다. 또한 두 분포를 한 그래프에 겹칠 때는 `alpha`를 사용하며, 누적 분포를 보고 싶을 때는 `cumulative=True`를 사용한다.

추가로 말할 수 있으면 좋은 내용
- 히스토그램을 그리기 전에는 `np.sort()`, `np.min()`, `np.max()`, `np.mean()`, `np.median()`, `np.std()`, `np.percentile()` 같은 함수로 데이터의 범위, 중심, 퍼짐을 먼저 점검할 수 있습니다.

## 8. 기존 문서와 연결 포인트

- 난수와 분포 기초: [../week03/Week03_Note.md](../week03/Week03_Note.md)
- `matplotlib` 기본 함수 복습: [../week06/Week06_Note.md](../week06/Week06_Note.md)
- 실습 코드: [../../week07_Matplotlib_Histogram.ipynb](../../week07_Matplotlib_Histogram.ipynb)

## 9. 빠른 요약

- 히스토그램은 숫자 데이터의 분포를 보는 그래프입니다.
- 히스토그램 전에 `sort`, `min/max`, `mean/median`, `std`, `percentile`로 먼저 상태를 점검하면 해석이 쉬워집니다.
- 핵심은 `bins`입니다.
- 두 분포를 비교할 때는 `alpha`, `density=True`가 중요합니다.
- 누적 분포를 볼 때는 `cumulative=True`를 씁니다.
- `hist()`는 분포, `bar()`는 범주 비교라는 점을 구분해야 합니다.
