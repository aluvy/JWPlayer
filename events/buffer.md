# Buffer Events

이 API 호출은 **플레이어에 버퍼링된 파일의 퍼센티지(%) 정보를 클라이언트에 업데이트**하는 데 사용됩니다.

> 이 기능은 **VOD(주문형 비디오)** 미디어에만 적용됩니다.  
> 실시간 스트리밍 미디어(HLS/DASH)에서는 이 동작이 노출되지 않습니다.

<br>
<br>

## .on('bufferChange')

현재 재생 중인 항목이 **버퍼에 추가 데이터를 로드할 때** 트리거됩니다.

```json
{
  "bufferPercent": 2.7706319444444443,
  "position": 7.90229,
  "duration": 648,
  "currentTime": 7.90229,
  "seekRange": {
    "start": 0,
    "end": 648
  },
  "absolutePosition": null,
  "type": "bufferChange"
}
```

- **absolutePosition** (number \| null)

  - 미디어 파일 내에서 시청자의 **절대 위치**를 나타냅니다. 이는 영상의 현재 재생 시간(`currentTime`)과 스트림의 `startDateTime` 값(날짜 형식)을 합한 값입니다.

- **bufferPercent** (number)

  - 현재 미디어에서 **버퍼링된 비율(%)**을 0~100 사이의 값으로 표시합니다.

- **currentTime** (number) <sup>v8.19.0+</sup>

  - 플레이어가 탐색(seek)을 수행하기 전의 **스트림 위치(초 단위)**

- **duration** (number) <sup>v8.19.0+</sup>

  - 미디어의 **현재 전체 재생 길이(초 단위)**

- **position** (number)

  - 미디어 파일의 **현재 재생 위치(초 단위)**

- **seekRange** (object)

  - DVR 스트림의 탐색 가능 범위 또는 라이브 스트림의 버퍼링 가능 구간을 나타내는 **시간 범위 객체**<br>참조: `seekRange`
  - **seekRange[]**

    - **seekRange.end** (number)

      - 스트림의 현재 시간(`currentTime`)을 기준으로 한 **범위의 종료 시점(초 단위)**

    - **seekRange.start** (number)

      - 스트림의 현재 시간(`currentTime`)을 기준으로 한 **범위의 시작 시점(초 단위)**

- **type** (string)

  - 플레이어 이벤트의 카테고리 (이 이벤트의 경우 항상 `"bufferChange"`)
