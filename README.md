<h4 align="center">Exploring Tokenizing and Word Embedding Techniques for Sentiment Analysis</h4>
<h1 align="center">감성 분석을 위한 토크나이징 및 워드 임베딩 기법 탐색</h1>
<h4 align="center">20203065 소프트웨어학부 박규연</h4>
&nbsp;
&nbsp;


## Introduction
감성 분석을 수행하는데 있어서 효과적인 토크나이징 및 워드 임베딩 기법에 대해서 탐색하는 것을 목적으로 한다.

## Dataset

#### 네이버 영화 리뷰 데이터셋 (nsmc)
200,000 lines

## Tokenizer Performance comparison
### Accuracy
#### 띄어쓰기가 잘 되어있는 문장
|       |'내 나이와 같은 영화를 지금 본 나는 감동적이다..하지만 훗날 다시보면대사하나하나 그 감정을완벽하게 이해할것만 같다...'|
|-------|--------------|
|Kkma   |['내', '나이', '영화', '나', '감동적', '훗날', '대사', '대사하나하나', '하나하나', '감정', '완벽', '이해']|
|Komoran|['나이', '영화', '감동', '하지', '훗날', '다시', '보면', '대사', '하나하나', '감정', '을', '완벽', '이해', '것']|
|Okt    |['내', '나이', '영화', '지금', '나', '감동', '훗날', '다시', '사하나', '그', '감정', '이해']|
|Mecab  |['내', '나이', '영화', '나', '감동', '훗날', '보면대', '사', '하나', '하나', '감정', '완벽', '이해', '것']|
|KoNLTK |['나', '나이', '영화', '나', '감동적', '훗날', '다시보면대사하나', '감정을완벽', '이해할것']|

#### 띄어쓰기가 잘 안 되어있는 문장
|       |'이거어렸을때되게재밌게봄ㅋㅋ이정재 이범수ㅋㅋㅋㅋ연기쩜'|
|-------|--------------|
|Kkma   |['이거', '때', '봄', '이정재', '이', '이범수', '범수', '연기']|
|Komoran|[]|
|Okt    |['거', '때', '봄', '이정재', '이범수', '연기', '쩜']|
|Mecab  |['이거', '때', '이정재', '이범수', '연기']|
|KoNLTK |['이거어렸을때되게재밌게봄', '이정', '이범수', '연기쩜']|

> Komoran은 띄어쓰기가 잘 되어있지 않은 문장에 대해서 형태소 분석 성능이 떨어짐

### Speed

![image](https://user-images.githubusercontent.com/66217855/234276851-d1db1141-9771-4d89-b068-c5a883de69c1.png)

> Mecab이 속도 측면에서 가장 우수함

## Word Embedding comparison

|       |Word2Vec      |FastText     |
|-------|--------------|-------------|
|Okt    |‘연기자’, ‘영화배우’, ‘김혜수’, ‘양동근’, ‘아역배우’|‘배우진’, ‘단역배우’, ‘재연배우’, ‘영화배우’, ‘유명배우’|
|Mecab  |‘연기자’, ‘조연’, ‘차승원’, ‘신인’, ‘손예진’|‘파배우’, ‘남배우’, ‘명배우’, ‘영화배우’, ‘여배우’|

> Word2Vec는 키워드와 의미적으로 유사한 단어를 보여주지만, FastText는 키워드 텍스트 자체와 유사한 것을 보여줌

|       |Word2Vec      |FastText     |
|-------|--------------|-------------|
|Okt    |‘김영호’, ‘경력’, ‘김갑수’, ‘백성현’, ‘이성민’|‘상우’, ‘백성현’, ‘손현주’, ‘김영호’, ‘정려원’|
|Mecab  |‘차승원’, ‘디카프리오’, ‘엄정화’, ‘손예진’, ‘임창정’|‘김혜수’, ‘차승원’, ‘안성기’, ‘엄정화’, ‘신인’|

> 키워드를 여러개 줌으로써 Word2Vec와 FastText가 비슷한 결과를 보여줌

## Modeling and Performance comparison

### Logistic Regression

|     |Word2Vec(CBOW)|Word2Vec(Skip-gram)|FastText|
|-----|--------------|-------------------|--------|
|Okt  |69.61%        |72.00%             |69.45%  |
|Mecab|69.48%        |71.00%             |69.04%  |

> Okt로 토크나이징하고 Word2Vec(Skip-gram)을 이용해 워드임베딩을 했을 때 로지스틱 회귀 모델의 성능이 가장 좋음

### LSTM

|     |Word2Vec(CBOW)|Word2Vec(Skip-gram)|FastText|
|-----|--------------|-------------------|--------|
|Okt  |63.28%        |72.67%             |70.45%  |
|Mecab|69.58%        |71.08%             |70.00%  |

> Okt으로 토크나이징하고 Word2Vec(Skip-gram)을 이용해 워드임베딩을 했을 때 LSTM 모델의 성능이 가장 좋음

## Conclusion
Okt - Word2Vec(Skip-gram) - LSTM 조합이 감성 분석 모델에서 가장 좋은 성능을 내었다.  
하지만 Okt와 Mecab의 성능 차이가 감성 분석 모델의 성능에 크게 유의미한 영향을 미친다고 보기 어려웠으므로  
대용량 텍스트 분석에 있어서는 Mecab을 쓰는 것이 더 유리할 것이라고 판단된다.  
&nbsp;
