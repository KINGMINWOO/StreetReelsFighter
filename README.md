# **Street Reels Fighter👊👊**

## **🥇인텔 직강 최우수 프로젝트 선정**

<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/1ade4a2a-72c3-42eb-bf2e-7239daa02ea8" />


## **👥팀 구성 및 역할**
### **팀명:뚝's딱's**
| 팀원       | 역할                                                |
|------------|---------------------------------------------------|
| **박진수** | 프로젝트 총괄 및 관리, 점수 계산 로직 구현, GUI |
| **김민우** | <ins>__프로젝트 총괄 보조, AI 모델 설계 및 최적화, Jetson Nano 구현, GUI__</ins> |
| **김재용** | AI 모델 최적화, Hailo 8 구현, GUI |
| **민주영** | 캐릭터 변환 전처리 및 변환 로직 구현 |
| **이진이** | 캐릭터 변환 전처리 및 변환 로직 구현 |
| **황정태** | 하드웨어 제어 (팬틸트), GUI |


## **📌 프로젝트 개요**
**Street Reels Fighter(스릴파)는** 자세 추정 실시간 점수 추산 및 캐릭터 변환 프로그램입니다.

Pose estimation AI(YOLOv8-pose)를 활용하여 사용자의 실시간 포즈와 릴스 챌린지 영상의 포즈를 비교해 점수를 출력합니다. 

또한 사용자 영상의 포즈 데이터를 활용하여 원하는 캐릭터로 변환할 수 있습니다.

이 프로그램을 통해 사용자들로 하여금 단순 숏폼 챌린지 영상 업로드를 넘어 경쟁 플랫폼 경험 기회를 제공하여 글로벌 숏폼 시장 성장에 기여할 수 있을 것으로 기대됩니다.

## **🚩 주요 목표**
1. **실시간 포즈 추정 점수 계산 시스템 구현**
   - 실시간으로 사용자의 자세와 기준 영상의 자세를 비교하여 점수를 계산하는 로직 구현
   - 여라가지 정확도와 속도를 비교하여 최적의 모델 선택
2. **자연스러운 캐릭터 변환 로직 구현**
   - 사용자의 움직임을 캐릭터로 자연스럽게 변환
3. **인상깊은 사용자 경험 제공**
   - 버벅임 없이 자연스러운 플레이 환경 구축
   - 퀄리티 높은 UI 제작
5. **엣지 환경에서 구현**
   - Jetson nano, Hailo 8 등의 엣지 디바이스에서 원활한 서비스 제공

## **📅 프로젝트 일정**
### **<2024.09.01-2024.09.18>**
<img width="955" height="370" alt="image" src="https://github.com/user-attachments/assets/0b0a438e-e962-4751-b0dc-8367ac773b37" />


## **🪛 개발 환경**
1. 운영체제
   - PC: ubuntu 22.04
   - jetson nano: ubuntu 20.04, Jetpack 4.6.1
   - Raspberry PI + Hailo 8: 24.04
2. 언어
   - Python(모델 추론, GUI), C(팬틸트 캠)
3. 도구
   - PC: OpenCV + YOLOv8l-pose + RTX 4060
   - Jetson nano: OpenCV + YOLO11n-pose + DOCKER + Tensor RT
   - Raspberry PI + Hailo 8: YOLOv8m-pose + GStreamer
4. 하드웨어
   - 카메라: Logitech BRIO 4K
   - 서보모터: MG995

## **📐 System Architechure**
<img width="1647" height="925" alt="image" src="https://github.com/user-attachments/assets/4f8186dc-f79e-4042-a3a0-43ad578e5d54" />

### 1. S/W Architechure
#### **모델 추론 파이프라인**
  - YOLOv8l-pose를 사용하여 영상 속 사람의 포즈 데이터를 추정.
  - 원본 영상의 포즈 데이터와 사용자 실시간 영상의 포즈 데이터를 비교하여 점수 계산
  - 저장된 사용자의 포즈 데이터를 이용해 캐릭터 변환
