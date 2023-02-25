---
description: >-
  데이터 분석에 있어 첫 단계인 데이터를 수집하는 기술입니다. 본 문서에서는 Web Crawling을 중심으로 데이터 수집 기법을 기술하며,
  추가적으로 RDBMS에서 데이터 추출에 필요한 SQL에 대해 기술합니다.
---

# Data Collection

## 데이터 수집

데이터를 분석하기로 마음을 먹었다면 가장 먼저 해야할 일은 당연하게도 분석할 데이터를 수집하는 일입니다. 기후 변화에 따른 곡물 수확량의 변화든 시장 변화에 따른 주식 가격의 추이 예측이든 모두 가장 먼저 필요로 하는 것은 분석의 재료가 되는 데이터를 수집하는 것입니다.

불행하게도 데이터를 수집하는 것은 그리 쉽지 않은 일입니다. Kaggle, Daycon과 같은 경진대회에 참여하거나 기업, 관공서에서 따로 데이터를 제공하지 않는 한 우리가 필요로 하는 데이터를 얻는 것은 현실적으로 어렵습니다. 그렇기에 데이터를 분석하고 토이 프로젝트를 진행하고자 할 때 직면할 가장 큰 문제는 데이터 수집이라고 볼 수 있습니다.

그러나 다행히도 직접적으로 제공되는 데이터가 존재하지 않더라도 우리는 원하는 데이터를 수집할 수 있습니다. 여러 가지 방법이 존재하지만 본 글에서는 가장 대표적인 세 가지만 소개하고자 합니다.

### 1. Open API

#### - 공공 데이터 포털 ([https://www.data.go.kr/](https://www.data.go.kr/))

대한민국 행정안전부에서 운영하는 공공데이터포털은 공기관을 포함한 여러 데이터를 손쉬운 방법으로 제공합니다. 게다가 데이터를 제공하는 데 그치지 않고 요청하는 데이터를 대신해서 수집해주기도 합니다. 더 나아가 데이터 분석 공부를 하는데 있어 도움이 되는 시각화, 데이터 활용사례를 제공해줍니다.

#### - 네이버 오픈 API ([https://developers.naver.com/products/intro/plan/plan.md](https://developers.naver.com/products/intro/plan/plan.md))

기업들 중에서는 대표적으로 네이버가 여러 API를 통해 자신의 사업 영역에서 수집한 데이터를 무료로 제공합니다. 다만 공공데이터포털과는 다르게 API를 사용하는 횟수에 제한이 있습니다. 하지만 데이터 분석에 활용하기엔 충분한 양을 제공하고 그 이용 또한 무척 간단합니다

#### - GitHub REST API ([https://docs.github.com/ko/rest?apiVersion=2022-11-28](https://docs.github.com/ko/rest?apiVersion=2022-11-28))

협업에서 빠질 수 없는 GitHub은 그 자체만으로도 정말 방대한 양의 데이터를 보유하고 있습니다. 더 나아가 GitHub은 정말 셀 수도 없이 많은 API를 제공하여 GitHub에 업로드된 많은 데이터셋을 쉽게 사용할 수 있도록 도와줍니다. 다만 GitHub의 API를 이용하기 위해서는 인증을 받아야합니다. 인증을 받는 것이 까다롭게 느껴진다면, 원하는 데이터를 검색하여 직접 데이터를 내려받는 것도 한 가지 방법입니다.

### 2. Requests & BeautifulSoup

방대한 양의 정보, 데이터는 이미 수많은 웹 사이트 페이지에 게재되어 있습니다. 그렇기에 저희는 굳이 힘들게 데이터를 수집할 필요 없이 특정 웹 사이트로부터 원하는 데이터만을 가져오면 됩니다. 이와 같은 과정을 웹 크롤링이라고 부르며 BeatifulSoup는 대표적인 웹 크롤러입니다.

