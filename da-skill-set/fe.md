# FE

### 1. Feature Engineering이란?

Feature Engineering은 데이터 분석보다는 머신러닝/딥러닝에서 더 많이 쓰이는 용어입니다. EDA를 통해 데이터를 탐색해보았다면 어떤 데이터가 분석에 유용하게 쓰일 수 있을지 대략적인 파악이 됐을 것입니다. 그렇다면 이제는 최종적으로 모델링에 어떤 변수를 사용할 지 결정해야 할 때입니다. Feature Engineering은 EDA를 통해 알아낸 데이터에 대한 이해를 바탕으로 Feature를 생성하거나 선택하는 작업을 포함하여, 선택한 Feature들을 모델링의 Input으로 사용하기 전까지 필요한 모든 과정(Imputation, Outlier Handling, Log transformtation, Encoding, Scaling 등)을 의미합니다.

### 2. Feature Engineering의 목적

Feature Engineering의 목적은 결국 모델링의 성능을 올리는 것입니다. 주어진 모든 데이터를 활용해 모델링을 하면 좋은 성능이 나올 것 같지만, 실제로는 그렇지 않습니다. 다중공선성 등의 이유로 인해 유용한 Feature들만 선택해서 분석하는 것이 훨씬 정확한 결과를 만들어냅니다. 또한 선택된 데이터를 그대로 모델링에 사용하는 것보다 데이터의 특성에 따라 Imputation, Scaling, Log transformation 등의 가공을 더해주는 것이 훨씬 좋은 모델링 성능을 낼 수 있습니다. 구체적인 이유에 대해서는 이후 Feature Engineering의 종류를 설명하며 같이 알아보도록 하겠습니다.

### 3. Feature Engineering의 종류

#### 1) Feature Creation

* Feature Creation이란 모델에 유용하다고 판단되는 독립변수를 추가하는 것을 의미합니다. Feature Creation은 꼭 외부 데이터를 가져오는 것 뿐만 아니라 기존에는 존재하지 않았지만 기존 컬럼들을 조합하여 만들 수 있는 변수를 추가하는 것도 포함됩니다. 예를 들어, 인구와 면적이 독립변수로 주어져 있을 때, 인구를 면적으로 나눈 인구밀도 변수를 추가하는 것도 Feature Creation에 해당합니다. 종속변수를 설명할 수 있는 변수를 추가하는 것은 당연하게도 좋은 성능에 도움을 줄 수 있습니다. 따라서 목표로 하는 종속변수를 설명할 수 있는 변수들 중 아직 고려되지 않은 것이 있는지 논리적으로 검토해보아야 합니다.

#### 2) Feature Extraction

* Feature Extraction이란 유용한 정보라고 예상되는 Feature만 색출하는 것을 의미합니다. 특히 독립변수들 간 상관관계가 높을 때에는 다중공선성을 제거하기 위해 변수를 줄이는 것이 유리합니다. 예를 들어 비만도를 예측하기 위해 키와 몸무게를 모두 변수로 사용하는 것보다, 키와 몸무게 중 비만도에 더 큰 영향을 줄 것으로 예상되는 몸무게만을 변수로 사용하는 것입니다. 이는 키와 몸무게 자체의 상관관계가 높기 때문에 예측에 악영향을 끼칠 수 있기 때문입니다.

#### 3) Imputation

1. Imputation의 정의
   * Imputation이란 결측치를 채우는 것을 의미합니다. 머신러닝/딥러닝을 할 때 가장 대표적인 전처리가 결측치 처리입니다. 결측치는 Human Error나 Data Flow Interruption 등에 의해 발생할 수 있습니다. 결측치는 결측치의 비율이 얼마나 높은지, 해당 변수가 연속형인지 범주형인지에 따라 처리하는 방법이 다릅니다.
2. 데이터에 따른 Imputation 방법
   * 결측값의 비율에 따른 결측치 처리 방법
     * 결측값이 10% 이하일 경우, 해당 데이터를 지우거나 대치합니다.
     * 결측값이 10% 이상일 경우, 변수를 제거하거나 대치합니다.
   * 변수 유형에 따른 결측치 처리 방법
     * 연속형 변수의 경우, 0으로 대치하거나 평균 대치법 등을 사용할 수 있습니다.
     * 범주형 변수의 경우, 가장 빈도가 높은 범주로 대치하거나 Knn 대치법을 사용할 수 있습니다.
   * Imputation의 종류
     * 제거
       * 데이터의 개수가 적은 경우, 단순 제거는 바람직하지 않습니다. 데이터가 많을 수록 모델이 다양한 데이터를 학습할 수 있기 때문입니다. 하지만 전체 데이터 개수에 비해 결측 데이터 개수가 매우 적고, 적절한 대치값이 없다면 제거하는 것이 바람직합니다. 결측치를 잘못 대치해 오류가 있는 데이터를 학습시키는 것 역시 모델 성능에 악영향을 미치기 때문입니다.
     * 평균 대치법
       * 평균 대치법은 연속형 변수의 결측값에 사용하는 대표적인 대치법으로, 사용 방법이 간단하다는 장점이 있습니다. 하지만 관측된 자료를 토대로 한 추정값을 사용하기 때문에 완벽한 대치 방법은 아니라고 할 수 있습니다.
     * Knn 대치
       * Knn 알고리즘을 사용하여 예측하고자 하는 데이터로부터 가장 가까운 k개 이웃을 찾은 뒤, 이들 이웃으로부터 예측하고자 하는 데이터의 분류를 정하는 방법입니다. 해당 방법은 다른 변수들이 비슷했을 때의 결측값의 가장 그럴 듯한 분류를 알아내는 것이기 때문에 가장 빈도가 높은 범주로 결측값을 대치하는 것보다 오류를 범할 가능성이 적다는 장점이 있습니다.