#### **사용자 추적 알고리즘**
   - 실시간 영상의 사용자 중심 좌표 값의 변화(*A*, *S*, *D*)를 이용해 STM32과 Serial 통신
#### **QUI**
   - PyQt5를 이용해 사용자 인터페이스 구성
   - QML문서로 페이지별 레이아웃 구성
  

### 2. H/W Architechure
#### **팬틸트**
   - STM32로 서보모터(SG 955)를 제어하여 사용자를 추적
   - *A*: 카메라 좌측으로
   - *S*: 카메라 정지
   - *D*: 카메라 우측으로

## **♾️ Flow Chart**
<img width="800" height="600" alt="image" src="https://github.com/user-attachments/assets/0be56c70-1a36-4c04-87ed-320c457bc601" />

## **🚀 주요기능**

### 1. **1인 모드**
- 1인 모드는 YOLOv8l-pose모델을 사용해 원본 챌린지 영상과 1명의 사용자의 포즈 데이터의 유사도를 비교하여 점수를 계산하고, B, A, S의 랭크가 매겨집니다
- 점수 책정은 코사인 유사도(Cosine similarity)를 활용해 이루어집니다.
   - 코사인 유사도: 두 벡터 간의 각도에 대한 코사인 값을 계산하여 두 벡터의 유사한 정도를 측정하는 방법.

      <img width="357" height="141" alt="image" src="https://github.com/user-attachments/assets/a92d6b15-3e64-4e70-9928-f02dd07600b3" />

   - 원본 영상과 사용자의 신체 비율, 카메라 각도가 다를 수 있기 때문에 관절의 좌표 값이 아닌 포인트를 연결한 벡터의 유사도를 이용해 점수를 책정했습니다.

- 플레이 종료 후 저장된 사용자 영상의 포즈 데이터를 바탕으로 원하는 캐릭터 변환이 가능합니다.
  
### 2. **2인 모드**
- 2인 모드는 2명의 포즈 데이터를 각각 원본 영상의 포즈 데이터와 실시간으로 비교하여 두 사용자가 점수 대결을 할 수 있습니다.
- YOLOv8의 tracking 기능을 활용하여 각각의 사용자를 인식하고 추적
  
### 3. **캐릭터 변환**
- 1인 모드가 종료되고 사용자는 캐릭터 변환을 시도할 수 있습니다.
- 제공되는 4가지 캐릭터(나루토, 신형만, 렌고쿠, 루미) 중 사용자가 변환을 원하는 캐릭터를 선택 할 수 있습니다.
- 약 10~30초의 렌더링 과정 후 캐릭터 영상 재생이 가능합니다.
  
### 4. **엣지 환경**
- 엣지 디바이스(Jetson nano, Raspberry PI 5 + Hailo 8)환경에서도 시스템이 원활하게 작동되도록 했습니다.
- Jetson nano는 TensorRT를, Hailo 8은 GStreamer를 활용해 성능 최적화를 진행했습니다.

## **🎯 Trouble Shooting**
프로젝트를 진행하는 과정에서 수많은 어려움과 시행 착오를 겪었습니다. 

하지만 팀원들과 협력하여 하나하나 해결해 가는 과정에서 훨씬 많은 것을 배울 수 있었고 한 단계 성장할 수 있었습니다!!

### 1. 모델 선정의 어려움
- 수 많은 자세 추정 AI 모델들 중 우리의 프로젝트에 가장 적절한 모델 1가지를 선택
- 팀원 한 명씩 1개의 모델 성능 테스트 진행 -> 가장 적절한 모델 선택
- 선정 기준: 1순위 -> 추론 속도, 2순위 -> 정확도 => YOLOv8-pose 선택
<img width="1472" height="448" alt="image" src="https://github.com/user-attachments/assets/fea56196-c85b-4e45-896c-fdff972bc9e5" />

### 2. FPS 저하 문제

