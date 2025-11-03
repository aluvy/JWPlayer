# Volume Events

<br>
<br>

# .on('mute')

Fires when the player has gone in or out of a mute state

### 호출시점

- 플레이어의 **음소거 상태(Muted)가 변경될 때** 발생합니다.
- 즉, 사용자가 **음소거 버튼을 클릭하거나**,
  스크립트로 `player.setMute(true/false)` 를 실행했을 때 호출됩니다.
- 또한 자동재생(`autostart: true`)이 음소거 조건에서 허용될 때(`player.setMute(true)` 자동 적용됨)에도 트리거됩니다.
- 이벤트는 **음소거 ON/OFF 전환 시마다 한 번씩** 발생합니다.

### 이벤트 객체 구조 (콜백 파라미터)

```json
{
  "mute": true,
  "type": "mute"
}
```

| Value              | Description                                                   |
| :----------------- | :------------------------------------------------------------ |
| **mute** (boolean) | 현재 음소거 상태 (`true` = 음소거됨, `false` = 음소거 해제됨) |

### 활용

#### 1. UI 상태 반영

- 음소거 상태에 따라 아이콘·버튼·툴팁 변경.

```javascript
player.on('mute', (e) => {
  const icon = document.querySelector('#muteIcon');
  icon.classList.toggle('muted', e.mute);
});
```

#### 2. 사용자 행동 분석

- 사용자가 음소거를 얼마나 자주 켜거나 끄는지 트래킹하여
  콘텐츠/광고 볼륨 전략에 반영:

```javascript
player.on('mute', (e) => {
  sendAnalytics('volume_change', { muted: e.mute });
});
```

#### 3. 자동재생(Autostart) 대응

- 브라우저 자동재생 정책 상, 음소거 상태에서만 재생이 허용되는 경우:  
  → 자동재생 허용 환경에서 음소거 상태일 때 재생 자동 시작.

```javascript
player.on('mute', (e) => {
  if (e.mute && !player.getState().includes('playing')) player.play();
});
```

#### 4. 환경 유지 (사용자 선호도 저장)

- 다음 재생 시 동일한 음소거 상태로 시작 가능.

```javascript
player.on('mute', (e) => {
  localStorage.setItem('muteState', e.mute);
});
```

### 주의사항

- `mute` 이벤트는 **상태 전환 시점에만 호출**되며,  
  동일 상태(`setMute(true)` → `true` 다시 설정)에서는 호출되지 않습니다.
- 음소거/해제 시점은 **볼륨 변경(`volume`) 이벤트와 다릅니다.**
  - `mute` = 소리를 완전히 끄거나 켜는 행위
  - `volume` = 볼륨 크기(0~100%) 변경
- 브라우저 정책에 따라 자동재생 시 플레이어가 강제로 `mute: true` 상태로 시작할 수 있으며,  
  이때도 `mute` 이벤트가 발생할 수 있습니다.
- 광고(Ad) 재생 중에는 별도의 음소거 제어 로직이 있을 수 있으므로,  
  광고 모듈의 `adMute` 이벤트와 혼동하지 않도록 주의하세요.
- 모바일 환경(iOS Safari 등)은 시스템 볼륨 제어와 JW 내부 `mute` 상태가 일치하지 않을 수 있습니다.

### 특징

- **음소거 상태 변화를 감지하는 유일한 이벤트.**
- 재생 경험(Autoplay, UX) 제어에 직접적으로 연결되는 핵심 이벤트.
- `volume` 이벤트와 함께 사용자 오디오 인터랙션 분석에 필수.
- 광고 환경(특히 프리롤)에서 **정책적 음소거 자동 적용 감지** 용도로 자주 활용됨.

<br>
<br>

# .on('volume')

Fires when the player's volume is changed

### 호출시점

- 플레이어의 **볼륨(Volume) 크기가 변경될 때** 발생합니다.
- 즉, 다음과 같은 상황에서 호출됩니다:
  1. 사용자가 볼륨 슬라이더를 조절했을 때
  2. `player.setVolume(value)` (0~100) 으로 볼륨을 변경했을 때
  3. 음소거 해제 후 자동으로 이전 볼륨 복원 시 (`mute: false` → 이전 값)
  4. 초기 설정(`volume: <number>`)이 동적으로 변경될 때
- 발생 시점은 **볼륨 수치가 실제 반영된 직후**입니다.
- `mute` 이벤트와 다르게, **음소거 여부와 상관없이 볼륨 크기 자체가 변경되면 호출**됩니다.

### 이벤트 객체 구조 (콜백 파라미터)

```json
{
  "volume": 35,
  "type": "volume"
}
```

| Value               | Description               |
| :------------------ | :------------------------ |
| **volume** (number) | 현재 볼륨 값 (0–100 범위) |

### 활용

### 1. UI 동기화

- 볼륨 슬라이더나 숫자 표시 갱신.

```javascript
player.on('volume', (e) => {
  document.querySelector('#volumeDisplay').textContent = `${e.volume}%`;
});
```

### 2. 사용자 선호도 저장

- 다음 영상에서도 동일한 볼륨으로 재생 가능.

```javascript
player.on('volume', (e) => {
  localStorage.setItem('lastVolume', e.volume);
});
```

### 3. 자동 음량 정책 적용

- 특정 볼륨 이하일 경우 자동 음소거:

```javascript
player.on('volume', (e) => {
  if (e.volume === 0) player.setMute(true);
});
```

### 4. 사용자 행동 분석

- “볼륨 상호작용” 이벤트로 취급하여 UX 분석, 광고 음량 제어, 시청 환경 모니터링 등에 활용.

### 주의사항

- **볼륨 0과 mute(true)는 다릅니다.**
  - `volume: 0` → 볼륨 슬라이더를 내렸지만 음소거 버튼은 활성화되지 않음.
  - `mute: true` → 완전 음소거 상태로 `volume` 이벤트가 발생하지 않음.
- 같은 볼륨 값을 반복 설정해도(`setVolume(50)` → 다시 `50`) 이벤트는 발생하지 않습니다.
- 일부 모바일 브라우저(iOS Safari 등)는 시스템 볼륨과 웹 오디오 볼륨이 연동되지 않아  
  사용자가 기기 버튼으로 조절해도 이벤트가 발생하지 않을 수 있습니다.
- 광고 중 볼륨 조절이 제한되어 있을 수 있으며,  
  광고 SDK(예: IMA, FreeWheel)에서는 `volume` 이벤트가 발생하지 않습니다.
- `player.getVolume()` 으로 언제든 현재 값 확인 가능.

### 특징

- **음량 크기 변경을 감지하는 유일한 이벤트.**
- `mute` 이벤트와 짝을 이루며, 오디오 상태 변화를 세밀하게 추적 가능.
- 사용자 선호 볼륨(UX), 광고 볼륨 조절, 접근성(Accessibility) 기능 구현에 필수.
- 값이 단순(`0–100`)하고 호출 빈도도 낮아 **가벼운 UI 업데이트용 이벤트**로 적합.
