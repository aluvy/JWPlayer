# Related Events

<br>
<br>

# .on('relatedReady')

Indicates event that triggers when the Related plugin is ready. This is the earliest point at which any API calls should be made.

Returns an object with the following:

### 호출시점

- `relatedReady` 이벤트는 **Related 플러그인(추천 영상 기능)** 이  
  **추천 콘텐츠(related playlist)** 를 불러오고 DOM 렌더링 준비가 끝났을 때 발생합니다.

- 즉, “추천 콘텐츠가 성공적으로 로드되어 표시할 수 있는 상태”가 되었을 때 호출됩니다.

- 일반적으로 다음과 같은 순서로 이벤트가 발생합니다:

```
영상 재생 완료 (complete)
→ relatedReady  (추천 콘텐츠 로드 완료)
→ relatedOpen   (추천 콘텐츠 오버레이 표시)
→ relatedClose  (추천 콘텐츠 닫힘)

```

#### 트리거 조건

- `related` 플러그인이 활성화되어 있을 때 (`related: { file: ... }` or `related: true`)

- 추천 콘텐츠(playlist)가 성공적으로 로드된 경우

### 이벤트 객체 구조 (콜백 파라미터)

```json
{
  "playlist": [
    {
      "title": "다음 추천 영상 1",
      "mediaid": "abc123",
      "link": "https://example.com/next1",
      "image": "https://example.com/thumb1.jpg"
    },
    {
      "title": "다음 추천 영상 2",
      "mediaid": "def456",
      "link": "https://example.com/next2",
      "image": "https://example.com/thumb2.jpg"
    }
  ],
  "type": "relatedReady"
}
```

| Value                        | Description                                               |
| :--------------------------- | :-------------------------------------------------------- |
| **playlist** (Array<Object>) | 추천 콘텐츠 목록 (각 항목은 JW Playlist Item 구조와 동일) |

### 활용

#### 1.추천 콘텐츠 로딩 후 UI 조정

- 로딩된 추천 콘텐츠 수에 따라 UI 표시 여부 조정.

```javascript
player.on('relatedReady', (e) => {
  console.log('추천 콘텐츠 개수:', e.playlist.length);
  if (e.playlist.length === 0) {
    document.querySelector('.related-section').style.display = 'none';
  }
});
```

#### 2.추천 데이터 커스터마이징

- 로드된 리스트를 가공하거나, 특정 조건의 콘텐츠만 필터링:

```javascript
player.on('relatedReady', (e) => {
  const filtered = e.playlist.filter((item) => item.title.includes('KBS'));
  renderCustomRelatedList(filtered);
});
```

#### 3.분석(Analytics) / 트래킹

- 추천 콘텐츠가 언제 준비되었는지, 몇 개가 로드되었는지 추적 가능:

```javascript
player.on('relatedReady', (e) => {
  sendAnalytics('related_ready', { count: e.playlist.length });
});
```

#### 4. 자동 추천 재생(Autoplay Next) 준비

- 추천 콘텐츠 로드 완료 시 다음 영상 자동 재생 예약:

```javascript
player.on('relatedReady', (e) => {
  if (e.playlist.length > 0) {
    setTimeout(() => player.load(e.playlist[0]), 3000);
  }
});
```

### 주의사항

- `related` **플러그인이 활성화되어 있지 않으면** 이벤트가 발생하지 않습니다.  
  (즉, `related` 설정이 `false`이거나 미지정 시 무효)

- 추천 리스트 로딩 실패 시 (`404`, JSON 파싱 에러 등)  
  `relatedReady`는 호출되지 않으며, 대신 내부 `relatedError` 이벤트가 발생합니다.

- `relatedReady`는 **추천 데이터 로드가 완료된 시점**이지,  
  오버레이(`relatedOpen`)가 실제로 사용자에게 표시된 시점은 아닙니다.  
  → 두 이벤트를 혼동하지 말 것.

- 추천 콘텐츠가 캐싱되어 있을 경우,  
  `relatedReady`가 매우 빠르게 (즉시) 발생할 수 있으므로  
  UI 초기화 로직은 항상 **비동기 안전하게 구**성해야 합니다.

- 라이브 스트림에서는 `related` 기능이 비활성화되는 경우가 많습니다.

### 특징

- **추천(related) 콘텐츠 로딩 완료를 감지하는 유일한 이벤트.**

- `relatedOpen` 은 “표시됨”, `relatedReady` 는 “데이터 준비됨”에 해당.

- UI/UX 및 추천 알고리즘 동기화 시 **가장 안정적인 후킹 포인트**.

- 추천 플레이리스트를 직접 커스터마이징하거나,  
  외부 API와 결합하여 “맞춤형 추천 콘텐츠” 렌더링할 때 필수적으로 사용됨.

<br>
<br>

