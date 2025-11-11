# Playback Events

이 API 호출은 플레이어의 **현재 재생 상태를 가져오거나 변경**하는 데 사용됩니다.

<br>
<br>

## .on('autostartNotAllowed')

플레이어가 **자동 재생(autostart)** 으로 설정되어 있지만, **브라우저 설정으로 인해 자동 재생이 차단될 때** 트리거됩니다.

```json
{
  "code": 303220,
  "error": "The play method is not allowed by the user agent or the platform in the current context, possibly because the user denied permission.",
  "reason": "autoplayDisabled",
  "type": "autostartNotAllowed"
}
```

- **code** (number)

  - 오류 식별자(Identifier)
  - 참조: _Player Errors Reference (Web Player)_

- **error** (string)

  - 감지된 경고에 대한 텍스트 메시지
  - 참조: _Player Errors Reference (Web Player)_

- **reason** (string)

  - 플레이어가 자동 재생을 시작할 수 없었던 이유
  - 이 값은 항상 `"autoplayDisabled"` 입니다.

- **type** (string)

  - 플레이어 이벤트의 카테고리
  - 이 값은 항상 `"autostartNotAllowed"` 입니다.

<br>
<br>

## .on('buffer')

다음 이벤트 중 하나가 발생할 때 트리거됩니다:

- 플레이어가 **재생을 시작할 때**
- 플레이어가 **버퍼링 상태에 진입할 때**

```json
{
  "type": "buffer",
  "newstate": "buffering",
  "oldstate": "idle",
  "reason": "loading"
}
```

- **newstate** (string)

  - 플레이어가 이동한 새 상태
  - 가능한 값:
    - `buffering`
    - `idle`
    - `playing`
    - `paused`
    - `stalled`

- **oldstate** (string)

  - 플레이어가 이동하기 전의 이전 상태
  - 가능한 값:
    - `buffering`
    - `idle`
    - `playing`
    - `paused`
    - `stalled`

- **reason** (string)

  - 버퍼 이벤트가 발생한 이유
  - 가능한 값:
    - `paused`
    - `loading`
    - `complete`
    - `stalled`
    - `error`

- **type** (string)

  - 이벤트의 카테고리
  - 이 값은 항상 `"buffer"`

<br>
<br>

## .on('complete')

비디오의 **끝에 도달했을 때** 트리거됩니다.  
단, 다음의 경우에는 트리거되지 않습니다:

- 시청자가 비디오가 끝나기 전에 **플레이리스트 항목을 건너뛸 때**
- `next()` 메서드가 호출될 때

```json
{
  "type": "complete"
}
```

- **type** (string)

  - 이벤트의 카테고리
  - 이 값은 항상 `"complete"`

<br>
<br>

## .on('error')

재생 과정에서 **치명적인 오류가 발생했을 때** 트리거됩니다.

```json
{
  "code": 224003,
  "sourceError": {},
  "type": "error",
  "message": "This video file cannot be played."
}
```

- **code** (number)

  - 오류 식별자(Identifier)
  - 참조: _Player Errors Reference (Web Player)_

- **message** (string)

  - 감지된 경고 메시지 텍스트
  - 참조: _Player Errors Reference (Web Player)_

- **sourceError** (object \| null)

  - 플레이어가 포착한 하위 수준의 오류나 이벤트 객체로, 해당 오류의 원인이 된 요소

- **type** (string)

  - 플레이어 이벤트의 카테고리
  - 이 값은 항상 `"error"`

<br>
<br>

## .on('firstFrame')

비디오의 **첫 번째 프레임이 표시되거나, 오디오 파일이 재생을 시작하는 순간** 트리거됩니다.

이 이벤트는 사용자가 **재생 버튼을 누른 시점부터 실제 콘텐츠가 표시되기까지의 시간**을 측정하는 데 사용됩니다.  
즉, 콘텐츠 재생이 **정확히 시작되는 시점**을 나타냅니다.

```json
{
  "loadTime": 118,
  "type": "firstFrame"
}
```

