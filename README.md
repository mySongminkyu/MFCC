# MFCC

- MFCC(Mel Freqeuncy Cepstral Coefficients)란 음성 및 오디오 신호처리에서 대표적으로 사용하는 기술이다.
  (음성데이터 feature extraction을 해주는 알고리즘)

  ![image](https://github.com/mySongminkyu/MFCC/assets/132251519/2e1c5ba7-2f6a-44a0-81ff-de1f91f2a32b)


  사람이 음성을 인식할 때를 생각해보면 달팽이관에서 각기 다른 주파수를 감지하는데, 보통 높은 주파수보다 낮은 주파수를 잘 감지한다고 한다.
  이러한 사람의 달팽이관의 특성을 고려하여 음성데이터에서 특징을 추출한 값이 **Mel-Scale**이다. (물리적인 주파수와 실제 사람이 인식하는 주파수의 관계를 표현한 것)

  또한 사람은 똑같은 문장을 말해도 각자 다른 속도로 말하기 때문에 이러한 특징을 학습시키기에는 어려움이 있다. 이러한 특징때문에 음성데이터
  를 20ms~40ms로 분할한다. (여러 연구에 의하면 사람은 20ms~40ms 사이에서 음소가 바뀌지 않는다는 연구결과가 있음)
  이런 점들을 이용하여 음성 데이터를 분석하는 알고리즘이 **MFCC**

- MFCC Block Diagram

  ![image](https://github.com/mySongminkyu/MFCC/assets/132251519/fc29a46c-60d6-4f24-8045-9768188152e5)

  - Pre-Emphasis
    간단히 말하면 High-pass Filter이다. 사람이 발성 시 몸의 구조 때문에 실제로 낸 소리에서 고주파 성분은 줄어든 채로 나오게 된다.
    (이 때문에 본인 목소리 녹음된거 들으면 소름)
    그래서 줄어든 고주파 성분을 강조하기 위해서 고주파 성분은 강조하고 저주파 신호를 약화하는 작업

  - Sampling and Windowing
    Pre-emphasis된 신호를 일전에 언급했듯이 20ms~40ms 단위의 frame으로 분할한다. 이때 frame을 50%씩 겹치게 분할한다.(연속성을 위해서)
    왜 연속성이 필요하냐면 만일 frame이 서로 떨어져있는 채로 sampling을 한다면 frame들끼리의 접합 부분에서 순간 변화율이 무한대가 될 수 있기 때문이다.

    그리고 나서 각각의 frame들에 대해 window를 적용한다. 보통 Hamming Window를 사용하는데, 이는 frame1과 frame2가 연속되지 않는다면 아까 말했듯이 접합 부분에서 문제가 생기기 때문이다.
    이러한 일을 방지하기 위해서 frame의 시작점과 끝점을 연결해주기 위해 적용.

    ![image](https://github.com/mySongminkyu/MFCC/assets/132251519/36ebac0f-3bf5-4560-a82a-ff022371761e)

  - FFT(Fast Fourier Transform)
    sampling과 windowing된 frame들에 대해서 Fourier Transform을 통해서 주파수 성분을 얻어냄. 여기까지만 하더라도 충분히 학습 가능한 feature들을 얻을 수 있지만 전에 말했듯 몸의 구조를 고려한 Mel-Scale까지 적용한다면 더 나은 성능을 얻어 낼 수 있기에 다음 과정도 진행한다.

  - Mel Filter Bank
    각각의 frame에 대해 얻어낸 주파수들에 대해서 Mel값을 얻어내기 위해 Filter를 적용한다. 
    
    **Mel Filter**
    ![image](https://github.com/mySongminkyu/MFCC/assets/132251519/911867d2-9620-4175-8ee8-b13539c669e4)

    Mel-Scale에 따라서 낮은 주파수에서는 작은 삼각형 Filter를 적용하고, 고주파 대역으로 갈수록 넓은 삼각현 filter 적용

    ![image](https://github.com/mySongminkyu/MFCC/assets/132251519/a56e37c7-0ad4-4775-a2bb-14913b15a66e)

    위와 같이 삼각형 filter N개를 모두 적용한 것을 Mel-filter Bank라고 한다.
    FFT 후 신호를 위의 Bank에 통과시키면 Mel-Spectrogram이라는 feature를 추출할 수 있다.

    ![image](https://github.com/mySongminkyu/MFCC/assets/132251519/9da784c3-a46a-4573-b3d7-dec6a6c719df)

    - **여기까지 작업한 것이 Mel-Spectrogram**

    이후에 Discrete Cosine Transform 연산(feature에 대해 행렬을 압축해서 표현해주는 연산)을 수행해주면 Output으로 MFCC가 나오게 된다.
 
**- MFCC vs Mel-Spectrogram**
  둘의 중요한 차이점은 Correlate와 De-Correlate이다.
  Mel-Spectrogrtam의 경우 주파수끼리 Correlate하기 때문에 domain이 한정적인 문제에서 더 좋은 성능을 보이고, MFCC의 경우는 De-Correlate을 해주기 때문에 일반적인 상황에서 더 좋은 성능을 보여준다고 함.


    