#### 정답 영상의 포즈 데이터 저장
- 처음에는 기준 영상과 실시간 영상의 포즈 데이터를 실시간으로 추정하려 했음
- 두 영상의 포즈 데이터를 동시에 진행 -> 성능 부하 발생
- 정답 영상의 포즈 데이터를 미리 json파일로 저장하고 이 json파일과 실시간 영상의 포즈 데이터를 비교

#### 모든 프레임마다 추정 -> 0.33초 마다 추정
- 위의 방법을 시도해도 여전히 FPS 저하 문제 발생
- 모든 순간 마다 포즈 추정 과정이 진행되고 있음을 발견
- 0.33초 마다 한번씩 포즈 추정을 실행하여 성능 부하를 해결

### 3. 2인 모드에서 트래킹 실패 현상 발생
- 부여된 id가 계속 바뀌고 트래킹이 풀리는 현상 발생 -> YOLO의 추적기 중 BoT-SORT를 이용하여 트래킹
- id 값이 계속 바뀌는 현상 -> 두 객체의 x좌표 값으로 id 값 부여 (x값이 작으면 player 1, 크면 player 2)
- 최대 id 값의 최대치를 2로 지정하여 트래킹이 풀려도 id 값이 변하지 않도록 지정

### 4. 캐릭터 변환 시 측면 데이터에서의 어색함
- 옆으로 돌아간 포즈 데이터를 2D 캐릭터를 이용하여 변환시 어색함

## **➕ 개선 사항**



## **🎬 시연 영상**



### 1) 시작 + 1인모드 챌린지 선택

#### <시연 모습>
![Image](https://github.com/user-attachments/assets/0eda76ae-86e1-4ac9-bf63-cf452ab7af51)

#### <메인 스크린 - 오프닝>
![Image](https://github.com/user-attachments/assets/85ee0e47-7443-4db6-8876-916d52b5bd82)

#### <메인 스크린 - 챌린지 선택>
![Image](https://github.com/user-attachments/assets/59ad37b4-ab1b-44e3-b68f-407c4857ba72)

#### <컨트롤>
![Image](https://github.com/user-attachments/assets/fc957501-82a5-454f-b834-e37a05470555)

### 2) 1인모드 플레이

#### <시연 모습>
![Image](https://github.com/user-attachments/assets/c0b1e41c-01d4-4e73-909d-987492c82865)

#### <메인 스크린>
![Image](https://github.com/user-attachments/assets/74260732-6f06-4c3d-84c1-1ee0ba331868)

### 3) 1인모드 결과 & 캐릭터 선택

#### <시연 모습>
![Image](https://github.com/user-attachments/assets/c3d3df66-324f-414d-8c2a-9a9b9e6a9512)

#### <메인 스크린>
![Image](https://github.com/user-attachments/assets/5eb4f101-ef6f-420c-a787-9f5f47630f84)

#### <컨트롤>
![Image](https://github.com/user-attachments/assets/9afb6b51-0b4a-4543-86ee-8346aee61587)

### 4) 1인모드 캐릭터 변환 완료
#### <시연 모습>
![Image](https://github.com/user-attachments/assets/2d66a993-4365-458c-8cf0-9903a948b358)

#### <메인 스크린>
![Image](https://github.com/user-attachments/assets/92a14c9d-d6cf-4461-81df-08c521a385cb)

#### <컨트롤>
![Image](https://github.com/user-attachments/assets/229d3644-8788-4a4a-9dc2-71497f163d88)

### 5) 2인모드 플레이

#### <시연 모습>
![Image](https://github.com/user-attachments/assets/34b3b495-eccc-475e-aad8-8c5836ac8404)

### 6) 엔딩 크래딧

#### <메인 스크린>
![Image](https://github.com/user-attachments/assets/7ef553e3-50f1-467c-95bb-4ee70e30f045)

#### <컨트롤>
![Image](https://github.com/user-attachments/assets/d00ce40e-5561-424b-a169-f17adbe55b37)