- **loadTime** (number)

  - 플레이어가 **재생 시도(play attempt)** 에서 **첫 프레임(firstFrame)** 이벤트로 전환되는 데 걸린 시간(밀리초 단위)

- **type** (string)

  - 플레이어 이벤트의 카테고리
  - 이 값은 항상 `"firstFrame"`

<br>
<br>

## .on('idle')

플레이어가 **idle(대기)** 상태로 전환될 때 트리거됩니다.

```json
{
  "type": "idle",
  "newstate": "idle",
  "oldstate": "playing",
  "reason": "complete"
}
```

- **newstate** (string)

  - 플레이어가 이동한 새로운 상태
  - 가능한 값:
    - `idle`
    - `playing`
    - `paused`

- **oldstate** (string)

  - 플레이어가 이동하기 전의 이전 상태
  - 가능한 값:
    - `buffering`
    - `playing`
    - `paused`

- **reason** (string)

  - `idle` 이벤트가 발생한 이유
  - 가능한 값:
    - `complete`
    - `idle`
    - `error`
    - `pause`

- **type** (string)

  - 이벤트의 카테고리
  - 가능한 값:
    - `idle`

<br>
<br>

## .on('pause')

플레이어 내의 **광고가 아닌 미디어 콘텐츠가 일시정지될 때** 트리거됩니다.

