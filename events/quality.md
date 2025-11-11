# Quality Events

이 API 호출은 **여러 화질(quality level)** 이 제공되는 비디오에서  
현재 화질을 **감지하거나 변경할 때** 사용됩니다.

> 인덱스 **0은 항상 “Auto(자동)”** 를 의미합니다.

<br>
<br>

## .on('levels')

사용 가능한 화질 수준 목록이 **업데이트될 때** 트리거됩니다.  
예를 들어, **플레이리스트 항목이 재생을 시작한 직후**에 발생합니다.

- **width** (number)

  - 플레이어의 새로운 너비

- **levels** (array)

  - 새로운 항목을 포함한 전체 화질 배열.
  - `getQualityLevels()`와 동일한 정보를 포함합니다.

<br>
<br>

## .on('levelsChanged')

**활성 화질 수준이 변경될 때** 트리거됩니다.  
이 이벤트는 사용자가 **품질 메뉴에서 옵션을 클릭했을 때** 또는  
스크립트에서 `setCurrentQuality()`를 호출했을 때 발생합니다.

- **currentQuality** (number)

  - `getQualityLevels()` 배열 내에서 새 화질 수준의 인덱스

<br>
<br>

## .on('visualQuality')

HLS에서 **활성 화질 수준이 변경될 때** 트리거됩니다.

이 이벤트는 `levelsChanged`와 다르게,  
**적응형 스트리밍(adaptive streaming) 이 자동으로 화질을 전환할 때**도 발생합니다.

```json
{
  "reason": "initial choice",
  "mode": "auto",
  "level": {
    "bitrate": 700000,
    "index": 1,
    "label": "auto",
    "width": 1280,
    "height": 536
  },
  "type": "visualQuality"
}
```

- **level** (object)

  - 미디어 항목의 각 화질 수준에 대한 정보
  - 참조: _level_

    - **level.bitrate** (string)

      - 미디어 파일의 비트레이트

    - **level.height** (number)

      - 미디어 파일의 세로 해상도

    - **level.index** (number)

      - 화질 수준의 인덱스

    - **level.label** (string)

      - 화질 수준의 라벨

    - **level.width** (number)

      - 미디어 파일의 가로 해상도

- **mode** (string)

  - 현재 화질 선택 모드
  - 가능한 값:
    - `auto`: 자동 화질 전환
    - `manual`: 고정 화질

- **reason** (string)

  - 화질이 변경된 이유
  - 가능한 값:
    - `api`: 재생 도중 사용자가 고정 화질을 선택했거나 API에서 화질을 설정한 경우
    - `auto`: 자동 화질 전환이 발생한 경우
    - `initial.choice`: 사용자가 기본 화질을 설정했으며 변경하지 않은 경우

- **type** (string)

  - 이벤트의 카테고리
  - 이 값은 항상 `"visualQuality"`
