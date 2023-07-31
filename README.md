# prj_mtn2

챗봇 API    <---->        챗봇 클라이언트               -->   (TCP 소켓)챗봇 엔진서버        <------>   DB서버
            TCP/IP                - chatbot_client_test.py              - 챗봇엔진
                                                                        -  bot.py
                                                                        -  local, 5050





# prj_mountain
### 자연어 처리에서 사전이 필요한 이유
### 의도모델 전처리, ner모델 전처리 정리

### 의도모델
* 의도설정 
  * 기존: [ 0: 인사, 1: 욕설, 2: 주문, 3: 예약, 4: 기타 ]
  * 변경: [ 0: 인사, 1: 욕설, 2: 조회, 3: 선택, 4: 기타 ]
    1. '조회' 데이터 생성
       1. corpus와 total_train_data 중 하나로 사용하기 --> total_train_data1.csv
       2. total_train_data1.csv 수정
          1. 등산로 데이터에서 등산로 '명' 가져오기
             1. 등산로 shp파일
              * GIS 프로그램에서 shp파일을 열기 위해 필수적인 파일은 shp, shx, dbf 이다.
              * shp : 도형파일(벡터형식), 지리사상의 기하학 정보를 저장
              * shx : 지리사상의 기하학 정보의 인덱스를 저장
              * dbf : 지리사상의 속성 정보를 제공하는 dBase(table)
              * sbn, sbx - 도형의 공간 인덱스(공간 로딩 속도를 빠르게 해주는 부가적인 파일)
              * prj : 좌표정보
              * shp.xml - 공간 메타데이터 xml 포맷
          2. total_train_data1.csv 읽어서 기존 2,3 라벨 데이터는 삭제
          3. 등산로 정보 추가
             1.  mtn_train_data.csv
             2.  mtn_user_dict.tsv
          4. 사전만들고, intent 모델 학습하기
       3. ner_train1.txt 수정안하고, NIKL_NE_2022자료 사용
          1. NIKL_NE_2022 자료
             1. N:신문 , S:일상대화, M: 온라인
             2. 읽기 테스트
### 개체인식 모델
* 데이터 추가
  * 기존 데이터 형태 따라하기
    * 문장 komoran으로 pos 태깅
    * 강릉부근 데이터만 pos 태깅해서 ner데이터 생산하기
    * 태깅데이터에 사용자 사전추가
  * 사전용, 훈련용, 사용자추가 사전 내용 일치(품사태깅 조심)
  * 문제되는 문장은 필터 [,/_ 숫자, 영어]
  * 사전에는 단어가 많으면 좋고, 훈련자료에는 빈도생각해서 for문 2개 중복?
  * LC 학습자료 개수에 따라 f1성능 변함, 대략 4000개 이상되야 90이상 나옴
  * 이 데이터는 I,O 뒤에 객체명이 B처럼 안붙음, I_LC 이런거 없음 B외에