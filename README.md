
## 감성사전 기반 맞춤형 제주도 호텔 추천 시스템(2023)

 **✔️ 개요**

- 제로베이스 스쿨 14기 과정에서 머신러닝을 주제로 진행한 프로젝트
- 목적 : 제주도 호텔 선택 시, 고객 시간 및 비용을 절감 시켜주는 호텔 추천 시스템
- 성과 :  고객이 원하는/원하지 않는 속성에 가중치를 부여하여 추천/비추천해주는 시스템 구축
- 활용 데이터 : 트립어드비이저 및 부킹닷컴에서 스크래핑한 한국어 리뷰 데이터
- 팀 구성 : 제로베이스 스쿨 머신러닝 프로젝트_2인조
- 역할 :  데이터 수집, 전처리, 데이터분석, 감성사전 구축, 추천 알고리즘 구현 방향 제시 등

```python
Tool : Pandas, Numpy, Scipy, Matplotlib, Gensim, Fastext, Collections, Sklearn ect.
Language : Python
```

**✔️ 프로젝트 내용**

- 문제 인식
    - 호텔 선택 시, 리뷰에 의존하게 되지만,  많은 리뷰를 일일히 읽어 자신에게 맞는 호텔을 찾기는 어렵
    - “누가 대신 읽어서 나에게 딱 맞는 호텔을 추천해준다면?”이라는 고민에서 시작
- 데이터 분석
    - 전처리 :  특수문자, 이모티콘, 자음 모음 제거
    - 형태소 분석(Mecab) : 불용어 제거, 특정 품사 추출, 호텔명 관련 형태소 및 제주/제주도 제외, 한 글자 단어 제외
- 감성사전 구축
    - 긍정/부정 감성사전 따로 구축하여 리뷰 평가 시에 활용
    - 긍부정 단어 분류 필요 : 로지스틱 회귀 계수 활용
    - 트립어드바이저 평점기준으로 긍정(4 ~ 5점), 부정(1 ~ 3점)리뷰 분류,  부킹닷컴 손가락 이모티콘기준으로 긍정(👍), 부정(👎)리뷰 분류
    - 로지스틱 회귀 분석을 통하여 각 단어들의 긍정성/부정성을 판별

![Slide14.JPG](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/2c6bd93e-fcdc-47d0-ae81-5f3fe6c9e79c/Slide14.jpg)

- 감성사전의 카테고리(호텔속성)를 도출하기 위하여 LDA분석을 하였으나 무의미하게 나타남

![Slide15.JPG](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5925d49f-8971-4306-9aae-d88f2f4241cf/Slide15.jpg)

- Fasttext로 임베딩한 후, K-Mean로 군집분석 시 토픽이 유의미하게 선정
    - 토픽을 감성사전 카테고리로 선정

![Slide19.JPG](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/bf66c043-dd71-4968-b443-89d972c0ba17/Slide19.jpg)

![Slide20.JPG](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8ece3d53-113c-4af8-9a19-4594b07770eb/Slide20.jpg)

![Slide21.JPG](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/68818240-1cb0-49a1-9986-0283af499f7d/Slide21.jpg)

- 평가모델 구축
    - 나이브 베이즈 클래스 함수에서  P(단어|긍정), P(단어|부정)확률값을 추출
    - 베이즈정리를 활용: P(긍정|단어)의 추정치 = P(단어|긍정), P(부정|단어)의 추정치 = P(단어|부정)
    - 긍정 리뷰 평가 시, 카테고리에 속하는 단어가 리뷰에 존재한다면 해당 카테고리 Dictionary에  P(단어|긍정)을 더해줌 (부정 리뷰 평가 시에도 동일)

![Slide24.JPG](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b79d5287-f2a6-4695-9c6d-36e31ad34ded/Slide24.jpg)

- 평가모델 적용 결과(제주 호텔 11개)
    
    ![Slide25.JPG](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/94e6724e-728b-470d-bdf2-39f6f57a6dbc/Slide25.jpg)
    

- 추천시스템
    - STEP1 : 긍정/부정 카테고리에서 1~3순위 속성을 선택
    - STEP2 : 순위별 가점을 부여 → 긍정 총합이 가장 높은 호텔 추천/부정 총합이 가장 높은 호텔비추천
    - STEP 3 : 속성별 수치 제공 + 추천/비추천 호텔 시각화 자료 제공(WordCloud)

