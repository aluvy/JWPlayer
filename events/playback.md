# Playback Events

These API calls are used to retrieve and change the current playback state of the player.

<br>
<br>

# .on('autostartNotAllowed')

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
<br>

# .on('buffer')

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
<br>

# .on('complete')

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
<br>

# .on('error')

Signals a critical error in the playback process

### 호출시점

- 플레이어가 **재생 중 오류를 감지했을 때 발생**합니다.
- 즉, 미디어 로드·디코딩·재생 중 네트워크나 포맷 관련 문제로 인해 **정상적인 재생이 중단될 때 즉시 트리거**됩니다.
- `setupError` 가 초기화 실패 시점의 에러라면, `error 는 재생 도중 발생하는 런타임 오류입니다.

#### 발생 예시 상황:

- 스트리밍 URL이 만료되거나 404 응답을 반환
- 네트워크 끊김, HLS 세그먼트 손상
- 브라우저 디코딩 불가(코덱 미지원)
  D- RM 또는 광고 SDK 오류 등

### 이벤트 객체 구조 (콜백 파라미터)

```json
{
  "code": 232011,
  "message": "A network error caused the media download to fail part-way.",
  "sourceError": {
    "code": 4,
    "message": "MEDIA_ERR_NETWORK"
  },
  "type": "error"
}
```

| Value                            | Description                                  |
| :------------------------------- | :------------------------------------------- |
| **code** (number)                | JW Player 내부 에러 코드 (2~6자리 숫자)      |
| **message** (string)             | 사람이 읽을 수 있는 오류 설명                |
| **sourceError** (object \| null) | 브라우저·네트워크·디코더 등의 원본 오류 객체 |

[jw8-player-errors-reference](https://docs.jwplayer.com/players/docs/jw8-player-errors-reference)

### 활용

#### 1. 오류 메시지 표시 / 사용자 안내

```javascript
player.on('error', (e) => {
  console.error(`JW Error ${e.code}: ${e.message}`);
  showErrorUI('영상을 재생할 수 없습니다. 네트워크 상태를 확인해주세요.');
});
```

#### 2. 자동 복구 / 재시도 로직

- 네트워크 일시 오류(code `232011`) 발생 시 재시도:

```javascript
player.on('error', (e) => {
  if (e.code === 232011) {
    setTimeout(() => player.play(true), 3000);
  }
});
```

#### 3. 품질 모니터링 / 로그 수집

- `code` + `message` + `sourceError` 를 서버로 전송 → CDN/네트워크 품질 분석.
- `error` 발생 빈도를 기준으로 스트리밍 품질 또는 사용자 환경 진단 가능.

#### 4. 광고·DRM 환경 분리 대응

- 광고 모듈 에러(`adError`)와 재생 에러(`error`)를 구분하여 별도 처리.

### 주의사항

- error 발생 후 플레이어는 일반적으로 **재생 불가 상태(idle)** 로 전환됩니다.
  재생을 이어가려면 `load()` 나 `setup()` 으로 다시 초기화해야 합니다.

- 특정 브라우저(특히 iOS Safari)에서는 `MEDIA_ERR_DECODE` 가 자주 발생할 수 있으며,
  실제 네트워크 문제보다 디바이스 정책/형식 제약에 의한 경우가 많습니다.

- 동일한 에러가 반복 발생할 수 있으므로, 로그 중복 필터링 필요.

- 광고, DRM, HLS, DASH 등 각 모듈에서 발생한 내부 오류도
  `error` 로 래핑되어 전달되므로, `sourceError` 확인 필수.

- `setupError` 와 구분:
  - `setupError` → 초기화 실패 시점
  - `error` → 재생 중(런타임) 오류

### 특징

- **재생 중 발생하는 모든 오류의 통합 포인트.**
- `setupError` 와 함께 플레이어 오류 처리의 두 축 중 하나.
- `code` 와 `sourceError` 의 병합 구조로, JW 내부·브라우저 내부 원인을 모두 확인 가능.
- 사용자가 오류 후 “다시 시도”하거나 “다음 콘텐츠로 이동”하도록 안내하는 트리거로 가장 많이 활용됨.

<br>
<br>

# .on('firstFrame')

Fires when a video's first frame event occurs or the instant an audio file begins playback

Use this to determine the period of time between a user pressing play and the same user viewing their content. This event pinpoints when content playback begins.

### 호출시점

- 영상이 실제로 **화면에 첫 번째 프레임을 렌더링했을 때** 발생합니다.
- 즉, “재생 요청(`play`)” 후 **사용자가 실제로 영상을 보기 시작하는 정확한 시점**입니다.
- 일반적인 이벤트 흐름:

```
beforePlay → play → firstFrame → time → complete

