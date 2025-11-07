# Audio Tracks

비디오에 **여러 오디오 트랙이 제공된 경우**,  
이 API 호출들을 사용하여 오디오 트랙을 **조회하거나 변경**할 수 있습니다.

<br>
<br>

## Endpoints

- **.getAudioTracks()**

  - 사용 가능한 오디오 트랙 목록을 가져옵니다.

- **.getCurrentAudioTrack()**

  - 현재 활성화된 오디오 트랙의 인덱스를 반환합니다.

- **.setCurrentAudioTrack(index)**

  - 지정한 인덱스의 오디오 트랙을 활성화합니다.

<br>

## Events

- **.on('audioTracks')**

  - 사용 가능한 오디오 트랙 목록이 업데이트될 때 트리거됩니다.
  - 이 이벤트는 **플레이리스트 아이템이 재생을 시작한 직후** 발생합니다.

- **.on('audioTrackChanged')**

  - 활성 오디오 트랙이 변경될 때 트리거됩니다.
  - 이 이벤트는 사용자가 **오디오 트랙 메뉴를 클릭하거나**, 또는 스크립트에서 **setCurrentAudioTrack()** 을 호출했을 때 발생합니다.

<br><br>

---

## getAudioTracks()

### Returns

- **array**
  - 각 오디오 트랙 객체의 배열을 반환합니다.
  - 이 정보는 **M3U8 매니페스트**에 명시된 내용을 기반으로 합니다.

<br>

### Value

- **autoselect** (boolean)

  - 명시적인 선택이 이루어지지 않은 경우, **시스템 언어를 기반으로 자동 선택**될 수 있습니다.

- **defaulttrack** (boolean)

  - 기본적으로 선택되어야 하는 트랙일 경우 `true`를 반환합니다.

- **language** (string)

  - 선택된 오디오 트랙의 **두 글자 언어 코드**

- **name** (string)

  - 선택된 오디오 트랙의 **표시 이름**

<br><br>

---

## getCurrentAudioTrack()

### Returns

- **number**

  - 현재 활성화된 오디오 트랙의 인덱스를 반환합니다.
  - 대체 오디오 트랙이 없는 경우 **`-1`** 을 반환합니다.

<br><br>

---

## setCurrentAudioTrack(index)

- **index\*** (number)
  - 지정된 인덱스로 **오디오 트랙을 변경**합니다.

<br><br>

---

# Audio Tracks Events

이 API 호출들은 비디오에 여러 개의 오디오 트랙이 제공될 때, 해당 오디오 트랙을 감지하거나 변경하는 데 사용됩니다.

<br>

## .on('audioTracks')

사용 가능한 오디오 트랙 목록이 업데이트될 때 발생합니다.  
이는 **플레이리스트 항목이 재생을 시작한 직후**에 발생합니다.

```json
{
  "tracks": [
    {
      "autoselect": true,
      "defaulttrack": true,
      "groupid": "aac",
      "language": "en",
      "name": "English",
      "hlsjsIndex": 0
    },
    {
      "autoselect": true,
      "defaulttrack": false,
      "groupid": "aac",
      "language": "sp",
      "name": "Spanish",
      "hlsjsIndex": 1
    },
    {
      "autoselect": false,
      "defaulttrack": false,
      "groupid": "aac",
      "language": "en",
      "name": "Commentary (eng)",
      "hlsjsIndex": 2
    }
  ],
  "currentTrack": 0,
  "type": "audioTracks"
}
```

- **currentTrack** (number)

  - 현재 활성화된 오디오 트랙의 인덱스

- **tracks** (array)

  - M3U8 매니페스트를 기반으로 한 트랙 정보
  - 참조: **tracks[]**

- **type** (string)

  - 플레이어 이벤트의 카테고리
  - 이 값은 항상 **audioTracks** 입니다.

<br>

### tracks[]

- **autoselect** (boolean)

  - 명시적인 선택이 없을 때, **시스템 언어를 기반으로 자동 선택**될 수 있음

- **defaulttrack** (boolean)

  - 기본적으로 선택되어야 하는 트랙인지 여부를 식별

- **groupid** (string)

  - 동일한 콘텐츠의 여러 오디오 버전을 관리하기 위해 HLS에서 사용되는 **오디오 그룹 식별자**

- **hlsjsIndex** (number)

  - `hls.js`가 오디오 트랙을 추적하기 위해 사용하는 **0부터 시작하는 내부 식별자**

- **language** (string)

  - 오디오 트랙의 **두 글자 언어 코드**

- **name** (string)

  - 오디오 트랙에 지정된 **레이블(이름)**

<br><br>

---

## .on('audioTrackChanged')

활성화된 오디오 트랙이 변경될 때 발생합니다.  
이 이벤트는 사용자가 트랙을 직접 선택하거나,  
스크립트에서 `setCurrentAudioTrack()` 메서드를 호출했을 때 발생합니다.

```json
{
  "tracks": [
    {
      "autoselect": true,
      "defaulttrack": true,
      "groupid": "aac",
      "language": "en",
      "name": "English",
      "hlsjsIndex": 0
    },
    {
      "autoselect": true,
      "defaulttrack": false,
      "groupid": "aac",
      "language": "sp",
      "name": "Spanish",
      "hlsjsIndex": 1
    },
    {
      "autoselect": false,
      "defaulttrack": false,
      "groupid": "aac",
      "language": "en",
      "name": "Commentary (eng)",
      "hlsjsIndex": 2
    }
  ],
  "currentTrack": 1,
  "type": "audioTrackChanged"
}
```

- **currentTrack** (number)

  - 현재 활성화된 오디오 트랙의 인덱스

- **tracks** (array)

  - M3U8 매니페스트를 기반으로 한 트랙 정보
  - 참조: **tracks[]**

- **type** (string)

  - 플레이어 이벤트의 카테고리
  - 이 값은 항상 **audioTrackChanged** 입니다.

<br>

### tracks[]

- **autoselect** (boolean)

  - 명시적 선호가 설정되지 않은 경우, 시스템 언어를 기준으로 자동 선택될 수 있음

- **defaulttrack** (boolean)

  - 기본적으로 선택되어야 하는 트랙인지 여부를 식별

- **groupid** (string)

  - 동일한 콘텐츠의 여러 오디오 버전을 관리하기 위해 HLS에서 사용되는 **오디오 그룹 식별자**

- **hlsjsIndex** (number)

  - `hls.js`가 오디오 트랙을 추적하기 위해 사용하는 **0부터 시작하는 내부 식별자**

- **language** (string)

  - 오디오 트랙의 **두 글자 언어 코드**

- **name** (string)

  - 오디오 트랙에 지정된 **레이블(이름)**