Requests가 가져온 웹 사이트의 텍스트 데이터는 BeautifulSoup를 이용하여 HTML 태그만 따로 추출할 수 있습니다. 만약 저희가 날짜 데이터만 얻고 싶다면 날짜 데이터가 존재하는 태그만을 추출하여 원하는 데이터셋을 매우 손쉽게 얻을 수 있습니다.

웹 사이트로부터 데이터를 추출하는 과정은 다음과 같습니다. 클라이언트는 데이터를 보유한 서버에게 원하는 데이터를 요청(Request)합니다. 서버는 요청을 확인하고 제한 사항이 없다면 요청한 데이터를 전송하여 응답(Response)합니다. 위와 같이 클라이언트가 서버로 보내는 요청 방법을 HTTP 메서드(Method)라고 부릅니다

* GET : HTTP 메서드 중 하나. 서버에게 조회할 리소스를 요청합니다.

그렇다면 이제부터 BeatifulSoup을 이용하여 실제로 어떻게 데이터를 추출하는 지 알아봅시다.

데이터를 추출하는 사이트는 CNN의 한 뉴스 기사입니다.('[https://edition.cnn.com/2023/01/25/asia/east-asia-cold-snap-climate-japan-korea-china-climate-intl-hnk/index.html](https://edition.cnn.com/2023/01/25/asia/east-asia-cold-snap-climate-japan-korea-china-climate-intl-hnk/index.html)')

BeautifulSoup와 requests를 먼저 설치합니다.

```python
!pip install bs4
!pip install requests
```

GET 메서드를 이용하여 서버에게 리소스, 데이터를 요청합니다.

```python
import requests

url = '<https://edition.cnn.com/2023/01/25/asia/east-asia-cold-snap-climate-japan-korea-china-climate-intl-hnk/index.html>'
res = requests.get(url)

print(res) # 응답 객체
res.text # HTML 텍스트
```

실행 결과, Response \[200]이라는 값을 반환합니다. 아래의 HTTP 상태 코드에 따르면 요청이 성공적으로 이루어졌다는 의미입니다.

* 1xx (정보): 요청을 받았으며 프로세스를 계속합니다
* 2xx (성공): 요청을 성공적으로 받았으며 인식했고 수용하였습니다
* 3xx (리다이렉션): 요청 완료를 위해 추가 작업 조치가 필요합니다
* 4xx (클라이언트 오류): 요청의 문법이 잘못되었거나 요청을 처리할 수 없습니다
* 5xx (서버 오류): 서버가 명백히 유효한 요청에 대해 충족을 실패했습니다

BeautifulSoup를 이용하여 서버로부터 받아온 텍스트 데이터를 HTML 태그에 따라 나눕니다.

```python
from bs4 import BeautifulSoup

soup = BeautifulSoup(res.text, 'html.parser')
soup
```

이제 저희는 태그를 이용하여 원하는 데이터만을 추출할 수 있습니다.

다음 두 개의 함수를 이용하여 태그를 통해 원하는 데이터를 찾을 수 있습니다.

1. find(tag, attributes, recursive, text, keywords)
2. find\_all(tag, attributes, recursive, text, limit, keywords)

```python
tag = soup.find('h3') # h3 태그를 가진 값 한 개를 반환 / 여러 개일 경우 첫 번째 값 반환
tags = soup.find('p') # p 태그를 가진 모든 값을 list 형태로 반환
print(tag)
print(tags)
```

get\_text()를 이용하여 태그에 속한 텍스트만을 추출할 수 있습니다

```python
tag = soup.find('h3')
print(tag.get_text())
```

아래와 같이 여러 태그를 입력하여 더 많은 문장을 가져올 수도 있습니다

```python
soup.find_all({'p','h3'})
```

### 3. Selenium

아쉽게도 Requests와 BeautifulSoup만을 이용한 데이터 수집에는 분명한 한계가 존재합니다. 페이지의 구성 요소가 바뀌거나 심지어 사라지기도 하는 동적 페이지는 위 방법이 제대로 작동하지 않습니다.

