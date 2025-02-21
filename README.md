# 🌐 Web Crawling & NLP Analysis



## 🎯목표

**한국의 주요 종교(기독교, 불교, 천주교)에 대한 뉴스 데이터를 수집·분석하여 각 종교가 뉴스에서 어떻게 다뤄지는지를 비교**



## 📌연구 의의 및 기대 효과


**뉴스에서 종교를 다루는 방식은 특정 종교에 대한 대중의 인식과도 밀접하게 연관될 수 있다. 미디어가 종교를 어떠한 방식으로 다루는지 파악하고, 그에 따른 사회적 영향을 탐색할 수 있음**

---

## 주요 키워드 선정
각 종교의 주요 개념과 연관된 단어를 선정하여 분석을 진행한다.  총 19개의 키워드를 선정



---

# 프로젝트 과정

## 1)데이터 수집

→ API 방식으로는 충분한 데이터를 확보할 수 없기 때문에, Requests와 BeautifulSoup을 활용한 정적 크롤링 방식 활용

- 구현된 크롤러

1️⃣ 크롤링(뉴스 URL 수집) → 2️⃣ 스크래핑(뉴스 본문 추출)

두 단계로 이루어진 자동 네이버 뉴스 데이터 크롤러를 설계


# 2. 데이터 전처리

뉴스 본문 데이터를 `kiwipiepy` 라이브러리를 활용하여 전처리

1. **불용어 처리**
    - `kiwipiepy`에서 제공하는 기본 불용어 리스트를 초기 불용어 리스트로 사용
    - 모델링을 진행하면서 불용어를 추가하면서 진행
2. **텍스트 정제**
    - 개행 문자 제거 (`\\n`)
    - 띄어쓰기 및 오탈자 교정 (`kiwipiepy` 내장 기능 활용)
3. **품사 필터링** 
    - **명사(Noun)와 대명사(Pronoun)만 추출(**`Pos tagging`을 활용하여 필터링)

---

# 3. 데이터 분석

## 3 -1. 종교 관련 Word Cloud 분석

**WordCloud는 텍스트 데이터에서 단어들의 빈도를 기반으로 시각화 하는 기법**

**🔍분석 방법과 알고자 하는 것**

1. 개별 키워드 비교 분석 -> 각 단어가 뉴스에서 어떤 맥락에서 사용되는지 비교하여, 종교별 인식 차이를 확인

2. 각 종교 WordCloud 비교 분석 -> 각 종교가 뉴스에 비추어지는 방식의 차이점은 무엇인지

3. 전체 데이터에서 어떤 키워드의 빈도가 가장 높은지 -> 종교(기독교, 불교, 천주교)를 뉴스에서 어떻게 다루는지 


### 1. **Word Cloud 분석 예시**




---


### **종교별 Word Cloud 분석**




<div style="display: flex; justify-content: space-between;">
    <img src="https://github.com/user-attachments/assets/3247fde7-1f7f-402a-b8e5-a9e6843d9193" style="width: 48%;">
    <img src="https://github.com/user-attachments/assets/429b699e-1a59-44a3-a637-663511b323b9" style="width: 48%;">
</div>







**기독교 Word Cloud**




<div style="display: flex; justify-content: space-between;">
    <img src="https://github.com/user-attachments/assets/071a4325-80d5-4523-8131-dd48630fb047" style="width: 48%;">
    <img src="https://github.com/user-attachments/assets/4c207f68-6448-4dbd-b5c4-a5c63b5f58cb" style="width: 48%;">
</div>



**불교 Word Cloud**




<div style="display: flex; justify-content: space-between;">
    <img src="https://github.com/user-attachments/assets/fd879ce9-9f2f-4a1a-a553-1390059b0ecf" style="width: 48%;">
    <img src="https://github.com/user-attachments/assets/a2e0ef80-06aa-48f4-9fb9-14b9c95dcbdc" style="width: 48%;">
</div>

**천주교 Word Cloud**





📌 **종합**


➡️ 결론 및 시사점

** 차별화된 요소**

천주교 → '프란치스코 교황', '김대건 신부' 등 역사적·국제적 **지도자 중심의 보도**가 많음 

기독교(개신교) → **사회적·정치적 논란**과 연결되는 경우가 많고, 

 불교 → '부처 날', '절' 등 **전통적인 행사 및 시설**이 중심**



➡️종교적 가르침보다 사회적 맥락에서 종교가 언급되는 빈도가 높음

✍ 이 프로젝트는 종교가 뉴스에서 어떻게 다뤄지는지에 대한 분석을 바탕으로 진행되었습니다.

