# 🌐 Web Crawling & NLP Analysis



## 🎯프로젝트 목표
본 프로젝트는 한국의 주요 종교(기독교, 불교, 천주교)별로 사용되는 텍스트의 차이를 분석하는 것

## 📌프로젝트 계획
각 종교별 주요 키워드를 설정하고, 해당 키워드가 포함된 뉴스 기사를 수집하여 텍스트 분석을 진행

---

## 키워드 선정
각 종교의 주요 개념과 연관된 단어를 선정하여 분석을 진행한다.

- **✝️기독교 관련 키워드 (6개)**
    - 기독교, 교회, 하나님, 예수, 개신교, 목사
- **☸️불교 관련 키워드 (6개)**
    - 불교, 승려, 스님, 절, 부처님_불교, 보살_불교
- **⛪천주교 관련 키워드 (7개)**
    - 천주교, 수녀님, 성당, 하느님, 교황, 천주교_신부, 추기경

총 19개의 키워드를 선정

_`(언더바)`가 포함된 키워드는 특정 키워드가 뉴스 기사에서 종교적 의미와 다른 의미로 사용되는 것을 구분하기 위해 추가되었다. 예를 들어, "부처님"이라는 단어는 정부 부처와 관련된 기사에서도 등장할 수 있어 종교적 의미를 분리하기 위해 "부처님_불교"로 지정하였다._

---

# 프로젝트 과정

## 1)데이터 수집

초기에는 네이버 검색 API를 활용하여 뉴스를 수집하는 방식을 고려하였습니다.

✅ 네이버 검색 API 방식

**장점**

네이버가 공식적으로 제공하는 API라 안정적이고 빠름

파이썬 예제 코드 제공으로 활용이 용이

웹페이지에 직접 요청하는 방식보다 서버 부담이 적음

교수님께서 권장한 방식

**한계**

API에서 제공하는 뉴스 개수가 최대 1000개로 제한됨

중복 기사 및 빈 뉴스 제거 후 실제 확보 데이터는 약 500개

최종적으로 확보 가능한 뉴스 개수는 500 × 키워드 20개 = 10,000개

📌 목표 데이터 수집량: 20,000개

→ API 방식으로는 충분한 데이터를 확보할 수 없기 때문에, Requests와 BeautifulSoup을 활용한 직접 크롤링 방식을 선택

1️⃣ URL 크롤링 (Crawling)

네이버 뉴스 검색 결과에서 기사 URL을 크롤링하는 과정입니다.

🔍 크롤링 과정

네이버 뉴스 검색 페이지 요청 (requests.get)

검색 결과에서 뉴스 기사 링크 추출 (BeautifulSoup.select())

네이버 뉴스인지 필터링

네이버 뉴스는 HTML 구조가 통일되어 있어 크롤링하기 적합함

중복 뉴스 및 빈 뉴스 제거

⚠️ 크롤링 한계 및 조정

네이버 뉴스 검색 결과에서 최대 200페이지(=2000개 기사)까지 크롤링 가능

하지만 필터링 과정에서 실제 확보된 기사는 1000개 미만 또는 초과할 수도 있음

→ 모든 키워드를 동일한 개수(1000개)로 크롤링하기 어려우므로, 최대한 균형 있게 데이터를 수집하도록 조정함.

2️⃣ 뉴스 본문 스크래핑 (Scraping)

수집한 뉴스 URL을 활용하여 기사 제목과 본문을 스크래핑하는 과정입니다.

🔍 스크래핑 과정

저장된 네이버 뉴스 URL 파일을 불러오기

각 뉴스 페이지 요청 (requests.get)

기사 제목과 본문 추출

CSV 파일로 저장하여 데이터 분석 준비 완료

📌 구현된 크롤러

1️⃣ 크롤링(뉴스 URL 수집) → 2️⃣ 스크래핑(뉴스 본문 추출)

두 단계로 이루어진 자동 네이버 뉴스 데이터 수집기를 개발하였습니다.

`Selenium`을 사용하지 않은 이유

