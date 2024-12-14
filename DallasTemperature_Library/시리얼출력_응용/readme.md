### 설명이 있는 카페 링크
https://cafe.naver.com/dgarduino/23950

# DS18B20 온도센서 다중 모니터링 시스템 문서

## 1. 프로그램 개요
- 목적: 최대 4개의 DS18B20 온도센서를 모니터링하며 다양한 예외상황 처리
- 주요기능: 센서 초기화, ID/주소 읽기, 온도측정, 예외처리
- 사용환경: Arduino

## 2. 핵심 컴포넌트
### 라이브러리
- OneWire.h: 1-Wire 통신 프로토콜 구현
- DallasTemperature.h: DS18B20 센서 제어

### 주요 변수
```cpp
const int ONE_WIRE_BUS = 2;          // 센서 연결 핀
const int arraySizeByUser = 4;       // 최대 센서 수
DeviceAddress deviceAddress[];       // 센서 주소 배열
int sortID[];                       // 정렬된 ID 배열
bool isValidSensor[];              // 센서 유효성 상태
```

### 3. 주요 함수 설명

initializeSensors()
기능: 센서 초기화 및 연결 검증
반환: bool (초기화 성공 여부)
예외처리:
센서 미감지 확인
예상/실제 센서 수 비교

readAddress()
기능: 센서 주소 읽기
동작: 각 센서의 주소를 읽고 유효성 검사
저장: deviceAddress 배열에 저장
readIdFromDS18B20()
기능: 센서 ID 읽기/검증
검사항목:
ID 범위(1~99)
ID 중복여부
센서 유효성

sortIdByBubbleSort()
기능: 센서 정보 정렬
방식: 버블정렬
정렬대상: ID, 주소, 유효성 상태

printSensorInfo()
기능: 센서 정보 출력
출력항목: 순서/ID/온도/주소/상태
상태구분:
정상
통신오류
온도범위초과
센서오류

### 4. 예외처리 케이스
센서 관련
센서 미감지 시 오류 메시지
센서 수 불일치 경고
주소 읽기 실패 처리
ID 관련
유효범위(1~99) 검증
중복 ID 검출
잘못된 ID 처리
온도 관련
측정범위(-55°C~125°C) 검증
통신오류 감지/처리
범위초과 상태 표시

### 5. 시스템 제한사항
최대 센서 수: 4개
ID 허용범위: 1~99
온도 측정범위: -55°C~125°C
업데이트 주기: 2초

### 6. 출력 형식
=== 센서 정보 ===
순서    ID    온도        주소            상태
----------------------------------------------------
1       1     23.5°C     28FF7A3C...     정상
2       2     ERROR      28FF7A3D...     통신오류

### 7. 문제해결 가이드

"No sensors detected"

물리적 연결 확인
ONE_WIRE_BUS 핀 번호 확인
