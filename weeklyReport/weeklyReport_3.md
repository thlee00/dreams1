# [OOP 11조 6주차 주간보고서]

### 회의 날짜 : 19.10.11 17:00 ~ 19:00 #3번째 회의

### 참석자 : 황성민, 전상규, 장경호, 이태희, 전제민

### 이탈자 : 박효준

### 특이사항:

* 박효준 학생 drop으로 인한 프로젝트 인원 변경
* 단체 사진 촬영 안함 ^^ (제민님 다음주에는 꼭 !)
* 단체 회식 날짜 결정 - 8주차 정규회의(10월 25일) 끝나고 난 뒤
* 중간고사로 인한 7주차 정규회의(10월 18일) 취소

##  회의 내용 :

1. **영상팀(황성민, 전제민, 이태희)**

   1. faceswap-GAN 오픈소스 단계

      1. MTCNN_video_face_Detection_alignment.ipynb
      2. prep_binary_masks.ipynb
      3. FaceSwap_GAN_v2.2_train_test.ipynb
      4. FaceSwap_GAN_v2.2_video_conversion.ipynb
   
   2. 총 4단계 중 1단계 테스트 완료
   
   1. 터미널에서 실행하기 위해 ipynb파일을 python 파일로 변환
   
      ``````python
         jupyter nbconvert --to script [filename].ipynb 
      ``````
   
   2. 유튜브에서 각자 다른 1~2분가량의 김연아 영상을 다운받아 `MTCNN_video_face_Detection_alignment.py`을 통해 얼굴 이미지파일 추출
   
      3. **Trouble issue at `MTCNN_video_face_Detection_alignmentd`** 
   
      1. `get_ipython()`코드로 인한 python 으로 실행 불가.
   
         - `python` 이 아닌 `ipython`으로 실행
   
         2.  tesorflow의 sess모드 코드 이용
   
         - tensorflow 2.x이 아닌 1.xx 으로 downgrade,
   
            - tensorflow 2.x 에서는 sess가 아닌 eager로 이용하기때문에 예전 버전의 코드사용으로 문제 발생.
   
      3. `prep_binary_masks.ipynb` 단계 임시 skip
   
         - GPU사용을 위한 CUDA 설치에서 관리자권한(sudo)으로 설치 불가능
   
         - skip하더라도, 정확도는 떨어지지만  다음단계 진행에 차질 없음
   
      
   
2. **음성팀(전상규, 장경호)**

   1. 음성 > 텍스트 > 음성
      1. 음성에서 텍스트를 뽑아내는 방법에서 문제 발생
         - 구글에서 제공하는 api 과금문제 발생
      2. 임시방편으로 수동으로 음성의 텍스트 작성

## 차주 목표

1. 영상팀
   * `FaceSwap_GAN_v2.2_train_test.ipynb` 까지 각자 디렉토리에서 테스트하고 만나기
     * trouble issue 팀 카톡방에 history 공유하기
2. 음성팀
   - 학습시킨 음성의 텍스트를 동일하게 입력하였을 때 음성 출력

## 요청 사항

1. 이전 영상합성 프로젝트 파일 요청하기
2. sudo 관리자 권한 관련 조교 문의