네이버 뉴스 검색 결과 및 기사 페이지가 정적 HTML 기반
→ requests와 BeautifulSoup만으로 충분히 데이터 수집 가능

Selenium은 브라우저 실행 방식이므로 속도가 느리고 메모리 사용량이 많음

차단 위험이 있어 크롤링 효율이 떨어짐

✅ 따라서, 보다 효율적인 Requests + BeautifulSoup 방식을 사용하여 크롤러를 구현하였습니다.

# 2. 데이터 전처리

뉴스 본문 데이터를 `kiwipiepy` 라이브러리를 활용하여 전처리

1. **불용어 처리**
    - `kiwipiepy`에서 제공하는 기본 불용어 리스트를 초기 불용어 리스트로 사용
    - 모델링을 진행하면서 불용어를 추가하여 보완
2. **텍스트 정제**
    - 개행 문자 제거 (`\\n` 등)
    - 띄어쓰기 및 오탈자 교정 (`kiwipiepy` 내장 기능 활용)
3. **품사 필터링** 
    - **명사(Noun)와 대명사(Pronoun)만 추출(**`Pos tagging`을 활용하여 필터링)

---

# 3. 데이터 분석

## 3 -1. 종교 관련 Word Cloud 분석

**Introduction**
WordCloud는 텍스트 데이터에서 단어들의 빈도를 기반으로 시각화 하는 기법  

분석 목적 및 기대 인사이트

- 종교 관련 뉴스 키워드 패턴 분석 - > 종교가 뉴스에서 어떤 방식으로 다루어지고 있는지

- 각 종교가 뉴스에 비추어지는 방식의 차이점은 무엇인지

### 1. **개별 키워드 Word Cloud 분석**

**1.1 '목사' 키워드 Word Cloud**


<img src="https://github.com/user-attachments/assets/88c5b9de-ceed-407b-8792-5347b128cde5" style="width: 50%; height: auto;">


**주요 특징**

'최 목사', '김여사', '명품가방' 등 사회적·정치적 이슈와 관련된 키워드가 포함됨

'목사' 키워드는 단순한 종교적 맥락을 넘어 정치적 맥락에서도 빈번하게 등장

다른 종교 관련 키워드보다 논란과 연결되는 경향이 강함

📌 **결과 해석**

분석 결과, 기독교와 관련된 뉴스는 종교적 논의보다 정치적 이슈와 연결되는 경우가 많음

'목사' 키워드로 수집한 뉴스 개수가 약 1800개이며, 상당수가 정치적 논란과 관련됨

이는 뉴스에서 기독교를 다루는 방식이 다소 부정적인 경향을 보일 가능성을 시사

**1.2 '수녀님' 키워드 Word Cloud**


<img src="https://github.com/user-attachments/assets/77a65d47-1423-4c44-8898-095880f091bf" style="width: 50%; height: auto;">






**주요 특징**

'사랑', '위로', '마음' 등 종교적·도덕적 가치와 관련된 키워드가 두드러짐

예상했던 종교적 핵심 가치(공동체, 도덕성, 선한 영향력 등)가 나타난 유일한 사례

📌 **결과 해석**

분석 전에는 모든 종교 키워드에서 '사랑', '위로', '공감' 같은 단어가 많이 등장할 것이라 예상

하지만 실제 분석 결과, '수녀님' 키워드에서만 이러한 공동체적·도덕적 가치가 강조됨

이는 뉴스가 종교를 다루는 방식이 예상보다 건조하며, 일부 종교는 특정 이슈와의 연관성이 더 강조될 가능성이 있음을 시사

### **2. 종교별 Word Cloud 분석**

**2.1 기독교 Word Cloud**


<div style="display: flex; justify-content: space-between;">
    <img src="https://github.com/user-attachments/assets/3247fde7-1f7f-402a-b8e5-a9e6843d9193" style="width: 48%;">
    <img src="https://github.com/user-attachments/assets/429b699e-1a59-44a3-a637-663511b323b9" style="width: 48%;">
</div>





**주요 특징**

'교회', '한국 교회', '최 목사', '김 여사' 등의 키워드가 두드러짐

