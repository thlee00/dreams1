# [OOP 11조 12주차 주간보고서]

> 회의 날짜 : 19.11.22,  17:00 ~ 22:30  #회의 참석자 : 황성민, 전상규, 장경호, 이태희
>
> gpu 설치 도중 conda 환경이 망가져 conda 재설치로 인한 기존 가상환경이 모두 삭제되어 새로 시작.



#  VoiceSwap

###  GPU  설치 시도

* 아직까지는   CPU로 학습을 실시하고 있다. 그래서 이번에는  conda 환경에서 gpu를 활용기 위한 환경을 설치하려 노력했다.

* 먼저 server의 gpu를 확인후, Titan XP가 4개가 존재했다. Nividia site로 접속 후, 알맞는 cuda 버전과 cuDNN 버전을 확인하였다.  Titan XP는 Computation Capability가 6.1이며, 이것을 지원하는 cuda version 10.0과 이에 맞는 cuDNN인 6.1을 conda 환경에 설치하였다.

* 설치 방법은 앞의 faceSwap 팀이 시도했던 방법으로 동일하게 적용 시켰다.

  `` conda install -c anaconda cudnn ``

* 그리고 tensorflow도 gpu version인 tensorflow 1.3.0으로 설치하였다.
* 하지만 결과는 수 GPU로 compile이 될 수 없다며 CPU로 학습을 시킨다는 답을 console에서 확인 하였으며, 이 문제를 고치는 과정에서 numpy error와 certifi error로 인해 conda 환경이 망가 졌다.
  
  * 문제의 원인은, 2명이 동시서 한 환경의  packet을 설치했다 삭제했다를 반복해서 충돌이 난 것으로 예상이 됨.
* 결국, conda를 밀고 재 설치 후 CPU로 학습을 시키는 방향으로 잠정 결정했다.



###  한국어 Data 모집

* 현재 영어 data를 지속적으로 학습 시키고 있으며,  그 결과는 학습의 시간이 지나면 지날 수 록,  loss가 줄어들면서 큰 차이는 아니지만, 구분가능한 성능 향상을 확인 할 수 가 있다. 
* 현재 2만번 가량의 학습을 시켰으며, 일단 학습된 음성이 말하려는 대사를 확인 후 들으면 알아 들을 수 있을 만큼 학습이 되었기에 한국어 data를 모으기로 하였다.
* 한국어 음성을 먼저 구한 뒤, clova의 음성 API service를 활용하여 text를 뽑아낼 계획이다.



## Face Swap

### GPU 환경 설치 방법

sudo 권한 없이, conda 환경별로 설치하기

~~~python
#Tensorflow-gpu 버전 설치
conda install -c anaconda tensorflow-gpu=1.x

#CUDA 설치
conda install -c anaconda cudatoolkit=9.x or 10.x ... 

#cuDNN설치
conda install -c anaconda cudnn=7.x.x or ... 

#출처: https://eehoeskrap.tistory.com/293
~~~

### Deepfakes

- cpu로 학습 시키기 위한 환경과 gpu로 학습 시키기 위한 환경을 분리하여 다시 진행

- cpu 가상 환경 이름 : cpu_py36

  - -python setup.py 명령어 입력 후 프롬프트 따라
    amd support n
    Enable docker n
    Enable cuda n
    continue y
    -> train 진행 되지 않던 현상 사라짐. cpu로 train 진행 가능.

- gpu 가상 환경 이름 : gpu_py36

  - CUDA와 cuDNN 설치
    conda install -c anaconda cudatoolkit==9.0
    conda install -c anaconda cudnn==7.1.3

    -> gpu를 통한 train 진행 가능.

    

1. To extract image from video

2. Train command

~~~
$python faceswap.py train -A ~/OOP/LTH/faceswap/faces/YUNA -B ~/OOP/LTH/faceswap/faces/BORAM -m ~/OOP/LTH/faceswap/gpu_model/
~~~

3. Convert command

~~~
$python faceswap.py convert -i ~/OOP/LTH/faceswap/faces/YUNA/ -o ~/OOP/LTH/faceswap/converted_gpu/ -m ~/OOP/LTH/faceswap/gpu_model/
~~~

4. Generating a video

   - 모델 학습이 완료되면 비디오 프레임 별로 converting(face swap) 진행 후 프레임을 연결하여 비디오를 만들어야 함

   1. face swap하고자 하는 비디오의 프레임 추출

   ~~~asdfasdf
   ffmpeg -i /OOP/LTH/faceswap/src/INPUT_VIDEO.mp4 /OOP/LTH/faceswap/output/video-frame-%d.png
   ~~~
   2. train한 model을 이용해 프레임 converting

   ~~~
   python faceswap.py convert -i ~/OOP/LTH/faceswap/output/ -o ~/OOP/LTH/faceswap/output/converted_frame/ -m ~/OOP/LTH/faceswap/gpu_model/
   ~~~
   3. 프레임 연결하여 비디오 만들기

   ~~~
   ffmpeg -i ~/OOP/LTH/faceswap/output/converted_frame/video-frame-%0d.png -pix_fmt yuv420p -r 25 output.mp4
   ~~~



