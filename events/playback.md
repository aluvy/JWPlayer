# Playback Events

These API calls are used to retrieve and change the current playback state of the player.

<br>

## .on('autostartNotAllowed')

Fires when the player is configured to autostart but the browser's settings are preventing it

### 호출시점

- 플레이어 설정(`autostart: true`) 또는 `player.play()` 호출로 자동재생을 시도했으나,  
  **브라우저나 OS의 미디어 자동재생 정책에 의해 차단되었을 때** 발생합니다.
- 즉, **“재생 명령은 실행되었지만 실제 재생이 시작되지 못한 시점”**에 트리거됩니다.
- 모바일 Safari, Chrome 데스크톱, Android WebView 등에서 주로 발생합니다.

### 이벤트 객체 구조 (콜백 파라미터)

```json
{
  "code": 303220,
  "error": "The play method is not allowed by the user agent or the platform in the current context, possibly because the user denied permission.",
  "reason": "autoplayDisabled",
  "type": "autostartNotAllowed"
}
```

| Value               | Description                                  |
| :------------------ | :------------------------------------------- |
| **code** (number)   | JW Player 내부 오류 코드 (일반적으로 303220) |
| **error** (string)  | 브라우저 또는 플랫폼이 반환한 에러 메시지    |
| **reason** (string) | 차단 원인 (보통 `"autoplayDisabled"`)        |

### 활용

#### 1. 사용자 재생 유도 UI 표시

- 자동재생이 막힌 경우 “탭하여 재생” 오버레이 버튼을 띄워 사용자 행동으로 재생 유도.

#### 2. 자동재생 정책 모니터링

- 국가·브라우저별 자동재생 차단 비율 통계 수집.
- 광고 시스템 또는 AB 테스트에서 “자동재생 성공률” 지표로 활용.

#### 3. 대체 로직 적용

- 자동재생이 불가할 때 음소거 모드로 재시도 하거나 썸네일 재생으로 대체:

```javascript
player.on('autostartNotAllowed', () => {
  player.setMute(true);
  player.play(); // 음소거 상태로 재시도
});
```

### 주의사항

- `autostartNotAllowed` 가 발생하면 `beforePlay`, `play` 등 일부 재생 이벤트는 실행되지 않을 수 있습니다.
- 사용자 동작(클릭 또는 터치)이 없으면 브라우저가 음소거 여부와 상관없이 재생을 막을 수 있습니다.
- Chrome 정책상 일부 사이트는 Media Engagement Index (사용자 시청 빈도)에 따라 차단 해제 되기도 함.
- 광고 자동재생 환경에서는 이 이벤트가 자주 발생하므로, 로그 과다 적재 주의.
- 이벤트 후 재생 가능 상태로 돌리려면 사용자의 직접 인터랙션이 필요할 수 있습니다.

### 특징

- **자동재생 실패를 감지할 수 있는 유일한 이벤트.**
- 브라우저 정책 또는 OS 제약으로 재생이 시작되지 않았다는 명확한 신호를 제공.
- `autostart: true` 환경에서 반드시 구독해야 하는 보조 이벤트.
- 광고 프리롤 또는 앱 내 임베드 환경에서 **재생 정책 대응 코드의 트리거** 역할을 함.

<br>

## .on('buffer')

Fires when one of the following events occurs:

- Player starts playback
- Player enters a buffering state

### 호출시점

- 플레이어가 **재생 도중 데이터 로딩이 부족하여 일시적으로 멈출 때** 발생합니다.
- 즉, 미디어가 원활히 재생되다가 **버퍼링 상태로 전환되는 순간**(`playing → buffering`)에 트리거됩니다.
- 네트워크 속도 저하, 시킹(seek) 시점, 품질 변경(adaptive bitrate switch) 등으로 인해 데이터가 충분하지 않을 때 발생합니다.
- 대응 이벤트로 `bufferChange`(버퍼 진행률 변경)와 `play`(재생 재개)가 있습니다.