# ("related").on('open')

Triggers when the recommendations interface is opened

### 호출시점

- relatedOpen 이벤트는 **추천(related) 콘텐츠 오버레이가 실제로 열릴 때** 발생합니다.

- 즉, `relatedReady`(추천 리스트 로드 완료) 이후,  
  **추천 썸네일/목록 UI가 플레이어 위에 표시되는 순간** 트리거됩니다.

#### 일반적인 이벤트 순서

```
complete (영상 종료)
→ relatedReady (추천 리스트 로드 완료)
→ relatedOpen (추천 오버레이 표시됨)
→ relatedClick (사용자가 추천 아이템 클릭)
→ relatedClose (추천 오버레이 닫힘)

```

> `relatedOpen`은 “데이터 준비”가 아니라
> “UI가 실제 표시되는 시점”을 의미합니다.

### 이벤트 객체 구조 (콜백 파라미터)

```json
{
  "method": "autoplay",
  "type": "relatedOpen"
}
```

| Value               | Description                                                       |
| :------------------ | :---------------------------------------------------------------- |
| **method** (string) | 추천 오버레이가 열린 원인 (`"autoplay"`, `"click"`, `"complete"`) |

- 예시:
  - `"complete"` → 영상 재생 완료 후 자동 표시
  - `"click"` → 사용자가 수동으로 관련 콘텐츠 버튼 클릭
  - `"autoplay"` → 자동 재생 준비 중 표시된 경우

### 활용

#### 1. 추천 콘텐츠 오버레이 표시 감지

- 오버레이가 표시된 상태에서 UI 전환, 배경 블러, 기타 효과 적용.

```javascript
player.on('relatedOpen', (e) => {
  console.log(`추천 오버레이 표시됨 (원인: ${e.method})`);
  document.body.classList.add('related-active');
});
```

#### 2. 재생 완료 후 추천 노출 트래킹

- “시청 완료 후 추천 UI가 실제로 노출되었는가”를 측정:

```javascript
player.on('relatedOpen', (e) => {
  if (e.method === 'complete') {
    sendAnalytics('related_open_after_complete');
  }
});
```

#### 3. 자동재생 / 다음 영상 준비 시점 제어

- 자동재생(`autoplay`) 모드에서 추천이 열리면 다음 영상 카운트다운 표시:

```javascript
player.on('relatedOpen', (e) => {
  if (e.method === 'autoplay') startNextCountdown();
});
```

#### 4. UX 최적화

- 추천 콘텐츠 오버레이가 열릴 때 컨트롤 버튼을 숨기거나,  
  플레이어 주변 요소 레이아웃 조정 가능:

```javascript
player.on('relatedOpen', () => {
  document.querySelector('.player-controls').style.opacity = 0;
});
```

### 주의사항

- `relatedOpen`은 `relatedReady` 이후에만 발생합니다.  
  (데이터가 로드되지 않으면 오버레이가 열리지 않음)

- 오버레이가 이미 열려 있는 상태에서 다시 호출되면 **중복 이벤트가 발생하지 않습니다.**

- `related` 플러그인이 비활성화되어 있거나(`related: false`),  
  `related.file` 설정이 없으면 이벤트가 발생하지 않습니다.

- 추천 콘텐츠의 표시 방식(`oncomplete`, `onclick`, `autoplay`)에 따라  
  `method` 값이 달라집니다 → **조건 분기 시 필수 확인 항목.**

- 모바일(iOS Safari 등)에서는 전체화면 모드에서 `relatedOpen`이  
  비정상적으로 호출되지 않거나 지연될 수 있습니다.  
  (JW Player가 네이티브 전체화면을 사용하기 때문)

### 특징

- `related` 플러그인의 **UI 표시 상태 변화**를 감지하는 핵심 이벤트.

- `relatedReady`가 "데이터 준비",  
  `relatedOpen`이 "시각적 표시 시작"을 의미.

- **사용자 인터랙션, 자동재생, 추천 전략 분석**에 활용 가능.

- `relatedClose` 이벤트와 짝을 이루며,  
  두 이벤트를 통해 “추천 노출 → 닫힘” 전체 세션을 추적할 수 있음.

- 특히 OTT/뉴스 사이트에서는 “추천 노출률(View Rate)”을 계산할 때  
  가장 많이 사용되는 이벤트입니다.

<br>
<br>

# ("related").on('close')

Triggers when the recommendations interface is closed

### 호출시점

- `relatedClose` 이벤트는 **추천(related) 콘텐츠 오버레이가 닫힐 때** 발생합니다.

- 즉, `relatedOpen`을 통해 표시된 **추천 목록 UI가 사라지는 시점**입니다.

#### 주요 트리거 조건

1. 사용자가 **“닫기(X)” 버튼을 클릭**했을 때

