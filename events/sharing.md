# Sharing Events

Sharing API calls work in conjunction with our `getPlugin() method`. For instance, all of our sharing instances are using the `getPlugin(‘sharing’)` API call to refer to this particular plugin. The following will target our sharing plugin:

```javascript
player.on('ready', function (event) {
  sharingPlugin = jwplayer().getPlugin('sharing');
});
```

All `sharingPlugin` references below will assume that the above code is implemented on your page.

<br>
<br>

# ("sharing").on('open')

Listens for the opening of the plugin

### 호출시점

- 사용자가 **공유 패널(Sharing Overlay)** 을 열었을 때 발생합니다.

- 즉, 플레이어 내부의 “공유 버튼”을 눌러 공유창이 표시되는 순간입니다.

### 이벤트 객체 구조 (콜백 파라미터)

```json
{
  "type": "open"
}
```

### 활용

#### 1. UX / UI 동기화

- 공유 창이 열릴 때 백그라운드 블러나 오버레이 표시.

```javascript
jwplayer().on('sharingOpen', () => {
  document.querySelector('.overlay-bg').classList.add('show');
});
```

#### 2. 사용자 행동 분석

- 공유창 오픈 빈도를 통해 **공유 의도(Intent)** 측정 가능.

- “공유 시도 → 실제 클릭” 전환율 분석.

### 주의사항

- 실제 공유가 이뤄지지 않아도 단순 열기만으로 발생.

- `on('sharingOpen')` 은 단일 공유 UI에 대해서만 발생하며,  
  커스텀 공유 버튼을 직접 구현한 경우에는 수동 트리거 필요.

### 특징

- “공유 의도(intent)”를 측정하는 이벤트로,  
  사용자의 관심도·참여도 분석에 유용.

- `click`과 달리 UI 오픈 행위 자체를 감지.

<br>
<br>

# ("sharing").on('close')

Listens for the closing of the plugin

### 호출시점

- 사용자가 **공유 패널을 닫았을 때** 발생합니다.

- 패널 외부 클릭, ESC 키, 닫기 버튼 클릭 등 모든 닫힘 액션을 포함합니다.

### 이벤트 객체 구조 (콜백 파라미터)

```json
{
  "type": "close"
}
```

### 활용

#### 1. UI 정리 및 복원

```javascript
jwplayer().on('sharingClose', () => {
  document.querySelector('.overlay-bg').classList.remove('show');
});
```

#### 2. 세션 추적

- 공유창을 열었다 닫은 시점을 측정해 **사용자 체류 패턴** 파악 가능.

- `open` → `close` 간 시간 계산으로 실제 사용행동 분석.

### 주의사항

- `close` 이벤트는 `open` 후에만 발생합니다.

- 중복 호출 방지를 위해 `isSharingOpen` 상태를 관리하는 것이 좋습니다.

- `click` 이벤트(실제 공유 클릭) 이후에도 `close`가 호출되므로 순서 제어 필요.

### 특징

- 공유 UI가 종료되는 시점을 명확히 알려주는 이벤트.

- UI 복원 및 사용자 세션 관리의 기준 타이밍으로 활용.

<br>
<br>

# ("sharing").on('click')

Triggers whenever somebody shares content from within the sharing plugin

### 호출시점

- 사용자가 **공유 패널 내의 특정 플랫폼(예: Facebook, Twitter, KakaoTalk, Copy Link 등)** 아이콘을 클릭했을 때 발생합니다.

- 즉, **"공유 실행" 시점의 사용자 인터랙션 이벤트**입니다.

### 이벤트 객체 구조 (콜백 파라미터)

```json
{
  "type": "click",
  "method": "facebook"
}
```

| Value               | Description                                                                                       |
| :------------------ | :------------------------------------------------------------------------------------------------ |
| **method** (string) | 사용자가 선택한 공유 플랫폼 (`"facebook"`, `"twitter"`, `"email"`, `"copylink"`, `"linkedin"` 등) |

### 활용

#### 1. 공유 로그 / 통계 추적

- 어떤 플랫폼이 가장 많이 이용되는지 분석 가능.

```javascript
jwplayer().on('sharingClick', (e) => {
  console.log(`공유 플랫폼: ${e.method}`);
  sendAnalytics('share_click', e.method);
});
```

#### 2. 사용자 맞춤 UX

- 예: 특정 플랫폼 공유 시 맞춤 메시지 제공

- `"copylink"` 선택 시 클립보드 복사 완료 토스트 표시

### 주의사항

- 클릭만 감지하며, 실제 공유가 성공했는지는 확인할 수 없음 (브라우저 별 외부 리다이렉트 처리).

- 플랫폼별(`method`) 문자열은 JW Player 테마/설정에 따라 달라질 수 있음.

- `sharing` 플러그인이 활성화되어 있어야 작동.

### 특징

- “사용자 공유 행동”을 직접 감지할 수 있는 유일한 이벤트.

- 분석, 리포팅, UX 커스터마이징에 활용 가치 높음.
