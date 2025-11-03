# Viewability Events

> We do not recommend using viewability API with iframes.

<br>

## .on('containerViewable')

Event fired denoting the viewability of the player container (named `<div>` element)

### 호출시점

- 플레이어가 **브라우저 뷰포트(Viewport) 내에서 보이는 상태인지 감지될 때마다** 발생합니다.

- 즉, **플레이어 엘리먼트(container)** 가 화면 안에 들어오거나(out → in),  
  화면 밖으로 나갈 때(in → out) 호출됩니다.

- 주로 **Intersection Observer 기반**으로 감지되며,  
  JW Player가 자동으로 스크롤·탭 전환 등을 추적합니다.

#### 트리거 조건:

1. 플레이어가 화면에 50% 이상 노출 → `viewable: 1`
2. 플레이어가 화면 밖 또는 50% 미만 노출 → `viewable: 0`

### 이벤트 객체 구조 (콜백 파라미터)

```json
{
  "viewable": 1,
  "type": "containerViewable"
}
```

| Value                 | Description                                                    |
| :-------------------- | :------------------------------------------------------------- |
| **viewable** (number) | 플레이어가 현재 화면에 보이는 정도 (`1`=보임, `0`=보이지 않음) |

### 활용

#### 1. 자동 재생/일시정지 제어 (In-View Autoplay)

- 화면에 보일 때만 자동 재생, 벗어나면 자동 일시정지.
- 리스트형 뉴스/피드형 페이지에서 필수적으로 사용됨.

```javascript
player.on('containerViewable', (e) => {
  if (e.viewable) {
    player.play();
  } else {
    player.pause(true);
  }
});
```

#### 2. 광고/노출 기반 트래킹

- “동영상이 실제로 사용자에게 보였는가”를 판단할 수 있음.
- `viewable: 1`일 때 시청 노출(Impression) 트리거:

```javascript
player.on('containerViewable', (e) => {
  if (e.viewable === 1) sendViewabilityLog();
});
```

#### 3. UX 최적화

- 비가시 상태에서 불필요한 재생을 중지하여 CPU/배터리 절약.
- 사용자 스크롤 시 자동으로 “재생/일시정지” 전환 → **Feed-type UI 최적화**.

### 주의사항

- **기본 감지 임계값(threshold)은 약 50%** 로 고정되어 있으며, 세밀한 제어는 직접  
  IntersectionObserver를 사용하는 것이 좋습니다.
- `containerViewable`은 플레이어 **DOM 노출 상태만 감지**하므로,  
  탭 전환(`visibilitychange`)이나 OS-level focus 전환은 별도 이벤트(`viewable` 이벤트)로 감지해야 합니다.
- `viewable` 값은 `0` 또는 `1` 두 가지 뿐이며, 중간값(%)은 제공되지 않습니다.
- 광고 모듈이 활성화되어 있을 경우, JW 내부에서 **광고 Viewability 측정과 통합되기도 함**.
- iFrame 임베드 시에는 교차 도메인 정책(CORS) 때문에 브라우저가 시야 감지를 제한할 수 있습니다.

### 특징

- **플레이어의 “화면 내 노출 상태”를 알려주는 유일한 이벤트.**
- `viewable` 이벤트와 달리, **플레이어 자체(container)** 가 화면에 있는지를 감지.
- In-View 재생, 피드형 뉴스, SNS 동영상, 광고 노출 추적 등에서 매우 자주 사용됨.
- **IntersectionObserver** 기반이므로 정확하고 성능에 부담이 적습니다.
- JW Player의 자동재생(`autostart: 'viewable'`) 기능과 내부적으로 연계됩니다.

<br>

## .on('viewable')

Event fired denoting the viewability of the player

### 호출시점

- 플레이어가 **브라우저 탭 또는 OS 수준에서 “실제로 보이는 상태(Viewable)”로 전환될 때** 발생합니다.

- 즉, **탭이 활성화되어 있고, 플레이어가 화면에 표시되고 있으며, 사용자가 시야 내에 있는 경우** 감지됩니다.

- `containerViewable`이 “DOM 노출(스크롤 위치)”만 감지한다면,  
  `viewable`은 브라우저 포커스, 탭 가시성, 창 전환 등 **시스템 수준 노출 상태까지 포함**합니다.

#### 대표적인 발생 타이밍

1. 사용자가 플레이어가 포함된 탭으로 전환할 때
2. 브라우저가 백그라운드에서 포어그라운드로 복귀할 때
3. 화면에 보이던 플레이어가 다시 활성화되었을 때
4. 반대로 탭 이동, 창 최소화, 백그라운드 이동 시 `viewable: 0`

### 이벤트 객체 구조 (콜백 파라미터)

```json
{
  "viewable": 1,
  "type": "viewable"
}
```

| Value                 | Description                                                      |
| :-------------------- | :--------------------------------------------------------------- |
| **viewable** (number) | 플레이어가 현재 “가시 상태”인지 여부 (`1`=보임, `0`=보이지 않음) |

### 활용

#### 1. 자동 일시정지 / 재개 (탭 전환 대응)

- 사용자 탭이 비활성화될 때 자동 일시정지, 복귀 시 자동재생.

```javascript
player.on('viewable', (e) => {
  if (e.viewable === 0) {
    player.pause(true); // 백그라운드 시 정지
  } else {
    player.play(); // 다시 탭이 보이면 재생
  }
});
```

#### 2. 시청 노출(Visibility) 기반 트래킹

- 플레이어가 실제로 사용자에게 “보이는 상태”일 때만 노출 로그 기록:

```javascript
player.on('viewable', (e) => {
  if (e.viewable === 1) sendAnalytics('player_viewed');
});
```

#### 3. 광고 또는 콘텐츠 노출 관리

- 광고(특히 비디오 광고)의 **Viewability 기준(50%·1초 이상)** 계산에 활용.

- 백그라운드 재생 방지 정책(예: iOS Safari에서 자동 중단)을 적용할 수 있음.

#### 4. UX 최적화

- 사용자가 화면을 벗어나면 불필요한 CPU/GPU 자원 절약.

- 탭 포커스 변화에 따른 오디오·자막 동기화 오류 방지.

### 주의사항

- `viewable`은 **탭/브라우저 가시성 중심 이벤트**로,  
  **DOM 기반의 스크롤 노출(`containerViewable`)과 다릅니다.**  
  → 두 이벤트를 함께 써야 완전한 “In-View 감지” 구현 가능.

- `viewable` 값은 이진 상태(`1` 또는 `0`)이며, 중간값(%)은 제공되지 않습니다.

- iOS Safari에서는 시스템 정책상 일부 브라우저 포커스 이벤트가 감지되지 않을 수 있습니다.

- 광고 모듈 활성화 시 `viewable` 이벤트가 내부적으로 광고 SDK의 Viewability 트래킹과 연결될 수 있습니다.

- 브라우저의 `Page Visibility API` 를 기반으로 동작하므로,  
  **iframe cross-domain 환경**에서는 정상적으로 감지되지 않을 수 있습니다.

### 특징

- **JW Player의 “실제 사용자 시야 내 활성 상태”를 감지하는 이벤트.**

- `containerViewable`과 달리, 스크롤 외에도 **탭 전환·윈도우 포커스까지 감지**함.

- `autostart: 'viewable'` 모드 설정 시 내부적으로 이 이벤트를 사용하여 자동재생 제어.

- 광고·UX·시청률 트래킹 시스템에서 **실제 시청 가능 상태**를 판단하는 기준 이벤트로 사용됨.

- In-view 자동재생 구현 시 `containerViewable` + `viewable` **조합**이 가장 안정적.