#### 4) Outlier Handling

* Outlier의 정의 및 처리 방법
  * 이상치란 변수의 분포에서 비정상적으로 벗어난 값을 의미합니다. 결측치와 마찬가지로 Human Error, Data Flow Interruption 등에 의해 발생할 수 있습니다. 또한 해당 변수가 본래 가지고 있는 변동성 때문에 이상치가 발생할 수도 있습니다. 해당 변수의 변동성 때문에 발생한 이상치라고 할지라도 다른 값들과의 차이가 크면 모델의 성능에 안좋은 영향을 미칩니다. 따라서 제거 또는 상한/하한 값으로 대체하는 방안이 필요합니다.
* Outlier 판정 방법
  *   Boxplot

      ![](<../.gitbook/assets/image (17).png>)

      출처:[https://www.kdnuggets.com/2019/11/understanding-boxplots.html](https://www.kdnuggets.com/2019/11/understanding-boxplots.html)

      * 가장 많이 알려진 방법은 Boxplot입니다. Boxplot은 Q3 + 1.5\*(Q3-Q1)이면 upper outlier로, Q1 - 1.5\*(Q3-Q1)이면 lower outlier로 규정합니다.
  *   3-Sigma

      ![](<../.gitbook/assets/image (3).png>)

      출처:[https://news.mit.edu/2012/explained-sigma-0209](https://news.mit.edu/2012/explained-sigma-0209)

      * 3-Sigma는 일변량 자료들 중 𝞵 + 3𝞼 를 벗어나는 것들을 이상값이라고 규정합니다. 정규 분포를 따르는 데이터의 경우, 99.7%의 확률로 𝞵 + 3𝞼 내에 존재한다는 사실에 그 기반을 두고 있습니다. 하지만 대부분의 데이터가 정규 분포를 따르지 않는다는 점을 명심해야 합니다. 이렇게 데이터의 Skewness가 심할 경우, 이상치를 처리할 때 평균 대신 중앙값으로 대치하여야 합니다.

#### Encoding

1. Encoding의 정의
   * Encoding이란 범주형 컬럼을 모델링에 사용하고자 할 때 사용하는 방법입니다. 모델링을 하고자 할 때, 범주형 변수는 그대로 사용할 수 없으며 반드시 숫자 형태의 값으로 바뀌어야 합니다. Encoding에는 다양한 방법이 있지만, 여기서는 Encoding 중 가장 널리 쓰이는 방법인 One-hot Encoding을 알아보도록 하겠습니다.
2.  One hot Encoding

    ![](<../.gitbook/assets/image (18).png>)

    * One-Hot Encoding이란 위 그림과 같이 해당 컬럼에 존재하는 모든 범주 각각을 새로운 변수(Dummy Variables)로 만들어 0과 1로 어떤 범주에 속하는 지를 표기하는 것을 의미합니다. One-hot encoding Integer Encoding의 발전된 형태로, 범주 간 관계가 전혀 없다는 것을 가정하고 있습니다. 하지만 범주의 개수가 매우 많은 경우, 범주의 개수만큼 컬럼이 추가로 생성되어 희소행렬(Sparse Matrix, 행렬의 값이 대부분 0인 행렬)의 문제가 발생할 수 있습니다.

*   참고자료

    [http://kocw-n.xcache.kinxcdn.com/data/keris/2021/leeyoonmo1020/2-1.pdf](http://kocw-n.xcache.kinxcdn.com/data/keris/2021/leeyoonmo1020/2-1.pdf)

    [https://towardsdatascience.com/building-a-one-hot-encoding-layer-with-tensorflow-f907d686bf39](https://towardsdatascience.com/building-a-one-hot-encoding-layer-with-tensorflow-f907d686bf39)

    [https://towardsdatascience.com/what-is-feature-engineering-importance-tools-and-techniques-for-machine-learning-2080b0269f10#](https://towardsdatascience.com/what-is-feature-engineering-importance-tools-and-techniques-for-machine-learning-2080b0269f10)
