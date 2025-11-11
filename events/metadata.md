# Metadata Events

재생이 **새로운 메타데이터가 활성화되는 시간 구간에 진입할 때** 트리거됩니다.  
메타데이터는 아래에 나열된 형식 중 하나로 반환될 수 있습니다.

<br>
<br>

## .on('meta')

재생이 **새로운 메타데이터가 활성화되는 시간 구간에 진입할 때** 트리거됩니다.  
메타데이터는 아래에 나열된 형식 중 하나로 반환될 수 있습니다.

<br>

### Date range (meta)

HLS 스트림에서 `#EXT-X-DATERANGE` 태그가 지정된 구간에 재생이 진입할 때 트리거됩니다.

```json
{
  "type": "meta",
  "metadataType": "date-range",
  "metadataTime": 10,
  "metadata": {
    "tag": "EXT-X-DATERANGE",
    "content": "ID=309726842,PLANNED_DURATION=30.000,START_DATE=2018-10-30 20:45:29.216158+00:00,SCTE35-OUT=TBD",
    "attributes": [
      {
        "name": "ID",
        "value": "309726842"
      },
      {
        "name": "PLANNED_DURATION",
        "value": 30
      },
      {
        "name": "START_DATE",
        "value": "2018-10-30 20:45:29.216158+00:00"
      },
      {
        "name": "SCTE35-OUT",
        "value": "TBD"
      }
    ],
    "start": 10,
    "end": 40,
    "startDate": "2018-10-30 20:45:29.216158+00:00",
    "endDate": null,
    "duration": 30.0
  }
}
```

- **metadata** (object)

  - HLS `#EXT-X-DATERANGE` 태그와 관련된 모든 정보를 포함하는 객체
  - 참조: _Date range metadata (meta)_

    - **metadata.attributes** (array)

      - `EXT-X-DATERANGE` 태그 내 속성 목록

    - **metadata.content** (string)

      - HLS 태그 뒤에 오는 원본 콘텐츠 문자열

    - **metadata.duration** (number)

      - `EXT-X-DATERANGE`의 지속 시간

    - **metadata.end** (number)

      - 스트림의 `currentTime`을 기준으로 한 큐의 종료 시점(초 단위)

    - **metadata.endDate** (string)

      - `EXT-X-DATERANGE` 종료 날짜 (UTC 기준)

    - **metadata.start** (number)

      - 스트림의 `currentTime`을 기준으로 한 큐의 시작 시점(초 단위)

    - **metadata.startDate** (string)

      - `EXT-X-DATERANGE` 시작 날짜 (UTC 기준)

    - **metadata.tag** (string)

      - HLS 매니페스트 태그 이름
      - 이 이벤트의 경우 항상 `"EXT-X-DATERANGE"`

- **metadataTime** (number)

  - 메타데이터 큐의 시작 시간(초 단위)

- **metadataType** (string)

  - 메타 이벤트의 하위 카테고리
  - 이 이벤트의 경우 항상 `"date-range"`

- **type** (string)

  - 플레이어 이벤트의 카테고리
  - 이 이벤트의 경우 항상 `"meta"`

<br>

### EMSG (meta)

`meta` 이벤트가 **메타데이터 시작 시점과 함께 발생할 때** 트리거됩니다.

```json
{
    "type": "meta",
    "metadataType": "emsg",
    "metadataTime": 1594650340,
    "metadata": {
        "metadataType": "emsg",
        "id": 159465034,
        "schemeIdUri": "urn:scte:scte35:2013:xml",
        "timescale": 90000,
        "presentationTimeOffset": 900000,
        "duration": 900000,
        "start": 1594650340,
        "end": 1594650350,
        "messageData": {
            "0": 60,
            "1": 83,
            "2": 112,
            "3": 108,
            ...
            "386": 62
        }
    }
}
```

- **metadata** (object)

  - HLS `#EXT-X-DATERANGE` 태그와 관련된 모든 정보를 포함하는 객체
  - 참조: _EMSG metadata (meta)_

    - **metadata.duration** (number)

      - `EXT-X-DATERANGE`의 지속 시간

    - **metadata.end** (number)

      - 스트림의 `currentTime`을 기준으로 한-큐의 종료 시점(초 단위)

    - **metadata.id** (number)

      - 이 메시지 인스턴스를 식별하는 필드

    - **metadata.messageData** (array)

      - 메시지의 본문 데이터

    - **metadata.metadataType** (string)

      - 메타 이벤트의 하위 카테고리
      - 이 이벤트의 경우 항상 `"emsg"`

    - **metadata.presentationTimeOffset** (number)

      - 이벤트가 시작되는 오프셋 (이벤트가 포함된 세그먼트의 시작 기준- `timescale` 단위로 표시)

    - **metadata.start** (number)

      - 스트림의 `currentTime`을 기준으로 한-큐의 시작 시점(초 단위)

    - **metadata.schemeIdUri** (string)

      - 메시지 스킴(Scheme)을 식별하는 UR-

    - **metadata.timescale** (number)

      - 초당 틱 수 단위의 타임스케일 값

