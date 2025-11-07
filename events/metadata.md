# Metadata Events

이 API 호출은 개발자가 **미디어 파일에 포함된 메타데이터(metadata)** 를 감지(listen)할 수 있도록 합니다.  
예를 들어, HLS 스트림 내 **비디오의 해상도(dimension)** 정보나 **ID3 시간 기반 메타데이터(timed metadata)** 등을 들을 수 있습니다.

<br>
<br>

# .on('meta')

`meta` 이벤트는 **재생이 새로운 메타데이터가 활성화되는 시간 범위에 진입할 때** 트리거됩니다.  
메타데이터는 아래에 나열된 형식 중 하나로 반환될 수 있습니다.

<br>

## Date range (meta)

이 이벤트는 **재생 중 HLS 스트림 내 `#EXT-X-DATERANGE` 태그가 지정된 구간**에 진입할 때 트리거됩니다.

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

  - HLS `#EXT-X-DATERANGE` 태그와 관련된 모든 정보를 포함하는 객체입니다.

  - 참고: [Date range metadata (meta)](#date-range-metadata-meta)

- **metadataTime** (number)

  - 메타데이터 큐(cue)가 시작되는 시점(초 단위)

- **metadataType** (string)

  - `meta` 이벤트의 하위 분류(subcategory)<br><br>이 이벤트의 경우 항상 `"date-range"` 값입니다.

- **type** (string)

  - 플레이어 이벤트의 상위 카테고리<br><br>이 이벤트의 경우 항상 `"meta"` 값입니다.

<br>

### Date range metadata (meta)

- **attributes** (array)

  - `EXT-X-DATERANGE` 태그에 정의된 속성 배열

- **content** (string)

  - HLS 태그에 이어지는 전체 콘텐츠 문자열

- **duration** number

  - `EXT-X-DATERANGE`의 지속 시간(초 단위)

- **end** (number)

  - 스트림의 `currentTime` 기준으로 측정한 큐(cue)의 종료 시점(초 단위)

- **endDate** (string)

  - UTC 기준 `EXT-X-DATERANGE` 종료 시각

- **start** (number)

  - 스트림의 `currentTime` 기준으로 측정한 큐(cue)의 시작 시점(초 단위)

- **startDate** (string)

  - UTC 기준 `EXT-X-DATERANGE` 시작 시각

- **tag** (string)

  - HLS 매니페스트 태그 이름
  - 이 이벤트의 경우 항상 `"EXT-X-DATERANGE"` 입니다.

<br><br>

## EMSG (meta)

이 이벤트는 **메타 이벤트(meta event)** 가 **메타데이터 시작 시점(metadata start time)** 과 함께 발생할 때 트리거됩니다.

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

<br>

- **metadata** (object)

  - HLS `#EXT-X-DATERANGE` 태그와 관련된 모든 정보를 포함하는 객체입니다.
  - 참고: [EMSG metadata (meta)](#emsg-metadata-meta)

- **metadataTime** (number)

  - 메타데이터 큐(cue)의 시작 시점(초 단위)

- **metadataType** (string)

  - `meta` 이벤트의 하위 분류(subcategory)
  - 이 이벤트의 경우 항상 `"emsg"` 값입니다.

- **type** (string)

  - 플레이어 이벤트의 상위 카테고리
  - 이 이벤트의 경우 항상 `"meta"` 값입니다.

<br>

### EMSG metadata (meta)

- **duration** (number)

  - 메타데이터(`EXT-X-DATERANGE`)의 지속 시간(초 단위)

- **end** (number)

  - 스트림의 `currentTime` 기준으로 한 큐(cue)의 종료 시점(초 단위)

- **id** (number)

  - 메시지 인스턴스를 식별하는 필드

- **messageData** (array)

  - 메시지의 본문(body of the message)

- **metadataType** (string)

  - `meta` 이벤트의 하위 분류(subcategory)
  - 이 이벤트의 경우 항상 `"emsg"` 값입니다.

- **presentationTimeOffset** (number)

  - 이벤트가 시작되는 오프셋 (현재 세그먼트 시작 시점으로부터의 상대값, `timescale` 단위로 표시)

- **start** (number)

  - 스트림의 `currentTime` 기준으로 한 큐(cue)의 시작 시점(초 단위)

- **schemeIdUri** (string)

  - 메시지 스킴을 식별하는 URI

- **timescale** (number)

  - 초당 틱(ticks per second) 단위로 표현된 타임스케일 값

<br><br>

## ID3 (meta)

이 이벤트는 **재생이 ID3 태그를 포함한 HLS 스트림 구간에 진입할 때** 트리거됩니다.

```json
{
  "type": "meta",
  "metadataType": "id3",
  "metadataTime": 0,
  "metadata": {
    ...
  }
}
```

<br>

- **metadata** (object)

  - HLS `ID3` 태그와 관련된 모든 정보를 포함하는 객체입니다.
  - 참고: [ID3 metadata (meta)](#id3-metadata-meta)

- **metadataTime** (number)

  - 메타데이터 큐(cue)의 시작 시점(초 단위)

- **metadataType** (string)

  - `meta` 이벤트의 하위 분류(subcategory)
  - 이 이벤트의 경우 항상 `"id3"` 값입니다.

- **type** (string)

  - 플레이어 이벤트의 상위 카테고리
  - 이 이벤트의 경우 항상 `"meta"` 값입니다.

<br>

### ID3 metadata (meta)

이 이벤트는 **비디오의 초기 메타데이터가 로드되었을 때** 트리거됩니다.

```json
{
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
```

<br>

- **duration** (number)

  - 미디어 자산(media asset)의 전체 재생 길이(초 단위)

- **height** (number)

  - 미디어 자산의 세로 해상도

- **metadataType** (string)

  - `meta` 이벤트의 하위 분류(subcategory)
  - 이 이벤트의 경우 항상 `"media"` 값입니다.

- **seekRange** (object)

  - 라이브 스트림 또는 DVR 재생 시 버퍼링 또는 탐색(seeking)이 가능한 비디오의 시간 범위를 나타내는 객체
  - 참고: [ID3 seekRange (meta)](#id3-seekrange-meta)

- **type** (string)

  - 플레이어 이벤트의 상위 카테고리<br><br>이 이벤트의 경우 항상 `"meta"` 값입니다.

- **width** (number)

  - 미디어 자산의 가로 해상도

<br>

### ID3 seekRange (meta)

- **end** (number)

  - 스트림의 `currentTime` 기준으로 한 시간 범위의 종료 시점(초 단위)

- **start** (number)

  - 스트림의 `currentTime` 기준으로 한 시간 범위의 시작 시점(초 단위)

<br><br>

## Program-date-time (meta)

이 이벤트는 **재생이 `#EXT-X-PROGRAM-DATE-TIME` 태그가 포함된 HLS 스트림 구간에 진입할 때** 트리거됩니다.

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

<br>

- **metadata** (object)

  - HLS `#EXT-X-PROGRAM-DATE-TIME` 태그와 관련된 모든 정보를 포함하는 객체입니다.
  - 참고: [Program-date-time metadata (meta)](#program-date-time-metadata-meta)

- **metadataTime** (number)

  - 메타데이터 큐(cue)의 시작 시점(초 단위)

- **metadataType** (string)

  - `meta` 이벤트의 하위 분류(subcategory)
  - 이 이벤트의 경우 항상 `"program-date-time"` 값입니다.

- **programDateTime** (string)

  - 프로그램 메타데이터의 날짜 및 시간 (UTC 기준)

- **type** (string)

  - 플레이어 이벤트의 상위 카테고리
  - 이 이벤트의 경우 항상 `"meta"` 값입니다.

<br>

### Program-date-time metadata (meta)

- **end** (number)

  - 스트림의 `currentTime` 기준으로 한 큐(cue)의 종료 시점(초 단위)

- **programDateTime** (string)

  - 프로그램 메타데이터의 날짜 및 시간 (UTC 기준)

- **start** (number)

  - 스트림의 `currentTime` 기준으로 한 큐(cue)의 시작 시점(초 단위)

<br><br>

## SCTE-35 (meta)

이 이벤트는 **재생이 `#EXT-X-CUE-OUT` 또는 `#EXT-X-CUE-IN` 태그가 포함된 HLS 스트림 구간에 진입할 때** 트리거됩니다.

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

<br>

- **metadata** (object)

  - HLS `#EXT-X-CUE-OUT`, `#EXT-X-CUE-IN` 태그와 관련된 모든 정보를 포함하는 객체입니다.
  - 참고: [SCTE-35 metadata (meta)](#scte-35-metadata-meta)

- **metadataTime** (number)

  - 메타데이터 큐(cue)의 시작 시점(초 단위)

- **metadataType** (string)

  - `meta` 이벤트의 하위 분류(subcategory)
  - 이 이벤트의 경우 항상 `"scte-35"` 값입니다.

- **type** (string)

  - 플레이어 이벤트의 상위 카테고리
  - 이 이벤트의 경우 항상 `"meta"` 값입니다.

<br>

### SCTE-35 metadata (meta)

- **content** (string)

  - HLS 매니페스트 태그 뒤에 이어지는 콘텐츠 내용

- **end** (number)

  - 스트림의 `currentTime` 기준으로 한 큐(cue)의 종료 시점(초 단위)

- **start** (number)

  - 스트림의 `currentTime` 기준으로 한 큐(cue)의 시작 시점(초 단위)

- **tag** (string)

  - HLS 매니페스트 태그의 이름
  - 이 이벤트의 경우 항상 `"EXT-X-CUE-OUT"` 또는 `"EXT-X-CUE-IN"` 값입니다.

<br><br>

## Unknown (meta)

이 이벤트는 **서드파티 미디어 공급자(third-party media provider)** 또는 **레거시 Flash**에서 발생한 `meta` 이벤트임을 나타냅니다.

```json
{
  "type": "meta",
  "metadataType": "unknown",
  ...
}
```

<br>

- **metadataType** (string)

  - `meta` 이벤트의 하위 분류(subcategory)
  - 이 이벤트의 경우 항상 `"unknown"` 값입니다.

- **type** (string)

  - 플레이어 이벤트의 상위 카테고리
  - 이 이벤트의 경우 항상 `"meta"` 값입니다.

<br><br>

---

# .on('metadataCueParsed')

이 이벤트는 **메타데이터 큐 포인트(metadata cue point)가 버퍼링될 때** 트리거됩니다.

<br>

## Date range (metadataCueParsed)

플레이어가 `#EXT-X-DATERANGE` 태그가 포함된 **HLS 스트림의 특정 구간을 버퍼링할 때** 발생합니다.

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

<br>

- **metadata** (object)

  - HLS `#EXT-X-DATERANGE` 태그와 관련된 모든 정보를 포함하는 객체입니다.
  - 참고: [Date range metadata (metadataCueParsed)](#date-range-metadata-metadatacueparsed)

- **metadataTime** (number)

  - 메타데이터 큐(cue)의 시작 시점(초 단위)

- **metadataType** (string)

  - `metadataCueParsed` 이벤트의 하위 분류(subcategory)
  - 이 이벤트의 경우 항상 `"date-range"` 값입니다.

- **type** (string)

  - 플레이어 이벤트의 상위 카테고리
  - 이 이벤트의 경우 항상 `"meta"` 값입니다.

<br>

### Date range metadata (metadataCueParsed)

- **attributes** (array)

  - `EXT-X-DATERANGE`의 속성 목록

- **content** (string)

  - HLS 태그 뒤에 이어지는 콘텐츠

- **duration** (number)

  - `EXT-X-DATERANGE`의 지속 시간

- **end** (number)

  - 스트림의 `currentTime` 기준으로 한 큐(cue)의 종료 시점(초 단위)

- **endDate** (string)

  - `EXT-X-DATERANGE` 종료 시점의 UTC 날짜

- **start** (number)

  - 스트림의 `currentTime` 기준으로 한 큐(cue)의 시작 시점(초 단위)

- **startDate** (string)

  - `EXT-X-DATERANGE` 시작 시점의 UTC 날짜

- **tag** (string)

  - HLS 매니페스트 태그 이름
  - 이 이벤트의 경우 항상 `"EXT-X-DATERANGE"` 값입니다.

<br><br>

## EMSG (metadataCueParsed)

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

<br>

- **metadata** (object)

  - HLS `#EXT-X-DATERANGE` 태그와 관련된 모든 정보를 포함하는 객체입니다.
  - 참고: [EMSG metadata (metadataCueParsed)](#emsg-metadata-metadatacueparsed)

- **metadataTime** (number)

  - 메타데이터 큐(cue)의 시작 시점(초 단위)

- **metadataType** (string)

  - `metadataCueParsed` 이벤트의 하위 분류(subcategory)
  - 이 이벤트의 경우 항상 `"emsg"` 값입니다.

- **type** (string)

  - 플레이어 이벤트의 상위 카테고리
  - 이 이벤트의 경우 항상 `"metadataCueParsed"` 값입니다.

<br>

### EMSG metadata (metadataCueParsed)

- **duration** (number)

  - `EXT-X-DATERANGE`의 지속 시간

- **end** (number)

  - 스트림의 `currentTime` 기준으로 한 큐(cue)의 종료 시점(초 단위)

- **id** (number)

  - 메시지 인스턴스를 식별하는 필드

- **messageData** (array)

  - 메시지의 본문(body) 데이터

- **metadataType** (string)

  - `metadataCueParsed` 이벤트의 하위 분류(subcategory)
  - 이 이벤트의 경우 항상 `"emsg"` 값입니다.

- **presentationTimeOffset** (number)

  - 이벤트가 시작되는 오프셋(offset), 이 이벤트가 포함된 세그먼트의 시작점 기준 (단위: timescale)

- **start** (number)

  - 스트림의 `currentTime` 기준으로 한 큐(cue)의 시작 시점(초 단위)

- **schemeIdUri** (string)

  - 메시지 스킴(scheme)을 식별합니다.

- **timescale** (number)

  - 초당 틱 수 단위로 timescale 값을 제공합니다.

<br><br>

## ID3 (metadataCueParsed)

**HLS 스트림에 포함된 ID3 태그 구간이 버퍼링될 때** 트리거됩니다.

```json
{
  "type": "metadataCueParsed",
  "metadataType": "id3",
  "metadataTime": 0,
  "metadata": {
    ...
  }
}
```

<br>

- **metadata** (object)

  - HLS ID3 태그와 관련된 모든 정보를 포함하는 객체입니다.
  - 참고: [ID3 metadata (metadataCueParsed)](#id3-metadata-metadatacueparsed)

- **metadataTime** (number)

  - 메타데이터 큐(cue)의 시작 시점(초 단위)

- **metadataType** (string)

  - `metadataCueParsed` 이벤트의 하위 분류(subcategory)
  - 이 이벤트의 경우 항상 `"id3"` 값입니다.

- **type** (string)

  - 플레이어 이벤트의 상위 카테고리
  - 이 이벤트의 경우 항상 `"metadataCueParsed"` 값입니다.

<br>

### ID3 metadata (metadataCueParsed)

**비디오의 초기 메타데이터가 로드될 때** 발생합니다.

```json
{
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
```

<br>

- **duration** (number)

  - 미디어 에셋의 전체 재생 길이

- **height** (number)

  - 미디어의 세로 해상도

- **metadataType** (string)

  - `metadataCueParsed` 이벤트의 하위 분류(subcategory)
  - 이 이벤트의 경우 항상 `"media"` 값입니다.

- **seekRange** (object)

  - 라이브 스트림 또는 DVR에서 버퍼링되거나 탐색 가능한 비디오의 시간 범위
  - 참고: [ID3 seekRange (metadataCueParsed)](#id3-seekrange-metadatacueparsed)

- **type** (string)

  - 플레이어 이벤트의 상위 카테고리
  - 이 이벤트의 경우 항상 `"metadataCueParsed"` 값입니다.

- **width** (number)

  - 미디어의 가로 해상도

<br>

### ID3 seekRange (metadataCueParsed)

- **end** (number)

  - 스트림의 `currentTime` 기준으로 한 시간 범위의 종료 시점(초 단위)

- **start** (number)

  - 스트림의 `currentTime` 기준으로 한 시간 범위의 시작 시점(초 단위)

<br><br>

## Program-date-time (metadataCueParsed)

플레이어가 `#EXT-X-PROGRAM-DATE-TIME` 태그가 포함된 **HLS 스트림의 특정 구간을 버퍼링할 때** 트리거됩니다.

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

<br>

- **metadata** (object)

  - HLS `#EXT-X-PROGRAM-DATE-TIME` 태그와 관련된 모든 정보를 포함하는 객체입니다.
  - 참고: [Program-date-time metadata (metadataCueParsed)](#program-date-time-metadata-metadatacueparsed)

- **metadataTime** (number)

  - 메타데이터 큐(cue)의 시작 시점(초 단위)

- **metadataType** (string)

  - `metadataCueParsed` 이벤트의 하위 분류(subcategory)
  - 이 이벤트의 경우 항상 `"program-date-time"` 값입니다.

- **programDateTime** (string)

  - UTC 기준 프로그램 메타데이터의 날짜와 시간

- **type** (string)

  - 플레이어 이벤트의 상위 카테고리
  - 이 이벤트의 경우 항상 `"metadataCueParsed"` 값입니다.

<br>

### Program-date-time metadata (metadataCueParsed)

- **end** (number)

  - 스트림의 `currentTime` 기준으로 한 큐(cue)의 종료 시점(초 단위)

- **programDateTime** (string)

  - UTC 기준 프로그램 메타데이터의 날짜와 시간

- **start** (number)

  - 스트림의 `currentTime` 기준으로 한 큐(cue)의 시작 시점(초 단위)

<br><br>

## SCTE-35 metadata

플레이어가 `#EXT-X-CUE-OUT`, `#EXT-X-CUE-IN` 태그가 포함된 **HLS 스트림의 구간을 버퍼링할 때** 트리거됩니다.

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

<br>

- **metadata** (object)

  - HLS `#EXT-X-CUE-OUT`, `#EXT-X-CUE-IN` 태그와 관련된 모든 정보를 포함하는 객체입니다.
  - 참고: [SCTE-35 metadata (metadataCueParsed)](#scte-35-metadata-metadatacueparsed)

- **metadataTime** (number)

  - 메타데이터 큐(cue)의 시작 시점(초 단위)

- **metadataType** (string)

  - `metadataCueParsed` 이벤트의 하위 분류(subcategory)
  - 이 이벤트의 경우 항상 `"scte-35"` 값입니다.

- **type** (string)

  - 플레이어 이벤트의 상위 카테고리
  - 이 이벤트의 경우 항상 `"metadataCueParsed"` 값입니다.

<br>

### SCTE-35 metadata (metadataCueParsed)

- **content** (string)

  - HLS 매니페스트 태그 이후의 내용

- **end** (number)

  - 스트림의 `currentTime` 기준으로 한 큐(cue)의 종료 시점(초 단위)

- **start** (number)

  - 스트림의 `currentTime` 기준으로 한 큐(cue)의 시작 시점(초 단위)

- **tag** (string)

  - HLS 매니페스트 태그의 이름
  - 이 이벤트의 경우 항상 `"EXT-X-CUE-OUT"` 또는 `"EXT-X-CUE-IN"` 값입니다.
