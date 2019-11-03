# [OOP 11조 9주차 주간보고서]

> 회의 날짜 : 19.11.02 17:00 ~ 19:00 #4번째 회의
>
> 참석자 : 황성민, 전상규, 장경호, 이태희

## 회의 내용 :

1. 환경 구축
   * 환경을 구성하기 위해서는 tensorflow와  conda 환경 구축

* ​	음성 데이터  제작

1. Data
   * 손석희 파일 직접 다 받음
   * youtube를 통해서 5분 앵커브리핑  다운로드 받은 뒤, 일일이 wav파일로 변환하였다.
   * jtbc 5분 브리핑에서 직접 손석희 앵커가 말했던 대사도 뽑아왔다.

2. 문제점
   * tensorflow contrib은 2.0 버전에 없다
   * 그래서  tensorflow 1.3.0번전을 받았다
     * 찾아바도 old version에는 tensorflow 1.3.0 이없다......그러다가 pip3로 설치하니 있었다......그동안 pip로 설치를 했었다.....아......
     * 하지만 변함없이 contrib 에러는 계속 떴다...
     * 다시 conda와 tensorflow를 지우고 다시 설치했다...gpu버전으로....그러더니 이번에 다음과 같ㅇ느 error가 떴다

        ```
        ImportError: cannot import name 'dense_features' from 'tensorflow.python.feature_column' (/home/kyu/anaconda3/envs/py374/lib/python3.7/site-packages/tensorflow/python/feature_column/__init__.py)
        ```
     
     * 이번에는 그냥 2.0을 포기하고 1.14.0을 받았다........이번에는 다른 error가 떴다......"tqdm"이 없단다....
     
     * 자 정석대로가자.....tensorflow를 찾아보니 1.3.0 버전이 있따
     
     * 하지만  bazel이란 것을 통해서 받아야 한다
       [참고 출처](https://docs.bazel.build/versions/master/install-ubuntu.html)
       [참고 출처](https://www.tensorflow.org/install/source)
     
     * ``pip install tensorflow==1.3.0``으로 설치한 설치가 되었다.
     
       > [시도한 싸이트](https://github.com/carpedm20/multi-speaker-tacotron-tensorflow/issues/61)
     
     * 그전에 data가 없기에 python3 -m datasets.generate_data ./datasets/son/alignment.json를 먼저 시도해야했다
     * 단, audio를 찾을 수 없다고 하기에, 이 문제를 해결해보자...path 문제였다
     * 또한  내가 직접 만든 json file의 경로에 오타가 있었다.

## 결과

* 엄청 많은 시간의 data양이 필요하다
* 그리고 mulit speaker까지 생각하면 2명의 Data가 필요하다
* 참고할 싸이트로 좋다....실제 결과 예제[engiecat](https://github.com/engiecat/multi-speaker-tacotron-tensorflow)
*  tensorflow는 무조건 1.3이어야만하다!!!
* gpu로는 아직 못돌려봤다..10시간 짜리 Data라......흠.........
* issue를 보면 single도 잘되고 multi도 잘되는 듯 하나,  한국어 issue에서는 multi를 선호하는 것 같다.......이건 그냥 하는말.........영어 issue에서는 성공적으로 single 을 돌린 사래가 있다...참고하자!!
* 참고 [실제Train된 set](https://github.com/carpedm20/multi-speaker-tacotron-tensorflow/issues/6).............가능성이 있다



