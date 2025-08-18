# 국민연금 텍스트 마이닝 및 감성 분석 프로젝트

## 1. 프로젝트 개요

본 프로젝트는 국민연금에 대한 대중의 인식을 파악하기 위해 네이버(Naver)의 뉴스, 블로그, 카페 데이터를 수집하고 분석합니다. 텍스트 마이닝 기술을 활용하여 관련 텍스트 데이터의 핵심 키워드를 추출하고, 감성 분석 및 토픽 모델링을 통해 시기별 여론 동향과 주요 이슈를 파악하는 것을 목표로 합니다.

## 2. 프로젝트 구조

```
nationalpension_textmining/
│
├───데이터 수집/
│   ├───pension_blog_crawling.ipynb       # 네이버 블로그 데이터 수집
│   ├───pension_cafe_crawling.ipynb       # 네이버 카페 데이터 수집
│   └───pension_news_crawling.ipynb       # 네이버 뉴스 데이터 수집
│
├───analysis_code/
│   ├───뉴스 데이터 시계열 그래프.ipynb        # 뉴스 데이터 감성 분석 시계열 시각화
│   ├───뉴스 데이터 전처리 및 분석.ipynb        # 뉴스 데이터 전처리, 토픽 모델링, 감성 분석
│   ├───블로그 데이터 시계열 그래프.ipynb      # 블로그 데이터 감성 분석 시계열 시각화
│   └───카페+블로그 전처리 및 분석.ipynb      # 블로그 및 카페 데이터 전처리, 토픽 모델링, 감성 분석
│
└───data/
    ├───Korean-NRC-EmoLex.txt             # 감성 분석용 사전 (EmoLex)
    ├───SentiWord_info.json               # 감성 분석용 사전 (SentiWord)
    ├───stopwords-ko.txt                  # 한국어 불용어 사전
    ├───raw_data/                         # 수집된 원본 데이터 (pickle)
    └───analysis_data/                    # 분석용으로 가공된 데이터
```

## 3. 실행 방법

### 3.1. 필요 라이브러리 설치

프로젝트 실행에 필요한 주요 라이브러리는 다음과 같습니다.

```bash
pip install pandas selenium beautifulsoup4 requests konlpy nltk matplotlib scikit-learn
```

### 3.2. 데이터 수집

1.  `데이터 수집/` 디렉토리의 `pension_news_crawling.ipynb`, `pension_blog_crawling.ipynb`, `pension_cafe_crawling.ipynb` 노트북을 순서대로 실행하여 데이터를 수집합니다.
2.  수집된 데이터는 `data/raw_data/` 디렉토리에 `pension_news.pkl`, `pension_blog.pkl`, `pension_cafe.pkl` 파일로 저장됩니다.

### 3.3. 데이터 분석

1.  `analysis_code/` 디렉토리의 전처리 및 분석 노트북을 실행합니다.
    *   `뉴스 데이터 전처리 및 분석.ipynb`
    *   `카페+블로그 전처리 및 분석.ipynb`
2.  위 노트북들을 실행하면 데이터 정제, 형태소 분석, 감성 분석, 토픽 모델링이 수행되며, 결과는 `data/analysis_data/`에 저장됩니다.
3.  마지막으로 시계열 그래프 노트북을 실행하여 시간에 따른 감성 변화 추이를 시각화합니다.
    *   `뉴스 데이터 시계열 그래프.ipynb`
    *   `블로그 데이터 시계열 그래프.ipynb`

## 4. 분석 프로세스

1.  **데이터 수집**: `Selenium`과 `BeautifulSoup`을 사용하여 네이버 뉴스, 블로그, 카페에서 '국민연금' 키워드로 검색된 데이터를 수집합니다.
2.  **데이터 전처리**:
    *   수집된 텍스트 데이터에서 불필요한 특수 문자, 공백 등을 제거합니다.
    *   `Konlpy`의 `Komoran` 형태소 분석기를 사용하여 텍스트를 명사 위주로 토큰화합니다.
    *   미리 정의된 불용어 사전을 이용하여 분석에 불필요한 단어를 제거합니다.
3.  **데이터 분석**:
    *   **키워드 빈도 분석**: 전처리된 데이터에서 단어 빈도를 분석하여 핵심 키워드를 추출하고, `data/analysis_data/` 내부에 `* top500.csv` 형태로 저장합니다.
    *   **감성 분석**: `SentiWord_info.json`, `Korean-NRC-EmoLex.txt` 등의 감성 사전을 활용하여 텍스트의 긍정/부정/중립 감성을 분석하고 점수화합니다.
    *   **토픽 모델링**: LDA(Latent Dirichlet Allocation)와 같은 토픽 모델링 기법을 적용하여 텍스트 데이터의 주요 주제를 식별합니다.
    *   **시계열 분석**: 월별 또는 분기별로 데이터의 감성 점수 변화를 추적하고, `matplotlib`을 사용하여 시계열 그래프로 시각화하여 여론의 동향을 파악합니다.
