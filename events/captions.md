# Captions Events

These API calls are used to listen to or update the active captions track if one or more closed captions tracks are provided with a video. The JavaScript API can be used to log captions usage or build your own CC menu outside JW Player. It is also possible to set caption styles dynamically using setCaptions() without having to reload the player.

> An index of 0 implies that captions are off.

<br>
<br>

# .on('captionsChanged')

Triggers whenever the active captions track is changed manually or via API

### 호출시점

- 사용자가 **자막 트랙을 변경했을 때 발생**합니다.
- 또는 API (`setCurrentCaptions(id)`)로 자막을 바꿨을 때 호출됩니다.
- 새 아이템(playlist item)이 로드되면서 자막 트랙이 초기화될 때에도 한 번 발생할 수 있습니다.

### 이벤트 객체 구조 (콜백 파라미터)

```json
{
  "tracks": [
    { "id": 0, "label": "Off" },
    { "id": 1, "label": "English" },
    { "id": 2, "label": "Korean" }
  ],
  "currentTrack": 2,
  "type": "captionsChanged"
}
```

| Value                     | Description                                                |
| :------------------------ | :--------------------------------------------------------- |
| **tracks** (array)        | 현재 플레이어에 인식된 모든 자막 트랙 목록 (`id`, `label`) |
| **currentTrack** (number) | 현재 활성화된 자막 트랙의 ID (0 = Off)                     |

### 활용

#### 1. UI 업데이트

- 사용자가 자막을 바꿀 때마다 UI에 선택 상태 표시.

#### 2. 분석 / 로그 트래킹

- 사용자가 어떤 언어 자막을 얼마나 사용했는지 통계 수집.
- 특히 다국어 영상 플랫폼에서 자막 선택 패턴 분석에 활용.

#### 3. 다국어 UX 처리

- `captionsChanged` 발생 시 오디오 트랙과 동기화하거나,  
  별도 언어 UI (예: 오디오 코멘터리) 전환 로직 연동 가능.

### 주의사항

- `captionsChanged`는 **트랙 변경이 일어난 뒤** 호출됩니다.  
  → 현재 선택된 자막을 반영하는 최종 상태임.
- `captionsList` 이벤트와 구분 필요:
  - `captionsList`: 자막 트랙이 로드되거나 목록이 갱신될 때
  - `captionsChanged`: 사용자 선택 또는 API로 변경될 때
- 자막이 없는 영상에서는 이 이벤트가 발생하지 않습니다.
- 플레이리스트 전환 시 기존 자막 ID가 무효화될 수 있으므로,  
  `captionsList` 이후 `captionsChanged` 순서를 확인해야 합니다.
- 자동 번역 또는 YouTube 자막처럼 외부 자막 소스는 이 이벤트를 트리거하지 않을 수 있습니다.

### 특징

- **자막 트랙 선택 변경 시점의 유일한 이벤트.**
- UI와 트래킹 양쪽에 필수적으로 사용.
- `captionsList`와 항상 쌍으로 사용하면 안정적인 자막 제어 가능.
- 이벤트가 비교적 단순(데이터 용량 작음)하여 빈번하게 발생해도 성능 부담이 거의 없음.

<br>
<br>

# .on('captionsList')

Fires when the list of available captions tracks changes

This event is the ideal time to set default captions with the API.

> `captionsList` will always return an array of at least **1** item due to **off**, but will trigger again once captions are fully loaded. We suggest only changing captions when the tracks array length exceeds **1**.

### 호출시점

- 플레이어가 **사용 가능한 자막 트랙(caption tracks)을 처음 인식하거나 갱신했을 때** 발생합니다.
- 주로 다음 상황에서 호출됩니다:
  1. 새 영상이 로드되어 자막 트랙이 로드된 직후
  2. 외부 자막 파일(.vtt, .srt 등)이 동적으로 로드되었을 때
  3. 플레이리스트 아이템이 바뀌면서 자막 목록이 새로 세팅될 때
- 즉, “**현재 사용할 수 있는 자막 목록이 확정된 시점**” 에 호출됩니다.

### 이벤트 객체 구조 (콜백 파라미터)

```json
{
  "tracks": [
    { "id": 0, "label": "Off" },
    { "id": 1, "label": "English" },
    { "id": 2, "label": "Korean" }
  ],
  "currentTrack": 0,
  "type": "captionsList"
}
```

| Value                        | Description                                                   |
| :--------------------------- | :------------------------------------------------------------ |
| **tracks** (array)           | 사용 가능한 모든 자막 트랙 목록. 항상 `"Off"`(id: 0)가 포함됨 |
| **tracks[i].id** (number)    | 자막 트랙의 고유 ID (0은 Off 상태)                            |
| **tracks[i].label** (string) | 자막 표시 이름 (예: `"English"`, `"Korean"`)                  |
| **currentTrack** (number)    | 현재 선택된 자막 트랙의 ID (기본값: 0)                        |

### 활용

#### 1. 자막 선택 UI 생성

- 플레이어가 인식한 자막 목록을 기반으로 커스텀 자막 선택 UI 생성 가능.

#### 2. 자막 자동 선택 / 기본 설정

- 특정 언어(예: `"Korean"`)가 있을 경우 자동 활성화:

```javascript
player.on('captionsList', (e) => {
  const kr = e.tracks.find((t) => t.label === 'Korean');
  if (kr) player.setCurrentCaptions(kr.id);
});
```

#### 3. 접근성/다국어 처리

- 여러 언어 버전을 제공할 때, 사용자의 언어 설정에 맞춰 자막을 초기 적용 가능.

### 주의사항

- 자막이 없는 콘텐츠에서는 이 이벤트가 발생하지 않음.
- 자막 로딩이 네트워크 상태에 따라 지연되면, `ready` 이후가 아니라 나중에 발생할 수 있음.
- `captionsList`는 **목록이 확정된 시점**이므로, 자막 변경(`captionsChanged`) 시점과 혼동하지 말 것.
- 재생 중 자막 파일이 교체되면 이벤트가 다시 발생할 수 있으므로, 중복 UI 생성 방지 로직 필요.
- 일부 외부 스트리밍(YouTube, DRM 콘텐츠 등)에서는 `tracks` 배열이 비어 있을 수 있음.

### 특징

- **자막 시스템 초기화의 시작점** - 이 이벤트가 발생해야 자막 UI나 설정이 가능함.
- 항상 `"Off"` 트랙이 포함되어 있어, 자막 끄기 기능을 구현하기 용이.
- `captionsChanged` 이벤트와 쌍을 이뤄 자막 제어 전 주기 관리 가능:
  - `captionsList` → 자막 목록 확정
  - `captionsChanged` → 사용자 선택 변경
