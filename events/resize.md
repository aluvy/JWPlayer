# Resize Events

These API calls are used to retrieve and update the current player dimensions and fullscreen state.

<br>
<br>

# .on('fullscreen')

Fires when the player toggles to/from fullscreen

### 호출시점

- 플레이어가 **전체화면 모드(Fullscreen Mode)** 로 **진입하거나 해제될 때** 발생합니다.

- 즉, 사용자가 전체화면 버튼을 클릭하거나,  
  스크립트로 `player.setFullscreen(true/false)` 를 호출할 때 트리거됩니다.

#### 일반 호출 흐름

```
사용자 클릭 or API 호출
→ fullscreen (fullscreen: true)
→ 사용자 ESC / 버튼 클릭 / player.setFullscreen(false)
→ fullscreen (fullscreen: false)

```

> `fullscreen: true` → 전체화면 진입
> `fullscreen: false` → 전체화면 해제

### 이벤트 객체 구조 (콜백 파라미터)

```json
{
  "fullscreen": true,
  "type": "fullscreen"
}
```

| Value                    | Description                                    |
| :----------------------- | :--------------------------------------------- |
| **fullscreen** (boolean) | 현재 전체화면 상태 (`true`=진입, `false`=해제) |

### 활용

#### 1. UI 동기화

- 전체화면 전환 시 CSS 레이아웃이나 버튼 스타일 변경.

```javascript
player.on('fullscreen', (e) => {
  if (e.fullscreen) {
    document.body.classList.add('is-fullscreen');
  } else {
    document.body.classList.remove('is-fullscreen');
  }
});
```

#### 2. 가로/세로 화면 제어 (특히 모바일)

- 모바일 웹(iOS Safari, Android Chrome 등)에서는  
  `fullscreen` 진입 시 강제로 가로모드로 전환하는 로직 구성 가능:

```javascript
player.on('fullscreen', (e) => {
  if (e.fullscreen && screen.orientation) {
    screen.orientation.lock('landscape').catch(() => {});
  } else {
    screen.orientation.unlock();
  }
});
```

#### 3. 로그 및 사용 분석

- 사용자가 전체화면 기능을 얼마나 자주 사용하는지,  
  혹은 해제 타이밍을 분석해 UI 최적화.

```javascript
player.on('fullscreen', (e) => {
  sendAnalytics('fullscreen_toggle', { state: e.fullscreen });
});
```

#### 4. 광고, 오버레이, 인터랙션 제어

- 전체화면 상태에서는 광고 배너, 자막, 컨트롤 레이아웃 등 조정 필요:

```javascript
player.on('fullscreen', (e) => {
  toggleOverlayVisibility(!e.fullscreen);
});
```

### 주의사항

- **모바일 Safari(iOS)** 는 네이티브 전체화면 API(`webkitEnterFullscreen`)를 사용하기 때문에,  
  JW Player의 `fullscreen` 이벤트가 일부 시점에서 **지연되거나 중복 발생**할 수 있습니다.

- `player.setFullscreen()` **호출 직후 곧바로 상태를 확인하지 말고,**  
  반드시 `fullscreen` 이벤트를 감지하여 상태 변화를 처리해야 합니다.

- `fullscreen`은 **플레이어 컨테이너 기준의 전체화면**이며,  
  브라우저 자체의 “전체 페이지 확대(⌘+Ctrl+F)”는 감지하지 않습니다.

- 일부 브라우저(특히 iOS 17 이전 Safari)는  
  **CSS 기반 fullscreen** 모드(`object-fit`, `position: fixed`) 와 JWPlayer 내장 모드가 충돌할 수 있으므로
  UI 레벨 제어 시 주의 필요.

- 광고(IMA, VAST 등)가 전체화면에서 자동 재배치되는 경우,  
  광고 SDK 내부 `fullscreen` 이벤트와 혼동하지 않도록 해야 합니다.

### 특징

- **플레이어 전체화면 전환을 감지하는 유일한 이벤트.**

- UI, 레이아웃, 디바이스 회전 제어 등 다양한 환경 제어의 기준이 됨.

- `fullscreen: true/false` 로 단순하지만, **플랫폼별 동작 타이밍이 다를 수 있음**  
  (특히 iOS Safari는 별도 렌더 레이어를 생성).

- 사용자 경험(UX) 측면에서는 **“몰입 모드 전환”의 기준 이벤트**로,  
  UI/컨트롤러 동작을 조정하기에 가장 적합.

<br>
<br>

# .on('resize')

