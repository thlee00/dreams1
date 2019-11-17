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



