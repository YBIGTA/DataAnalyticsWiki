# EDA 실습

### EDA 1편 요약

* EDA의 목적
  * 문제, 패턴, 관계를 발견하는 것
    * 문제: 결측치, 이상치, 중복값
    * 패턴: 좁은 의미에서는 주기성, 증가/상승 추세, 넓은 의미에서는 데이터의 특성 전체
* EDA의 단계
  * 데이터 전체적으로 살펴보기
    * 컬럼별 결측치 분포 확인하기(개수+msno만 확인)
    * 연속형 - 컬럼별 기본 통계값 확인하기(Describe 혹은 시각화)
    * 범주형 - unique 개수 확인하기
    * 컬럼별 데이터형 확인하기
  * 컬럼별로 데이터 살펴보기 (컬럼 1개)
    * 결측치 유형 확인하기(무작위적인 누락인가? 혹은 패턴이 존재하는가?)
    * 이상치 유형 확인하기(존재 가능한 이상치인가? 혹은 삭제해야 하는 이상치인가?)
    * 중복값 유형 확인하기(단순 실수인가? 중복이 발생한 이유가 무엇인가?)
    * 컬럼별 패턴 찾기 (주로 Count를 사용)
  * 컬럼들 간의 관계 살펴보기 (컬럼 여러 개)
    * 연속형 - 상관계수를 통해 상관관계 확인
    * 범주형 - 시각화를 통해 상관관계 확인
    * 패턴 찾기

### 사용할 데이터셋 소개