2. 사용자가 **추천 썸네일을 클릭하여 새 콘텐츠 재생이 시작될 때**

3. 또는 `player.playlistItem(...)`, `load()` 등을 호출해 새 재생이 시작될 때

4. `autoplay`로 다음 영상이 자동 재생되어 오버레이가 닫힐 때

#### 일반적인 이벤트 흐름

```
complete
→ relatedReady (추천 데이터 로드)
→ relatedOpen  (추천 오버레이 표시)
→ relatedClick (사용자가 추천 항목 클릭)
→ relatedClose (오버레이 닫힘)
→ playlistItem / play (새 영상 재생 시작)

```

### 이벤트 객체 구조 (콜백 파라미터)

```json
{
  "method": "click",
  "type": "relatedClose"
}
```

| Value               | Description                                                             |
| :------------------ | :---------------------------------------------------------------------- |
| **method** (string) | 오버레이가 닫힌 원인 (`"click"`, `"autoplay"`, `"playlist"`, `"close"`) |

- `"click"` → 추천 썸네일 클릭으로 닫힘 (다음 영상 재생 시작)
- `"autoplay"` → 자동재생으로 닫힘
- `"playlist"` → 수동 playlistItem 변경으로 닫힘
- `"close"` → 사용자가 닫기 버튼을 눌러 직접 닫음

### 활용

#### 1. 추천 오버레이 종료 후 UI 복원

- 추천 창이 닫힐 때 페이지 레이아웃, 컨트롤러, 배경 블러 등을 원래 상태로 되돌림.

```javascript
player.on('relatedClose', (e) => {
  document.body.classList.remove('related-active');
  console.log(`추천 오버레이 닫힘 (원인: ${e.method})`);
});
```

#### 2. 사용자 행동 분석

- 어떤 이유로 추천 콘텐츠가 닫혔는지 추적 가능:
- → 예:
  - `method: "click"` → 추천 영상 클릭 → 재방문 유도 성공
  - `method: "close"` → 사용자가 닫기만 함 → 추천 콘텐츠 무시

```javascript
player.on('relatedClose', (e) => {
  sendAnalytics('related_close', { reason: e.method });
});
```

#### 3. 추천 세션 관리

- 추천 영역이 열리고 닫히는 주기를 기록해,  
  사용자 세션 내 추천 노출률·체류 시간 분석 가능:

```javascript
let openTime;
player.on('relatedOpen', () => (openTime = Date.now()));
player.on('relatedClose', () => {
  const duration = (Date.now() - openTime) / 1000;
  sendAnalytics('related_view_duration', { seconds: duration });
});
```

#### 4. 자동재생 전환 처리

- `autoplay`로 닫힐 경우, 다음 영상 재생에 맞춰 애니메이션이나  
  카운트다운 UI를 제거:

```javascript
player.on('relatedClose', (e) => {
  if (e.method === 'autoplay') removeCountdownUI();
});
```

### 주의사항

- `relatedClose`는 `relatedOpen` **이후에만 호출**됩니다.  
  (`related` 플러그인이 로드되지 않으면 이벤트 발생 X)

- `relatedClose`는 **추천 오버레이 UI가 완전히 사라진 후**에 트리거되므로,  
  애니메이션과 맞물릴 경우 타이밍 차이가 있을 수 있습니다.

- **광고(Ad)와 동시에 닫힘이 발생**하면,  
  광고 SDK에 의해 `relatedClose` 호출이 지연될 가능성이 있습니다.

- `method` 값은 사용자의 행동을 직접적으로 나타내므로
  **로그 및 분석 시 필수적으로 함께 수집**해야 합니다.

- 모바일(iOS Safari)에서는 `relatedClose` 이벤트가  
  전체화면 모드 종료와 거의 동시에 발생할 수 있어,  
  UI 복원 로직은 `resize` 이벤트와 함께 처리하는 것이 안정적입니다.

### 특징

- `related` 플러그인에서 **“추천 화면 종료” 시점을 명확히 알려주는 유일한 이벤트.**

- `relatedOpen`과 한 쌍으로 동작하며,  
  두 이벤트를 조합하면 추천 노출의 **전체 세션 단위**를 파악할 수 있음.

- 특히 **사용자 전환 행동(추천 클릭 vs 닫기)** 을 구분할 수 있는 점이 핵심.

- UI 및 UX 상태 전환, 분석, 다음 재생 시나리오 연결에 유용.

- 광고·추천 통합 플레이어에서 **UX 일관성 유지 및 세션 분석의 기준 이벤트.**

<br>
<br>

# ("related").on('play')

Triggers when a user selects an object in a related feed.

### 호출시점

- `relatedPlay` 이벤트는 사용자가 **추천 콘텐츠(related item)를 클릭하여 재생이 시작될 때** 발생합니다.

