# Control Events

This API call allows developers to interact with the built-in player controls (control bar and display icons).

> Controls will still fade out during playback if the video has no keyboard/mouse focus. When controls are disabled, JW Player is completely chrome-less.

<br>
<br>

# .on('controls')

Fired when controls are enabled or disabled by a script.

### 호출시점

- `controls` 이벤트는 **플레이어 컨트롤(Play/Pause 버튼, 시킹바, 볼륨, 설정 버튼 등)** 의 **표시 상태가 변경될 때** 발생합니다.

- 즉, **플레이어 UI가 나타나거나 사라질 때(show/hide)** 트리거됩니다.

#### 발생 조건

1. 사용자가 마우스나 손가락으로 **플레이어 영역을 터치/호버**해 컨트롤이 표시될 때

2. 일정 시간 동안 입력이 없어서 **컨트롤이 자동으로 사라질 때**

3. 코드로 `player.setControls(false)` 또는 `player.setControls(true)` 호출 시

4. 모바일 환경에서 전체화면 전환 시 UI 전환이 일어날 때

#### 일반적인 흐름

```
play → controls(show=true)
→ idle (사용자 입력 없음)
→ controls(show=false)
```

### 이벤트 객체 구조 (콜백 파라미터)

```json
{
  "controls": true,
  "type": "controls"
}
```

| Value                  | Description                                     |
| :--------------------- | :---------------------------------------------- |
| **controls** (boolean) | `true` → 컨트롤 표시됨, `false` → 컨트롤 숨겨짐 |

### 활용

#### 1. 사용자 인터랙션 감지

- 사용자 활동(Active/Idle)을 간단히 감지 가능.
- UX 분석이나 “사용자 인게이지먼트 추적”에도 유용.

```javascript
player.on('controls', (e) => {
  if (e.controls) {
    console.log('사용자가 플레이어를 터치/호버하여 컨트롤 표시');
  } else {
    console.log('컨트롤이 자동으로 숨김 상태로 전환됨');
  }
});
```

#### 2. 커스텀 UI 전환

- 컨트롤이 표시되면 오버레이를 감추고, 숨겨지면 다시 노출.
- 뉴스, OTT, 인터랙티브 플레이어에서 자주 쓰이는 패턴.

```javascript
player.on('controls', (e) => {
  const overlay = document.querySelector('.custom-overlay');
  overlay.style.opacity = e.controls ? 0 : 1;
});
```

#### 3. 자동 UI 타임아웃 제어

- 내부 `controls` 상태를 외부 레이어와 동기화 가능.

```javascript
player.on('controls', (e) => {
  if (e.controls) clearTimeout(hideUITimer);
  else startAutoHideTimer();
});
```

#### 4. 전체화면 UX 개선

- 전체화면 상태에서 컨트롤 표시 시 **상단 로고나 자막 위치 조정**

```javascript
player.on('controls', (e) => {
  if (document.fullscreenElement && e.controls) {
    document.querySelector('.logo').classList.add('hide');
  } else {
    document.querySelector('.logo').classList.remove('hide');
  }
});
```

### 주의사항

- `controls` 이벤트는 **표시 상태 변화가 있을 때만** 발생합니다.  
  즉, 같은 상태(`true → true`, `false → false`)로 유지될 때는 호출되지 않습니다.

- `controls` 표시 여부는 **플레이어 내부 상태 + 사용자 입력**의 조합으로 결정되므로,  
  코드로 `player.setControls()` 호출 시에도 동일하게 트리거됩니다.

- 모바일(iOS Safari)에서는 네이티브 플레이어 UI를 사용하므로,  
  `controls` 이벤트가 **제대로 발생하지 않거나 누락될 수 있습니다.**

- `controls`를 비활성화한(`controls: false`) 경우, 이 이벤트 자체가 발생하지 않습니다.

- 오버레이나 커스텀 UI를 직접 덮어쓰는 경우  
  JW Player의 내부 타이머(`_controlsTimeout`) 동작과 충돌할 수 있으므로 **CSS만 제어**하는 것이 안전합니다.

### 특징

- **JW Player의 내부 UI 상태를 외부 스크립트와 동기화할 수 있는 유일한 이벤트.**

- `viewable`, `containerViewable`과 달리 “보이는 영역”이 아니라 **플레이어 UI(컨트롤 바)**의 표시 여부를 알려줌.

- UX 개선, 사용자 활동 트래킹, UI 연동용 로직에 필수.

- 재생 이벤트(`play`, `pause`, `idle`)보다 더 세밀하게 **사용자 행동 시점**을 포착할 수 있음.

<br>
<br>

# .on('displayClick')

Fires when a user clicks the video display

This is especially useful for wiring your own controls when the built-in ones are disabled.

> The default click action (toggling play/pause) will still occur if controls are enabled.

### 호출시점

