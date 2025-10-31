# Advertising Events

> Video ad insertion requires a JWP Enterprise license. Please contact our team to upgrade your account.

This API provides developers with more control over the functionality of the Advertising edition of JWP. For VAST and IMA plugins, this API allows for things like impression verification, custom scheduling, and multiple companions.

<br>

## .on('adBidRequest')

Fired when header bidding starts requesting for bids.

### 호출시점

- 광고 입찰이 시작될 때, 즉 플레이어가 **광고 요청(bid request)을 보내기 직전**에 발생합니다.
- 구체적으로는 광고 설정이 되어 있고 헤더입찰(bidding) 구성요소가 활성화된 상황에서, 광고 시스템(예: VAST, IMA, FreeWheel 등)으로 입찰 요청을 시작하는 타이밍입니다.
- 이 이벤트 이후에 입찰 결과를 담은 `adBidResponse` 이벤트가 이어집니다.

### 이벤트 객체 구조 (콜백 파라미터)

```json
{
  "bidders": [
    { "id": 1234, "name": "BidderX" },
    { "id": 5678, "name": "BidderY" }
  ],
  "bidTimeout": 1000,
  "client": "vast",
  "floorPriceCents": 500,
  "floorPriceCurrency": "usd",
  "mediationLayerAdServer": "jwp",
  "offset": "pre",
  "viewable": 1,
  "type": "adBidRequest"
}
```

| Value                                  | Description                                                                              |
| :------------------------------------- | :--------------------------------------------------------------------------------------- |
| **bidders** (array)                    | 현재 입찰 요청에 포함된 **모든 입찰자(bidders)** 목록. 각 항목에 `id`, `name` 등이 있음. |
| **bidTimeout** (number)                | 입찰 요청 후 응답을 기다릴 시간(ms)                                                      |
| **client** (string)                    | 현재 사용 중인 광고 클라이언트. 가능 값 예: `dai`, `freewheel`, `googima`, `vast`        |
| **floorPriceCents** (number (선택))    | 입찰이 통과해야 할 최소 가격(센트 단위) (헤더입찰 + JWP 직접 중계 시)                    |
| **floorPriceCurrency** (string (선택)) | `floorPriceCents`의 통화 단위(`usd` 등)                                                  |
| **mediationLayerAdServer** (string)    | 광고 중개(mediation) 레이어의 AdServer 식별자. 예: `dfp`, `jwp`, `jwpdfp`                |
| **offset** (string)                    | 광고 위치 기준(예: `pre`, `mid`, `post`)                                                 |
| **viewable** (number)                  | 플레이어가 현재 뷰포트 내에서 보이는지 여부(1/0)                                         |

### 활용

- **입찰 시작 시점 로그 수집**: 입찰 요청이 언제, 어떤 bidders와 함께 시작됐는지 기록해 입찰 성능 분석 가능.
- **입찰 타이밍 조정**: `bidTimeout`, `floorPriceCents` 등을 참조해 요구 조건 변경 또는 최적화 가능.  
  예: 입찰 시간이 너무 짧으면 `bidTimeout` 연장 로직.
- **입찰 대상 필터링**: `bidders` 목록을 확인해 특정 입찰자만 포함되었는지 테스트하거나 UI/광고 전략에 반영.
- **뷰어빌리티 기반 제어**: `viewable` 값이 0이면 입찰 요청을 취소하거나 다른 흐름으로 대체 가능.
- **A/B 테스트나 광고 정책 분석**: 입찰 요청 시점 데이터를 측정해 전환율 또는 광고 수익 연관 분석 가능.

### 주의사항

- 이 이벤트는 **헤더 입찰 환경이 구성된 경우에만 발생**합니다. 광고 시스템이 단순 VAST 요청만 있을 경우엔 발생하지 않을 수 있습니다.
  Platform
- 이벤트가 매우 **초기 단계**에서 발생하므로, 여기서 입찰이 성공했다는 보장은 없습니다. 이후 `adBidResponse`나 `adImpression` 등의 이벤트 흐름을 함께 추적해야 합니다.
- `floorPriceCents`, `floorPriceCurrency` 등의 필드는 모든 상황에서 반환되지 않을 수 있으므로 존재 여부 체크 후 사용해야 합니다.
- 입찰 요청이 자주 발생하거나 실시간 스트림에서 반복될 경우, 로깅 과다로 인한 퍼포먼스 저하 가능. 특히 `all` 이벤트처럼 사용하지 않도록 주의해야 합니다.
- 광고 클라이언트 및 설정(`client`, `mediationLayerAdServer`)에 따라 반환 필드 또는 동작이 달라질 수 있으므로, 사용하는 라이선스 및 버전에 맞게 테스트 필수입니다.
- 이 이벤트는 광고 노출 전 단계이므로 **사용자에게 직접 보이는 변화는 거의 없음**. 따라서 UI 변화보다는 백엔드/트래킹 목적에 적합합니다.

