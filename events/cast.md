# Cast Events

This API call allows a developer to listen for cast events.

<br>
<br>

# .on('cast')

This API call allows a developer to listen for cast events.

### 호출시점

- `cast` 이벤트는 **플레이어의 Cast 상태(Chromecast, AirPlay 등)** 가 **시작되거나 종료될 때** 발생합니다.

- 즉, **플레이어가 외부 디바이스로 스트림을 전송하거나 연결을 끊을 때** 트리거됩니다.

#### 발생 조건

1. 사용자가 JW Player의 **Cast 버튼(Chromecast 아이콘)** 을 클릭 → 연결 시도 성공

2. 플레이어가 **Cast 모드로 전환**되어 미디어를 외부 기기(Chromecast, Cast-enabled TV)로 전송 시작

3. 사용자가 **Cast 세션 종료** 버튼 클릭 → JW Player가 다시 로컬 재생으로 복귀

#### 일반적인 흐름

```
cast (active: true)   ← Chromecast 연결 시작
→ play / buffer / firstFrame (외부 재생)
→ cast (active: false) ← Cast 세션 종료
→ idle / play (로컬 재생 복귀)
```

> 즉, `cast` 이벤트는 “**플레이어의 출력 경로 변경 상태**”를 알려주는 이벤트입니다.

### 이벤트 객체 구조 (콜백 파라미터)

```json
{
  "active": true,
  "available": true,
  "deviceName": "Living Room TV",
  "type": "cast"
}
```

| Value                   | Description                                              |
| :---------------------- | :------------------------------------------------------- |
| **active** (boolean)    | `true` → 현재 Cast 모드 활성화, `false` → Cast 세션 종료 |
| **available** (boolean) | Cast 장치 탐색 가능 여부 (`true`면 사용 가능)            |
| **deviceName** (string) | 연결된 기기의 이름 (예: `"Living Room TV"`)              |

> `available`은 브라우저와 네트워크에 Cast 기기가 탐지되었는지 여부를 나타내며,  
> `active`는 실제로 세션이 연결된 상태를 의미합니다.

### 활용

#### 1. Cast 세션 상태 모니터링

- 사용자 Cast 연결/해제 시 로그 또는 UI 업데이트.

```javascript
player.on('cast', (e) => {
  if (e.active) {
    console.log(`Cast 시작: ${e.deviceName}`);
  } else {
    console.log('Cast 세션 종료');
  }
});
```

#### 2. UI 표시 제어 (Cast 모드 안내)

- 사용자에게 현재 재생 경로(기기)를 안내.

```javascript
player.on('cast', (e) => {
  const notice = document.querySelector('.cast-notice');
  notice.textContent = e.active ? `현재 ${e.deviceName}에서 재생 중` : `Cast 연결이 해제되었습니다.`;
});
```

#### 3. Cast 사용 가능 상태 감지

- Cast 기기 탐색 가능 시 버튼 활성화, 없을 시 숨김 처리.

```javascript
player.on('cast', (e) => {
  if (e.available) {
    document.querySelector('.cast-btn').classList.remove('hidden');
  } else {
    document.querySelector('.cast-btn').classList.add('hidden');
  }
});
```

#### 4. 재생 흐름 제어

- Cast 종료 후 자동으로 로컬 재생 재개:

```javascript
player.on('cast', (e) => {
  if (!e.active) player.play();
});
```

### 주의사항

- `cast` 이벤트는 **Chrome 및 Android WebView 환경에서만 완전 지원**됩니다.  
  Safari(iOS/macOS)는 Chromecast가 아닌 AirPlay를 사용하므로 구조가 다릅니다.

- **`available` = true**는 Cast 장치가 탐지된 상태를 의미하며,  
  실제 연결(`active`)과는 다릅니다.

- **Cast 모드 중에는 JW Player가 로컬 재생을 중단**하고,  
  미디어 제어(재생/정지/볼륨 등)가 **Chromecast 세션을 통해 전달**됩니다.

- `active: true` 상태에서는 **`time`, `bufferChange`, `visualQuality` 등의 이벤트 빈도**가 감소하거나 지연됩니다.  
  (Cast SDK가 프록시로 재생 상태를 전달하기 때문)

- `cast` 이벤트는 **동일한 상태가 반복되어도 호출되지 않습니다.**  
  즉, `active: true` → `true` 유지 시에는 새 이벤트 발생하지 않음.

- **광고(ad)** 재생 중 Cast 연결을 시도하면 SDK에서 **Cast 지원 불가 오류**가 발생할 수 있음  
  (IMA SDK가 Chromecast 세션을 인계받지 못하는 경우).

### 특징

- **플레이어의 출력 경로(로컬 ↔ 외부기기)** 변화를 감지하는 유일한 이벤트.