> 광고 구간의 일시정지 이벤트를 감지하려면 [`.on('adPause')`](https://docs.jwplayer.com/players/reference/advertising-events#onadpause)를 사용하십시오.

```json
{
  "type": "pause",
  "newstate": "paused",
  "oldstate": "playing",
  "reason": "paused",
  "pauseReason": "interaction",
  "viewable": 1
}
```

- **newstate** (string)

  - 플레이어가 이동한 새로운 상태
  - 가능한 값:
    - `buffering`
    - `idle`
    - `loading`
    - `paused`
    - `playing`
    - `stalled`

- **oldstate** (string)

  - 플레이어가 이동하기 전의 이전 상태
  - 가능한 값:
    - `buffering`
    - `idle`
    - `loading`
    - `paused`
    - `playing`
    - `stalled`

- **pauseReason** (string)

  - 일시정지의 원인
  - 가능한 값:
    - `external`
    - `interaction`
    - `viewable`

- **reason** (string)

  - `pause` 이벤트가 발생한 이유
  - 가능한 값:
    - `autostart`
    - `clickthrough`
    - `external`
    - `interaction`
    - `paused`

- **type** (string)

  - 이벤트의 카테고리
  - 가능한 값:
    - `pause`

- **viewable** (number)

  - 플레이어가 **화면에 보이는 상태인지 여부**
  - 가능한 값:
    - `1` (보임)
    - `0` (보이지 않음)

<br>
<br>

## .on('play')

플레이어 내의 **광고가 아닌 미디어 콘텐츠가 재생을 시작할 때** 트리거됩니다.

광고 구간의 재생 이벤트를 감지하려면 [`.on('adPlay')`](https://docs.jwplayer.com/players/reference/advertising-events#onadplay).를 사용하십시오.

```json
{
  "type": "play",
  "newstate": "playing",
  "oldstate": "buffering",
  "reason": "playing",
  "playReason": "interaction",
  "viewable": 1
}
```

- **newstate** (string)

  - 플레이어가 이동한 새로운 상태
  - 가능한 값:
    - `buffering`
    - `idle`
    - `loading`
    - `paused`
    - `playing`
    - `stalled`

- **oldstate** (string)

  - 플레이어가 이동하기 전의 이전 상태
  - 가능한 값:
    - `buffering`
    - `idle`
    - `loading`
    - `paused`
    - `playing`
    - `stalled`

- **playReason** (string)

  - 재생이 시작된 이유
  - 가능한 값:
    - `autostart`
    - `external` (API 사용)
    - `interaction` (클릭, 터치, 키보드 입력)
    - `playlist` (자동 다음 재생)
    - `related-audio` (JW 추천 오디오 플레이리스트의 자동 재생)
    - `related-interaction` (JW 추천 플레이리스트 항목으로의 사용자 이동)

- **reason** (string)

  - `play` 이벤트가 발생한 원인
  - 가능한 값:
    - `autostart`
    - `clickthrough`
    - `external`
    - `interaction`

- **type** (string)

  - 이벤트의 카테고리
  - 이 값은 항상 `"play"`

- **viewable** (number)

  - 플레이어가 화면에 표시되는 상태인지 여부
  - 가능한 값:
    - `1` (보임)
    - `0` (보이지 않음)

<br>
<br>

## .on('playAttemptFailed')

**재생이 중단되거나 차단될 때** 트리거됩니다.

실패한 재생 시도는 실제 재생으로 이어지지 않습니다.
비디오를 일시정지하거나 미디어를 변경하면 재생 시도가 **중단(aborted)** 됩니다.
모바일 브라우저에서는 **사용자 제스처 없이 시작된 재생 시도**가 차단됩니다.

```json
{
    "code": 303230,
    "sourceError": {},
    "error": {},
    "item": {...},
    "playReason": "interaction",
    "type": "playAttemptFailed"
}
```

- **code** (number)

  - 오류 식별자(Identifier)
  - 참조: _Player Errors Reference (Web Player)_

- **error** (object)

  - `play` 프라미스(promise)에서 발생한 오류 객체

- **item** (object)

  - 현재 재생 중이거나 재생 시도된 **플레이리스트 아이템의 모든 속성**

- **playReason** (string)

  - 재생 시도가 발생한 이유
  - 가능한 값:
    - `api`
    - `custom`
    - `external`
    - `interaction`

- **sourceError** (object \| null)

  - 플레이어가 포착한 **하위 수준 오류나 이벤트 객체**로, 해당 오류의 원인이 된 요소

- **type** (string)

  - 플레이어 이벤트의 카테고리
  - 이 값은 항상 `"playAttemptFailed"`

<br>
<br>

## .on('playbackRateChanged')

재생 속도가 **변경되었을 때** 트리거됩니다.

```json
{
  "playbackRate": 1.25,
  "type": "playbackRateChanged"
}
```

- **playbackRate** (number)

  - 새롭게 설정된 재생 속도

- **type** (string)

  - 플레이어 이벤트의 카테고리
  - 이 값은 항상 `"playbackRateChanged"`

<br>
<br>

## .on('warning')

**설정(setup)** 또는 **재생 과정(playback process)** 에 치명적이지 않은 오류가 발생했을 때 트리거됩니다.

```json
{
  "code": 305001,
  "sourceError": {
    "stack": "Error: Failed to load https://playertest-cdn.longtailvideo.com/assets/404/jwpsrv.js\n    at scriptTag.onerror (https://player-develop-test-jenkins.longtailvideo.com/builds/lastSuccessfulBuild/archive/bin-debug/jwplayer.js:8349:30)",
    "message": "Failed to load https://playertest-cdn.longtailvideo.com/assets/404/jwpsrv.js"
  },
  "type": "warning"
}
```

- **code** (number)

  - 오류 식별자(Identifier)
  - 참조: _Player Errors Reference (Web Player)_

- **sourceError** (object \| null)

  - 플레이어가 포착한 **하위 수준의 오류나 이벤트 객체**, 해당 오류를 유발한 원인을 포함
  - 참조: _sourceError_

- **sourceError.message** (string)

  - 감지된 경고 메시지 텍스트
  - 참조: _Player Errors Reference (Web Player)_

- **sourceError.stack** (string)

  - 발생한 오류의 **스택 트레이스(stack trace)**
  - 이 값은 오류가 발생하기까지의 **함수 호출 및 이벤트의 순서를 상세히 보여줍니다.**

- **type** (string)

  - 플레이어 이벤트의 카테고리
  - 이 값은 항상 `"warning"`
