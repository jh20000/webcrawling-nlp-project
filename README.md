# 🌐 Web Crawling & NLP Analysis

📎 [자세한 분석 내용 보기 (Notion 링크)]( https://vanilla-argon-6c3.notion.site/Webscraping-and-NLP-analysis-19283f92ac808096bf0eebfc7ceb2395?source=copy_link)

> 이 프로젝트는 뉴스 데이터 크롤링과 NLP 분석을 통해, 기독교·불교·천주교가 미디어에서 어떻게 다뤄지는지를 비교하고 종교별 담론의 차이를 분석한 프로젝트입니다.
## 목표

**한국의 주요 종교(기독교, 불교, 천주교)에 대한 뉴스 데이터를 수집·분석하여 각 종교가 뉴스에서 어떻게 다뤄지는지를 비교**



## 연구 의의 및 기대 효과


**뉴스에서 종교를 다루는 방식은 특정 종교에 대한 대중의 인식과도 밀접하게 연관될 수 있다. 미디어가 종교를 어떠한 방식으로 다루는지 파악하고, 그에 따른 사회적 영향을 탐색할 수 있음**

---

## 주요 키워드 선정
각 종교의 주요 개념과 연관된 단어를 선정하여 분석을 진행한다.  총 19개의 키워드를 선정



---

# **프로젝트 과정**

# 1. 데이터 수집

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

# 1. **Word Cloud 분석**


**WordCloud는 텍스트 데이터에서 단어들의 빈도를 기반으로 시각화 하는 기법**

**분석 방법과 알고자 하는 것**

1. 개별 키워드 비교 분석 -> 각 단어가 뉴스에서 어떤 맥락에서 사용되는지 비교하여, 종교별 인식 차이를 확인

2. 각 종교 WordCloud 비교 분석 -> 각 종교가 뉴스에 비추어지는 방식의 차이점은 무엇인지

3. 전체 데이터에서 어떤 키워드의 빈도가 가장 높은지 -> 종교(기독교, 불교, 천주교)를 뉴스에서 어떻게 다루는지 






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





 **종합**


➡️ 결론 및 시사점

**차별화된 요소**

천주교 → '프란치스코 교황', '김대건 신부' 등 역사적·국제적 **지도자 중심의 보도**가 많음 

기독교(개신교) → **사회적·정치적 논란**과 연결되는 경우가 많고, 

 불교 → '부처 날', '절' 등 **전통적인 행사 및 시설**이 중심**



➡️종교적 가르침보다 사회적 맥락에서 종교가 언급되는 빈도가 높음



# 2. Topic Modeling(LDA) 분석

### LDA 분석 개요
LDA는 **문서 내에서 함께 등장하는 단어 패턴을 분석하여 숨겨진 주제를 추출하는 확률 기반 알고리즘**입니다.  
단순히 **자주 등장하는 단어를 시각화하는 Word Cloud와는 차별화된 방식**을 사용합니다.   


LDA에서 단어 사전을 만들때 **너무 자주 등장하거나, 너무 적게 등장한 단어**들을 제거하여 보다 의미 있는 토픽도출

→ Word Cloud와 LDA를 활용하여 상호보완적인 분석가능.


###  **LDA 분석 과정**

1. Word_dict 생성 및 BOW 형식으로 변환
2. `"Coherence Score"`를 기준으로 Topic 수 결정
3. LDA 모델 학습
4. `'pyLDAvis'` 로 토픽 시각화
5. 토픽별 키워드 및 문서별 토픽비중분석

[pyLDAvis'` 로 토픽 시각화]

![image](https://github.com/user-attachments/assets/2e3191f5-6067-474d-9dce-76a1e566091b)

- 왼쪽 영역
    - 각 원은 하나의 토픽을 의미
    - 원의 크기 -> 해당 토픽이 전체문서에서 차지하는 비율    
    - 원 간 거리 -> 토픽간 유사도

    
- 오른쪽 영역
    - 해당 토픽에서 핵심단어 목록
    - 토픽이 어떤 주제를 갖는지 파악
    - λ 값 조절 -> 해당 토픽에서의 빈도(빨간색)과 전체토픽에서의 빈도(파란색)중 어떤 값을 기준으로 핵심단어 정렬

[토픽 별 핵심단어와 비율]

![image (1)](https://github.com/user-attachments/assets/7b4ca0b4-0a7e-4fb8-9842-998a6782d1c5)

- 각 토픽들의 핵심단어들과 비율을 보고 토픽의 주제 파악
- WordCloud보다 세밀한 분석이 가능(WordCloud와 비교했을때 각 종교의 다양한 면들을 확인할 수 있음)

[각 문서들의 토픽조합과 비율]

![image (2)](https://github.com/user-attachments/assets/a3daf2f8-8787-4ff8-870f-50f29e4d1b9c)

이런 식으로 문서들이 어떠한 경향성을 가지는지 토픽의 조합으로 파악할 수 있음


## 최종 결론 및 시사점

본 프로젝트에서는 **뉴스에서 종교가 다뤄지는 방식의 차이를 분석**하기 위해

Word Cloud와 LDA 기반 Topic Modeling을 활용하여 **각 종교별 주요 텍스트 패턴을 분석**하였습니다.

### **🔹 주요 발견점**

✅ **기독교 관련 뉴스**는 종교적 논의보다 **사회·정치적 맥락에서 등장**하는 경우가 많았음

✅ **불교 관련 뉴스**는 **전통 행사 및 특정 지역과의 연관성**이 높았으며, 종교적 맥락이 뚜렷함

✅ **천주교 관련 뉴스**는 **프란치스코 교황 등 특정 인물 중심의 보도** 경향이 강함

✅ **LDA 분석을 통해** 각 문서가 단일 토픽이 아닌 **여러 개의 주제들이 혼합된 형태로 구성**되어 있음을 확인

---

### **🔹 분석 방법별 시사점**

**Word Cloud**를 통해 **각 종교별 주요 키워드를 직관적으로 파악**할 수 있음

**LDA Topic Modeling**을 통해 **뉴스 기사에서 숨겨진 주제 구조를 분석**할 수 있음

**pyLDAvis 시각화**를 활용하여 **토픽 간 관계와 중요 단어를 효과적으로 분석**할 수 있음

---

### **🔹 향후 활용 방안**

본 연구 결과는 **뉴스가 특정 종교를 어떻게 다루는지에 대한 프레임을 분석하는 데 활용 가능**

향후 종교별 **미디어 담론 변화 추적**, **문서 분류**, **뉴스 추천 시스템** 등에 적용 가능

데이터 수집 범위를 확장하고, 사회·문화적 요소를 추가 분석하면 더욱 정교한 결과 도출 가능