- `active`와 `available`을 함께 사용하면 **“Cast 버튼 표시 → 연결 성공 → 연결 종료”** 흐름을 완벽히 제어 가능.

- **실제 재생 상태(`playing`, `paused`)와 별개로, “어디서 재생 중인지”를 알려주는 메타 이벤트.**

- OTT, 교육, 프리미엄 스트리밍 서비스 등에서 **“디바이스 전환 UX”** 구현 시 핵심.

<br>
<br>

# .on('castIntercepted')

Fired when [interceptCast](https://docs.jwplayer.com/players/reference/casting) is enabled, and a customer tries to cast content.

This event also indicates whether a cast session is currently active.

### 호출시점

- `castIntercepted` 이벤트는 **사용자가 브라우저(Chrome)의 네이티브 Cast 버튼을 직접 클릭했을 때**,  
  JW Player가 **그 Cast 요청을 가로채서 자사 플레이어 세션으로 처리할 때** 발생합니다.

#### 정확한 발생 조건

1. 사용자가 브라우저의 기본 Cast 메뉴(오른쪽 상단 Cast 아이콘 또는 OS Cast 메뉴) 클릭

2. Cast 대상 기기를 선택

3. JW Player가 감지하여 **외부 네이티브 Cast 세션 대신 JW Player Cast 모듈로 연결을 전환할 때**

4. 그 즉시 `castIntercepted` 이벤트 트리거

#### 흐름 예시

```
사용자: Chrome Cast 아이콘 클릭 → 기기 선택
→ JW Player: 외부 요청 감지
→ castIntercepted (플레이어가 Cast 세션 인계받음)
→ cast (active: true)
```

> 즉, **JW Player가 네이티브 Cast 요청을 "가로채어" 자체 Cast 로직으로 연결할 때 발생하는 초기 신호**입니다.

### 이벤트 객체 구조 (콜백 파라미터)

```json
{
  "type": "castIntercepted"
}
```

> 별도의 추가 필드는 없습니다.  
> 이 이벤트는 "Cast 요청을 감지함" 자체만 알려주며,  
> 실제 연결 성공 여부는 이후 cast 이벤트(`active: true`)에서 확인할 수 있습니다.

### 활용

#### 1. Cast 세션 시작 트래킹

- 사용자가 브라우저 기본 Cast UI를 통해 연결을 시도했는지 추적 가능.

```javascript
player.on('castIntercepted', () => {
  console.log('JW Player가 네이티브 Cast 요청을 감지했습니다.');
  sendAnalytics('cast_intercepted', { time: Date.now() });
});
```

#### 2. UI 안내 메시지 표시

- 사용자가 외부에서 Cast 버튼을 눌렀을 때 즉시 피드백 제공.

```javascript
player.on('castIntercepted', () => {
  showToast('Cast 연결을 설정 중입니다...');
});
```

#### 3. Cast 모듈 초기화 로직과 연결

- `castIntercepted` → 내부 Cast 초기화 중

- `cast (active: true)` → 연결 완료
  따라서:

```javascript
player.on('castIntercepted', () => {
  startCastInitUI();
});
player.on('cast', (e) => {
  if (e.active) completeCastInitUI();
});
```

### 주의사항

- `castIntercepted`는 **브라우저 또는 OS의 Cast 요청을 JW Player가 가로챘을 때만 발생**합니다.
  즉, **플레이어 내부 Cast 버튼을 눌렀을 때는 호출되지 않습니다.**

- `castIntercepted`가 발생한다고 해서 **항상 연결이 성공하는 것은 아닙니다.**

  - 연결 실패 시 `cast (active: false)` 또는 아무 추가 이벤트도 발생하지 않을 수 있습니다.

- 이 이벤트는 Chrome 환경(또는 Chromecast SDK 지원 브라우저)에서만 감지됩니다.  
  Safari, Firefox, Edge 등에서는 호출되지 않습니다.

- IMA 광고 중 브라우저 Cast 요청이 감지되면,  
  JW Player는 광고를 중단하고 본편 Cast 세션으로 전환을 시도할 수 있으므로 **UX 제어 필요.**

- `castIntercepted`는 매우 짧은 순간(약 100ms 내외)에 호출되며,  
  `cast` 이벤트가 곧이어 발생하므로, **로딩 UI나 로깅 이외의 로직은 최소화**하는 것이 좋습니다.

### 특징

- **JW Player가 외부 Cast 요청을 인지하고 자체 세션으로 인계받았음을 알리는 이벤트.**

- `cast` 이벤트보다 앞서 발생하며, 연결 시도 타이밍을 정확히 잡을 수 있음.

- 사용자 행동이 “내부 버튼 클릭”이 아닌 “외부 브라우저 UI를 통한 Cast 요청”인 경우를 구분 가능.

- OTT, 방송 플랫폼 등에서 **“디바이스 연결 출처 구분(내부 vs 외부)”** 로깅에 활용됨.