- **1차 FaceSwap학습 결과물**

  - 학습 조건
    - 김연아 얼굴 사진 3270장 / 보람이 얼굴 사진 5845장 / 24,131회 Training(using GPU, 1hour)

  - 얼굴이 확대되어 나오는 경우에는 얼굴 화질이 많이 흐리고 부정확함
  - 결과물 영상 링크 : https://youtu.be/omPEt6VEB74



### Web-Crawling

현재 하나의 영상에서 서로 다른 사람의 얼굴을 인지하고 분리해내는 방법이 없기에, MOOC에 나오는 서로 다른 교수님 얼굴을 Swap을 하여 테스트를 진행하기 하기로 함. 추후에 많은 영상들을 학습시키기 위해 웹크롤링을 통한 영상 다운로드가 필요하다고 판단. 가장 많이 활용되는 파이썬 라이브러리인 Selenium을 이용,

**설치조건**

- Python 3.5 이상
- selenium, scrapy 라이브러리
- 브라우저 driver



1. Selenium 설치

   ```>>conda install -c anaconda selenium``` or ```>>>pip install selenium```

2. 브라우저 driver 설치

| Chrome:  | https://sites.google.com/a/chromium.org/chromedriver/downloads |
| -------- | ------------------------------------------------------------ |
| Edge:    | https://developer.microsoft.com/en-us/microsoft-edge/tools/webdriver/ |
| Firefox: | https://github.com/mozilla/geckodriver/releases              |
| Safari:  | https://webkit.org/blog/6900/webdriver-support-in-safari-10/ |

3. Selenium 코드 진행

   ~~~python
   from selenium import webdriver
   import urllib #동영상 링크주소로 영상 다운로드를 위한
   import time
   
   #브라우저 driver 위치
   path = "/Users/seongminhwang/Downloads/chromedriver"
   
   #조금만 기다리면 selenium으로 제어할 수 있는 브라우저 새창이 뜬다
   driver = webdriver.Chrome(path)
   
   #해당 URL에 접속한다.
   driver.get("https://commons.postech.ac.kr/index.php?module=xn_sso2013&act=procXn_sso2013LoginGateway&from=web_redirect&login_type=standalone&auto_login=false&sso_only=true&callback_url=https%3A%2F%2Ficlass.postech.ac.kr%2Flogin%2Fcallback")
   
   #해당 id를 찾아 변수에 할당
   ele_id = driver.find_element_by_id("login_user_id")
   ele_pwd = driver.find_element_by_id("login_user_password")
   
   #해당 변수에 문자열 입력
   ele_id.send_keys("smhwang0515")
   ele_pwd.send_keys("-------")
   
   #해당 class-name을 찾아 변수에 할당
   ele_login = driver.find_element_by_class_name("login_btn")
   
   #해당 변수 클릭 가능한지 확인
   print(ele_login.is_enabled())
   
   #해당 변수 클릭실행
   ele_login.click()
   
   #로딩을 위한 기다림.
   time.sleep(3)
   
   # 예비 POSTECHIAN 프로그램 박스의 xpath 저장
   link2 = '/html/body/main/div/div/div/div/div[2]/div[1]/div/div[2]/span[2]/div[7]/div'
   ele2 = driver.find_elements_by_xpath(link2)
   ele2[0].click()
   
   time.sleep(3)
   
   # (19학번용)컴퓨터공학 입문 (Pre-Postechian) - 강의 클릭(이미 수강중인 상태임)
   link3 = '//*[@id="xn-catalog"]/div/div/div[2]/div[1]/div/div[2]/span[2]/div[3]/div'
   ele3 = driver.find_elements_by_xpath(link3)
   ele3[0].click()
   
   # 학습하러가기 버튼 클릭
   link4 = '//*[@id="xnc-goto-course-button"]'
   ele4 = driver.find_elements_by_xpath(link4)
   ele4[0].click()
   
   
   ### 현재 열린 탭들 확인
   print(driver.window_handles)
   ### 새로 열린 탭으로 활성 탭 변경
   driver.switch_to.window(driver.window_handles[-1])
   
   
   # 강좌학습 버튼 클릭
   link5 = '//*[@id="section-tabs"]/li[3]/a'
   ele5 = driver.find_elements_by_xpath(link5)
   ele5[0].click()
   
   ### Ifram, name 출력하여 확인하기 - 강좌학습 오른쪽 부분이 ifame으로 삽입되어 있음.
   iframes = driver.find_elements_by_css_selector('iframe')
   for iframe in iframes:
       print(iframe.get_attribute('name'))
       
   # #frame 전환하기    
   # driver.switch_to.frame("ifame_name")
   # #default frame으로 돌아오기(전환후 default로 돌아와야한다.)
   # driver.switch_to.default_content()
   
   
   driver.switch_to.default_content()
   driver.switch_to.frame('tool_content')
   
   time.sleep(3)
   
   #Week1-1 클릭
   link6 = '//*[@id="xn-subsection-learn"]/div[2]/div[1]'
   ele6 = driver.find_elements_by_xpath(link6)
   ele6[0].click()
   
   time.sleep(3)
   
   #재생버튼 클릭하기
   ~~~



- 참고 
  -  https://sacko.tistory.com/12?category=643535 /
  -  https://youngq.tistory.com/30?category=764296

## 차주목표

### FaceSwap

- 더 많은 데이터로 학습 시키기
- 다른 라이브러리를 이용하여 convert 과정에서 얼굴의 경계나 색깔을 자연스럽게 처리 하기