- 즉, **추천 오버레이(related UI)** 에서 **썸네일을 클릭하거나 자동재생(autoplay)** 으로  
  다음 영상이 로드될 때 트리거됩니다.

#### 일반적인 호출 흐름

```
complete                     ← 영상 재생 종료
→ relatedReady               ← 추천 콘텐츠 데이터 로드 완료
→ relatedOpen                ← 추천 오버레이 표시
→ relatedClick               ← 추천 썸네일 클릭
→ relatedPlay                ← 선택된 추천 아이템 로드 시작
→ playlistItem               ← 새 콘텐츠 로드 완료
→ play / firstFrame          ← 재생 시작
```

즉, `relatedPlay`는 **“추천 콘텐츠를 선택하여 실제 재생이 개시되는 순간”** 에 발생합니다.  
(`relatedClick`은 클릭 동작만 감지하고, `relatedPlay`는 실제 재생 실행을 의미합니다.)

### 이벤트 객체 구조 (콜백 파라미터)

```json
{
  "auto": false,
  "item": {
    "mediaid": "abc123",
    "title": "추천 영상 제목",
    "link": "https://example.com/next-video",
    "image": "https://example.com/thumbnail.jpg"
  },
  "type": "relatedPlay"
}
```

| Value              | Description                                          |
| :----------------- | :--------------------------------------------------- |
| **auto** (boolean) | 자동재생 여부 (`true`=autoplay, `false`=사용자 클릭) |
| **item** (object)  | 재생될 추천 콘텐츠 정보 (JW Playlist Item 구조)      |

> `item` 객체는 `mediaid`, `title`, `image`, `file`, `link` 등  
> 일반적인 JWPlayer playlist item과 동일한 구조를 가집니다.

### 활용

#### 1. 추천 콘텐츠 클릭(또는 자동재생) 로깅

- 클릭 기반 추천 전환율(CTR), 자동재생 비율 등을 트래킹.

```javascript
player.on('relatedPlay', (e) => {
  console.log(`추천 콘텐츠 재생 시작: ${e.item.title}`);
  sendAnalytics('related_play', {
    title: e.item.title,
    mediaid: e.item.mediaid,
    auto: e.auto,
  });
});
```

#### 2. 자동재생(Auto Next) UX 제어

- 자동재생인 경우 별도 안내 배너 표시:

```javascript
player.on('relatedPlay', (e) => {
  if (e.auto) showToast(`다음 영상 자동 재생: ${e.item.title}`);
});
```

#### 3. 추천 세션 전환 관리

- 현재 재생 콘텐츠와 추천 콘텐츠 간 세션 구분:

```javascript
player.on('relatedPlay', (e) => {
  sessionStorage.setItem('prevVideoId', player.getPlaylistItem().mediaid);
  sessionStorage.setItem('nextVideoId', e.item.mediaid);
});
```

#### 4. UI 갱신 및 사용자 전환 표시

- 추천 재생이 시작되면 오버레이 닫기, 로딩 표시 전환:

```javascript
player.on('relatedPlay', () => {
  document.body.classList.remove('related-active');
  showLoadingSpinner(true);
});
```

### 주의사항

- `relatedPlay`는 `related` **플러그인이 활성화되어 있어야만** 발생합니다.  
  (설정: `related: { file: "related.json" }` 또는 `related: true`)

- `relatedClick` 이벤트보다 **항상 나중에** 발생합니다.  
  (`relatedClick` = UI 클릭 감지 / `relatedPlay` = 실제 재생 시작)

- 자동재생(`auto: true`)일 경우, 사용자의 직접 클릭 없이도 호출됩니다.  
  이 때 `relatedClick`은 발생하지 않습니다.

- 추천 영상의 **로드가 실패**(`404`, `CORS`, `setupError`) 한 경우  
  `relatedPlay`는 발생하지 않습니다.

- iOS Safari(특히 네이티브 전체화면 모드)에서는  
  자동재생(`autoplay`)이 브라우저 정책에 의해 차단될 수 있습니다 →  
  `relatedPlay` 이벤트가 발생하지 않음.

### 특징

- **추천 콘텐츠 재생 시작을 감지하는 핵심 이벤트.**

- `relatedClick`은 행동(intent), `relatedPlay`는 실행(action)에 해당.

- 추천 콘텐츠 간 “전환(Conversion)”을 측정하거나  
  “다음 영상 자동재생 시점”을 제어하기에 최적화된 이벤트.

- `relatedReady` → `relatedOpen` → `relatedPlay`로 이어지는  
  **추천 UX의 마지막 단계 이벤트**로 볼 수 있습니다.

- OTT, 뉴스, 쇼츠(Shorts) 플랫폼 등에서  
  **“다음 영상 재생율(Next Play Rate)” 측정의 기준 이벤트**로 사용됩니다.
