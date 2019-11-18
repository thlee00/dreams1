# [OOP 11조 11주차 주간보고서]

> 회의 날짜 : 19.11.15 17:00 ~ 19:30 #6번째 회의 참석자 : 황성민, 전상규, 장경호, 이태희

## Voice Swap





## Face swap(GAN)

### libstdc++6 compile 문제

### cuDNN 설치

- 빠른 train 속도를 위해 서버 GPU를 사용하기 위해서 설치가 필요하다. 기존에 CUDA는 깔려 있는 것으로 확인되고 cuDNN을 sudo 명령을 통해서 조교님께 부탁드리려고 했지만 설치가 되지 않았다.

**NVIDIA 드라이버 확인**

`nvidia-smi`

**CUDA 버전확인**

` $ cat /usr/local/cuda/version.txt`

**cuDNN 버전확인**

` cat /usr/local/cuda-9.1/include/cudnn.h | grep -E "CUDNN_MAJOR|CUDNN_MINOR|CUDNN_PATCHLEVEL"`

**cuDNN 설치 방법(sudo 권한 없이)**

`conda install -c anaconda cudnn`

위 명령어로 설치가 가능하지만, 패키지로 설치되어 기존에 설치되어 있던 tensorflow의 버전이 변경됨으로 추후에 추가 작업이 필요하다.

 

### [3.train_test 단계]에서 발생하는 Error - 해결

-  OSError: Unable to open file (truncated file: eof = 1089536, sblock->base_addr = 0, stored_eof = 94694792)
  - 같은 폴더에 위치했던 h5 파일들을 삭제하고 다시 다운로드 방법 진행하였지만 동일한 문제 발생
  - **모델을 중복 선언**해주었던 것이 문제.

### TRAIN 속도 문제

- 이후 train이 진행되었으나 CPU를 사용하여 굉장히 **느린 속도**와 개인 노트북으로 접속하다보니 **이동시 인터넷이 끊기는 문제**가 발생하여 추후 청암 컴퓨터로 서버에 접속하여 장시간 train을 시킬 예정
- GPU 사용하여 tensorflow를 빠른 속도로 이용하기 위해 cuDNN은 설치를 완료하였지만, **추가적인 설정이 필요**하다.





## FACESWAP(deepfakes)



## Voice Swap

### Train Progress

| ![](../src/2019-11-15_17-36-26.manual.png) | ![](../src/2019-11-17_02-09-02.manual.png) |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 4,000 train                                                  | 11,000 train                                                 |

* LJSpeech의 목소리를 학습했을 때,첫번째 사진은, 4,000번을, 두 번째는 11,000번을 학습 시킨 후 "winter is coming"이라는 문장을 넣을 때 만들어진 음성을 audio spectrum으로 나타낸 것이다.
* 실제  wav 파일이 만들어진 것을 들어 봤을때도, 첫 번째  학습 모델의 음성은 전혀 알아 들을 수 없었다.  사진에서도 확인이 되듯,  각 문자에 따른 decoder값이 거의 없는 것을 볼 수 있다.
* 반대로,  11,000 번 학습이 된 model은 희미하게 그 decoder 값을 알 수가 있다. 음성에서도 확연한 차이를 볼 수 있다. 실제로 음성을 들었을때,  부드러운 소리라고 할 순 없지만, "Winter is coming"이라는 문장을 말하는 것을 들을 수 있다. 또한, 실제 LJSPeech에서 제공하는 음색과 유사한 것도 인지 할 수 있다.

### 목표

*  첫번째는 앞에서 faceswap team에서 설치한 cudaDNN을 활용하여 GPU를 활용하여 학습 시키는 것이다. 현재까지는 GPU를 활용하지 않고 CPU를 활용하여 학습을 시켰다. 그 11,000 번을 학습 시키는데는 대략 11시간 정도가 걸렸다. 고로 GPU를 활용하는 학습은 분명히 필요하다
* 두번째는, 지금 학습시킨 모델은 single 모델이다. 즉 한 음성의 목소리를 학습하여 text기반으로 책을 읽거나 말을 할때 본인의 말투로 음성을 만드는 것이다. 하지만, 최종 목표는 A라는 아이가 B라는(자신이 되고 싶은 사람) 의  attention을 통하여 본인의 음색으로 이야기 하는 것이다. 고로 multi model을 생성하는 학습을 해야한다.
* 마지막으로는, 현재까니 한국어 data가 없어서 영어 data를 활용하였다. 한국어 data가 필요하기 때문에 음성을 통해 한글로 text file을 만들어주는 프로그램을 찾고, 무엇보다도 한국어로 만들어진 음성 data를 찾아야 한다.