```

### 이벤트 객체 구조 (콜백 파라미터)

```json
{
  "loadTime": 512,
  "viewable": 1,
  "type": "firstFrame"
}
```

| Value                 | Description                                                            |
| :-------------------- | :--------------------------------------------------------------------- |
| **loadTime** (number) | `play` → `firstFrame` 까지 걸린 시간(ms) (로딩 시간, 초기 지연 측정용) |
| **viewable** (number) | 플레이어가 화면에 보이는 상태 (1 = 보임, 0 = 숨김)                     |

### 활용

#### 1. 재생 지연(Latency) 분석

```javascript
player.on('firstFrame', (e) => {
  console.log(`첫 프레임 표시까지 ${e.loadTime}ms`);
});
```

- 실제 영상이 눈에 보이기까지 걸린 시간을 측정 → 로딩 성능 분석용.

#### 2. 재생 시작 시점 트래킹

- 광고·버퍼링이 끝나고 실제 콘텐츠가 시작되는 타이밍 기록:

#### 3. UX 전환

- 로딩 스피너 제거, 오버레이 UI 숨김 등:

#### 4. 사용자 시청률 계산 기준

- `firstFrame`을 “시청 시작” 기준으로 정의하면
  **시작 대비 완료율(Completion Rate)** 계산에 정확도 향상.

### 주의사항

- **영상이 눈에 보이는 첫 시점**이므로,
  재생 요청이 실패하거나 자동재생이 차단되면 발생하지 않습니다.
- 광고 재생이 포함된 경우, **본편 시작 시점에 따로 발생**합니다.
  (즉, 광고에서 `firstFrame`이 발생하지 않음)
- `loadTime` 값은 네트워크, 브라우저, CDN, 디코딩 속도에 따라 달라지며
  정확히 “영상 로드 시간”을 의미하지는 않습니다 (프레임 렌더링까지 걸린 전체 지연).
- 빠른 연속 아이템 전환 시, 첫 프레임 로딩이 스킵될 수도 있으므로
  `ready` → `firstFrame` 간 타이밍 측정 시점 명확히 분리 필요.

### 특징

- **사용자에게 영상이 실제로 보이기 시작하는 유일한 시점 이벤트.**
- `play` 이벤트는 “재생 요청”, `firstFrame`은 “재생이 실제로 시작된 시점”을 의미.
- `loadTime`으로 재생 성능(초기지연)을 수치화할 수 있어
  **UX 최적화 및 CDN 성능 평가의 핵심 지표**로 사용됨.
- `ready`, `play`, `firstFrame` 세 이벤트를 조합하면
  “초기화 → 요청 → 표시”의 전체 로딩 파이프라인을 정밀하게 추적 가능.

<br>
<br>

# .on('idle')

Fires when the player enters the idle state

### 호출시점

- 플레이어가 **재생 가능한 미디어가 없거나**, **현재 재생이 완전히 종료되어 대기 상태로 전환될 때** 발생합니다.
- 즉, “플레이어가 아무 것도 재생하지 않는 상태(idle state)”로 들어간 시점입니다.
- 일반적인 이벤트 흐름:

```
ready → play → firstFrame → complete → idle
```

- 또한 다음과 같은 경우에도 발생할 수 있습니다:
  - `player.stop()` 호출 시
  - `setup()` 이후 재생 전 상태
  - 플레이리스트의 마지막 아이템 재생 완료 후
  - 에러(`error`), `remove()` 등으로 재생이 종료된 뒤

### 이벤트 객체 구조 (콜백 파라미터)

```json
{
  "oldstate": "playing",
  "newstate": "idle",
  "reason": "complete",
  "type": "idle"
}
```

| Value                 | Description                                                      |
| :-------------------- | :--------------------------------------------------------------- |
| **oldstate** (string) | 이전 플레이어 상태 (`"playing"`, `"buffering"`, `"paused"` 등)   |
| **newstate** (string) | `"idle"` 고정                                                    |
| **reason** (string)   | 상태 전환 원인 (`"complete"`, `"stopped"`, `"error"`, `"setup"`) |

### 활용

#### 1. 재생 종료 후 UI 초기화

```javascript
player.on('idle', (e) => {
  console.log(`플레이어가 idle 상태로 전환됨. 이유: ${e.reason}`);
  document.querySelector('.end-screen').classList.add('show');
});
```

- 재생이 끝난 후 “다음 보기”, “추천 영상” 등의 종료 화면 표시.

#### 2. 재생 루프 또는 자동재생 처리

- `reason: "complete"` 일 때 다음 콘텐츠 자동재생:

```javascript
player.on('idle', (e) => {
  if (e.reason === 'complete') playNextItem();
});
```

#### 3. 플레이어 상태 복원 / 정리

- `remove()` 전 UI/트래킹 정리
- “idle → ready” 전환 전, 이전 재생 데이터 초기화

### 주의사항

- `idle` 은 단순히 “대기 상태”일 뿐, 에러나 중단 등 다양한 원인으로 발생할 수 있음.
  → `reason` 값으로 명확한 원인 구분 필요 (`complete`, `error`, `stopped` 등).
- `complete` 이벤트와 함께 발생할 수 있으므로,
  **중복 처리(예: 다음 영상 재생 로직)** 방지 로직 필요.
- 초기 `setup()` 직후 재생 전에도 `idle` 상태가 될 수 있으므로
  “초기화 단계”와 “재생 종료 단계” 구분이 필요.
- `remove()` 호출 후 `idle` 이벤트가 즉시 발생하지 않을 수 있으며,
  내부 정리 이후 다음 `setup()` 호출 시점에만 상태가 갱신되기도 함.

### 특징

- **플레이어가 아무 것도 재생하지 않는 “완전한 대기 상태”를 나타내는 유일한 이벤트.**
- `complete` 는 “콘텐츠 재생 완료”, `idle` 은 “플레이어 전체가 쉬는 상태”라는 점에서 다름.
- UI·로직 측면에서 “재생 종료 후 초기화”의 기준 타이밍으로 가장 자주 사용됨.
- `reason` 필드가 포함되어 있어 **종료 원인 분석**이 가능한 점이 특징.

<br>
<br>

# .on('pause')

Fires when non-advertising media within the player is paused

> To listen for an ad break pause event, use [.on('adPause')](https://docs.jwplayer.com/players/reference/advertising-events#onadpause)

### 호출시점

- 재생 중이던 영상이 **일시정지(Pause)** 상태로 전환될 때 발생합니다.
- 다음과 같은 경우에 트리거됩니다:
  1. 사용자가 플레이어의 일시정지 버튼 클릭
  2. 스크립트로 `player.pause(true)` 호출
  3. 브라우저 탭이 비활성화되거나 시스템 자원 부족으로 재생이 중단될 때
  4. 자동 일시정지 정책(`autopause` or `viewability` 옵션)으로 인한 정지
  5. 광고 재생을 위해 본편이 일시정지될 때
- 즉, **재생이 중단되는 모든 정상적인 상황의 진입점 이벤트**입니다.
  (`buffer`는 네트워크 지연, `pause`는 재생 중단)

### 이벤트 객체 구조 (콜백 파라미터)

```json
{
  "oldstate": "playing",
  "newstate": "paused",
  "reason": "interaction",
  "type": "pause"
}
```

| Value                 | Description                                                                               |
| :-------------------- | :---------------------------------------------------------------------------------------- |
| **oldstate** (string) | 일시정지 전의 상태 (`"playing"`, `"buffering"` 등)                                        |
| **newstate** (string) | `"paused"` 고정                                                                           |
| **reason** (string)   | 일시정지 원인 (`"interaction"`, `"external"`, `"autostartFail"`, `"ad"`, `"viewability"`) |

### 활용

#### 1. UI 동기화

- 일시정지 상태일 때 버튼, 오버레이, 썸네일 표시 등 시각적 피드백 제공.

```javascript
player.on('pause', (e) => {
  console.log(`Paused by: ${e.reason}`);
  document.body.classList.add('paused');
});
```

#### 2. 사용자 행동 분석

- `reason: "interaction"`이면 사용자가 직접 누른 경우
- `reason: "viewability"`이면 플레이어가 화면 밖으로 나가 자동 일시정지된 경우
- 이를 이용해 **사용자 행동 통계(직접 중단 vs 자동 중단)** 구분 가능.

#### 3. 광고/트래킹 처리

- 광고 재생 시작 전 본편이 일시정지되는 시점 감지 → 광고 모듈 트리거.
- 혹은 일시정지 시 광고 오버레이, 추천 배너 등을 표시.

#### 4. 자동재개 제어

- 특정 구간에서 pause가 발생하면 일정 시간 후 자동 재개:

```javascript
player.on('pause', (e) => {
  if (e.reason === 'interaction') {
    setTimeout(() => player.play(), 5000);
  }
});
```

### 주의사항

- `pause` 이벤트는 “상태 변경”이 일어날 때만 발생하므로,
  이미 `paused` 상태에서 다시 `pause()`를 호출하면 이벤트가 중복 발생하지 않음.
- 자동 일시정지(`viewability`, `autopause`)가 켜져 있을 경우
  사용자가 직접 멈추지 않아도 자주 호출될 수 있음 → 로그 필터링 필요.
- 광고 중단/재생 시점에도 `pause`가 발생할 수 있으므로,
  본편/광고 상태 구분(`player.getState()` 혹은 `ad` 관련 이벤트) 필수.
- 일부 모바일 브라우저(iOS Safari 등)는 시스템 이벤트(전화, 홈 이동 등)로 인한 일시정지 시에도 트리거될 수 있음.

### 특징

- **“사용자 의도 또는 정책적 재생 중단”을 감지하는 대표 이벤트.**
- `buffer`와 달리 네트워크 이슈가 아닌 **논리적 일시정지**임.
- `reason` 필드 덕분에 **중단 원인을 정밀하게 추적**할 수 있음.
- `pause` → `play` 이벤트 조합은 **사용자 참여도(Engagement Rate)** 측정의 기본 단위로 활용됨.

<br>
<br>

# .on('play')

Fires when non-advertising media within the player begins playback

To listen for an ad break play event, use [.on('adPlay')](https://docs.jwplayer.com/players/reference/advertising-events#onadplay).

### 호출시점

- 플레이어가 **실제 재생 상태(`playing`)로 전환될 때** 발생합니다.
- 즉, `beforePlay` → `play` → `firstFrame` 순서 중 **재생 명령이 실행되어 미디어가 재생되기 시작한 시점**입니다.
- 발생 조건은 다음과 같습니다:
  1. 사용자가 “재생 버튼”을 클릭했을 때
  2. `player.play()` 또는 `autostart: true` 설정으로 자동재생이 시작될 때
  3. 일시정지(`pause`) 상태에서 다시 재생될 때
  4. 광고 재생 후 본편이 이어질 때

### 이벤트 객체 구조 (콜백 파라미터)

```json
{
  "oldstate": "paused",
  "newstate": "playing",
  "playReason": "interaction",
  "viewable": 1,
  "type": "play"
}
```

| Value                   | Description                                                                              |
| :---------------------- | :--------------------------------------------------------------------------------------- |
| **oldstate** (string)   | 재생 전 상태 (`"paused"`, `"buffering"`, `"idle"`, 등)                                   |
| **newstate** (string)   | `"playing"` 고정                                                                         |
| **playReason** (string) | 재생이 시작된 이유 (`"autostart"`, `"interaction"`, `"external"`, `"playlist"`, `"api"`) |
| **viewable** (number)   | 플레이어가 현재 화면에 보이는 여부 (1 = 보임, 0 = 숨김)                                  |

### 활용

#### 1. 시청 시작 트래킹

- 사용자가 실제로 재생을 시작한 시점을 기록.
  (`firstFrame`은 화면 렌더링, `play`는 재생 명령 실행 시점)

```javascript
player.on('play', (e) => {
  console.log(`Play event! Reason: ${e.playReason}`);
  sendAnalytics('video_play');
});
```

#### 2. UI 제어

- 로딩 스피너 제거, 일시정지 버튼 활성화, 재생 애니메이션 표시 등

#### 3. 자동재생 감지 및 정책 처리

- `playReason: "autostart"` 값으로 자동재생 성공 여부 확인.
- `autostartNotAllowed` 대비 분기 처리 로직 작성 가능.

#### 4. 재생 분석 (Engagement Metrics)

- `pause` / `play` 이벤트를 조합해 사용자의 시청 패턴(중단·재개)을 추적.
- `viewable` 필드를 이용해 실제 화면에서 재생되는 경우만 카운트.

### 주의사항

- `play`는 “재생 명령이 내려졌을 때” 호출되므로,
  실제 영상 표시(`firstFrame`)보다 먼저 발생합니다.
- 자동재생(`autostart`)이 브라우저 정책에 의해 차단되면
  `play` 대신 `autostartNotAllowed` 가 호출될 수 있습니다.
- 광고 재생 중에도 내부적으로 `play` 이벤트가 발생할 수 있으므로
  **콘텐츠/광고 재생 구분(`adPlaying` 상태 확인)** 필요.
- 동일한 세션 내에서 여러 번 발생할 수 있습니다.
  (예: 일시정지 후 다시 재생할 때마다 `play` 이벤트 발생)

### 특징

- **재생 상태로 진입하는 모든 순간을 감지할 수 있는 기본 이벤트.**
- `beforePlay`(준비 단계)와 `firstFrame`(화면 표시) 사이의 연결고리 역할.
- `playReason` 필드를 통해 **재생 트리거의 출처**를 명확히 구분할 수 있음.
- `pause` 이벤트와 짝을 이루며,
  **시청 패턴·이탈률·참여도(Engagement Rate)** 분석의 핵심 포인트가 됨.

<br>
<br>

# .on('playAttemptFailed')

Fires when playback is aborted or blocked

A failed play attempt does not result in a play. Pausing the video or changing the media results in play attempts being aborted. In mobile browsers play attempts are blocked when not started by a user gesture.

### 호출시점

- 플레이어가 **재생을 시도했으나(`play()` 실행됨)**
  **브라우저 또는 플랫폼 정책, 미디어 문제 등으로 인해 재생이 시작되지 못했을 때** 발생합니다.
- 즉, `play` 이벤트가 **성공적으로 완료되지 못한 시점**에서 트리거됩니다.
- 주로 다음과 같은 경우에 발생합니다:
  1. 브라우저 자동재생 정책으로 인해 재생 거부됨
  2. 사용자 상호작용 없이 `play()` 호출됨
  3. 코덱 불일치, 미디어 파일 로드 실패, DRM 오류 등으로 실제 재생이 시작되지 못함
  4. iOS Safari에서 음소거되지 않은 자동재생 시도

### 이벤트 객체 구조 (콜백 파라미터)

```json
{
  "code": 303016,
  "error": "play() request was interrupted by a call to pause().",
  "playReason": "autostart",
  "type": "playAttemptFailed"
}
```

| Value                   | Description                                                            |
| :---------------------- | :--------------------------------------------------------------------- |
| **code** (number)       | JW Player 내부 오류 코드 (예: `303016`, `303220`)                      |
| **error** (string)      | 브라우저나 플랫폼이 반환한 오류 메시지                                 |
| **playReason** (string) | 재생 시도 원인 (`"autostart"`, `"interaction"`, `"external"`, `"api"`) |

### 활용

#### 1. 자동재생 실패 대응 (Fallback)

- **자동재생 실패 시 음소거 재시도 또는 UI 버튼 표시**로 사용자 경험 유지.

```javascript
player.on('playAttemptFailed', (e) => {
  console.warn('재생 시도 실패:', e.error);
  if (e.playReason === 'autostart') {
    player.setMute(true);
    player.play(); // 음소거 상태로 재시도
  } else {
    showPlayButton(); // 사용자 수동재생 유도
  }
});
```

#### 2. 오류 로깅 및 정책 분석

- 브라우저·디바이스·플랫폼별 자동재생 실패율 추적.
- `error` 문자열을 로그에 남겨 **정책 변경(Chrome/Safari)** 대응에 활용.

#### 3. 재생 정책 테스트

- 사내 QA나 A/B 테스트 환경에서 “자동재생 성공률” 측정용 트리거로 사용 가능.

### 주의사항

- `playAttemptFailed`는 `play` **이벤트가 발생하지 않은 경우에만 호출됩니다.**
  즉, 실제 재생이 시작되지 않았음을 의미.

- 오류 코드(`code`)는 브라우저 환경·JW Player 버전에 따라 다를 수 있으므로,
  **정확한 원인 분류는 `error` 문자열 기반으로 처리하는 것이 안전**합니다.
- `autostartNotAllowed` 와 유사하지만, 다음과 같이 구분됩니다:

| 비교항목  | `autostartNotAllowed`    | `playAttemptFailed`            |
| :-------- | :----------------------- | :----------------------------- |
| 발생 시점 | 자동재생 시도 즉시 차단  | 재생 시도 중 실제 실패 감지    |
| 대상      | `autostart` 설정 시 전용 | 모든 `play()` 시도 (수동 포함) |
| 공통점    | 재생 불가 상황을 알림    | 재생 불가 상황을 알림          |

- 모바일(iOS, Android WebView 등)에서는 시스템 제약(오디오 정책, 화면 포커스 등)으로
  예측 불가능하게 발생할 수 있습니다 → UI fallback 필수.

### 특징

- **재생 시도가 실패한 유일한 감지 이벤트.**
- `autostartNotAllowed`보다 더 광범위하게 적용되며,
  **자동재생 + 수동재생** 실패 모두를 커버합니다.
- 브라우저 정책 변화나 미디어 포맷 오류 등 **재생 실패 원인 진단의 핵심 이벤트.**
- 에러 객체(`error`, `code`, `playReason`)가 상세해 디버깅·분석 용도로 매우 유용.
- 특히 프리롤 광고 또는 미디어 로딩 도중 중단되는 케이스에서도 감지 가능.

<br>
<br>

# .on('playbackRateChanged')

Fires when the playback rate has been changed

### 호출시점

- 플레이어의 **재생 속도(playbackRate)** 가 변경될 때마다 발생합니다.
- 즉, 사용자가 속도 조절 메뉴(예: 1.5x, 2x)를 바꾸거나,
  스크립트로 `player.setPlaybackRate(rate)` 호출했을 때 트리거됩니다.
- 일반적인 호출 흐름:

```
ready → play → playbackRateChanged (사용자 조작 시)
```

- 속도 변경 시점마다 **_즉시 발생_**하며, 동일한 값으로 다시 설정해도 중복 호출되지 않습니다.

### 이벤트 객체 구조 (콜백 파라미터)

```json
{
  "playbackRate": 1.5,
  "type": "playbackRateChanged"
}
```

| Value                     | Description                                         |
| :------------------------ | :-------------------------------------------------- |
| **playbackRate** (number) | 현재 적용된 재생 속도 (예: 0.5, 1, 1.25, 1.5, 2 등) |

### 활용

#### 1. UI 동기화

- 사용자가 재생 속도를 바꿀 때 UI에 실시간 표시.

```javascript
player.on('playbackRateChanged', (e) => {
  document.querySelector('#rateDisplay').textContent = `${e.playbackRate}x`;
});
```

#### 2. 사용자 환경 저장 (Persistence)

- 다음 영상에서도 동일한 속도로 재생되도록 사용자 선호도 저장.

```javascript
player.on('playbackRateChanged', (e) => {
  localStorage.setItem('lastPlaybackRate', e.playbackRate);
});
```

#### 3. 분석 및 트래킹

- 어떤 속도가 가장 많이 사용되는지 트래킹하여 UX 개선에 활용.
  (예: 평균 재생 속도, 2배속 이용률 등)

#### 4. 특수 콘텐츠 제어

- 교육·강의 콘텐츠 등에서 **속도 제한** 정책 구현:

```javascript
player.on('playbackRateChanged', (e) => {
  if (e.playbackRate > 2.0) player.setPlaybackRate(2.0);
});
```

### 주의사항

- 일부 브라우저(특히 iOS Safari)는 **속도 변경을 완전히 지원하지 않거나, 오디오·자막 동기 문제**가 발생할 수 있습니다.
  → 실제 적용 가능한 값(`player.getPlaybackRates()`)을 미리 확인하는 것이 안전합니다.
- 같은 속도로 반복 설정(`setPlaybackRate(1.0)` → 다시 `1.0`) 시에는 이벤트가 발생하지 않습니다.
- 광고 재생 중(`adPlaying`)에는 속도 변경이 비활성화될 수 있으며, 이벤트가 호출되지 않을 수도 있습니다.
- 이벤트는 **변경 직후 단 한 번**만 호출되므로, 지속적인 추적에는 별도 상태 관리가 필요합니다.
- `setPlaybackRate()` 로 프로그램matically 변경해도 동일하게 발생합니다 (사용자 조작 구분 없음).

### 특징

- **재생 속도 변경을 감지하는 유일한 이벤트.**
- 간단한 구조지만, UX 개선(속도 표시·저장·제한)이나 **학습 콘텐츠 분석** 등에 매우 자주 활용됨.
- `baseSetup.playbackRates` 와 함께 설정하면 사용자 경험 일관성 유지에 용이.
- playbac`kRate 값은 실시간으로 `player.getPlaybackRate()` 에서도 확인 가능.