- **metadataTime** (number)

  - 메타데이터 큐의 시작 시간(초 단위)

- **metadataType** (string)

  - 메타 이벤트의 하위 카테고리
  - 이 이벤트의 경우 항상 `"emsg"`

- **type** (string)

  - 플레이어 이벤트의 카테고리
  - 이 이벤트의 경우 항상 `"meta"`

<br>

### ID3 (meta)

재생이 **ID3 태그가 포함된 HLS 스트림 구간에 진입할 때** 트리거됩니다.

```json
{
  "type": "meta",
  "metadataType": "id3",
  "metadataTime": 0,
  "metadata": {
    "type": "meta",
    "metadataType": "media",
    "duration": 60,
    "height": 1280,
    "width": 720,
    "seekRange": {
      "start": 0,
      "end": 60
    }
  }
}
```

- **metadata** (object)

  - HLS ID3 태그와 관련된 모든 정보를 포함하는 객체
  - 비디오의 초기 메타데이터가 **로드될 때** 트리거됩니다.
  - 참조: _ID3 metadata (meta)_

    - **metadata.duration** (number)

      - 미디어 에셋의 총 재생 길이

    - **metadata.height** (number)

      - 미디어 에셋의 세로 해상도

    - **metadata.metadataType** (string)

      - 메타 이벤트의 하위 카테고리
      - 이 이벤트의 경우 항상 `"media"`

    - **metadata.seekRange** (object)

      - **라이브 스트림의 버퍼링 가능 범위** 또는 **DVR의 탐색 가능 구간**을 나타내는 시간 범위 객체
      - 참조: _ID3 seekRange (meta)_

        - **metadata.seekRange.end** (number)

          - 스트림의 `currentTime`을 기준으로 한 시간 범위의 종료 시점(초 단위)

        - **metadata.seekRange.start** (number)

          - 스트림의 `currentTime`을 기준으로 한 시간 범위의 시작 시점(초 단위)

    - **metadata.type** (string)

      - 플레이어 이벤트의 카테고리
      - 이 이벤트의 경우 항상 `"meta"`

    - **metadata.width** (number)

      - 미디어 에셋의 가로 해상도

- **metadataTime** (number)

  - 메타데이터 큐의 시작 시간(초 단위)

- **metadataType** (string)

  - 메타 이벤트의 하위 카테고리
  - 이 이벤트의 경우 항상 `"id3"`

- **type** (string)

  - 플레이어 이벤트의 카테고리
  - 이 이벤트의 경우 항상 `"meta"`

<br>

### Program-date-time (meta)

재생이 `#EXT-X-PROGRAM-DATE-TIME` 태그가 지정된 HLS 스트림 구간에 진입할 때 트리거됩니다.

```json
{
  "type": "meta",
  "metadataType": "program-date-time",
  "programDateTime": "2018-09-28T16:50:46Z",
  "metadataTime": 19.9,
  "metadata": {
    "programDateTime": "2018-09-28T16:50:46Z",
    "start": 19.9,
    "end": 29.9
  }
}
```

- **metadata** (object)

  - HLS `#EXT-X-PROGRAM-DATE-TIME` 태그와 관련된 모든 정보를 포함하는 객체
  - 참조: _Program-date-time metadata (meta)_

    - **metadata.end** (number)

      - 스트림의 `currentTime`을 기준으로 한 큐의 종료 시점(초 단위)

    - **metadata.programDateTime** (string)

      - 프로그램 메타데이터의 날짜와 시간 (UTC 기준)

    - **metadata.start** (number)

      - 스트림의 `currentTime`을 기준으로 한 큐의 시작 시점(초 단위)

- **metadataTime** (number)

  - 메타데이터 큐의 시작 시간(초 단위)

- **metadataType** (string)

  - 메타 이벤트의 하위 카테고리
  - 이 이벤트의 경우 항상 `"program-date-time"`