### 특징

- 광고모듈 중에서도 **입찰(request) 시작 시점**을 공식적으로 인식할 수 있는 **유일한 이벤트**입니다.
- 광고의 수익 최적화 관점에서 특히 중요하며, 광고 로딩 앞단에서 **입찰 구조·타이밍을 분석** 가능.
- 일반 단순 광고 요청 이벤트보다 더 깊이 있는 데이터(`bidders`, `floorPriceCents`, `mediationLayerAdServer`)를 제공합니다.
- 헤더입찰 구조가 복잡해지는 현대 광고 플로우에서, 수익 모니터링/문제 진단 시 매우 유용한 진단 포인트가 됩니다.

<br>

## .on('adBidResponse')

<br>

## .on('adBlock')

<br>

## .on('adBreakEnd')

<br>

## .on('adBreakIgnored')

<br>

## .on('adBreakStart')

<br>

## .on('adClick')

<br>

## .on('adCompanions')

<br>

## .on('adComplete')

<br>

## .on('adError')

<br>

## .on('adImpression')

<br>

## .on('adItem')

<br>

## .on('adLoaded')

<br>

## .on('adManager')

<br>

## .on('adMeta')

<br>

## .on('adPause')

<br>

## .on('adPlay')

<br>

## .on('adRequest')

<br>

## .on('adRequestedContentResume')

<br>

## .on('adSchedule')

<br>

## .on('adLoadedXML')

<br>

## .on('adSkipped')

<br>

## .on('adStarted')

<br>

## .on('adTime')

<br>

## .on('adViewableImpression')

<br>

## .on('adWarning')

<br>

## .on('adsManager')

<br>

## .on('beforeComplete')

Fired just before the player completes playing. Unlike the onComplete event, the player will not have moved on to either showing the replay screen or advancing to the next playlistItem, which makes this the right moment to insert post-roll ads using `playAd()`.

### 호출시점

- 현재 재생 중인 영상이 **끝나기 직전(마지막 프레임 재생 직전)** 에 발생합니다.
- 즉, `complete` **이벤트가 발생하기 바로 직전 단계**로,  
  영상 종료 직전 작업(예: 포스트롤 광고, 다음 영상 로딩 준비 등)을 실행할 수 있는 마지막 시점입니다.
- `beforePlay`가 재생 전 이벤트라면, `beforeComplete는` 종료 직전 이벤트입니다.

### 이벤트 객체 구조 (콜백 파라미터)

```json
{
  "type": "beforeComplete"
}
```

> 추가적인 속성(`position`, `duration`, `reason` 등)은 포함되지 않습니다.
> 광고 SDK와 연동된 경우에만 내부적으로 확장 데이터가 포함될 수 있으나 공식 API에는 노출되지 않습니다.

### 활용

#### 1. 포스트롤(Post-roll) 광고 트리거

- 영상이 완전히 끝나기 전에 광고를 삽입해 끊김 없는 전환 가능.

#### 2. 다음 영상 준비 로직

- `complete` 후 즉시 다음 아이템이 재생될 수 있도록 미리 로드.

#### 3. 재생 종료 애니메이션 / 트래킹

- 영상 종료 직전 “이어서 보기”, “다음 영상 예고” 등 UI 표시.
- 트래킹 도구(GA, Firebase 등)에 “watch almost complete” 신호 전송.

### 주의사항

- `complete` **이벤트 직전 단 한 번만 호출**됨.
- 이벤트 발생 후 곧바로 `complete` 가 이어지므로,  
  긴 네트워크 요청이나 비동기 작업을 실행하면 재생 완료 타이밍이 어긋날 수 있음.
- 재생 중 강제 정지(`stop()`)나 다른 아이템으로 전환 시에는 호출되지 않습니다.
- 포스트롤 광고를 여기에 삽입할 경우, 광고 완료 시 `complete` 가 정상적으로 이어지는지 반드시 확인 필요.
- 이 이벤트는 **광고 모듈(ad schedule)** 과 함께 사용되는 경우가 많으므로,  
  광고 클라이언트가 비활성화된 환경에서는 발생하지 않을 수도 있습니다.