Fires when the player's on-page dimensions have changed

> The `.on('resize')` event is not fired in response to a fullscreen change.

### 호출시점

- 플레이어 컨테이너의 **크기(width/height)가 변경될 때마다** 발생합니다.

- 즉, 다음과 같은 상황에서 호출됩니다:

  1. **브라우저 창 크기 변경 (window resize)**

  2. **반응형 레이아웃(media query 등)에 따른 플레이어 크기 변경**

  3. **전체화면 모드 진입 또는 해제 (`fullscreen` 연동)**

  4. **API로 `player.resize(width, height)` 호출 시**

#### 이벤트 호출 순서 예시:

```
fullscreen(true)
→ resize
→ fullscreen(false)
→ resize

```

또한, JW Player 내부적으로 영상의 **aspectratio(비율)** 이 자동 조정될 때도 트리거됩니다.

### 이벤트 객체 구조 (콜백 파라미터)

```json
{
  "width": 1280,
  "height": 720,
  "type": "resize"
}
```

| Value               | Description                           |
| :------------------ | :------------------------------------ |
| **width** (number)  | 변경 후 플레이어의 실제 너비(px 단위) |
| **height** (number) | 변경 후 플레이어의 실제 높이(px 단위) |

### 활용

#### 1. 반응형 UI/레이아웃 제어

- 플레이어 크기에 맞게 자막, 오버레이, 버튼, 광고 배너 등의 위치를 재조정.

```javascript
player.on('resize', (e) => {
  console.log(`JW Player resized → ${e.width}x${e.height}`);
  adjustOverlayLayout(e.width);
});
```

#### 2. 고정 비율(Aspect Ratio) 동기화

- `aspectratio: '16:9'` 옵션과 함께 사용하면,  
  플레이어 리사이즈 시 비율을 유지하면서 내부 컴포넌트도 자동 조정 가능:

```javascript
player.on('resize', (e) => {
  const ratio = (e.width / e.height).toFixed(2);
  console.log(`현재 비율: ${ratio}`);
});
```

#### 3. 전체화면 / 축소 시 UI 재배치

- 전체화면 진입 시 인터페이스 변경, 해제 시 복원.

```javascript
player.on('resize', (e) => {
  if (e.width > window.innerWidth * 0.9) {
    document.body.classList.add('jw-fullscreen-ui');
  } else {
    document.body.classList.remove('jw-fullscreen-ui');
  }
});
```

#### 4. 광고 및 오버레이 반응형 대응

- 광고 UI 또는 오버레이가 플레이어 크기에 따라 깨지지 않도록  
  `resize` 이벤트에서 위치 및 크기 재계산:

```javascript
player.on(
  'resize',
  debounce(() => repositionAdOverlay(), 300)
);
```

### 주의사항

- **이벤트 발생 빈도가 높습니다.**  
  → 브라우저 리사이즈 중 연속 호출되므로, 반드시 `debounce` 또는 `throttle` 처리 필요.

```javascript
let timer;
player.on('resize', (e) => {
  clearTimeout(timer);
  timer = setTimeout(() => handleResize(e.width, e.height), 200);
});
```

- width / height 값은 **픽셀 단위 정수**로 반환되며,  
  CSS 비율(`%`, `vw`, `vh`)로 지정해도 최종 계산된 픽셀 값이 들어옵니다.

- **iFrame 임베드 환경**에서는 부모 컨테이너 리사이즈 시  
  이벤트가 정확히 동기화되지 않을 수 있습니다.

- `fullscreen` 이벤트와 거의 동시에 발생할 수 있으므로,  
  두 이벤트를 함께 사용하는 경우 이벤트 순서에 의존하지 말고 상태값(`player.getFullscreen()`)으로 확인하는 것이 안전합니다.

- 광고 재생 중(`adPlaying`)에는 일부 광고 SDK가 JW Player의 `resize` 이벤트를 가로채거나 중복 발생시킬 수 있습니다.

### 특징

- **플레이어 크기 변화를 감지하는 유일한 이벤트**.

- 반응형 페이지, 전체화면 UI, 오버레이 위치, 광고 레이아웃 조정 등에서 핵심적으로 활용됨.

- `fullscreen`, `viewable`, `containerViewable` 등 **디스플레이 관련 이벤트들과 연동**되어 동작.

- 내부 렌더링 엔진이 리사이징 완료 후 호출되므로, DOM 조정 시점으로도 안전함.

- 영상 재생 상태(`playing`, `paused`, `idle`)와 무관하게 발생할 수 있음.