'한국 교회'라는 표현이 자주 등장하는 이유:

개신교에서 '교회'는 종교 시설뿐만 아니라 기독교 자체를 지칭하는 표현으로 사용됨

불교나 천주교가 국제적 통합성이 강한 반면, 개신교는 한국적 맥락에서 자주 언급됨

📌 **결과 해석**

'최 목사', '김 여사' 등의 키워드는 특정 사건과 연관된 것으로 보임

뉴스에서 특정 인물이나 사건이 조명될 경우, 해당 키워드가 집중적으로 나타남

**2.2 불교 Word Cloud**



<div style="display: flex; justify-content: space-between;">
    <img src="https://github.com/user-attachments/assets/071a4325-80d5-4523-8131-dd48630fb047" style="width: 48%;">
    <img src="https://github.com/user-attachments/assets/4c207f68-6448-4dbd-b5c4-a5c63b5f58cb" style="width: 48%;">
</div>



**주요 특징**

'부처 날', '절', '서울 종로구' 등의 키워드가 포함됨

'부처님 오신 날'이 전처리 과정에서 '부처 날'로 축약됨

📌 **결과 해석**

'서울 종로구'는 조계사가 위치한 지역으로, 불교 행사와 관련된 뉴스에서 자주 언급되었을 가능성이 큼

데이터 수집 시점이 2024년 5월 말이었기 때문에, '부처님 오신 날(5월 15일)' 직후의 보도량 증가가 반영된 것으로 보임

**2.3 천주교 Word Cloud**


<div style="display: flex; justify-content: space-between;">
    <img src="https://github.com/user-attachments/assets/fd879ce9-9f2f-4a1a-a553-1390059b0ecf" style="width: 48%;">
    <img src="https://github.com/user-attachments/assets/a2e0ef80-06aa-48f4-9fb9-14b9c95dcbdc" style="width: 48%;">
</div>


**주요 특징**

'프란치스코 교황', '김대건 신부', '수녀' 등의 키워드가 두드러짐

특정 단어가 압도적으로 크지 않고, 비교적 다양한 키워드가 균형 있게 분포됨

📌 **결과 해석**

'프란치스코 교황'이 가장 큰 키워드로 등장 → 교황이라는 직위가 천주교에서 차지하는 영향력이 크기 때문

'김대건 신부' 등장 → 한국 천주교 역사에서 중요한 인물이며, 천주교는 특정 인물을 역사적·종교적 영웅으로 기리는 경향이 있음

### **3. 전체 문서 Word Cloud 분석**


<div style="display: flex; justify-content: space-between;">
    <img src="https://github.com/user-attachments/assets/320c0d37-63ba-4073-b765-697b3d65cf52" style="width: 48%;">
    <img src="https://github.com/user-attachments/assets/ae361e93-b1c9-47e8-a415-99f1b928be41" style="width: 48%;">
</div>



**공통적으로 나타난 키워드**

'생각', '이유', '지금' → 현재 사회에서 종교가 논의되는 방식 반영

'기도', '시작', '무속' → 종교적 실천 및 전통 신앙과의 연결

'한국', '서울', '성당', '교회' → 특정 장소 및 종교 시설과 연결된 이슈

**차별화된 요소**

천주교 → '프란치스코 교황', '김대건 신부' 등 역사적·국제적 지도자가 중심

기독교(개신교) → '한국 교회', '목사' 등 국내적인 종교적 논의가 많음

불교 → '부처 날', '절' 등 전통적인 행사 및 개념이 중심

➡️ 결론 및 시사점

뉴스에서 특정 종교를 다루는 방식이 서로 다르게 나타날 수 있음

개신교는 **사회적·정치적 논란**과 연결되는 경우가 많고, 천주교는 **지도자 중심의 보도**가 많음

불교는 **행사 및 전통적인 개념**이 중심으로 다뤄지는 경향이 있음

종교적 가르침보다 사회적 맥락에서 종교가 언급되는 빈도가 높음

✍ 이 프로젝트는 종교가 뉴스에서 어떻게 다뤄지는지에 대한 분석을 바탕으로 진행되었습니다.