- `displayClick` 이벤트는 사용자가 **플레이어의 메인 비디오 영역(Display Area)** 을 클릭했을 때 호출됩니다.

- 즉, **재생화면(영상 자체)** 을 클릭 또는 탭했을 때, **플레이어의 기본 클릭 액션(Play/Pause 토글 등)** 이 수행되기 직전에 발생합니다.

#### 대표적 발생 조건

1. 영상 위(재생 화면 중앙 등) 클릭 → 재생/일시정지 동작 직전

2. 모바일 기기 탭 → JW Player 내부 터치 핸들러가 감지한 경우

3. 커스텀 overlay가 없는 경우 (overlay가 클릭 이벤트를 막으면 발생하지 않음)

4. `controls: false` 상태여도 Display 영역 클릭은 감지됨 (단, autoplay 등으로 제어 중이면 무시될 수 있음)

#### 일반 이벤트 흐름

```
displayClick (사용자 클릭 감지)
→ play (또는 pause)
→ firstFrame / buffer / idle
```

> 즉, `displayClick`은 실제 재생 동작이 일어나기 직전의 “사용자 입력 이벤트” 입니다.

### 이벤트 객체 구조 (콜백 파라미터)

```json
{
  "type": "displayClick",
  "reason": "play"
}
```

| Value               | Description                                                               |
| :------------------ | :------------------------------------------------------------------------ |
| **reason** (string) | 클릭 시 수행된 동작 (`"play"`, `"pause"`, `"replay"`, `"next"`, `"none"`) |

- reason은 클릭이 어떤 플레이어 동작을 유발했는지를 나타냅니다.
- 예를 들어,

  - 일시정지 중 클릭 → `"play"`

  - 재생 중 클릭 → `"pause"`

  - 재생 완료 후 클릭 → `"replay"`

  - 플레이리스트 다음 영상으로 전환 시 `"next"`

### 활용

#### 1. 사용자 인터랙션 트래킹

- 사용자의 “화면 클릭”을 정량적으로 기록 (CTR, 시청 의도, 인터랙션 빈도 등).

```javascript
player.on('displayClick', (e) => {
  console.log(`화면 클릭됨 (동작: ${e.reason})`);
  sendAnalytics('display_click', { reason: e.reason });
});
```

#### 2. 커스텀 UI 제어 (Overlay, Banner 등)

- JW Player의 내부 동작(재생/일시정지)과 커스텀 UI를 동기화.

```javascript
player.on('displayClick', (e) => {
  if (e.reason === 'pause') {
    showPauseOverlay();
  } else if (e.reason === 'play') {
    hidePauseOverlay();
  }
});
```

#### 3. 광고 시 클릭 방지 / 대체 행동 구현

- 광고 중에는 클릭을 차단하거나 별도 행동 수행:

```javascript
player.on('displayClick', (e) => {
  if (player.getState() === 'ads') {
    e.preventDefault?.();
    openAdInfoModal();
  }
});
```

#### 4. 재생 완료 후 Replay 버튼 커스터마이징

```javascript
player.on('displayClick', (e) => {
  if (e.reason === 'replay') {
    console.log('사용자가 영상 재시작을 요청함');
    customReplayAnimation();
  }
});
```

### 주의사항

- `displayClick`은 **사용자 입력 기반 이벤트**이므로,  
  자동재생(`autostart: true`) 중에는 **초기 재생 시 발생하지 않습니다.**

- 클릭 영역이 **커스텀 오버레이나 HTML 엘리먼트로 덮여 있다면**  
  (예: <div class="cover-layer">) → JW Player 내부에서 클릭을 감지하지 못할 수 있습니다.

- `displayClick` 이벤트 자체를 **중단(`preventDefault`)할 수 없습니다.**  
  즉, JW Player 내부의 기본 동작(Play/Pause)은 항상 수행됩니다.

- 광고 중에는 클릭 이벤트가 광고 SDK(예: Google IMA)에 의해  
  **가로채어질 수 있으며**, `displayClick`은 호출되지 않거나 `reason: "none"` 으로 표시될 수 있습니다.

- iOS Safari는 터치 기반으로 동작하므로, **이벤트가 터치 엔드 시점**에 발생하며  
  클릭 타이밍이 약간 지연될 수 있습니다.

### 특징

- JW Player에서 “**사용자가 직접 재생 화면을 클릭했다**” 는 사실을 감지하는 유일한 이벤트.

- `play`/`pause` 이벤트는 상태 변화를 의미하지만,  
  `displayClick`은 **그 상태 변화를 유발한 “사용자 행동” 자체**를 의미합니다.

- 사용자 인터랙션 기반 UI (예: 클릭 시 광고 배너 표시, 클릭으로 자막 토글 등) 제작에 적합.

- 자동재생·광고·추천(related) 전환 시에는 발생하지 않으므로,  
  “명확한 사용자 의도 기반 트리거”로 활용할 수 있습니다.
