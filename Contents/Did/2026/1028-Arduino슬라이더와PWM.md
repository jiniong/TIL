# Arduino 슬라이더 입력과 PWM 밝기 제어

- 분류: 실습
- 기록일: 2026-10-28
- 갱신일: 2026-10-29
- 주제: Arduino, ADC, PWM, map
- 출처: Codex 대화 「아두이노 쉬운 실습 주제 정리」
- 상태: 사용자 동작 확인 취지의 보고, 재실행 미확인

## 목차

- [목표와 환경](#목표와-환경)
- [ADC와 PWM의 차이](#adc와-pwm의-차이)
- [회로 극성과 값 변환](#회로-극성과-값-변환)
- [결과와 다음 확인](#결과와-다음-확인)
- [관련 지식](#관련-지식)
- [업데이트 기록](#업데이트-기록)

## 목표와 환경

슬라이더 값을 읽어 LED 밝기를 조절하는 실습이다.

- 당시 확인한 프로젝트: [Wokwi 슬라이더 실습](https://wokwi.com/projects/479095596323746817)
- 보드: Arduino Uno
- 슬라이더 SIG: A0
- LED 출력: 10번 핀

## ADC와 PWM의 차이

analogRead는 입력 전압을 디지털 수치로 읽는다. PWM은 HIGH·LOW를 빠르게 반복하고 듀티비를 바꿔 평균적인 전력 전달을 조절한다. PWM 핀에서 연속적인 아날로그 전압이 직접 출력된다고 이해하지 않는다.

| Uno 기준 | 범위 |
| --- | --- |
| 기본 ADC 읽기 | 0~1023 |
| analogWrite PWM 설정 | 0~255 |

map은 입력 범위를 출력 범위로 비례 변환한다. 기본 map은 정수 연산이며 입력을 자동으로 범위 안에 제한하는 함수는 아니다. [Arduino map 문서](https://docs.arduino.cc/language-reference/en/functions/math/map/)

## 회로 극성과 값 변환

당시 프로젝트는 LED가 전원 쪽에 연결된 active-low 구조였다. 값이 커질수록 밝아지게 하려면 출력 범위를 반대로 매핑한다.

~~~cpp
const int sliderPin = A0;
const int ledPin = 10;

void setup() {
  pinMode(ledPin, OUTPUT);
}

void loop() {
  int value = analogRead(sliderPin);
  int pwm = map(value, 0, 1023, 255, 0);
  analogWrite(ledPin, pwm);
}
~~~

위 코드는 당시 회로를 설명하기 위해 정리한 예시이며 이번 기록 작업에서 컴파일·시뮬레이션을 실행하지 않았다. active-high 배선이면 0~255 방향을 사용한다. 보드가 바뀌면 ADC 해상도·PWM API·핀 기능도 다시 확인한다.

## 결과와 다음 확인

사용자가 동작한 것으로 보인다는 취지로 프로젝트를 공유했고, 저장된 코드·배선과 반전 방향을 확인했다. 다음 실습에서는 Serial에 원시 입력과 PWM 값을 함께 출력해 끝값·중간값을 확인한다.

## 관련 지식

- [아두이노·GPIO 기초](1027-아두이노기초와GPIO.md)
- [버튼 디바운싱](1027-ESP32버튼LED와디바운싱.md)
- [DHT22와 OLED](1028-ArduinoDHT22와OLED.md)

## 업데이트 기록

- 2026-10-29: Uno 입력 범위와 active-low 회로의 반전 이유 정리