<br>
<br>

# .on('warning')

Signals a failure that is not critical to the setup or playback process

### 호출시점

- **재생은 계속 진행되지만 경고성 문제(Warning)가 발생했을 때** 호출됩니다.
- 즉, **치명적 오류(`error`)는 아니지만, 품질 저하나 기능 제한**이 감지된 시점입니다.
- 플레이어 동작은 유지되나, 내부적으로 다음과 같은 상황에서 트리거될 수 있습니다:
  - 비정상적인 네트워크 응답 (일부 세그먼트 손상 등)
  - 자막 파싱 실패 또는 일부 트랙 누락
  - 광고 SDK 경고 (타임아웃, fallback 발생 등)
  - 품질 전환 실패 후 기본 스트림으로 복귀
  - 미디어 메타데이터 오류 또는 손상된 HLS manifest 감지
- 즉, `warning`은 “경미한 문제”를 알려주는 **비치명적 에러 알림 이벤트**입니다.

### 이벤트 객체 구조 (콜백 파라미터)

```json
{
  "code": 332002,
  "message": "The captions track could not be loaded.",
  "sourceError": null,
  "type": "warning"
}
```

| Value                            | Description                                         |
| :------------------------------- | :-------------------------------------------------- |
| **code** (number)                | 경고 코드 (JW Player 내부 코드, 300000~399999 범위) |
| **message** (string)             | 경고 내용 - 경미한 문제 설명                        |
| **sourceError** (object \| null) | 브라우저 또는 네트워크 수준의 실제 오류 정보        |