- **programDateTime** (string)

  - 프로그램 메타데이터의 날짜와 시간 (UTC 기준)

- **type** (string)

  - 플레이어 이벤트의 카테고리
  - 이 이벤트의 경우 항상 `"meta"`

<br>

### SCTE-35 (meta)

재생이 `#EXT-X-CUE-OUT` 또는 `#EXT-X-CUE-IN` 태그가 지정된 HLS 스트림 구간에 진입할 때 트리거됩니다.

```json
{
  "type": "meta",
  "metadataType": "scte-35",
  "metadataTime": 10,
  "metadata": {
    "tag": "EXT-X-CUE-OUT",
    "content": "...",
    "start": 10,
    "end": 40
  }
}
```

- **metadata** (object)

  - HLS `#EXT-X-CUE-OUT`, `#EXT-X-CUE-IN` 태그와 관련된 모든 정보를 포함하는 객체
  - 참조: _SCTE-35 metadata (meta)_

    - **metadata.content** (string)

      - HLS 매니페스트 태그 뒤에 오는 콘텐츠 문자열

    - **metadata.end** (number)

      - 스트림의 `currentTime`을 기준으로 한 큐의 종료 시점(초 단위)

    - **metadata.start** (number)

      - 스트림의 `currentTime`을 기준으로 한 큐의 시작 시점(초 단위)

    - **metadata.tag** (string)

      - HLS 매니페스트 태그 이름
      - 이 이벤트의 경우 항상 `"EXT-X-CUE-OUT"` 또는 `"EXT-X-CUE-IN"`

- **metadataTime** (number)

  - 메타데이터 큐의 시작 시간(초 단위)

- **metadataType** (string)

  - 메타 이벤트의 하위 카테고리
  - 이 이벤트의 경우 항상 `"scte-35"`

- **type** (string)

  - 플레이어 이벤트의 카테고리
  - 이 이벤트의 경우 항상 `"meta"`

<br>

### Unknown (meta)

서드파티 미디어 제공자 또는 **레거시 Flash**로부터 발생한 **메타 이벤트가 트리거되었음을 나타냅니다.**

```json
{
  "type": "meta",
  "metadataType": "unknown",
  ...
}
```

- **metadataType** (string)

  - 메타 이벤트의 하위 카테고리
  - 이 이벤트의 경우 항상 `"unknown"`

- **type** (string)

  - 플레이어 이벤트의 카테고리
  - 이 이벤트의 경우 항상 `"meta"`

<br>
<br>

## .on('metadataCueParsed')

**메타데이터 큐 포인트가 버퍼링되면** 트리거됩니다.

<br>

### Date range (metadataCueParsed)

플레이어가 `#EXT-X-DATERANGE` 태그가 지정된 HLS 스트림 구간을 버퍼링할 때 트리거됩니다.

```json
{
  "type": "metadataCueParsed",
  "metadataType": "date-range",
  "metadataTime": 10,
  "metadata": {
    "tag": "EXT-X-DATERANGE",
    "content": "ID=309726842,PLANNED_DURATION=30.000,START_DATE=2018-10-30 20:45:29.216158+00:00,SCTE35-OUT=TBD",
    "attributes": [
      {
        "name": "ID",
        "value": "309726842"
      },
      {
        "name": "PLANNED_DURATION",
        "value": 30
      },
      {
        "name": "START_DATE",
        "value": "2018-10-30 20:45:29.216158+00:00"
      },
      {
        "name": "SCTE35-OUT",
        "value": "TBD"
      }
    ],
    "start": 10,
    "end": 40,
    "startDate": "2018-10-30 20:45:29.216158+00:00",
    "endDate": null,
    "duration": 30.0
  }
}
```

- **metadata** (object)

  - HLS `#EXT-X-DATERANGE` 태그와 관련된 모든 정보를 포함하는 객체
  - 참조: _Date range metadata (metadataCueParsed)_

    - **metadata.attributes** (array)

      - `EXT-X-DATERANGE` 태그 내의 속성 목록

    - **metadata.content** (string)

      - HLS 태그 뒤에 오는 원본 콘텐츠 문자열

    - **metadata.duration** (number)

      - `EXT-X-DATERANGE`의 지속 시간

    - **metadata.end** (number)

      - 스트림의 `currentTime`을 기준으로 한 큐의 종료 시점(초 단위)

    - **metadata.endDate** (string)

      - `EXT-X-DATERANGE` 종료 날짜 (UTC 기준)

    - **metadata.start** (number)

      - 스트림의 `currentTime`을 기준으로 한 큐의 시작 시점(초 단위)

    - **metadata.startDate** (string)

      - `EXT-X-DATERANGE` 시작 날짜 (UTC 기준)

    - **metadata.tag** (string)

      - HLS 매니페스트 태그 이름
      - 이 이벤트의 경우 항상 `"EXT-X-DATERANGE"`

