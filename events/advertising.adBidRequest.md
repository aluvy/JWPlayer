# .on('adBidRequest')

Fired when header bidding starts requesting for bids.

## 호출시점

- 광고 입찰이 시작될 때, 즉 플레이어가 **광고 요청(bid request)을 보내기 직전**에 발생합니다.
- 구체적으로는 광고 설정이 되어 있고 헤더입찰(bidding) 구성요소가 활성화된 상황에서, 광고 시스템(예: VAST, IMA, FreeWheel 등)으로 입찰 요청을 시작하는 타이밍입니다.
- 이 이벤트 이후에 입찰 결과를 담은 `adBidResponse` 이벤트가 이어집니다.

## 이벤트 객체 구조 (콜백 파라미터)

```javascript
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

## 활용

- **입찰 시작 시점 로그 수집**: 입찰 요청이 언제, 어떤 bidders와 함께 시작됐는지 기록해 입찰 성능 분석 가능.
- **입찰 타이밍 조정**: `bidTimeout`, `floorPriceCents` 등을 참조해 요구 조건 변경 또는 최적화 가능.  
  예: 입찰 시간이 너무 짧으면 `bidTimeout` 연장 로직.
- **입찰 대상 필터링**: `bidders` 목록을 확인해 특정 입찰자만 포함되었는지 테스트하거나 UI/광고 전략에 반영.
- **뷰어빌리티 기반 제어**: `viewable` 값이 0이면 입찰 요청을 취소하거나 다른 흐름으로 대체 가능.
- **A/B 테스트나 광고 정책 분석**: 입찰 요청 시점 데이터를 측정해 전환율 또는 광고 수익 연관 분석 가능.

## 주의사항

- 이 이벤트는 **헤더 입찰 환경이 구성된 경우에만 발생**합니다. 광고 시스템이 단순 VAST 요청만 있을 경우엔 발생하지 않을 수 있습니다.
  Platform
- 이벤트가 매우 **초기 단계**에서 발생하므로, 여기서 입찰이 성공했다는 보장은 없습니다. 이후 `adBidResponse`나 `adImpression` 등의 이벤트 흐름을 함께 추적해야 합니다.
- `floorPriceCents`, `floorPriceCurrency` 등의 필드는 모든 상황에서 반환되지 않을 수 있으므로 존재 여부 체크 후 사용해야 합니다.
- 입찰 요청이 자주 발생하거나 실시간 스트림에서 반복될 경우, 로깅 과다로 인한 퍼포먼스 저하 가능. 특히 `all` 이벤트처럼 사용하지 않도록 주의해야 합니다.
- 광고 클라이언트 및 설정(`client`, `mediationLayerAdServer`)에 따라 반환 필드 또는 동작이 달라질 수 있으므로, 사용하는 라이선스 및 버전에 맞게 테스트 필수입니다.
- 이 이벤트는 광고 노출 전 단계이므로 **사용자에게 직접 보이는 변화는 거의 없음**. 따라서 UI 변화보다는 백엔드/트래킹 목적에 적합합니다.

## 특징

- 광고모듈 중에서도 **입찰(request) 시작 시점**을 공식적으로 인식할 수 있는 **유일한 이벤트**입니다.
- 광고의 수익 최적화 관점에서 특히 중요하며, 광고 로딩 앞단에서 **입찰 구조·타이밍을 분석** 가능.
- 일반 단순 광고 요청 이벤트보다 더 깊이 있는 데이터(`bidders`, `floorPriceCents`, `mediationLayerAdServer`)를 제공합니다.
- 헤더입찰 구조가 복잡해지는 현대 광고 플로우에서, 수익 모니터링/문제 진단 시 매우 유용한 진단 포인트가 됩니다.
