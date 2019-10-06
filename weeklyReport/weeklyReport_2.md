# [OOP 11조 2주차 주간보고서]

### 회의 날짜 : 19.10.04 17:00 ~ 19:00
### 참석자 : 황성민, 박효준, 전상규, 장경호, 이태희

### 결석자 : 전제민(스타디움 선발전)

### 정규회의 규칙 수정:

* 매주 금요일 17시 ~ **19시**
* 지각비 : 1분 ~ 30분 : 2천원 / 31분 ~ 지각 : 1만원
* 정규 회의 끝나기 전 단체 사진 촬영하기
* **개인 사유로 인한 불참시 24시간 이전 연락하여 회의 일자 조정**

##  회의 내용 :

1. **개발 서버 연결**

   1. 서버 ip : 141.223.163.207 (port : 22)

      > Mac os : terminal 
      >
      > ```
      > ssh smhwang0515@141.223.163.207 -p 22
      > ```
      >
      > Windows : Xshell, Putty 이용

   2. 계정 정보 : smhwang0515 / 

   3. 셋팅 상황 : 

      1. anaconda 설치

         ```python
         wget https://repo.anaconda.com/archive/Anaconda3-2019.07-Linux-x86_64.sh
         ```

      2. 'py37' 이름으로 python3.7 가상환경 생성

         > 가상환경 사용 목적 : **python 버젼 관리**와 **패키지 충돌 방지**
         >
         > [참고자료](https://teddylee777.github.io/python/anaconda-가상환경설정-팁-강좌)

         ```python
          # 가상환경 생성
           conda create -n <가상환경명> python=3.7
           
           #설치 된 가상환경 리스트 확인
           conda info --envs
           
           #가상환경 활성화
           activate <가상환경명>
           
           #가상환경 비활성화
           deactivate <가상환경명>
         ```

      3. 각자  활용할 수 있는 디렉토리 각각 생성

         1. 위치 : /home/smhwang0515/OOP/ ~
         2. 디렉토리명 : 본인 이름 이니셜

2. **Github 사용법 공유**

   > '전상규' 학생의 주도로 진행

   1. 각자 깃헙 계정 생성
   2. '소스트리'를 이용한 손쉬운 깃헙 푸쉬 과정 공유
      1. 풀 / 커밋 / 푸쉬 진행
   3. 프로젝트 깃헙 생성
      1. https://github.com/hasm65/dreams1

3. **텍스트 에디터(SublimeText)와 개발 서버간의 FTP 연결**

   > 개발 서버상의 코드를 텍스트 에디터로 로컬에서 손쉽게 수정하기 위해

   1. 서브라임텍스트(SublimeText) 다운로드

   2. 초기설정

      * [참고자료](https://intro0517.tistory.com/162)

   3. FTP 연결

      * [참고자료](https://kkamikoon.tistory.com/152)

        ```json
        "type" : "sftp",
        ...
        "upload_on_save" : true, #파일 저장시 서버로 업로드
        "sync_down_on_open" : true, #파일 open시 서버에서 다운로드
        ...
        ```

4. **(기타) 마크다운 사용법**
   * 익숙치 않을시 **typora** 프로그램 이용
   * 마크다운 사용법
     * https://heropy.blog/2017/09/30/markdown/

## 차주 목표

1. 깃헙 사용법 숙지
   * 깃헙의 이해
     * https://milooy.wordpress.com/2017/06/21/working-together-with-github-tutorial/
   * 많이 쓰는 깃헙 명령어
     * https://tagilog.tistory.com/377 
2. CLI(Command Line Interface) 숙지
   * 윈도우 / 맥 기본 명령어
     * https://tutorial.djangogirls.org/ko/intro_to_command_line/
3. **팀별 진행하던 오픈소스 각자 개발 서버 디렉토리에서 활용**



## 요청 사항

1. 이전 영상합성 프로젝트 파일 요청하기