- **metadataTime** (number)

  - 메타데이터 큐의 시작 시간(초 단위)

- **metadataType** (string)

  - `metadataCueParsed` 이벤트의 하위 카테고리
  - 이 이벤트의 경우 항상 `"date-range"`

- **type** (string)

  - 플레이어 이벤트의 카테고리
  - 이 이벤트의 경우 항상 `"meta"`

<br>

### EMSG (metadataCueParsed)

```json
{
  "type": "metadataCueParsed",
  "metadataType": "emsg",
  "metadataTime": 1594650340,
  "metadata": {
    "metadataType": "emsg",
    "id": 159465034,
    "schemeIdUri": "urn:scte:scte35:2013:xml",
    "timescale": 90000,
    "presentationTimeOffset": 900000,
    "duration": 900000,
    "start": 1594650340,
    "end": 1594650350,
    "messageData": {
      "0": 60,
      "1": 83,
      "2": 112,
      "3": 108,
      ...
      "386": 62
    }
  }
}
```

- **metadata** (object)

  - HLS `#EXT-X-DATERANGE` 태그와 관련된 모든 정보를 포함하는 객체
  - 참조: _EMSG metadata (metadataCueParsed)_

    - **metadata.duration** (number)

      - `EXT-X-DATERANGE`의 지속 시간

    - **metadata.end** (number)

      - 스트림의 `currentTime`을 기준으로 한 큐의 종료 시점(초 단위)

    - **metadata.id** (number)

      - 이 메시지 인스턴스를 식별하는 필드

    - **metadata.messageData** (array)

      - 메시지의 본문 데이터

    - **metadata.metadataType** (string)

      - `metadataCueParsed` 이벤트의 하위 카테고리
      - 이 이벤트의 경우 항상 `"emsg"`

    - **metadata.presentationTimeOffset** (number)

      - 이벤트가 시작되는 오프셋 (이 이벤트가 포함된 세그먼트의 시작 기준, `timescale` 단위로 표시)

    - **metadata.start** (number)

      - 스트림의 `currentTime`을 기준으로 한 큐의 시작 시점(초 단위)

    - **metadata.schemeIdUri** (string)

      - 메시지 스킴(Scheme)을 식별하는 URI

    - **metadata.timescale** (number)

      - 초당 틱(tick) 수 단위의 타임스케일 값

- **metadataTime** (number)

  - 메타데이터 큐의 시작 시간(초 단위)

- **metadataType** (string)

  - `metadataCueParsed` 이벤트의 하위 카테고리
  - 이 이벤트의 경우 항상 `"emsg"`

- **type** (string)

  - 플레이어 이벤트의 카테고리
  - 이 이벤트의 경우 항상 `"metadataCueParsed"`

<br>

### ID3 (metadataCueParsed)

재생이 **ID3 태그가 포함된 HLS 스트림의 구간을 버퍼링할 때** 트리거됩니다.

```json
{
  "type": "metadataCueParsed",
  "metadataType": "id3",
  "metadataTime": 0,
  "metadata": {
    "type": "metadataCueParsed",
    "metadataType": "media",
    "duration": 60,
    "height": 1280,
    "width": 720,
    "seekRange": {
      "start": 0,
      "end": 60
    }
  }
}
```

- **metadata** (object)

  - HLS ID3 태그와 관련된 모든 정보를 포함하는 객체
  - 비디오의 초기 메타데이터가 로드될 때 트리거됩니다.
  - 참조: _ID3 metadata (metadataCueParsed)_

    - **metadata.duration** (number)

      - 미디어 자산의 전체 재생 길이

    - **metadata.height** (number)

      - 미디어 자산의 세로 해상도

    - **metadata.metadataType** (string)

      - `metadataCueParsed` 이벤트의 하위 카테고리
      - 이 이벤트의 경우 항상 `"media"`

    - **metadata.seekRange** (object)

      - **라이브 스트림의 버퍼링 가능 범위** 또는 **DVR 탐색 가능 구간**을 나타내는 시간 범위 객체
      - 참조: _ID3 seekRange (metadataCueParsed)_

        - **metadata.seekRange.end** (number)

          - 스트림의 `currentTime`을 기준으로 한 시간 범위의 종료 시점(초 단위)

        - **metadata.seekRange.start** (number)

          - 스트림의 `currentTime`을 기준으로 한 시간 범위의 시작 시점(초 단위)

    - **metadata.type** (string)

      - 플레이어 이벤트의 카테고리
      - 이 이벤트의 경우 항상 `"metadataCueParsed"`

    - **metadata.width** (number)

      - 미디어 자산의 가로 해상도

