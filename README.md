# deepspeechpratice

## 목표
- 오픈소스 stt (speech to text) 모델 deepspeech의 성능을 테스트 후 정확성을 분석해본다.
- 분석 결과를 통해 deepspeech 모델을 학습 시켜본다.

## deepspeech test 1
- 인터넷에 제공하는 원어민 음성파일을 다운받아 테스트 해봄.
- 결과 : 
- 결과 분석 : 몇몇 단어 제외하고 정확률이 높음.

## deepspeech test 2
- 초등학교 3학년~6학년 대표 문장 각 20개씩 총80개를 직접 녹음한 후 deepspeech 성능을 테스트를 진행함.
- 결과 : stt_test1.xlsx
- 결과 분석 : 
  1. deepspeech
    - 총 80문장 중 테스트 문장과 똑같이 나온 문장은 28개, 완전히 다른 문장은 11개, 단어 오차율이 있는 문장은 41개
    - 정확도 : 35%
  2. azure 
    - 총 80문장 중 테스트 문장과 똑같이 나온 문장은 67개, 완전히 다른 문장은 2개, 단어 오차율이 있는 문장은 11개
    - 정확도 : 84%

-> 목표 : 한국인 발음인 영어도 잘 인식시키기 위해 deepspeech를 학습시켜 azure만큼 정확율을 끌어올리기.

## deepspeech test 3
- test2에서 작업한 녹음 파일 80개중 대표 문장 20문장을 뽑아서 진행함.
- 20문장을 각각 5번씩 녹음을 추가 진행함. ( 발음을 다양하게 녹음함)
- 결과 : 
1. cpu 환경에서 모델 학습 (I 행)
  - 20문장 중 5문장을 학습 시킨 결과 학습 자체는 잘되지만 학습외의 다른 문장을 테스트 해봤더니 학습에 사용된 문장이 결과로 나옴
  -> 문제점 : deepspeech에서 제공하는 기존 모델에서 학습 시키지 않아 기존 모델보다 성능이 떨어짐
  
## deepspeech test 4  
### 1. train, dev, test (8:1:1)로 학습
  #### test2.jpynb의 test2 chapter
  - gpu 환경에서 train, dev, test의 dataset을 8:1:1의 비율로 나누어 학습 시켰다.
  - 정확율 : 89%
  - 띄어쓰기 보정 후 : 91%
  - 대체 단어 보정 후 : 91% 
  -> 문제점 : 학습 속도가 느리다 1step 당 4분 30초 * 20 epoches = 약 1시간, batch size를 고려하지 않음
  #### test2.jpynb의 test2의 train, dev, test 별로 정확성 
  - train, dev, test을 분류해서 정확율을 분석함
  - train : 90%, dev : 81%, test : 82%
  #### test2_othersentece.jpynb
  - 학습에 사용되지 않은 문장 test
  - 정확율 (띄어쓰기 보정, 대체단어 보정) : 63%
  #### test4_othervoice.jpynb
  - 학습에 사용되지 않은 목소리 test
  - 정확율 (띄어쓰기 보정, 대체단어 보정) : 78%
 ### 1-2. batch size 설정
  - test.jpynb의 test3 chapter
  - batch size : train 64, dev 32, test 32 설정 
  - 학습 속도 1step 당 50초
  - 정확율 : 36%

## 음성합성된 파일 훈련
  - test 결과 : csv정확률계산.jpynb

