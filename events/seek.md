# Seek Events

These API calls are used to retrieve and update the current media playback position.

<br>

## .on('absolutePositionReady')

Fired when [.getAbsolutePosition()](https://docs.jwplayer.com/players/reference/getabsoluteposition) is ready to return data.

### 호출시점

- 플레이어가 **타임라인 상의 “절대 위치(absolute position)” 정보를 계산할 준비가 완료된 시점**에 호출됩니다.

- 즉, **콘텐츠의 전체 길이(duration)** 가 확정되고,  
  내부적으로 **재생 위치 추적(absolute positioning)** 기능을 사용할 수 있게 되었을 때 트리거됩니다.

- 일반적으로 다음과 같은 시점에서 발생합니다:

  1. `playlistItem` 로드 후 미디어 메타데이터가 완전히 파싱됨

  2. `duration` 값이 유효해지고, `seek`, `time`, `position` 이벤트 처리가 가능한 상태

  3. 첫 번째 프레임(`firstFrame`) 이전, 또는 거의 동시에 발생

- 즉, `absolutePositionReady`는 **“재생 타임라인이 유효해진 직후”** 발생하는 초기화 이벤트입니다.

### 이벤트 객체 구조 (콜백 파라미터)

```json
{
  "position": 0,
  "duration": 132.84,
  "type": "absolutePositionReady"
}
```

| Value                 | Description                          |
| :-------------------- | :----------------------------------- |
| **position** (number) | 현재 재생 위치 (초 단위, 초기값 `0`) |
| **duration** (number) | 전체 영상 길이 (초 단위)             |

### 활용

#### 1. 타임라인 기반 UI 초기화

- 전체 길이가 확정된 시점에 진행 바(Progress Bar)나 시킹 컨트롤 초기화.

```javascript
player.on('absolutePositionReady', (e) => {
  initProgressBar(e.duration);
  console.log(`영상 길이: ${e.duration}s`);
});
```

#### 2. 초기 구간 제어(Seek, Bookmark)

- 이전 시청 위치로 이동하거나 특정 시점 자동 시킹 가능.

```javascript
player.on('absolutePositionReady', () => {
  const resumeTime = localStorage.getItem('lastWatchTime');
  if (resumeTime) player.seek(Number(resumeTime));
});
```

#### 3. 타임라인 분석 및 메타데이터 연동

- 구간별 자막, 하이라이트, 챕터 마커 등의 “절대시간” 매핑 시 기준 이벤트로 사용.

- 예: `absolutePositionReady` 후에만 `time()` 이벤트 기반 구간 표시 정확도 확보.

### 주의사항

- `absolutePositionReady`는 **“한 번만 발생”**합니다 (각 playlistItem마다 1회).

- `ready` 이벤트 직후 또는 `firstFrame` 직전에 발생하므로,  
  재생 제어나 UI 조작 로직은 반드시 이 이벤트 이후 실행해야 안정적입니다.

- **HLS / DASH 등 스트리밍 콘텐츠의 경우**  
  초기 세그먼트가 모두 파싱되기 전에는 `duration` 값이 정확하지 않을 수 있으므로,  
  해당 이벤트에서 받은 `duration`은 “추정치(approximation)”일 수 있습니다.

- 라이브 스트림(live)에서는 `duration` 값이 없거나 `Infinity` 로 표시될 수 있습니다.

- `absolutePositionReady` 이후 `time` 이벤트가 주기적으로 발생하기 시작합니다.

### 특징

- **재생 타임라인 초기화 완료 시점**을 알려주는 유일한 이벤트.

- `ready` 이벤트는 “플레이어 셋업 완료”,  
  `absolutePositionReady`는 “미디어 타임라인 준비 완료”로 역할이 다릅니다.

- `duration이` 확정되므로 **진행 바, 챕터, 구간 표시, Seek 가능 여부** 등의 기준으로 사용.

- 반복 재생(playlist 순환) 시 각 아이템마다 새로 호출됩니다.

- UI 및 시킹 관련 기능을 초기화하기에 가장 안정적인 시점입니다.

<br>

## .on('seek')

Fired when a seek has been requested either by scrubbing the control bar or through the API.

### 호출시점

- 사용자가 **재생 위치를 변경(Seek)** 했을 때 발생합니다.

- 즉, **타임라인(Progress bar)** 을 드래그하거나,  
  스크립트로 `player.seek(timeInSeconds)` 를 호출했을 때 트리거됩니다.

- 내부적으로 “이전 재생 위치 → 새 재생 위치”로 이동하는 **즉시 시점**에서 발생하며,  
  실제 재생이 다시 시작될 때(`play`)보다 먼저 호출됩니다.

#### 발생 시점 요약

```
(사용자 드래그 or API 호출)
→ seek (from X to Y)
→ buffer (필요 시)
→ play / firstFrame

```

### 이벤트 객체 구조 (콜백 파라미터)

```json
{
  "position": 25.412,
  "offset": 120.0,
  "type": "seek"
}
```

| Value                 | Description                                     |
| :-------------------- | :---------------------------------------------- |
| **position** (number) | 시킹 전 위치(초 단위, 이전 재생 지점)           |
| **offset** (number)   | 시킹 후 위치(초 단위, 이동하려는 타임라인 지점) |

### 활용

#### 1. 사용자 시킹 행동 로그

- 사용자의 “되감기”, “건너뛰기”, “앞으로 이동” 행동 분석.

```javascript
player.on('seek', (e) => {
  console.log(`Seeked from ${e.position}s → ${e.offset}s`);
  sendAnalytics('seek_event', e);
});
```

#### 2. 구간 컨트롤

- 특정 구간 시킹 차단:  
  → 예: 인트로 구간(0–10초) 건너뛰기 방지.

```javascript
player.on('seek', (e) => {
  if (e.offset < 10) {
    e.preventDefault?.();
    player.seek(10);
  }
});
```

#### 3. 자막/메타데이터 동기화

- 시킹 후 별도 API 요청이나, 시점 기반 UI(예: 챕터 표시) 리셋:

```javascript
player.on('seek', (e) => {
  updateSubtitle(e.offset);
  highlightChapter(e.offset);
});
```

#### 4. 재버퍼링 트리거 제어

- 시킹 시 `buffer` 이벤트가 함께 발생하므로,  
  시킹 중에는 로딩 스피너 표시:

```javascript
player.on('seek', () => showSpinner(true));
player.on('firstFrame', () => showSpinner(false));
```

### 주의사항

- `seek` 이벤트는 **“시도” 시점**에 발생하며,  
  실제 재생 재개가 성공했는지 여부는 `firstFrame` 또는 `play` 이벤트로 확인해야 합니다.

- 라이브 스트림에서는 시킹이 불가능하거나 (`seekable` 구간 제한),  
  이벤트가 호출되지 않을 수 있습니다.

- 시킹 중 발생하는 `buffer` 이벤트를 감안해, UI에서 **중복 로딩 표시**를 피해야 합니다.

- 같은 위치로 시킹(`player.seek(currentTime)`)하면 이벤트가 발생하지 않습니다.

- 광고 중에는 본편(`content`)의 `seek` 이벤트가 일시적으로 비활성화될 수 있습니다.

### 특징

- **재생 위치 변화(Seek Action)를 감지하는 유일한 이벤트.**

- `position`(이전 위치)과 `offset`(이동 후 위치)을 함께 제공하여
  **사용자 행동 분석 및 시청 패턴 파악**에 매우 유용.

- `seeked` 이벤트는 별도로 존재하지 않으며, seek 단일 이벤트로 처리.

- 시킹 후 `buffer` → `play` → `firstFrame` 순으로 이어지는
  **재생 전환 파이프라인의 첫 단계 이벤트.**

<br>

## .on('seeked')

Fired when video position changes after seeking, as opposed to on('seek') which triggers as a seek occurs.

### 호출시점

- `seeked` 이벤트는 **시킹(Seek) 동작이 완료된 직후**,  
  즉 플레이어가 **새 위치로 이동하고 재생 가능한 상태로 복귀했을 때** 발생합니다.

- `seek` 이벤트가 “시도” 시점이라면,
  `seeked`는 **“완료” 시점**입니다.

#### 일반적인 시퀀스 흐름:

```
사용자 드래그 or player.seek(x)
→ seek (from A to B)
→ buffer (데이터 로드)
→ seeked (시킹 완료)
→ play / firstFrame (재생 재개)

```

따라서 `seeked`는 **데이터 로드 및 디코딩 완료 후**,  
**실제 재생 위치가 갱신된 시점**을 명확히 알려주는 이벤트입니다.

### 이벤트 객체 구조 (콜백 파라미터)

```json
{
  "position": 120.004,
  "type": "seeked"
}
```

| Value                 | Description                             |
| :-------------------- | :-------------------------------------- |
| **position** (number) | 시킹 완료 후의 최종 재생 위치 (초 단위) |

### 활용

#### 1. 시킹 완료 후 UI 복원

- 사용자가 시킹을 마친 뒤 로딩 UI나 상태 표시를 제거.

```javascript
player.on('seeked', (e) => {
  console.log(`시킹 완료. 현재 위치: ${e.position}s`);
  hideSpinner();
  updateProgress(e.position);
});
```

#### 2. 데이터 동기화

- 자막, 챕터, 썸네일, 통계 등 시킹 이후 필요한 데이터 재동기화:

```javascript
player.on('seeked', (e) => {
  updateSubtitles(e.position);
  refreshOverlay(e.position);
});
```

#### 3. 통계 및 사용자 행동 분석

- 실제 시킹 완료 시간을 기준으로 **시청 패턴** 분석:

```javascript
player.on('seeked', (e) => {
  sendAnalytics('seeked', { position: e.position });
});
```

#### 4. 시킹 완료 후 자동 재생/정지 제어

- 특정 시점에 도달 후 재생/정지 정책을 자동으로 적용:

```javascript
player.on('seeked', (e) => {
  if (e.position >= 300) player.pause();
});
```

### 주의사항

- `seeked`는 항상 `seek` 이벤트 **이후에 한 번만 발생**합니다.  
  (`seek` → `buffer` → `seeked` 순)

- 시킹이 실패하거나 중간에 error가 발생하면 `seeked`는 호출되지 않습니다.

- 라이브 스트림(HLS live, DASH live)의 경우,  
  **seekable 구간**을 벗어난 시킹은 무시되거나 `seeked` 없이 복귀할 수 있습니다.

- 시킹 후 바로 `pause()`를 호출하는 경우,  
  `seeked`는 호출되지만 `play`는 발생하지 않습니다.

- 빠른 연속 시킹(사용자 드래그 중 다중 호출)은 이벤트 중첩을 유발할 수 있으므로  
  **마지막 seeked만 처리하도록 디바운스**하는 것이 좋습니다.

### 특징

- `seek`의 **보완 이벤트**로,
  **“재생 위치 이동이 실제로 반영된 순간”**을 명확히 알려줍니다.

- `seek`이 intent(의도), `seeked`가 result(결과)에 해당합니다.

- `position` 값이 항상 **이동 후의 최종 위치**이므로
  콘텐츠 기반 구간 제어나 시점 연동에 가장 정확한 기준 이벤트입니다.

- UI/UX 관점에서는 **로딩 → 완료 전환 타이밍의 기준**으로 활용됨.

<br>

## .on('time')

Fired when the playback position updates as the player is playing. This may occur as frequently as 10 times per second.

### 호출시점

- 플레이어가 **재생 중일 때, 매 프레임(또는 일정 주기)** 마다 호출됩니다.

- 즉, **재생 위치(position)** 가 변할 때마다 **지속적으로 발생하는 주기적 이벤트**입니다.

- 일반적으로 0.25초(250ms) 간격으로 호출되며,  
  내부적으로 `requestAnimationFrame` 또는 `setInterval` 기반으로 실행됩니다.

#### 주요 호출 흐름 예시:

```
ready → play → firstFrame
→ time (매 프레임마다)
→ buffer (일시 버퍼링 시)
→ time (재개)
→ complete (끝)
```

### 이벤트 객체 구조 (콜백 파라미터)

```json
{
  "position": 14.276,
  "duration": 120.032,
  "viewable": 1,
  "type": "time"
}
```

| Value                 | Description                                      |
| :-------------------- | :----------------------------------------------- |
| **position** (number) | 현재 재생 위치(초 단위, 소수점 포함)             |
| **duration** (number) | 전체 영상 길이(초 단위)                          |
| **viewable** (number) | 플레이어가 화면에 보이는지 여부 (1=보임, 0=숨김) |

### 활용

#### 1. 재생 위치 기반 UI 업데이트

- 실시간으로 진행 바(Progress Bar) 갱신.

```javascript
player.on('time', (e) => {
  const progress = (e.position / e.duration) * 100;
  document.querySelector('.progress-bar').style.width = `${progress}%`;
});
```

#### 2. 시청 구간 트래킹 / 분석

- 시청 진행률(10초 단위, 25/50/75% 등)을 추적하여 통계 수집.

```javascript
player.on('time', (e) => {
  if (Math.floor(e.position) % 10 === 0) {
    sendAnalytics('watch_progress', { sec: e.position });
  }
});
```

#### 3. 구간 제어 / 구간별 기능

- 특정 타임라인 구간 진입 시 UI 전환, 자막 표시, 이벤트 트리거 등.

```javascript
player.on('time', (e) => {
  if (e.position >= 30 && e.position < 31) showHintOverlay();
  if (e.position >= e.duration - 5) prepareEndScreen();
});
```

#### 4. 뷰어빌리티(Viewability) 기반 제어

- `viewable` 값(`0` or `1`)을 활용해 시야 내에서만 로그 또는 UI 업데이트:

```javascript
player.on('time', (e) => {
  if (e.viewable === 1) updateVisibleMetrics(e.position);
});
```

### 주의사항

- `time` 이벤트는 매우 **빈번하게 발생**하므로,

  - DOM 조작, 네트워크 호출, 복잡한 계산을 직접 넣지 말 것.

  - **Throttle / Debounce** 적용 필요:

```javascript
let last = 0;
player.on('time', (e) => {
  if (e.position - last > 1) {
    last = e.position;
    console.log(`1초마다 로깅: ${e.position}`);
  }
});
```

- `duration`이 없는 **라이브 스트림(Live HLS/DASH)** 은 `duration = Infinity` 또는 `NaN`으로 표시될 수 있습니다.  
  이 경우 상대적 퍼센티지 계산 금지.

- `time` 이벤트는 `pause` 상태일 때는 발생하지 않습니다.  
  (정지 중에는 position 값이 고정)

- 시킹(`seek`) 이후에는 잠시 중단되었다가,  
  새 위치에서 다시 time 이벤트가 이어집니다.

- `viewable` 값은 `containerViewable` 이벤트와 연동되며,  
  실제 DOM 노출이 아닌 플레이어 내부 상태를 기반으로 합니다.

### 특징

- **JW Player에서 가장 빈번히 호출되는 이벤트.**

- “현재 재생 위치(position)”와 “전체 길이(duration)”를 실시간으로 제공하는  
  **타임라인 동기화의 핵심 이벤트.**

- 대부분의 UX/UI 기능(진행바, 챕터 표시, 자막 동기화, 광고 인서트 등)은  
  이 이벤트를 기반으로 동작.

- 실시간 분석/트래킹 시스템에서도 `time` 이벤트를 기준으로  
  시청률, 평균 시청 구간, 이탈률 등을 계산.