- **metadataTime** (number)

  - 메타데이터 큐의 시작 시간(초 단위)

- **metadataType** (string)

  - `metadataCueParsed` 이벤트의 하위 카테고리
  - 이 이벤트의 경우 항상 `"id3"`

- **type** (string)

  - 플레이어 이벤트의 카테고리
  - 이 이벤트의 경우 항상 `"metadataCueParsed"`

<br>

### Program-date-time (metadataCueParsed)

플레이어가 `#EXT-X-PROGRAM-DATE-TIME` 태그가 지정된 HLS 스트림 구간을 버퍼링할 때 트리거됩니다.

```json
{
  "type": "metadataCueParsed",
  "metadataType": "program-date-time",
  "programDateTime": "2018-09-28T16:50:46Z",
  "metadataTime": 19.9,
  "metadata": {
    "programDateTime": "2018-09-28T16:50:46Z",
    "start": 19.9,
    "end": 29.9
  }
}
```

- **metadata** (object)

  - HLS `#EXT-X-PROGRAM-DATE-TIME` 태그와 관련된 모든 정보를 포함하는 객체
  - 참조: _Program-date-time metadata (metadataCueParsed)_

    - **metadata.end** (number)

      - 스트림의 `currentTime`을 기준으로 한 큐의 종료 시점(초 단위)

    - **metadata.programDateTime** (string)

      - 프로그램 메타데이터의 날짜와 시간 (UTC 기준)

    - **metadata.start** (number)

      - 스트림의 `currentTime`을 기준으로 한 큐의 시작 시점(초 단위)

- **metadataTime** (number)

  - 메타데이터 큐의 시작 시간(초 단위)

- **metadataType** (string)

  - `metadataCueParsed` 이벤트의 하위 카테고리
  - 이 이벤트의 경우 항상 `"program-date-time"`

- **programDateTime** (string)

  - 프로그램 메타데이터의 날짜와 시간 (UTC 기준)

- **type** (string)

  - 플레이어 이벤트의 카테고리
  - 이 이벤트의 경우 항상 `"metadataCueParsed"`

<br>

### SCTE-35 metadata

플레이어가 `#EXT-X-CUE-OUT` 또는 `#EXT-X-CUE-IN` 태그가 지정된 HLS 스트림 구간을 버퍼링할 때 트리거됩니다.

```json
{
  "type": "metadataCueParsed",
  "metadataType": "scte-35",
  "metadataTime": 10,
  "metadata": {
    "tag": "EXT-X-CUE-OUT",
    "content": "...",
    "start": 10,
    "end": 40
  }
}
```

- **metadata** (object)

  - HLS `#EXT-X-CUE-OUT`, `#EXT-X-CUE-IN` 태그와 관련된 모든 정보를 포함하는 객체
  - 참조: _SCTE-35 metadata (metadataCueParsed)_

    - **metadata.content** (string)

      - HLS 매니페스트 태그 뒤에 오는 콘텐츠 문자열

    - **metadata.end** (number)

      - 스트림의 `currentTime`을 기준으로 한 큐의 종료 시점(초 단위)

    - **metadata.start** (number)

      - 스트림의 `currentTime`을 기준으로 한 큐의 시작 시점(초 단위)

    - **metadata.tag** (string)

      - HLS 매니페스트 태그 이름
      - 이 이벤트의 경우 항상 `"EXT-X-CUE-OUT"` 또는 `"EXT-X-CUE-IN"`

- **metadataTime** (number)

  - 메타데이터 큐의 시작 시간(초 단위)

- **metadataType** (string)

  - `metadataCueParsed` 이벤트의 하위 카테고리
  - 이 이벤트의 경우 항상 `"scte-35"`

- **type** (string)

  - 플레이어 이벤트의 카테고리
  - 이 이벤트의 경우 항상 `"metadataCueParsed"`
