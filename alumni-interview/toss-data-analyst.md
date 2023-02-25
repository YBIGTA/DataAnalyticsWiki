---
description: YBIGTA DA 14기
---

# Toss Data Analyst

### 현업에서 맡은 업무

* 실험 설계 및 개선 방향을 도출하는 Data Analyst
* DA가 하는 일은 도메인마다 조금씩 다르다.
* 반기 목표가 MAU를 늘려서 일정 수준에 도달해야 하는 것으로 만들어졌을 때, 어떤 전략을 짜야 하는 가를 결정해야 한다.
*   제품이 완성되었을 때, 이 제품이 MAU 증대 목표에 효과적이었는지 결과를 분석하고, 어떤 feature나 Lever를 조절하여 개선해야 할 지 방향성을 확인하는 실험들을 많이 진행한다.

    > MAU를 늘리기 위해서 우리가 어떤 전략을 짜야 하는가를 분석하는 일을 하고 있습니다. 예를 들어, 신규 user pool이 부족하니까 신규 user를 늘리는 전략을 수립하기로 했다고 합시다. 이를 위해 어떤 제품을 개발해야 하는가부터 시작해서, 제품이 완성됐을 때 이 제품이 효과성을 평가하고 개선 방향을 도출하기 위해 결과 분석을 진행합니다. 이처럼 여러 가지 실험들을 설계하고 분석하여 제품이 나아갈 방향을 정하는 일을 담당하고 있습니다.

### Tools

*   SQL

    DB에서 데이터를 가져오고 집계하는 단계까지 작업한다.
*   Tableau, Excel

    SQL에서 가져온 데이터를 활용하여 Excel에서 Chart로 만들어 활용한다. Tableau의 경우에는 실시간 연동성을 활용해야 하는 경우 가끔 사용한다. Dashboard 구축은 Excel보다 Tableau를 활용한다.

    > Excel은 실시간으로 업데이트가 안 됩니다. Tableau는 실시간으로 업데이트가 되게 배치를 돌릴 수 있어요. 즉, 데이터베이스랑 연동해서 계속 실시간으로 업데이트가 될 수 있어요. 그게 모니터링 가능한 대시 보드의 목적이기도 하고요. 그래서 Tableau는 지표를 주기적으로 모니터링하고 트래킹 할 때 대시보드를 만들어서 이제 공유하죠. Excel은 주로 일회성 분석을 할 때 사용하게 됩니다.
* Toss에서는 자체 Tool을 개발하여 사용하는 편이다.

### 문제를 잘 정의하는 방법

*   구조적인 사고

    > 어떤 결과에 대해 정확히 분석하려면 합당한 요인을 찾는 것이 가장 중요한데, 중요하게 작용했을 법한 요인을 놓치는 실수를 해서는 안되잖아요. 그래서 문제를 계속 좁혀 나가면서 합당한 근거를 찾아나가는 능력이 굉장히 중요한 것 같습니다.
*   도메인 지식과 데이터 분석 경험

    > Toss가 신입은 잘 안 뽑고 경력직만 뽑거나 아니면 다른 일을 하다가 직무 전환하는 분들이 많아요. 그분들은 데이터 분석과 관련된 업무가 진행되는 프로세스나 경험이 있으니까 더 잘하시거든요. 그래서 경험은 필요한 것 같긴 하지만 그것도 사실 업무를 하면서 쌓이는 거죠. 도메인 지식은 그 산업에 뛰어들어서 익히면 된다고 생각해요.
    >
    > 분석할 데이터 받았으면 EDA 해보고 문제를 발견해서 가설을 세우는 거잖아요. 보통 도메인 지식이 있으면 좀 더 가설을 세우기 수월하죠. 하지만 데이터를 보고자 하면 결국에는 그 뒤에 있는 배경을 찾게 됩니다. 그리고 그에 대한 공부를 다시 하게 됩니다.

### 좋은 데이터 분석이란?

*   가능한 모든 변수를 살펴본 데이터 분석: 수치만 보고 성급하게 결론 내리지 않기

    > 모든 변수들을 고려하지 않은 거죠. 예를 들어서 갑자기 어떤 지표의 수치가 낮아졌다고 합시다. 그냥 옛날에 무언가 잘못을 해서 수치가 낮아졌다고 해석할 수도 있고, 사실은 더 들여다보면 특정 연령대에서 일어난 현상이 있어서 그 연령대에서 배포가 잘못됐다고 해석할 수도 있어요. 수치에 영향을 미칠 수 있는 변수가 더 있을 수 있는데 확인해보지 않고 그냥 수치가 낮아졌네, 이슈가 있었나 봐 이렇게 분석하면 오류가 있는 거죠. 그래서 조금 더 철저하게 구체적으로 다 확인해보고 어떤 잘못이 있었는지 확인하는 분석이 잘 된 분석이라고 생각합니다.