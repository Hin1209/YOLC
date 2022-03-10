# YOLC_Backend

YOLC 프로젝트의 백엔드입니다. 모바일 어플리케이션과 현관 출입문과의 통신, 딥러닝 코드의 동작을 맡고 있습니다.  

딥러닝 파트는 빵형의 개발도상국 코드를 기반으로 작성했습니다. 
- face recognition: <br/>
[YOUTUBE](https://www.youtube.com/watch?v=3LNHxezPi1I&list=PL-xmlFOn6TULrmwkXjRCDAas0ixd_NtyK&index=61) <br/>
[CODE](https://github.com/kairess/simple_face_recognition) <br/>
- eye blink detection: <br/>
[YOUTUBE](https://www.youtube.com/watch?v=dJjzTo8_x3c&list=PL-xmlFOn6TULrmwkXjRCDAas0ixd_NtyK&index=56) <br/>
[CODE](https://github.com/kairess/eye_blink_detector)


## Libraries 
- dlib
- cv2
- tensorflow(keras)
- numpy
- matplotlib
- imutils 
- django
- djangorestframework
- mysqlclient
- Pillow
- django-allauth
- django-rest-auth
- django-model-utils

## Detail 
- 현관문에서 출입 요청이 들어오면 check_face.py 파일을 실행합니다. 
  - 이 때 현관문의 라즈베리파이에서 열어둔 서버와 http 통신을 통해 현관문 카메라의 이미지를 프레임 단위로 받아옵니다. 
  - 10번의 프레임동안 eye blink를 감지하고 저장합니다.
  - face detection 이후 eye crop을 진행한 뒤, 사전훈련된 model로 open, close를 classification합니다.
  - eye blink가 5회 감지되었다면 face recognition으로 넘어갑니다.
  - eye blink가 감지되지 않고 특정 프레임이 지나면 사람이 아닌 사진으로 인식합니다.
  - face recognition은 사전에 사진이 등록된 사람에 한해 recognition이 가능합니다. (모바일 어플리케이션을 통해 등록) 
  - 출입하려는 호수의 데이터와 일치하는 얼굴로 인식되면 현관문에 문을 열어주는 요청을 보내고, 다른 사람으로 인식되면 거주민의 핸드폰 어플을 통해 푸쉬 알림을 요청합니다. 
- 어플의 회원가입을 처리합니다. 
  - 거주민의 이름, 주소, 휴대폰 번호, 아이디, 비밀번호를 저장합니다.
  - 회원가입 과정에서는 문자를 통한 휴대폰 인증이 필요합니다. 
  - 로그인 요청이 들어오면, 아이디와 비밀번호가 일치하는지 확인하고, 확인 이후 요청으로 같이 온 FCM 토큰을 유저 정보에 업데이트 합니다. 
  - 로그인 이후 프로필 페이지에서 프로필 이미지 업로드 요청이 오면, 해당 유저에 해당하는 호수에 프로필 이미지를 등록합니다. 