동적 페이지의 데이터를 추출할 때 보통 사용하는 것이 Selenium입니다. Selenium은 웹 브라우저를 자동으로 제어할 수 있게 해주는 패키지로, 저희가 입력한 명령에 따라 컴퓨터가 알아서 동작하고 원하는 데이터를 수집해줍니다. 다만 그 코드가 복잡하고 속도가 느리다는 단점이 존재합니다.

이제부터 Selenium을 이용하여 Naver에서 연세대학교 이미지를 가져오도록 하겠습니다.

Selenium을 사용하기 위해서는 가장 먼저 자신의 크롬 버전을 확인해야합니다. 크롬에서 아래의 코드를 입력하여 버전을 확인합니다.

```python
chrome://version
```

아래의 사이트에서 자신의 버전에 맞는 웹드라이버를 다운로드합니다

([https://sites.google.com/chromium.org/driver/downloads?authuser=0](https://sites.google.com/chromium.org/driver/downloads?authuser=0))

다운받은 크롬 웹드라이버의 압축 파일을 풀고 드라이버 파일을 작업하고 있는 파일과 같은 경로에 둡니다. 현재 경로를 모를 시에는 아래 코드를 입력하여 확인합니다.

```python
import os

os.getcwd()
```

이제 Selenium을 설치하고 Webdriver를 실행합니다.

```python
!pip install selenium
```

```python

import selenium
from selenium import webdriver

driver = webdriver.Chrome()
```

만약 webdriver 실행 시 버전 오류가 지속적으로 발생한다면 아래 코드를 통해 해결할 수 있습니다.

```python
!pip install webdriver_manager
```

```python
import selenium
from selenium import webdriver
from webdriver_manager.chrome import ChromeDriverManager
driver = webdriver.Chrome(ChromeDriverManager().install())
```

다음과 같은 옵션을 추가하여 webdriver를 실행할 수도 있습니다.

```python
# webdriver에 옵션 적용 가능
options = webdriver.ChromeOptions()

# 브라우저 창이 안뜨고 실행
options.add_argument('headless')
# 브라우저 사이즈 설정
options.add_argument('window-size=1920x1080')

# 옵션 적용
driver = webdriver.Chrome(options=options)
```

webdriver를 네이버로 이동시키고 연세대학교를 검색창에 입력합니다.

* driver.get() : 입력한 URL로 이동
* implicitly\_wait() : 찾으려는 요소가 로드될 때까지 지정한 시간만큼 대기할 수 있도록 설정
* find\_element(By.XPATH, ‘’) : 해당 요소의 XPATH를 이용하여 인식
* send\_keys() : 검색어 입력
* send\_keys(Keys.RETURN) : Enter 입력하여 페이지 이동

```python
from selenium.webdriver.common.by import By
from selenium.webdriver.common.keys import Keys

# 해당 URL로 이동
driver.get('<https://www.naver.com/>') 

# 웹 페이지 요소 로드를 기다림
driver.implicitly_wait(5)

# XPATH를 이용하여 검색창에 해당하는 요소 찾기
search = driver.find_element(By.XPATH, '//*[@id="query"]')

# 검색어 입력
search.send_keys('연세대학교')

# Keys.RETURN = Enter 입력
search.send_keys(Keys.RETURN)
```

연세대학교 썸네일 이미지의 URL을 가져옵니다.

```python
img = driver.find_element(By.XPATH, '//*[@id="main_pack"]/div[3]/div[2]/div[1]/div/div[2]/a/img')
img_url = img.get_attribute('src')
print(img_url)
```

이렇게 webdriver를 통해 가져온 데이터들은 미리 만들어놓은 파일이나 객체에 추가하여 저장합니다.

마지막으로 사용한 webdriver를 종료합니다.

```python
driver.close()
```

본 글에서는 XPATH를 이용하여 요소를 선택하였지만 CSS selector와 JS path와 같은 다른 방법들도 모두 가능합니다. 또한 webdriver를 조작하는 함수들은 무척 많기에 직접 Selenium을 사용하여 데이터를 수집하면서 몸소 배워나가길 추천합니다.