### 이벤트 객체 구조 (콜백 파라미터)

```json
{
  "oldstate": "playing",
  "newstate": "buffering",
  "reason": "buffering",
  "type": "buffer"
}
```

| Value                 | Description                                                                    |
| :-------------------- | :----------------------------------------------------------------------------- |
| **oldstate** (string) | 버퍼링 이전 상태 (`"playing"`, `"paused"`, `"idle"` 등)                        |
| **newstate** (string) | `"buffering"` 고정                                                             |
| **reason** (string)   | 버퍼링이 발생한 원인 (`"buffering"`, `"stalled"`, `"seek"`, `"qualityChange"`) |

### 활용

#### 1. 로딩 인디케이터(UI) 표시

- 버퍼링이 시작되면 로딩 스피너 표시, 재생 재개 시 숨김.

#### 2. 품질 모니터링 및 로그 분석

- `reason` 필드를 통해 버퍼 원인을 수집:
  - `"buffering"` → 일반 네트워크 지연
  - `"seek"` → 탐색 후 버퍼링
  - `"qualityChange"` → 스트리밍 품질 전환 시 발생
- 각 원인별 빈도를 로그로 기록해 스트리밍 품질 분석에 활용.

#### 3. UX 제어

- 특정 조건에서 버퍼링이 길어질 경우 재시도 로직 수행:

```javascript
let bufferTimer;
player.on('buffer', () => {
  bufferTimer = setTimeout(() => player.load(), 8000); // 8초 이상 멈춤 시 재시도
});
player.on('play', () => clearTimeout(bufferTimer));
```

### 주의사항

- `buffer` 이벤트는 **“상태 전환” 이벤트**로, 실제 버퍼 데이터 양을 알려주지 않습니다.  
  → 버퍼 진행률을 알고 싶다면 `bufferChange` 이벤트를 사용해야 합니다.
- 발생 빈도는 콘텐츠 종류(HLS/DASH/VOD)에 따라 다르며, 일부 라이브 스트림에서는 발생하지 않거나 짧게만 트리거될 수 있습니다.
- 네트워크 품질 저하나 사용자 시킹 등으로 인해 잦은 버퍼링이 일어날 수 있으므로,  
  UI 업데이트 시 **성능 영향 최소화** 필요.
- `buffer` → `play` → `buffer` 루프가 반복되면 사용자 경험 저하로 이어지므로,  
  발생 횟수/시간을 로깅해 진단하는 것이 좋습니다.

### 특징

- **플레이어 상태가 “버퍼링”으로 전환될 때만 발생하는 단발 이벤트.**
- `bufferChange`와 달리 발생 횟수는 상대적으로 적지만, **버퍼링 시작 타이밍**을 명확히 잡을 수 있음.
- `reason` 필드를 통해 “버퍼링 원인”을 구분할 수 있는 유일한 이벤트.
- `beforePlay`와 `beforeComplete`처럼 **상태 전환 이벤트 그룹** 중 하나로,
  재생 상태 플로우(`idle → playing → buffering → playing → complete`)를 구성하는 핵심 포인트.

<br>

## .on('complete')

Fires when the end of a video is reached, but not when one of the following occurs:

- The viewer advances playlist items prior to the end of a video.
- `next()` is called.

### 호출시점

- 플레이어가 **현재 재생 중인 영상의 마지막 프레임을 모두 재생 완료했을 때** 발생합니다.
- 즉, 콘텐츠 재생이 완전히 끝나고 **“idle 상태”로 전환되기 직전**에 트리거됩니다.
- `beforeComplete` 이벤트 직후, 다음 아이템이 없을 경우 `idle` 상태로 진입합니다.
- 자동 반복(loop) 설정이 되어 있으면 다음 루프를 시작하기 전에도 발생합니다.

### 이벤트 객체 구조 (콜백 파라미터)

```json
{
  "type": "complete"
}
```

- 일부 내부 빌드에서는 `playlistItem` 정보가 함께 포함될 수 있으나, 공식 API에서는 `type`만 보장됩니다.

