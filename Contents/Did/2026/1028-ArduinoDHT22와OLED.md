# Arduino Mega의 DHT22 온도와 OLED 표시

- 분류: 실습
- 기록일: 2026-10-28
- 갱신일: 2026-10-29
- 주제: Arduino, DHT22, OLED, I2C, 상태 갱신
- 출처: Codex 대화 「아두이노 쉬운 실습 주제 정리」
- 상태: 배선 원인 확인·문법 수정, 상태 갱신 미완성

## 목차

- [목표와 환경](#목표와-환경)
- [시행착오 1: Uno 배선과 Mega 배선 혼동](#시행착오-1-uno-배선과-mega-배선-혼동)
- [시행착오 2: 여러 값을 print에 한 번에 전달](#시행착오-2-여러-값을-print에-한-번에-전달)
- [남아 있는 상태 관리 문제](#남아-있는-상태-관리-문제)
- [다음 검증 순서](#다음-검증-순서)
- [관련 지식](#관련-지식)
- [업데이트 기록](#업데이트-기록)

## 목표와 환경

DHT22의 온도를 읽어 SSD1306 OLED에 표시하고, 온도가 바뀔 때 이전 변경 이후 경과 시간을 표시하려는 실습이다.

- 당시 확인한 프로젝트: [Wokwi DHT22·OLED 실습](https://wokwi.com/projects/479098529674228737)
- 실제 보드: Arduino Mega
- DHT22 데이터: 디지털 2번
- OLED: 128×64, 코드의 I2C 주소 0x3C
- 라이브러리: DHT, Wire, Adafruit GFX, Adafruit SSD1306

## 시행착오 1: Uno 배선과 Mega 배선 혼동

OLED가 Uno 기준 A4·A5에 연결되어 있었지만 실제 프로젝트는 Mega였다.

| 보드 | SDA | SCL |
| --- | --- | --- |
| Uno | A4 | A5 |
| Mega 2560 | 20 | 21 |

OLED의 SDA를 Mega 20번, SCL을 21번으로 옮기도록 수정안을 제시했다. [Arduino I2C 공식 문서](https://docs.arduino.cc/learn/communication/wire)

display.begin이 성공했다는 사실만으로 실제 화면 배선이 모두 확인된 것은 아니다. 보드명과 diagram.json의 연결을 먼저 대조해야 한다.

## 시행착오 2: 여러 값을 print에 한 번에 전달

display.print에 문장·온도·시간을 쉼표로 나열해 전달하려 했다. 출력 조각마다 호출을 나누어야 한다.

~~~cpp
display.print("Temp: ");
display.println(currentTemperature);
display.print("Elapsed: ");
display.print(currentTime - previousTime);
display.print(" ms");
display.display();
~~~

위 코드는 문법 수정 부분 코드다. display.print(value, digits) 같은 두 번째 인자는 일반적인 문자열 연결 용도가 아니다. 당시 사용한 기본 글꼴의 한글 출력 문제를 피하려고 영어 문구를 사용했다.

## 남아 있는 상태 관리 문제

마지막 원문 코드에는 previousTemperature와 previousTime을 갱신하는 부분이 없었다. 문법 수정만으로 온도 변화 감지와 경과 시간 기능이 완성되는 것은 아니다.

- 첫 측정은 이전 값이 없으므로 초기값 처리 필요
- 온도 읽기 실패로 NaN이 나오면 정상 측정과 분리
- 유효한 온도 변경을 처리한 뒤 비교 기준과 시각 갱신 필요
- float의 변화 기준을 정확한 일치로 둘지 허용 오차로 둘지 결정
- 128×64 화면에서 글자 크기·개행·표시 길이 확인 필요

## 다음 검증 순서

1. Serial에 센서 값만 출력
2. OLED에 Hello만 표시하고 일정 시간 유지
3. 온도 한 값 표시
4. 첫 측정·같은 온도·바뀐 온도·읽기 실패 분기 확인
5. 경과 시간과 이전 상태 갱신 통합

배선 원인과 문법 수정은 확인했지만 수정 후 회로의 실제 표시·최종 동작은 대화에서 확정하지 못했다.

## 관련 지식

- [슬라이더·PWM 실습](1028-Arduino슬라이더와PWM.md)
- [이전 상태를 저장하는 버튼 실습](1027-ESP32버튼LED와디바운싱.md)
- [Wokwi DHT22 문서](https://docs.wokwi.com/parts/wokwi-dht22)
- [Wokwi SSD1306 문서](https://docs.wokwi.com/parts/board-ssd1306)

## 업데이트 기록

- 2026-10-29: 실제 보드 기준 배선 수정과 미완성 상태 갱신 로직 정리
