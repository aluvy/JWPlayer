# Captions Events

이 API 호출은 비디오에 하나 이상의 클로즈드 캡션(Closed Captions) 트랙이 제공될 때, **활성 자막 트랙을 감지하거나 업데이트**하는 데 사용됩니다.  
JavaScript API를 사용하여 자막 사용 내역을 기록하거나, **JW Player 외부에 독자적인 CC(자막) 메뉴를 구성**할 수 있습니다.  
또한 `setCaptions()`을 사용하면 **플레이어를 다시 로드하지 않고 자막 스타일을 동적으로 설정**할 수도 있습니다.

> 인덱스 값이 `0`이면 자막이 꺼져(off) 있음을 의미합니다.

<br>
<br>

## .on('captionsList')

사용 가능한 자막 트랙 목록이 변경될 때 트리거됩니다.

이 이벤트는 API를 사용하여 **기본 자막을 설정하기에 이상적인 시점**입니다.

> `captionsList`는 항상 최소 1개의 항목(`off`)을 반환하지만, 자막이 완전히 로드되면 한 번 더 트리거됩니다.  
> 따라서 `tracks` 배열의 길이가 1을 초과할 때만 자막을 변경하는 것이 좋습니다.

```json
{
  "tracks": [
    {
      "id": "off",
      "label": "Off"
    },
    {
      "id": "//playertest-cdn.longtailvideo.com/assets/os/captions/bunny-en.srt",
      "label": "English",
      "language": "en"
    }
  ],
  "track": 0,
  "type": "captionsList"
}
```

- **track** (number)

  - 활성화된 자막 트랙의 인덱스

- **tracks** (array)

  - 포함된 모든 자막 트랙 목록
  - **tracks[]**

    - **tracks.id** (string \| number)

      - **(외부 자막: .srt, .vtt)** 외부 자막 파일의 URL
      - **(내장 자막: .vtt, CEA-608, CEA-708)** 자막의 인덱스
      - 첫 번째 `tracks[]` 객체의 `id` 값은 항상 `"off"`이며, 첫 번째 트랙은 자막이 꺼져 있음을 나타냅니다.

    - **tracks.label** (string)

      - **(외부 자막)** 플레이어 설정에서 구성된 라벨
      - **(내장 자막)** 내장된 자막 내부에 지정된 라벨
      - 첫 번째 `tracks[]` 객체의 `label` 값은 항상 `"Off"`이며, 첫 번째 트랙은 자막이 꺼져 있음을 나타냅니다.

    - **tracks.language** (string)

      - 자막 트랙의 두 글자 언어 약어 (예: `"en"`, `"ko"`)

- **type** (string)

  - 플레이어 이벤트의 카테고리 (이 이벤트의 경우 항상 `captionsList`)

<br>
<br>

## .on('captionsChanged')

활성 자막 트랙이 **수동으로** 또는 **API를 통해** 변경될 때마다 트리거됩니다.

```json
{
  "tracks": [
    {
      "id": "off",
      "label": "Off"
    },
    {
      "id": "//playertest-cdn.longtailvideo.com/assets/os/captions/bunny-en.srt",
      "label": "English",
      "language": "en"
    }
  ],
  "track": 0,
  "type": "captionsChanged"
}
```

- **track** (number)

  - 새로 활성화된 자막 트랙의 인덱스

- **tracks** (array)

  - 포함된 모든 자막 트랙 목록
  - **tracks[]**

    - **tracks.id** (string \| number)

      - **(외부 자막: .srt, .vtt)** 외부 자막 파일의 URL
      - **(내장 자막: .vtt, CEA-608, CEA-708)** 자막의 인덱스
      - 첫 번째 `tracks[]` 객체의 `id` 값은 항상 `"off"`이며, 첫 번째 트랙은 자막이 꺼져 있음을 나타냅니다.

    - **tracks.label** (string)

      - **(외부 자막)** 플레이어 설정에서 구성된 라벨
      - **(내장 자막)** 내장된 자막 내부에서 지정된 라벨
      - 첫 번째 `tracks[]` 객체의 `label` 값은 항상 `"Off"`이며, 첫 번째 트랙은 자막이 꺼져 있음을 나타냅니다.

    - **tracks.language** (string)

      - 자막 트랙의 두 글자 언어 약어 (예: `"en"`, `"ko"`)

- **type** (string)

  - 플레이어 이벤트의 카테고리
  - 이 이벤트의 경우 항상 `"captionsChanged"`
