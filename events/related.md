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
<br>

# ("related").on('play')

Triggers when a user selects an object in a related feed.

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