### 활용

#### 1. 다음 영상 재생 (플레이리스트 or 연속재생)

```javascript
player.on('complete', () => {
  console.log('영상 재생 완료');
  playNextItem(); // 커스텀 플레이리스트 제어
});
```

#### 2. 영상 종료 후 UI 전환

- “다시보기”, “다음 영상 추천”, “관련 콘텐츠” 같은 종료 화면 표시

#### 3. 트래킹 / 통계

- **완주율(Finish Rate)** 측정에 필수 이벤트

#### 4. 광고나 후처리 로직

- 콘텐츠 종료 후 포스트롤 광고, CTA 팝업, 피드백 폼 표시 등 후속 동작 실행.

### 주의사항

- `complete`는 콘텐츠가 끝났을 때만 호출되며,
  **사용자가 직접** `stop()` 하거나 다른 아이템으로 전환한 경우에는 발생하지 않습니다.
- 광고(Ad) 재생이 끝나더라도 `complete`는 **콘텐츠 재생 완료** 시점에만 발생합니다.
- `loop: true` 인 경우, 반복 재생마다 `complete`가 매번 호출됩니다.
- 이벤트가 발생하면 내부 상태는 곧 `"idle"` 로 바뀌므로,
  `player.getState()` 를 즉시 호출하면 `"complete"`가 아니라 `"idle"`로 반환될 수 있습니다.
- 짧은 클립(1~2초)에서는 `firstFrame`과 거의 동시에 호출될 수도 있으니 타이밍 의존 로직 주의.

### 특징

- **콘텐츠 재생이 완전히 끝난 유일한 상태 전환 이벤트.**
- `beforeComplete` 와 짝을 이루며, 재생 흐름의 마지막 단계 (`beforeComplete → complete → idle`).
- 사용자의 “시청 완료” 트래킹, 다음 콘텐츠 로딩, UI 전환의 **기준 시점**으로 가장 자주 활용됨.
- 이벤트 데이터가 단순하고 빈도가 낮아, 로깅이나 통계 수집용으로 매우 안정적.

<br>

## .on('error')

### 호출시점

### 이벤트 객체 구조 (콜백 파라미터)

```json

```

| Value | Description |
| :---- | :---------- |
|       |             |

### 활용

### 주의사항

### 특징

<br>

## .on('firstFrame')

### 호출시점

### 이벤트 객체 구조 (콜백 파라미터)

```json

```

| Value | Description |
| :---- | :---------- |
|       |             |

### 활용

### 주의사항

### 특징

<br>

## .on('idle')

### 호출시점

### 이벤트 객체 구조 (콜백 파라미터)

```json

```

| Value | Description |
| :---- | :---------- |
|       |             |

### 활용

### 주의사항

### 특징

<br>

## .on('pause')

### 호출시점

### 이벤트 객체 구조 (콜백 파라미터)

```json

```

| Value | Description |
| :---- | :---------- |
|       |             |

### 활용

### 주의사항

### 특징

<br>

## .on('play')

### 호출시점

### 이벤트 객체 구조 (콜백 파라미터)

```json

```

| Value | Description |
| :---- | :---------- |
|       |             |

### 활용

### 주의사항

### 특징

<br>

## .on('playAttemptFailed')

### 호출시점

### 이벤트 객체 구조 (콜백 파라미터)

```json

```

| Value | Description |
| :---- | :---------- |
|       |             |

### 활용

### 주의사항

### 특징

<br>

## .on('playbackRateChanged')

### 호출시점

### 이벤트 객체 구조 (콜백 파라미터)

```json

```

| Value | Description |
| :---- | :---------- |
|       |             |

### 활용

### 주의사항

### 특징

<br>

## .on('warning')

### 호출시점

### 이벤트 객체 구조 (콜백 파라미터)

```json

```

| Value | Description |
| :---- | :---------- |
|       |             |

### 활용

### 주의사항

### 특징