### 활용

#### 1. 비정상 상황 로깅

- 치명적 에러는 아니지만, 스트리밍 품질 문제나 기능 실패를 서버 로그에 기록하여 추적.

```javascript
player.on('warning', (e) => {
  console.warn(`[JW Warning] ${e.code}: ${e.message}`);
  sendLog('jw_warning', e);
});
```

#### 2. 사용자 알림 (필요 시)

- 일반 사용자는 대부분 무시해도 되지만,
  HLS나 광고 시스템을 사용하는 관리자 화면에서는 경고 팝업 표시 가능.

```javascript
player.on('warning', (e) => {
  if (e.code >= 332000 && e.code < 333000) {
    showToast('일부 자막을 불러오지 못했습니다.');
  }
});
```

#### 3. 진단/모니터링

- `error`와 함께 수집하면 **정상 재생 중 발생한 경고**를 별도로 통계화 가능.
- 네트워크 품질 모니터링, 광고 SDK 성능 분석, 자막 파일 상태 점검 등에 활용.

### 주의사항

- `warning`은 **재생 중단을 유발하지 않으며, 대부분 자동 복구됩니다.**
  따라서 사용자에게 직접적인 오류 메시지를 노출할 필요는 없습니다.
- 단, 동일한 원인으로 연속 발생 시 네트워크·콘텐츠 품질 이슈일 수 있으므로
  로그 수집 및 빈도 제한(throttling) 처리 권장.
- 광고·자막·스트리밍 모듈 등 **다양한 서브시스템에서 발생**할 수 있으므로,
  `code` 범위로 구분 관리하는 것이 좋습니다.
- 예를 들어:
  - **332xxx** : captions 관련 경고
  - **334xxx** : 광고 관련 경고
  - **336xxx** : HLS/DASH 관련 경고

### 특징

- **재생을 중단하지 않는 비치명적 문제 감지 이벤트.**
- `error` 이벤트와 구조는 동일하지만, 영향도 낮은 이슈에 사용됨.
- 스트리밍 품질 모니터링, 자막/광고 디버깅 등 **운영 환경 진단용 이벤트**로 유용.
- 로그 기반으로 품질 분석(QoE: Quality of Experience) 지표를 보완하는 데 적합.