[Trending YouTube Video Statistics](https://www.kaggle.com/datasets/datasnaek/youtube-new)

*   Trending YouTube Video Statistics

    YouTube (the world-famous video sharing website) maintains a list of the [top trending videos](https://www.youtube.com/feed/trending) on the platform. [According to Variety magazine](http://variety.com/2017/digital/news/youtube-2017-top-trending-videos-music-videos-1202631416/), “To determine the year’s top-trending videos, YouTube uses a combination of factors including measuring users interactions (number of views, shares, comments and likes). Note that they’re not the most-viewed videos overall for the calendar year”. Top performers on the YouTube trending list are music videos (such as the famously virile “Gangam Style”), celebrity and/or reality TV performances, and the random dude-with-a-camera viral videos that YouTube is well-known for.

    This dataset is a daily record of the top trending YouTube videos.

    Note that this dataset is a structurally improved version of [this dataset](https://www.kaggle.com/datasnaek/youtube).
* 해당 실습은 US의 Trending Video 데이터셋을 사용했습니다.

### EDA 실습

실습에 들어가기 전, EDA에는 정답이 존재하지 않는다는 점을 다시 한 번 강조드립니다. 같은 데이터셋으로 문제를 다르게 해석할 수도 있고, 다른 인사이트를 도출할 수도 있습니다. 또한 해당 실습보다 충분히 더 효율적인 EDA 방법이 생각날 수 있습니다. 따라서 해당 실습은 간단한 예시로 생각하고 더 좋은 방법이나 인사이트, 질문 등이 생각난다면 자유롭게 시도해보시길 바랍니다.

#### 1) 필요 라이브러리 다운로드

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
import missingno as msno
from datetime import date
```

#### 2) 데이터 전체적으로 살펴보기

```python
df = pd.read_csv('/경로/USvideos.csv', encoding = 'utf-8-sig')
df
```

* 인코딩은 보통 cp949, utf-8 중 하나를 사용하는데, 해당 데이터셋은 두 인코딩 방식 모두 지원되지 않아 utf-8-sig를 사용하였습니다.
* 데이터셋은 다음 16개의 컬럼으로 구성되어 있습니다.
  * 비디오 식별자
  * 인기 동영상에 올랐던 날짜
  * 비디오 제목
  * 채널명
  * 카테고리 ID
  * 비디오 업로드 시간
  * 비디오 태그
  * 조회수
  * 좋아요 수
  * 싫어요 수
  * 댓글 개수
  * 썸네일 링크
  * 댓글 허용 여부
  * 순위 매기기 허용 여부
  * 비디오 오류 혹은 삭제 여부
  * 비디오 설명

```python
# 결측치 개수 확인
df.isnull().sum()
```

* 아쉽게도 해당 데이터셋은 이미 결측치를 제거한 데이터셋으로 보입니다. 선택 사항인 Description을 제외한 모든 컬럼들은 결측치 없이 구성되어 있습니다.

```python
# msno를 사용해 결측치 위치 시각화
msno.matrix(df=df, figsize=(8, 8), color=(70/255, 172/255, 161/255))
```

* 결측치가 존재하지 않기 때문에 큰 의미가 없기는 하지만, 그래도 msno를 통해서 결측치를 시각화해보겠습니다. 전체 40949개의 데이터 중 Description에만 빈 흰 줄이 몇 개 보이는 것을 알 수 있습니다.
* msno를 결측치가 많은 데이터셋에서 사용하게 되면 결측치가 연속적으로 분포해 있는지, 불연속적으로 분포해 있는지 한눈에 파악하기 좋습니다. 결측치 분포를 살펴보면 결측치가 왜 발생했는지 유추할 때 좋은 힌트가 됩니다.

```python
# 연속형 범주인 컬럼들의 기본 통계값 확인
df.describe()
```

* Category\_id는 데이터 타입이 숫자로 되어 있어 통계값이 나왔지만, 기본적으로 범주형이니 무시해도 괜찮습니다.
*   혹시 scientific notation 때문에 통계값을 확인하기 어렵다면 다음 코드를 실행하여 숫자 그대로 보여지게 할 수 있습니다.

    ```python
    pd.options.display.float_format = '{:.2f}'.format
    ```
* 전체 연속형 컬럼들의 분포 차이를 한눈에 알고 싶은데 숫자로는 감이 잘 오지 않는다면 Boxplot, Violinplot 등으로 시각화를 하여 확인해보는 방법도 있습니다. 컬럼별로 값의 차이가 크거나 단위가 다르면 경우에는 그래프 형태를 알아보기 힘들어 유용하지 않을 수도 있지만, 어쨌든 한눈에 알아보기는 좋을 것입니다.

```python
cols = [df['views'], df['likes'], df['dislikes'], df['comment_count']]

fig, ax = plt.subplots()
plt.gcf().set_size_inches(4, 10) # 인치로 figure size 설정하는 코드. 반드시 subplots() 한 후에 정의해줘야 함!
ax.boxplot(cols)
plt.show()
```

* 해당 데이터셋의 경우, 아무래도 좋아요 수, 싫어요 수, 댓글 수는 조회수에 비해 매우 작기 때문에 좋아요 수, 싫어요 수, 댓글 수 사이의 분포 차이를 알기는 어려운 것 같습니다. 하지만 여전히 우리는 조회수가 좋아요 수, 싫어요 수, 댓글 수보다 훨씬 큰 값을 가진다는 인사이트 정도는 얻을 수 있습니다.
* 또한 box의 모양이 상당히 아래쪽으로 쏠려있고 upper outlier가 매우 많은 것을 볼 수 있습니다. Outlier라고 해서 꼭 이상치라고 볼 수는 없습니다. 데이터 간 편차가 매우 크고 조회수가 0과 가까운 수에 몰려 있으면 이러한 boxplot이 나올 수 있습니다.
* 해당 데이터의 경우도 describe에서 확인할 수 있듯이 최대 조회수가 225억뷰로, 유명한 가수의 뮤직비디오라면 충분히 달성 가능한 수치기 때문에 존재 가능한 이상치라고 볼 수 있습니다.

```python
# 범주형 - unique개수 구하기
df.nunique()
```

* title과 video\_id가 전체 데이터 개수에 비해 적은 것을 알 수 있습니다. 반면 views는 전체 데이터 수와 거의 비슷한 값을 가지고 있습니다. 이를 통해 집계일마다 조회수가 달랐으며 Trending video로 순위를 여러 번 올린 비디오가 상당수 존재함을 알 수 있습니다.

```python
# 데이터 타입 확인하기
df.dtypes
```

* 어차피 뒤에서 한 번쯤은 데이터타입을 확인하게 됩니다. 뒤에서 EDA를 하며 계속 이유 모를 에러가 발생하는데, 데이터 타입 때문이면 괜히 시간낭비를 할 수 있으므로 앞에서 한 번 확인해주는 습관을 들이는 것도 좋습니다.
* 대표적으로 EDA를 위해 필요한 데이터 타입 변환에는 Datetime이 String으로 저장되어 있는 경우, Int가 String으로 저장되어 있는 경우입니다. 이 데이터셋 역시 trending\_date와 publish\_time이 Object(String)으로 지정되어 있으므로, 후에 변환을 해주어야 할 것으로 예상됩니다.

***

이제 전체 컬럼에 대해 전체적으로 데이터를 살펴보았습니다. 이렇게 전체적으로 데이터를 살펴보고 나면 데이터가 가지고 있는 문제점이 무엇인지 대략적으로 파악할 수 있습니다. 이 데이터의 경우 결측치는 존재하지 않지만, 중복값이 존재합니다. 때문에 인기가 많아서 여러 번 인기 동영상에 이름을 올린 비디오들을 위주로, 어떤 영상이 얼마나 중복되었는지 확인해보아야 할 것 같습니다.

***

#### 3) 컬럼별로 데이터 살펴보기

1.  video\_id, title (중복값 위주로)

    ```python
    # title과 channel_title이 중복인 경우를 중복된 횟수 순으로 나열하기
    pd.set_option('display.max_colwidth', None)
    pd.set_option('display.min_rows', 50)
    df.groupby(['title', 'channel_title'], as_index = False).size().sort_values(by = ['size'], ascending = False)
    ```

    * 집계 기간동안 Lucas and Marcus의 ‘WE MADE OUR MOM CRY...HER DREAM CAME TRUE!’라는 동영상이 30번으로 가장 많이 인기 동영상에 이름을 올린 것을 알 수 있습니다.
    * 필자의 경우, 조회수가 높을수록 인기 동영상에 여러 번 이름을 올렸을 지 궁금해지는데요, 이러한 컬럼 간의 상관관계는 컬럼별 특징을 확인한 후 심층적으로 확인해보겠습니다.
    * 이 글을 보시는 분들은 실습을 따라가다 궁금해지는 것들을 순서에 상관없이 그때그때 EDA해보셔도 무방합니다. 반복해서 말씀드렸듯 EDA에는 정해진 순서도, 정답도 없기 때문입니다. 하지만 이 글에서는 짜임새 있게 전달드리기 위해 컬럼 간 상관관계는 한 번에 EDA해보도록 하겠습니다.
2.  Trending\_date

    ```python
    # trending_date의 데이터 타입을 날짜로 바꿔주기
    df['trending_date'] = pd.to_datetime(df['trending_date'], format = '%y.%d.%m')
    df['trending_date'] = df['trending_date'].dt.date
    ```

    * 앞서 모든 컬럼의 데이터 유형을 확인했을 때, 날짜로 지정되어 있어야 하는 trending\_date와 publish\_time이 string으로 지정되어 있음을 확인했습니다. EDA를 위해 우선 데이터 타입을 변환해주도록 하겠습니다.

    ```python
    # trending videos를 집계한 기간 확인하기
    print(min(df['trending_date']))
    print(max(df['trending_date']))
    print(df['trending_date'].nunique())
    ```

    * 2017년 11월 14일부터 2018년 6월 14일까지 총 205일동안 집계했음을 알 수 있습니다.

    ```python
    # 누락된 날이 있는지 확인
    d0 = min(df['trending_date'])
    d1 = max(df['trending_date'])
    delta = d1-d0
    print(delta.days + 1)
    ```

    * 전체 213일 중 205일동안 집계했음을 알 수 있습니다. 그렇다면 누락된 날이 언제인지 확인해보겠습니다.

    ```python
    # 누락된 날짜가 언제인지 확인
    uncounted = []
    dates = df['trending_date'].unique()
    dates = list(dates)

    for date in dates[:-1]:
      next = date + datetime.timedelta(days = 1)
      if next not in dates:
        uncounted.append(next)
        while True:
          next += datetime.timedelta(days = 1)
          if next in dates:
            break
          uncounted.append(next)
    ```

    * 2018.1.10 -2018.1.11, 2018-4.8 - 2018.4.13이 누락되어 있음을 알 수 있습니다.

    ```python
    # 하루에 몇 개의 영상을 집계했는지 확인
    num_of_videos = df.groupby(['trending_date'], as_index = False).size()
    num_of_videos
    ```

    * 대부분 하루에 200개의 영상을 집계한 것을 알 수 있습니다. 그렇다면 200개의 영상을 집계하지 않은 날도 있는지 확인해보겠습니다.

    ```python
    # 트래킹한 영상 개수가 200개가 아닌 날들 확인
    num_of_videos[num_of_videos['size'] != 200]
    ```

    * 196개에서 199개의 영상을 집계한 날도 있는 것으로 확인되었습니다. 총 33일동안은 200개가 조금 못되는 수의 영상을 집계하였습니다.
3.  Publish\_time

    ```python
    # Publish_time의 데이터 타입을 날짜로 바꿔주기
    df['publish_time'] = pd.to_datetime(df['publish_time'], format = '%Y-%m-%dT%H:%M:%S.%fZ')
    ```

    * Trending\_date와 마찬가지로 데이터 타입을 string에서 datetime으로 바꿔주겠습니다.

    ```python
    # 가장 오래된 비디오와 가장 최신 비디오 확인
    print(min(df['publish_time']))
    print(max(df['publish_time']))
    ```

    * 집계에 포함된 동영상들 중 가장 오래된 동영상과 가장 최신 동영상을 확인해보았습니다. 가장 오래된 영상은 2006년 영상, 가장 최근 영상은 2018년 6월 14일 영상입니다.
    * 그렇다면 연도별로 Trending videos에 오른 영상 개수를 시각화해보겠습니다.

    ```python
    # 연도별 동영상 개수
    x = df['publish_time'].dt.year
    sns.countplot(x = x, data = df)
    plt.show()
    # 대부분 2017년, 2018년
    ```

    * 2006년부터 영상이 존재하기는 하나, 대부분은 2017년, 2018년 영상임을 알 수 있습니다.
4.  Channel\_title

    ```python
    channel_df = df.groupby(['channel_title'], as_index = False).size().sort_values(by = 'size', ascending = False)
    channel_df.head(20)
    # ESPN은 NBA 채널, The Late Show with Stephen Colbert는 뉴스 채널, Jimmy Kimmel Live는 예능, Late Night with Seth Meyers는 뉴스
    ```

    * Trending Video에 이름을 가장 많이 올린 채널은 ESPN이라는 NBA 관련 채널이었습니다.
    * 이외에도 뉴스 채널인 The Late Show with Stephen Colbert, Late Night with Seth Meyers, 연예계 채널인 The Tonight Show Starring Jimmy Fallon, TheEllenShow도 상위권에 위치한 것을 확인할 수 있습니다.
    * 그렇다면 Trending Videos에 이름을 올린 횟수로 countplot을 그려보겠습니다.

    ```python
    # countplot
    sns.countplot(x = channel_df['size'], data = channel_df)
    plt.show()
    ```

    * 횟수가 적은 쪽에 데이터가 몰려 있는 카이제곱 분포를 띄는 것을 알 수 있습니다. 해당 데이터를 모델링을 위한 Feature로 사용할 경우에는 log 변환을 통해 정규분포에 가깝게 변환하는 것이 필요할 수도 있다는 점을 알고 계시면 도움이 될 것입니다.
5.  Category\_id

    ```python
    # category 별 개수
    x = df['category_id']
    plt.gcf().set_size_inches(20, 10)
    sns.countplot(x = x, data = df)
    plt.show
    ```

    * Music 카테고리와 Entertainment 카테고리가 Trending Video에 가장 자주 이름을 올린 것을 알 수 있습니다. 카테고리 ID를 가지고 다른 컬럼과 결합해 분포를 확인하는 작업은 이후 단계에서 살펴보도록 하겠습니다.

***

여기까지 각 컬럼별로 대략적인 특징을 확인할 수 있었습니다. 본격적으로 창의성이 발휘되는 EDA는 이 이후부터입니다. 컬럼을 조합하여 궁금한 가설을 직접 확인해보는 과정입니다. 중요한 인사이트를 도출할 수 있는 부분도 바로 이 과정입니다.

***

#### 4) 컬럼들 간의 관계 살펴보기

1.  연속형 변수 - 상관관계 확인

    ```python
    df_continuous = df[['views', 'likes', 'dislikes', 'comment_count']]
    sns.heatmap(df_continuous.corr(), annot = True, fmt = ".2f", linewidths = 1, cmap = 'crest')
    ```

    * views와 likes, likes와 comment\_count 사이의 상관관계가 매우 높음을 확인할 수 있습니다. 만약 views를 예측하는 task가 주어졌다면 likes를 Feature로 이용하기 좋을 것이라고 예상할 수 있습니다.
2.  범주형 변수 - 시각화를 통해 상관관계 확인

    * 범주형 변수는 여러 시각화를 통해 상관관계를 확인할 수 있습니다. 우선 우리의 데이터셋 중 가장 범주형이라고 할 만한 컬럼은 Category\_id기 때문에, Category\_id별 likes, views, dislikes, comment\_count 분포의 차이가 있는지 확인해보도록 하겠습니다.

    ```python
    # category_id별 likes
    plt.gcf().set_size_inches(20, 10)
    sns.boxplot(x = "category_id", y = 'likes', data = df)
    plt.show()
    ```

    * 음악이 1위, Entertainment가 2위, People & Blogs가 3위를 차지했습니다.

    ```python
    # category_id 별 views
    plt.gcf().set_size_inches(20, 10)
    sns.boxplot(x = "category_id", y = 'views', data = df)
    plt.show()
    ```

    * 상관관계가 높은 views와 likes 답게 1, 2, 3위가 모두 동등하게 나왔습니다.

    ```python
    # category_id 별 dislikes
    plt.gcf().set_size_inches(20, 10)
    sns.boxplot(x = "category_id", y = 'dislikes', data = df)
    plt.show()
    ```

    * 다음으로 dislikes를 확인해보았습니다. 이번에는 음악에 비해 Entertainment 분야의 Dislikes가 눈에 띄게 높음을 알 수 있습니다.
    * 또한 boxplot의 box가 거의 안보이는 것으로 보아 dislikes는 0에 가까운 값을 많이 가지고 있음을 알 수 있습니다.
3.  패턴 찾기

    * 패턴 찾기는 굳이 이름을 붙이긴 하였지만, 자유롭게 컬럼들을 조합하며 가설을 검증하는 단계입니다. 해당 실습에서는 앞서 언급했던 최대 조회수와 인기 동영상에 이름을 올린 횟수 사이의 관계를 예시로 알아보겠습니다.

    ```python
    # 최대 조회수가 높은 영상 순으로 몇 번 인기 동영상에 이름을 올렸는지 알아보기
    def func(x): 
      d = {}
      d['max_views'] = x['views'].max()
      d['size'] = x['trending_date'].nunique()
      return pd.Series(d, index = ['max_views', 'size'])

    df.groupby(['title', 'channel_title']).apply(func).sort_values(by = ['max_views'], ascending = False)
    ```

    * Childish Gambino의 ‘This is America’라는 곡의 최대 조회수가 가장 높지만 Trending videos에는 25번 이름을 올렸습니다. 그 다음으로 최대 조회수가 높은 ‘YouTube Rewind: The Shape of 2017’ 영상은 8번밖에 이름을 올리지 못하였습니다.
    * 하지만 대체로 최대 조회수가 높을수록 Trending Video에 이름을 자주 올릴 확률이 높은 것 같습니다. 이러한 컬럼 간 상관관계는 모델링을 위한 Feature Engineering에서 중요하게 사용될 수 있을 것 같습니다.
    * 두 번째로는 dislikes가 높은 Entertainment 카테고리의 영상이 무엇인지 알아보도록 하겠습니다.

    ```python
    # entertainment에서 dislikes가 높은 videos 확인
    dislikes_df = df[df['category_id'] == 24].drop_duplicates(subset = ['video_id']).sort_values(by = 'dislikes', ascending = False)
    dislikes_df.head(39)
    ```

    * logan paul의 사과영상이 가장 많은 dislikes를 기록하였고, 그 외 logan paul의 영상들도 높은 dislikes를 기록하였습니다. 마블 혹은 소니 픽쳐스와 같은 대형 영화사의 동영상들도 팬이 많은 만큼 안티팬도 많아 높은 dislikes를 기록했음을 알 수 있습니다.

***

여기까지가 실습에서 준비한 EDA입니다. 나머지 패턴은 실습을 따라하며 궁금했던 가설들을 검증해보는 방식으로 보충하여 진행해보시길 바랍니다. EDA는 끝이 없고 정답도 없습니다. 하지만 가설을 많이 검증해보면 검증해볼수록 실수를 범할 확률이 낮아지므로 가능한 깊게 파고들어보는 것을 추천합니다. 감사합니다.