### 특징

- **영상이 끝나기 직전의 유일한 이벤트.**
- `complete` 이벤트와 달리 **콘텐츠가 아직 종료되지 않은 상태에서 트리거**됨.
- `beforePlay`와 짝을 이루는 구조로,
  - `beforePlay`: 재생 시작 직전
  - `beforeComplete`: 재생 종료 직전
- **포스트롤 광고 / 다음 영상 로드 / 종료 직전 분석 이벤트** 등에 매우 적합.

<br>

## .on('beforePlay')

Fires before any of the following occur:

- Initial playback of a single media or individual playlist item begins.
- After a pause, media playback resumes initiated by a viewer or other mechanism.

> Since this event fires before `.on('play')`, use this event to trigger calling `loadAdTag(tag)` or `loadAdTag(xml)` to play a preroll ad when the `state` is `idle`.

### 호출시점

- 영상이 **재생되기 직전**, 즉 실제 `play()` 명령이 실행되기 직전에 발생합니다.
- `beforePlay` → `play` → `firstFrame` 순으로 이어지는 재생 초기 이벤트 체인 중 첫 단계입니다.
- 수동 재생(사용자 클릭) · 자동재생 · API 호출 등 모든 경로에서 발생합니다.
- 프리롤(Pre-roll) 광고 삽입 혹은 재생 시작 전 트래킹 포인트를 등록하는 용도로 가장 많이 사용됩니다.

### 이벤트 객체 구조 (콜백 파라미터)

```json
{
  "playReason": "autostart",
  "state": "idle",
  "viewable": 1,
  "type": "beforePlay"
}
```

| Value                   | Description                                                                     |
| :---------------------- | :------------------------------------------------------------------------------ |
| **playReason** (string) | 재생 시작 이유 (`"autostart"`, `"interaction"`, `"external"`, `"playlist"`, 등) |
| **state** (string)      | 이벤트 발생 시 플레이어 상태 (일반적으로 `"idle"`)                              |
| **viewable** (number)   | 플레이어가 현재 뷰포트 내에서 보이는 지 (1 = 보임, 0 = 안보임)                  |

### 활용

#### 1. 프리롤 (Pre-roll) 광고 삽입

```javascript
player.on('beforePlay', (e) => {
  console.log('beforePlay – 재생 직전:', e.playReason);
  player.loadAdTag('https://adserver.com/vasttag_pre.xml');
});
```

- 실제 콘텐츠 재생 전 광고를 삽입해 자연스럽게 시작 가능.

#### 2. 사용자 행동 분석 / 트래킹

- 사용자가 직접 클릭했는지 (`playReason === "interaction"`) 자동재생인지 구분해 로그 기록.
- “영상 시작 시점” 기준으로 노출 카운트 · 성과 분석 포인트 생성.

#### 3. 초기 UI 제어

- 재생 전 자막 · 품질 · 오디오 트랙 등 초기값 자동 설정.
- 로딩 UI 숨김 및 “재생 준비 중” 메시지 제거.

### 주의사항

- `play` **이벤트 보다 앞서 호출**되므로, 여기서 `player.play()` 같은 명령을 다시 호출하면 루프나 중복 재생 문제 발생 가능.

- 광고 삽입 로직이 있을 경우 비동기 지연 (광고 로드 대기) 때문에 실제 콘텐츠 재생이 늦어질 수 있음.

- 자동재생이 차단된 환경에서는 `beforePlay` 가 발생했더라도 곧바로 `autostartNotAllowed` 로 전환될 수 있음.

- `beforePlay` 는 플레이리스트 아이템이 변경될 때마다 매번 발생할 수 있으므로 필요 시 플래그로 중복 실행 방지.

- 광고 SDK 와 동시에 쓰는 경우 광고 시작 (`adStarted`) 보다 먼저 불릴 수 있으니 순서 확인 필요.

### 특징

- 재생 사이클의 **가장 초기 이벤트** → 초기화 및 광고 트리거 위치로 최적.
- `beforeComplete` 의 반대 개념으로, 콘텐츠 시작 전 개입할 수 있는 유일한 포인트.
- `playReason` 값을 통해 “재생 시작 원인”을 명확히 구분할 수 있는 유일한 이벤트.

<br>
