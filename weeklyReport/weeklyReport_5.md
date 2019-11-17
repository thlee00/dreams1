# [OOP 11조 10주차 주간보고서]

> 회의 날짜 : 19.11.10 17:00 ~ 21:00 #5번째 회의 참석자 : 황성민, 전상규, 장경호, 이태희

### 서버 정보 변경

- 서버 주소 변경 :  141.223.181.102



## Voice Swap

> 아직 한국어 dataset이 없어서, LJSpeech(영어)를 활용하여 train을 시켰다.

### CuDNN 설치 요청

- GPU를 사용하여 train을 시키기 위해서는 CuDNN library를 설치해야한다.
- 이미 Cuda는 설치되어 있는 것을 확인햇다

### Train 시작

- GPU가 없는 상태이기에 일단 train은 CPU를 통해서 실시 하였다.
- 또한, 아직 data가 적기 때문에, single model 버전으로 train을 시켰다.
  - 역시 엄청 느리다. 대략 1000번의 epochs를 돌리고 난 후 결과는 다음과 같다. [![img](https://github.com/hasm65/dreams1/raw/master/src/2019-11-10_17-16-16.manual.png)](https://github.com/hasm65/dreams1/blob/master/src/2019-11-10_17-16-16.manual.png)
  - spectrum이 만들어지지 않을 것을 볼 수 있다. issue에서 찾아보니, 적어도 110,000 epochs 정도를 train을 돌려야, 알아들을 수 있는 수준의 음성이 나온다고 한다
  - 일단 CPU를 통해서 train을 시키고, 요청한 CuDNN이 설치가 되면 그때서 부터 GPU를 활용하여 train을 시키겠다.

### 실행 방법

- single model

  - 우선, 음성파일에 대응하게 text을 json 파일로 만든다.

  - train시, 음성과 Text의 위치를 알 수 있게 *.npz파일로 변환을 한다 `python3 -m datasets.generate_data ./datasets/`

  - 이제 train을 시작하자 `python3 train.py --data_path=datasets/ --load_path logs/`

  - 이제 하염없이 기다린다.....

  - train을 어느 정도 실 시한 후, 다음 2가지 code를 실행시켜 output을 본다. (영어 Data기준) *

     

    ```
    python3 app.py --load_path logs/<log folder name> --num_speakers=1 --is_korean=False
    ```

    - web에서 실시간으로 삽이하는 text에 대하여 train 목소리로 text를 읽는다. * `python3 synthesizer.py --load_path logs/--text="Winter is coming." --is_korean=False`
    - "winder is coming"이라는 문장을 wav파일로 변환한다.



## Face swap

### libstdc++6 compile 문제

- cv2 라이브러리를 사용함으로써 libstdc++를 3.4.22버전으로 업데이트 해줘야함

- sudo 명령어 사용이 필요해 조교님께 문의 후 업데이트
- 그러나 업데이트 확인되지 않음
- sudo 명령어 사용과 관련하여 많은 불편함이 발생함



### Jupyter notebook 설치 및 원격 접속 설정

- **목적** : 사용하는 오픈소스가 ipynb 파일로 jupyter에서 작동되는 것인데 임의로 파이썬 파일로 변화하여 실행하려다 보니 많은 에러가 발생한다고 생각되어 jupyter로 작동시키기 위해

- **방법**

  1. jupyter notebook 설치 `$pip install jupyter`

  2. config 파일 생성을 위한 비밀번호 설정

     ```python
     $ python
     >>> from notebook.auth import passwd
     >>> passwd()
     Enter password: # 원격 접속 최초 1회시 필요한 비밀번호
     Verify password: # 위에서 타이핑한 비밀번호를 한번 더 입력
     'sha1:12j30t94230g208ehdsflhsdgt3908' 
     # 이런 식으로 입력한 비밀번호를 암호화 하여 반환
     # ctr+c
     ```

  3. config 파일 생성

     `$ jupyter notebook --generate-config`

     /home/**~username~**/.jupyter 디렉토리에 jupyter_notebook_config.py 파일이 생성됨

  4. config 파일 수정

     위의 위치의 config 파일을 다음과 같이 수정(주석 제거 및 편집)

     ```python
     c = get_config() # 최상단에 추가
     ...
     c.NotebookApp.allow_origin = '*' # 외부접속 허용
     ...
     c.NotebookApp.ip = '111.111.11.111' # 원격 접속 서버 ip 입력
     ...
     c.NotebookApp.port = 7777 #'사용할 포트번호 네자리, 초기값 8888'
     ...
     c.NotebookApp.password = u'복사해둔 암호화된 비밀번호 sha1:12j30t94230g208ehdsflhsdgt3908'
     ...
     c.NotebookApp.open_browser = False  # 서버로 실행될때 서버PC에서 주피터 노트북 창이 새로 열릴 필요가 없다.
     
     출처: https://light-tree.tistory.com/111 [All about]
     ```
    
     

  5. jupyter notebook 실행

     jupyter로 열고 싶은 디렉토리로 접근 후 

     `$jupyter notebook` 입력 후 반환되는 URL로 웹상으로 접속(config 파일에서 설정해둔 ip:port 가 반환됨)

     jupyter 종료 방법 `ctrl + c`

- **결과**
  - jupyter 실행후 141.223.181.102:7777 로 접속가능

  

  

### 현재 (3.train_test 단계) 발생하는 Error

-  OSError: Unable to open file (truncated file: eof = 1089536, sblock->base_addr = 0, stored_eof = 94694792)
  - .keras/model 에 위치한 h5 파일들을 삭제하고 다시 다운로드 방법 진행할 예정



### 추후 진행 방식

- 기존 GAN 을 통한 방식과 기존에 찾은 open source https://github.com/deepfakes/faceswap 을 이용한 방법으로 분할 진행
- tensorflow-gpu 사용 준비 과정이 필요함



