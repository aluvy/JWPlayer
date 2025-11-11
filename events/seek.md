# Seek Events

이 API 호출은 **현재 미디어의 재생 위치(playback position)** 를 조회하거나 업데이트하는 데 사용됩니다.

<br>
<br>

## .on('absolutePositionReady')

`.getAbsolutePosition()` 메서드가 데이터를 반환할 준비가 되었을 때 트리거됩니다.

```json
{
  "ready": true
  "startDateTime": 1702321975350,
  "type": "absolutePositionReady"
}
```

- **ready** (boolean)

  - `.getAbsolutePosition()`이 절대 위치 데이터를 반환할 준비가 되었는지 여부
  - `true`: 준비 완료
  - `false`: 준비되지 않음

- **startDateTime** (string)

  - 스트림의 **원본 방송 날짜 및 시간** (밀리초 단위)
  - `startDateTime` 은 다음 값들로부터 결정됩니다:
    - HLS의 `#EXT-X-PROGRAM-DATE-TIME`
    - DASH의 `presentationTimeOffset`

- **type** (string)

  - 수신 중인 이벤트의 식별자
  - 이 이벤트의 경우 항상 `"absolutePositionReady"`

<br>
<br>

## .on('seek')

컨트롤 바에서 **스크럽(scrubbing)** 하거나 **API를 통해 탐색(seek)** 이 요청되었을 때 트리거됩니다.

```json
{
  "position": 6.768026,
  "offset": 7.860885148195877,
  "duration": 118,
  "currentTime": 6.768026,
  "seekRange": {
    "start": 0,
    "end": 118
  },
  "metadata": {
    "currentTime": 6.768026
  },
  "type": "seek"
}
```

- **currentTime** (number) <sup>8.19.0+</sup>

  - 플레이어가 탐색하기 전의 스트림 위치(초 단위)

- **duration** (number) <sup>8.19.0+</sup>

  - 현재 플레이리스트 항목의 전체 재생 시간(초 단위)

- **position** (number)

  - 탐색 전 플레이어의 위치(초 단위)

- **seekRange** (object) <sup>8.19.0+</sup>

  - DVR 스트림의 탐색 가능 범위 또는 라이브 스트림의 버퍼링 가능 범위를 나타내는 시간 구간 객체
  - 참조: _seek.seekRange_

    - **seekRange.end** (number)

      - 스트림의 `currentTime`을 기준으로 한 탐색 범위의 종료 시점(초 단위)

    - **seekRange.start** (number)

      - 스트림의 `currentTime`을 기준으로 한 탐색 범위의 시작 시점(초 단위)

- **offset** (number)

  - 탐색이 요청된 목표 위치(초 단위)

- **metadata** (object)

  - 리소스 정의를 위한 변경 가능한 속성 집합
  - 참조: _seek.metadata_

    - **metadata.currentTime** (number) <sup>8.19.0+</sup>

      - 플레이어가 탐색하기 전의 스트림 위치(초 단위)

- **type** (string)

  - 수신 중인 이벤트의 식별자
  - 이 이벤트의 경우 항상 `"seek"`

<br>

> 탐색(Seeking)은 일반적으로 **키프레임(keyframe)** 의 존재 여부에 따라 수행됩니다.  
> 따라서 실제로 탐색되는 위치는 **요청된 위치와 다를 수 있습니다.**

<br>
<br>

## .on('seeked')

`on('seek')` 이벤트가 **탐색 동작이 발생할 때** 트리거되는 것과 달리,  
이 이벤트는 **탐색이 완료되어 비디오 위치가 실제로 변경된 후** 트리거됩니다.

```json
{
  "type": "seeked"
}
```

- **type** (string)

  - 수신 중인 이벤트의 식별자
  - 이 이벤트의 경우 항상 `"seeked"`

<br>
<br>

## .on('time')

플레이어가 재생 중일 때 **재생 위치가 업데이트될 때마다** 트리거됩니다.  
이 이벤트는 **초당 최대 10회까지** 발생할 수 있습니다.

```json
{
  "position": 20.523624,
  "duration": 231.509002,
  "currentTime": 20.523624,
  "seekRange": {
    "start": 0,
    "end": 231.509002
  },
  "metadata": {
    "currentTime": 20.523624
  },
  "absolutePosition": null,
  "type": "time",
  "viewable": 1
}
```

- **absolutePosition**

  - 미디어 파일에서의 시청자의 절대 위치로, 비디오의 현재 시간과 스트림의 `startDateTime` 값을 더한 값 (날짜 형식)

- **currentTime** (number) <sup>(8.19.0+)</sup>

  - 플레이어가 탐색하기 전의 스트림 위치(초 단위)

- **duration** (number) <sup>(8.19.0+)</sup>

  - 현재 플레이리스트 항목의 전체 재생 시간(초 단위)

- **metadata** (object)

  - 리소스를 정의하는 변경 가능한 속성
  - 참조: _time.metadata_

    - **metadata.currentTime** (number) <sup>(8.19.0+)</sup>

      - 플레이어가 탐색하기 전의 스트림 위치(초 단위)

- **position** (number)

  - 현재 재생 위치(초 단위)

- **seekRange** (object) <sup>(8.19.0+)</sup>

  - DVR 스트림에서 탐색 가능한 구간 또는 라이브 스트림의 버퍼링 가능한 범위를 나타내는 시간 구간 객체
  - 참조: _time.seekRange_

    - **seekRange.end** (number)

      - 스트림의 `currentTime`을 기준으로 한 범위의 종료 시점(초 단위)

    - **seekRange.start** (number)

      - 스트림의 `currentTime`을 기준으로 한 범위의 시작 시점(초 단위)

- **type** (string)

  - 수신 중인 이벤트의 식별자
  - 이 이벤트의 경우 항상 `"time"`

- **viewable** (boolean)

  - 플레이어가 화면에 표시되는지 여부
  - `1` = 표시됨, `0` = 표시되지 않음
