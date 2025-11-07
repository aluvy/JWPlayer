# Advertising Events

> Video ad insertion requires a JWP Enterprise license. Please contact our team to upgrade your account.

This API provides developers with more control over the functionality of the Advertising edition of JWP. For VAST and IMA plugins, this API allows for things like impression verification, custom scheduling, and multiple companions.

<br>
<br>

# .on('adBidRequest')

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
<br>

# .on('adBidResponse')

헤더 비딩(Header Bidding) 응답이 반환될 때 발생합니다.

아래와 같은 속성을 포함한 객체를 반환합니다.

- **bidders** (array)

  - 현재 입찰 요청에 포함된 모든 비더(bidder)의 배열

- **bidTimeout** (number)

  - 사용자가 재생을 클릭한 후, 입찰 응답을 기다리는 시간(밀리초)

- **client** (string)

  - 현재 사용 중인 클라이언트
  - **가능한 값:**
    - `dai`
    - `freewheel`
    - `googima`
    - `vast`

- **floorPriceCents** (number)

  - 반환된 입찰이 이 금액(센트 단위)을 초과해야 재생됨
  - ※ `dfp` 중재 계층(mediation layer)을 사용할 경우 반환되지 않음

- **floorPriceCurrency** (string)

  - floorPriceCents 값의 통화 단위
  - Facebook의 경우 `usd` 여야 하며, 중재 계층이 `jwp`로 설정된 경우에만 사용됨

- **mediationLayerAdServer** (string)

  - 어떤 광고를 재생할지를 결정하는 **중재 계층 (mediation layer)**
  - **가능한 값:**
    - `dfp`
    - `jwp`
    - `jwpdfp`
    - `jwpspotx`

- **offset** (string)

  - 광고의 오프셋(재생 시점)

- **placement** (string)

  - 플레이어의 위치를 식별하기 위해 입찰 요청에 전송되는 값
  - **가능한 값:**
    - `article`
    - `banner`
    - `feed`
    - `floating`
    - `instream`
    - `interstitial`
    - `slider`

- **type** (string)

  - 이벤트의 유형<br>이 값은 항상 **adBidResponse**

- **viewable** (number)

  - 플레이어가 뷰어블(Viewable) 상태인지 여부
  - **가능한 값:**
    - `0` (비표시 상태)
    - `1` (표시 상태)

<br>

### Bidder Object

각 비더 객체에는 다음 속성이 포함됩니다.

- **id** (number)

  - 각 비더에 대해 헤더 비딩에 사용되는 퍼블리셔 ID

- **name** (string)

  - 비더의 이름
  - 참조: [advertising.bids.bidders[].name](#)

- **priceInCents** (number)

  - 입찰 금액 (센트 단위)
  - JWP가 중재 계층인 경우에만 사용됨

- **result** (string)

  - 해당 비더의 입찰 결과
  - **가능한 값:**
    - `bid`
    - `noBid`
    - `timeout`

- **tagKey** (number)

  - 반환된 입찰의 tagKey
  - SpotX 비더에 대해서만 사용됨

- **timeForBidResponse** (number)

  - 입찰 응답이 반환되기까지 걸린 시간 (밀리초 단위)

- **winner** (boolean)

  - 이 비더가 낙찰자인 경우 `true` 로 설정됨
  - **가능한 값:**
    - `false`
    - `true`

<br>
<br>

# .on('adBlock')

이 이벤트는 JWP 설정 내에서 **광고 플러그인(VAST 또는 Google IMA)** 이 구성되어 있고,  
시청자의 브라우저에서 **광고 차단기(Ad Blocker)** 가 감지되었을 때 발생합니다.

이 이벤트를 통해 사용자가 광고 차단기를 비활성화하도록 요청할 수 있습니다.

<br>
<br>

# .on('adBreakEnd')

광고가 종료되고, **플레이어로 제어권이 다시 넘어올 때** 발생합니다.

아래와 같은 속성을 포함한 객체를 반환합니다.

- **adposition** (string)

  - 광고의 위치
  - **가능한 값:**
    - `mid`
    - `post`
    - `pre`

- **client** (string)

  - 광고 구간(ad break)에 사용된 광고 클라이언트
  - **가능한 값:**
    - `dai`
    - `freewheel`
    - `googima`
    - `vast`

- **type** (string)

  - 발생한 이벤트의 유형<br>이 값은 항상 **adBreakEnd**

- **viewable** (number)

  - 플레이어가 뷰어블(Viewable) 상태인지 여부
  - **가능한 값:**
    - `0`: 플레이어가 50% 미만 표시되거나 비활성 탭에 있음
    - `1`: 플레이어가 50% 이상 표시되고 활성 탭에 있음

<br>
<br>

# .on('adBreakIgnored')

**(VAST 전용)**
이 이벤트는 **이전 광고 구간(ad break)을 완전히 시청한 후, 다음 광고 구간이 재생되기까지의 경과 시간**이
`advertising.rules.timeBetweenAds` 에서 정의된 값보다 짧을 때 발생합니다.

adBreakIgnored

```json
{
  "id": "adbreak2",
  "tag": "{ad_tag_url}",
  "offset": 6,
  "timeSinceLastAd": 3.256,
  "type": "adBreakIgnored"
}
```

- **id** (string)

  - 광고 구간의 설명용 이름

- **offset** (number / string)

  - 광고의 위치
  - **가능한 값:**
    - `{number in seconds}` — 미드롤(mid-roll) 광고 구간에서 발생
    - `post`
    - `pre`

- **tag** (string)

  - 광고 태그의 URL

- **timeSinceLastAd** (number)

  - 현재 광고 구간과 마지막 광고 구간 사이의 경과 시간(초 단위)
  - 이 값은 항상 `advertising.rules.timeBetweenAds` 값보다 작습니다.

- **type** (string)

  - 발생한 이벤트의 유형
  - 이 값은 항상 **adBreakIgnored**

<br>
<br>

# .on('adBreakStart')

이 이벤트는 **광고 요청(ad request)** 후, 광고가 플레이어에 로드되기 **직전**에 발생합니다.  
단, **광고 구간(ad break)** 내 첫 번째 광고가 재생되기 전에만 발생합니다.

### 반환 객체

다음 속성들을 포함합니다:

- **adposition** (string)

  - 광고의 위치
  - **가능한 값:**
    - `mid`
    - `post`
    - `pre`

- **client** (string)

  - 광고 구간에 사용된 광고 클라이언트
  - **가능한 값:**
    - `dai`
    - `freewheel`
    - `googima`
    - `vast`

- **type** (string)

  - 발생한 이벤트의 유형<br>이 값은 항상 **adBreakStart**

- **viewable** (number)

  - 플레이어의 가시성(Viewability) 상태
  - **가능한 값:**
    - `0`: 플레이어가 50% 미만 표시되었거나 비활성 탭에 있음
    - `1`: 플레이어가 50% 이상 표시되고 활성 탭에 있음

<br>
<br>

# .on('adClick')

사용자가 광고를 클릭하여 랜딩 페이지로 리디렉션될 때마다 발생합니다.

DAI

```json
{
  "client": "dai",
  "viewable": 1,
  "id": "cfau4gxh3q00",
  "adPlayId": "of0i8kj1tkp0",
  "adtitle": "External NCA1C1L1 Preroll",
  "adsystem": "GDFP",
  "creativetype": "application/x-mpegURL",
  "linear": "linear",
  "adposition": "pre",
  "type": "adClick"
}
```

FreeWheel

```json
{
  "client": "freewheel",
  "tag": "placeholder_preroll",
  "freewheel": {
    "ad": {
      "adId": "17302931"
    }
  },
  "adposition": "pre",
  "id": "17302931",
  "linear": "linear",
  "creativetype": "video/mp4",
  "viewable": 1,
  "sequence": 1,
  "podcount": 2,
  "skipoffset": 3,
  "type": "adClick"
}
```

IMA

```json
{
    "client": "googima",
    "placement": 1,
    "viewable": 0,
    "adposition": "pre",
    "tag": "//playertest-cdn.longtailvideo.com/pre.xml",
    "adBreakId": "1fuk7qd71j5p",
    "adPlayId": "1fuk7qd71j5p",
    "id": "1fuk7qd71j5p",
    "ima": {...},
    "adtitle": "JW Test Preroll",
    "adsystem": "Alex_Vast",
    "creativetype": "video/mp4",
    "duration": 0,
    "linear": "linear",
    "description": "",
    "creativeAdId": "",
    "adId": "232859236",
    "universalAdId": [],
    "advertiser": "",
    "dealId": "",
    "mediaFile": {
        "file": "//content.bitsontherun.com/videos/1EI2jHpo-52qL9xLP.mp4"
    },
    "type": "adClick"
}
```

VAST

```json
{
  "client": "vast",
  "placement": 1,
  "adBreakId": "1bzytku1wlz6",
  "adPlayId": "1bzytku1wlz6",
  "offset": "pre",
  "id": "1jnyb7aup1ya",
  "tag": "//playertest-cdn.longtailvideo.com/pre-60s.xml",
  "adposition": "pre",
  "sequence": 1,
  "witem": 1,
  "wcount": 1,
  "adsystem": "Alex_Vast",
  "skipoffset": 5,
  "adschedule": {
    "item": 1,
    "breakid": "adbreak1",
    "tags": ["//playertest-cdn.longtailvideo.com/pre-60s.xml"],
    "offset": "pre"
  },
  "adtitle": "JW Test Preroll",
  "description": "",
  "adId": "232859236",
  "adVerifications": null,
  "advertiser": "",
  "advertiserId": "",
  "creativeId": "",
  "creativeAdId": "",
  "dealId": "",
  "request": {},
  "response": {
    "location": null
  },
  "conditionalAdOptOut": false,
  "vastversion": 2,
  "clickThroughUrl": "//jwplayer.com/",
  "mediaFileCompliance": false,
  "nonComplianceReasons": ["video/mp4 has only 2 qualities"],
  "mediafile": {
    "file": "//content.jwplatform.com/videos/zz4Abp0Z-bPwArWA4.mp4"
  },
  "viewable": 1,
  "creativetype": "video/mp4",
  "categories": [],
  "type": "adClick"
}
```

<br>

- **adBreakId** (string)

  - 각 광고 구간(ad break)에 대한 고유 ID.
  - 동일한 광고 구간 내의 모든 광고는 같은 **adBreakId**를 가집니다.

- **adId** (string)

  - 광고 XML 내 정의된 **광고 서버의 크리에이티브 식별자**.
  - 출처: _IAB Tech Lab Definition_

- **adPlayId** (string)

  - 각 개별 광고에 대한 고유 ID.
  - 동일한 광고 구간 내 여러 광고가 있을 경우, 각각 고유한 **adPlayId**를 가집니다.

- **adposition** (string)

  - 광고의 위치
  - **가능한 값:**
    - `mid`
    - `post`
    - `pre`

- **adschedule** (object)

  - 해당 광고 구간의 설정 정보

- **adsystem** (string)

  - 광고 XML에서 반환된 **광고 서버 이름**.
  - 출처: _IAB Tech Lab Definition_

- **adtitle** (string)

  - 광고 XML에 정의된 **광고의 일반 이름(Common Name)**.
  - 출처: _IAB Tech Lab Definition_

- **adVerifications** (object)

  - 광고 XML에서 정의된 **제3자 검증 코드 실행을 위한 리소스 및 메타데이터 목록**.
  - 이는 크리에이티브 재생 검증용입니다.
  - 출처: _IAB Tech Lab Definition_

- **advertiser** (string)

  - 광고 XML에 정의된 **광고주 이름**.
  - 광고 제공 측(ad serving party)에 의해 정의됨.
  - 출처: _IAB Tech Lab Definition_

- **advertiserId** (string)

  - 광고 XML 내 **광고주 식별자(선택적)**.
  - 광고 서버에서 제공.
  - 출처: _IAB Tech Lab Definition_

- **categories** (array)

  - 광고 XML에서 가져온 **광고 콘텐츠 카테고리**를 식별하는 **카테고리 코드 또는 라벨 목록**
  - 출처: _IAB Tech Lab Definition_

- **clickThroughUrl** (string)

  - 광고 XML에서 가져온 **광고주 사이트의 URI**.<br>시청자가 광고를 클릭했을 때 미디어 플레이어가 여는 URL.
  - 출처: _IAB Tech Lab Definition_

- **client** (string)

  - 현재 사용 중인 광고 클라이언트
  - **가능한 값:**
    - `dai`
    - `freewheel`
    - `googima`
    - `vast`

- **conditionalAdOptOut** (boolean)

  - (**VPAID 전용**) VAST 응답 내 `conditionalAd` 속성을 가진 광고를 **재생하지 않도록** 지시

- **creativeAdId** (string)

  - 광고 XML에서 가져온 **광고 서버의 고유 크리에이티브 식별자**
  - 출처: _IAB Tech Lab Definition_

- **creativeId** (string)

  - 광고 XML에서 가져온 **크리에이티브를 제공하는 광고 서버 식별자**
  - 출처: _IAB Tech Lab Definition_

- **creativetype** (string)

  - VAST XML에 지정된 **현재 미디어 파일의 MIME 유형**

- **dealId** (string)

  - 광고 XML에서 가져온 **현재 광고의 상위 래퍼(wrapper) 체인에서 발견된 첫 번째 딜 ID**
  - 출처: _Google Definition_

- **description** (string)

  - 광고 XML에서 제공하는 **광고의 상세 설명**
  - 출처: _IAB Tech Lab Definition_

- **duration** (number)

  - 광고 XML에서 제공하는 **선형 광고(linear ad)**의 지속 시간.
  - 형식: `HH:MM:SS.mmm`
  - 출처: _IAB Tech Lab Definition_

- **freewheel** (object)

  - `ad.adId` 속성 내에 **고유 광고 식별자**를 포함

- **id** (string)

  - 고유한 광고 식별자

- **ima** (object)

  - IMA SDK에서 **현재 재생 중인 광고 인스턴스**와 JWP가 IMA SDK에 광고 요청 시 전달하는 **userRequestContext** 포함

- **linear** (string)

  - 광고 XML의 `linear` 속성 값
  - **가능한 값:**
    - `linear`: 영상 재생을 중단하고 재생되는 **비디오 광고**
    - `nonlinear`: 플레이어의 일부 영역에 오버레이되는 **정적 디스플레이 광고**로, 재생을 중단하지 않음

- **mediafile** \| **mediaFile** (object)

  - 광고 XML에서 가져온 **선형 광고의 비디오 파일 정보**
  - 출처: _IAB Tech Lab Definition_

- **mediaFileCompliance** (boolean)

  - 광고가 **mediaFile 규격 준수 여부**를 나타냄.
  - 다음 중 하나라도 충족하면 준수로 간주됩니다:
    - `.m3u8` 파일
    - `VPAID` 형식
    - MIME 유형당 최소 3개의 화질 레벨

- **nonComplianceReasons** (array)

  - `mediaFileCompliance` 검증 실패 시의 **이유 목록**

- **offset** (number \| string)

  - 광고의 위치
  - **가능한 값:**
    - (미드롤) `{number in seconds}`
    - `post`
    - `pre`

- **placement** (number)

  - IAB 디지털 비디오 가이드라인에 따라, **플레이어의 위치를 식별하기 위해 입찰 요청에 전송되는 값**
  - **가능한 값:**
    - `1`: Instream
    - `2`: Accompanying Content
    - `3`: Interstitials
    - `4`: No Content / Standalone
  - 이 값은 `Object:Video` 내의 `plcmt` 속성으로 전송됩니다. 자세한 내용은 _List: Plcmt Subtypes - Video_ 참조.

- **podcount** (number)

  - **광고 팟(pod)** 내 전체 광고 수

- **request** (object)

  - **광고 태그 URL에 대한 XMLHttpRequest 객체**

- **response** (object)

  - 요청에 대한 **XML 응답 객체**

- **sequence** (number)

  - 광고가 속한 **시퀀스 번호** 반환

- **skipoffset** (number)

  - VAST 파일에 `skipoffset`이 없을 경우, **정적 VAST 광고에 추가되는 건너뛰기 오프셋 값**

- **tag** (string)

  - 광고 태그의 URL

- **type** (string)

  - 플레이어 이벤트의 유형.
  - 이 값은 항상 **adClick** 입니다.

- **universalAdId** (object)

  - 광고 XML에서 가져온 **시스템 간 광고 크리에이티브 추적용 고유 식별자**
  - 출처: _IAB Tech Lab Definition_

- **vastversion** (number)

  - VAST XML 내 **VAST 버전 참조 값**

- **viewable** (boolean)

  - 플레이어의 **가시성(Viewability)**
  - **가능한 값:**
    - `0`: 플레이어가 화면에 보이지 않음
    - `1`: 플레이어가 화면에 표시됨

- **wcount** (number)

  - 워터폴(waterfall) 카운트

- **witem** (number)

  - 워터폴(waterfall) 인덱스

<br>
<br>

# .on('adCompanions')

**(VAST, IMA)**
광고에 **컴패니언(Companion Ads)** 이 포함되어 있을 때마다 발생합니다.

IMA

```json
{
  "client": "googima",
  "placement": 1,
  "viewable": 1,
  "adposition": "pre",
  "tag": "<AD_TAG_URL>",
  "adBreakId": "h3utj61mmce1",
  "adPlayId": "h3utj61mmce1",
  "id": "h3utj61mmce1",
  "ima": {},
  "adtitle": "IAB Vast Samples Skippable",
  "adsystem": "GDFP",
  "creativetype": "video/webm",
  "duration": 51,
  "linear": "linear",
  "description": "IAB Vast Samples Skippable ad",
  "creativeAdId": "",
  "adId": "24283604",
  "universalAdId": [
    {
      "universalAdIdRegistry": "AD-ID",
      "universalAdIdValue": "ABCD1234567"
    }
  ],
  "advertiser": "",
  "dealId": "",
  "mediaFile": {
    "file": "file.webm"
  },
  "companions": [
    {
      "width": 300,
      "height": 250,
      "type": "html",
      "resource": "<a target=\"_blank\" id=\"32948875244\" href=\"<URL>\"><div class=\"overlayContainer\"><img src=\"<IMAGE_URL>\" height=\"250\" width=\"300\"><div class=\"overlayTextAttribution\"></div></div><iframe frameborder=\"0\" src=\"<IFRAME_URL>\" height=\"0\" width=\"0\" id=\"iframe734645111\" style=\"border: 0px; vertical-align: bottom; display: block; height: 0px; width: 0px;\"></iframe></a>"
    },
    {
      "width": 728,
      "height": 90,
      "type": "html",
      "resource": "<a target=\"_blank\" id=\"32948875364\" href=\"<URL>\"><div class=\"overlayContainer\"><img src=\"<IMAGE_URL>\" height=\"90\" width=\"728\"><div class=\"overlayTextAttribution\"></div></div><iframe frameborder=\"0\" src=\"<IFRAME_URL>\" height=\"0\" width=\"0\" id=\"iframe849947827\" style=\"border: 0px; vertical-align: bottom; display: block; height: 0px; width: 0px;\"></iframe></a>"
    }
  ],
  "type": "adCompanions"
}
```

VAST

```json
{
    "client": "vast",
    "placement": 1,
    "adBreakId": "1wn8z1bf6tas",
    "adPlayId": "1wn8z1bf6tas",
    "offset": "pre",
    "id": "31d1d4yumxpo",
    "tag": "//playertest-cdn.longtailvideo.com/pre.xml",
    "adposition": "pre",
    "sequence": 1,
    "witem": 1,
    "wcount": 1,
    "adsystem": "Alex_Vast",
    "skipoffset": 5,
    "adschedule": {...},
    "adtitle": "JW Test Preroll",
    "description": "",
    "adId": "232859236",
    "adVerifications": null,
    "advertiser": "",
    "advertiserId": "",
    "creativeId": "",
    "creativeAdId": "",
    "dealId": "",
    "request": {},
    "response": {
        "location": null
    },
    "conditionalAdOptOut": false,
    "vastversion": 2,
    "clickThroughUrl": "//jwplayer.com/",
    "mediaFileCompliance": false,
    "nonComplianceReasons": [
        "video/mp4 has only 1 qualities",
        "video/webm has only 1 qualities"
    ],
    "mediafile": {
        "file": "//content.bitsontherun.com/videos/1EI2jHpo-52qL9xLP.mp4"
    },
    "viewable": 1,
    "creativetype": "video/mp4",
    "companions": [
        {
            "width": 300,
            "height": 250,
            "type": "static",
            "resource": "//s3.amazonaws.com/qa.jwplayer.com/~alex/300x250_companion_1.swf",
            "creativeview": [
                "http://myTrackingURL/firstCompanion"
            ],
            "click": "http://jwplayer.com/"
        },
        {
            "width": 300,
            "height": 250,
            "type": "static",
            "resource": "//s3.amazonaws.com/qa.jwplayer.com/~alex/pre_300X250.jpg",
            "click": "http://jwplayer.com/"
        },
        {
            "width": 728,
            "height": 90,
            "type": "static",
            "resource": "//s3.amazonaws.com/qa.jwplayer.com/~alex/pre_728X90.jpg",
            "click": "http://jwplayer.com/"
        }
    ],
    "type": "adCompanions"
}
```

<br>

- **adBreakId** (string)

  - 각 광고 구간(ad break)의 고유 ID.
  - 동일한 광고 구간 내 여러 광고가 존재할 경우, 모두 같은 **adBreakId**를 가집니다.

- **adId** (string)

  - 광고 XML에서 제공되는 **광고 서버의 크리에이티브 식별자**.
  - 출처: _IAB Tech Lab Definition_

- **adPlayId** (string)

  - 각 광고의 고유 ID.
  - 동일한 광고 구간 내 여러 광고가 있을 경우, 각각 다른 **adPlayId**를 가집니다.

- **adposition** (string)

  - 광고의 위치.
  - **가능한 값:**
    - `mid`
    - `post`
    - `pre`

- **adschedule** (object)

  - 광고 구간의 설정 정보

- **adsystem** (string)

  - 광고 XML에 정의된 **광고 서버 이름**.
  - 출처: _IAB Tech Lab Definition_

- **adtitle** (string)

  - 광고 XML에 정의된 **광고의 일반 이름(Common Name)**.
  - 출처: _IAB Tech Lab Definition_

- **adVerifications** (object)

  - 광고 XML에서 정의된 **제3자 검증 코드 실행에 필요한 리소스 및 메타데이터 목록**.
  - 출처: _IAB Tech Lab Definition_

- **advertiser** (string)

  - 광고 XML에서 정의된 **광고주 이름**.
  - 광고 제공 주체에 의해 정의됨.
  - 출처: _IAB Tech Lab Definition_

- **advertiserId** (string)

  - 광고 XML에서 제공된 **선택적 광고주 ID**, 광고 서버에 의해 제공됨.
  - 출처: _IAB Tech Lab Definition_

- **clickThroughUrl** (string)

  - 광고 XML에서 정의된 **광고 클릭 시 이동할 광고주의 사이트 URI**.
  - 출처: _IAB Tech Lab Definition_

- **client** (string)

  - 현재 사용 중인 광고 클라이언트.
  - **가능한 값:**
    - `googima`
    - `vast`

- **companions** (array)

  - 사용 가능한 **컴패니언 광고 정보 목록**.
  - 참조: [companions[]](#companions)

- **conditionalAdOptOut** (boolean)

  - (**VPAID 전용**) VAST 응답에 포함된 `conditionalAd` 속성을 가진 광고를 **재생하지 않도록** 설정

- **creativeAdId** (string)

  - 광고 XML에서 정의된 **광고 서버의 고유 크리에이티브 식별자**.
  - 출처: _IAB Tech Lab Definition_

- **creativeId** (string)

  - 광고 XML에서 정의된 **크리에이티브를 제공하는 광고 서버 식별자**.
  - 출처: _IAB Tech Lab Definition_

- **creativetype** (string)

  - VAST XML에서 지정된 **현재 미디어 파일의 MIME 유형**

- **dealId** (string)

  - 광고 XML에서 **현재 광고의 래퍼(wrapper) 체인 상단부터 발견된 첫 번째 딜 ID** 반환.
  - 출처: _Google Definition_

- **description** (string)

  - 광고 XML에서 제공하는 **광고 설명(긴 형태)**.
  - 출처: _IAB Tech Lab Definition_

- **duration** (number)

  - 광고 XML에서 정의된 **선형 광고(linear ad)**의 재생 시간.
  - 형식: `HH:MM:SS.mmm`
  - 출처: _IAB Tech Lab Definition_

- **id** (string)

  - 고유 광고 식별자

- **ima** (object )
- **IMA SDK의 현재 재생 광고 인스턴스**와 JWP가 광고 요청 시 전달하는 **userRequestContext** 포함

- **linear** (string)

  - 광고 XML의 `linear` 속성 값.
  - **가능한 값:**
    - `linear`: 비디오 콘텐츠 재생을 중단하고 표시되는 **비디오 광고**
    - `nonlinear`: 플레이어 위에 오버레이되는 **정적 디스플레이 광고**, 콘텐츠 재생을 중단하지 않음

- **mediafile** | **mediaFile** (object)

  - 광고 XML에서 가져온 **선형 광고의 비디오 파일 정보**.
  - 출처: _IAB Tech Lab Definition_ |

- **mediaFileCompliance** (boolean)

  - 광고가 **mediaFile 표준을 준수하는지 여부**.
  - 다음 조건 중 하나를 충족해야 합니다:
    - `.m3u8` 파일
    - `VPAID` 형식
    - MIME 유형당 최소 3개의 품질 레벨 존재

- **nonComplianceReasons** (array)

  - `mediaFileCompliance` 검증 실패 사유 목록

- **offset** (number)

  - string

  - 광고 위치.
  - **가능한 값:**
    - (Midroll) `{number in seconds}`
    - `post`
    - `pre`

- **placement** (number)

  - IAB 디지털 비디오 가이드라인에 따라 **플레이어의 위치를 식별하기 위해 입찰 요청에 전송되는 값**.
  - **가능한 값:**
    - `1`: Instream
    - `2`: Accompanying Content
    - `3`: Interstitials
    - `4`: No Content / Standalone
  - 이 값은 `Object:Video` 내 `plcmt` 속성으로 전송됩니다. 자세한 내용은 _List: Plcmt Subtypes - Video_ 참조.

- **request** (object)

- **광고 태그 URL로 전송된 XMLHttpRequest 객체**

- **response** (object)

- **광고 서버에서 반환된 XML 응답**

- **sequence** (number)

  - 광고가 속한 **시퀀스 번호**

- **skipoffset** (number)

  - VAST 파일에 `skipoffset`이 없는 경우, **정적 VAST 광고에 추가되는 건너뛰기 오프셋 값**

- **tag** (string)

  - 광고 태그의 URL

- **type** (string)

  - 플레이어 이벤트 유형.
  - 이 값은 항상 **adCompanions** 입니다.

- **universalAdId** (object)

  - 광고 XML에서 제공된 **시스템 간 광고 크리에이티브 추적용 고유 식별자**.
  - 출처: _IAB Tech Lab Definition_

- **vastversion** (number)

  - VAST XML 내 **VAST 버전 참조 값**

- **viewable** (boolean)

  - 플레이어의 **가시성(Viewability)**.
  - **가능한 값:**
    - `0`: 플레이어가 화면에 표시되지 않음
    - `1`: 플레이어가 화면에 표시됨

- **wcount** (number)

  - 워터폴(Waterfall) 카운트

- **witem** (number)

  - 워터폴(Waterfall) 인덱스

<br>

### companions[]

- **click** (string)

  - **컴패니언 광고 클릭 시 이동할 링크 URL**.
  - `type`이 `static`인 경우에만 사용 가능.

- **creativeview** (array)

  - **creativeview 이벤트 트래킹 픽셀 목록**

- **height** (number)

  - **컴패니언 광고의 높이(px)**

- **resource** (string)

  - **정적(static)/iframe 리소스의 URL** 또는 **HTML 원본 콘텐츠**

- **type** (string)

  - **크리에이티브의 유형**.
  - **가능한 값:** `html`, `iframe`, `static`

- **width** (number)

  - **컴패니언 광고의 너비(px)**

<br>
<br>

# .on('adComplete')

광고 재생이 **완전히 종료될 때마다 발생**합니다.

DAI

```json
{
  "client": "dai",
  "viewable": 1,
  "id": "822vxuovefq0",
  "adPlayId": "g56ap7bne8u0",
  "adtitle": "External NCA1C1L1 Preroll",
  "adsystem": "GDFP",
  "creativetype": "application/x-mpegURL",
  "linear": "linear",
  "adposition": "pre",
  "type": "adComplete"
}
```

FreeWheel

```json
{
  "client": "freewheel",
  "tag": "placeholder_preroll",
  "freewheel": {
    "ad": {
      "adId": "17302931"
    }
  },
  "adposition": "pre",
  "id": "17302931",
  "linear": "linear",
  "creativetype": "video/mp4",
  "viewable": 1,
  "sequence": 1,
  "podcount": 2,
  "skipoffset": 3,
  "type": "adComplete"
}
```

IMA

```json
{
    "client": "googima",
    "placement": 1,
    "viewable": 1,
    "adposition": "pre",
    "tag": "//playertest-cdn.longtailvideo.com/vast-30s-ad.xml",
    "adBreakId": "lkjkoc1j7e5q",
    "adPlayId": "lkjkoc1j7e5q",
    "id": "lkjkoc1j7e5q",
    "ima": {...},
    "adtitle": "",
    "adsystem": "Alex_Vast",
    "creativetype": "video/mp4",
    "duration": 30,
    "linear": "linear",
    "description": "",
    "creativeAdId": "",
    "adId": "232859236",
    "universalAdId": [],
    "advertiser": "",
    "dealId": "",
    "mediaFile": {
        "file": "//content.jwplatform.com/videos/AEhg3fFb-bPwArWA4.mp4"
    },
    "type": "adComplete"
}
```

VAST

```json
{
    "client": "vast",
    "placement": 1,
    "adBreakId": "a87pfdzx2s35",
    "adPlayId": "a87pfdzx2s35",
    "offset": 10,
    "id": "c2mx5xekbclh",
    "tag": "//playertest-cdn.longtailvideo.com/mid.xml",
    "adposition": "mid",
    "sequence": 1,
    "witem": 1,
    "wcount": 1,
    "adsystem": "Alex_Vast",
    "skipoffset": 5,
    "adschedule": {...},
    "adtitle": "",
    "description": "",
    "adId": "232859236",
    "adVerifications": null,
    "advertiser": "",
    "advertiserId": "",
    "creativeId": "",
    "creativeAdId": "",
    "dealId": "",
    "request": {},
    "response": {...},
    "conditionalAdOptOut": false,
    "vastversion": 2,
    "clickThroughUrl": "//jwplayer.com/",
    "mediaFileCompliance": false,
    "nonComplianceReasons": [
        "video/mp4 has only 2 qualities"
    ],
    "mediafile": {
        "file": "//content.bitsontherun.com/videos/tafrxQYx-bPwArWA4.mp4"
    },
    "viewable": 0,
    "creativetype": "video/mp4",
    "categories": [],
    "type": "adComplete"
}
```

- **adBreakId** (string)

  - 각 광고 구간(ad break)의 고유 ID.
  - 동일한 광고 구간 내 여러 광고가 존재할 경우, 모든 광고는 동일한 **adBreakId**를 가집니다.

- **adId** (string)

  - 광고 XML에서 제공된 **광고 서버의 크리에이티브 식별자**.
  - 출처: _IAB Tech Lab Definition_

- **adPlayId** (string)

  - 각 광고의 고유 ID.
  - 동일한 광고 구간 내 여러 광고가 있을 경우, 각각 다른 **adPlayId**를 가집니다.

- **adposition** (string)

  - 광고의 위치.
  - **가능한 값:**
    - `mid`
    - `post`
    - `pre`

- **adschedule** (object)

  - 광고 구간 설정 정보

- **adsystem** (string)

  - 광고 XML에 정의된 **광고 서버 이름**.
  - 출처: _IAB Tech Lab Definition_

- **adtitle** (string)

  - 광고 XML에 정의된 **광고의 일반 이름(Common Name)**.
  - 출처: _IAB Tech Lab Definition_

- **adVerifications** (object)

  - 광고 XML에서 정의된 **제3자 검증 코드 실행을 위한 리소스 및 메타데이터 목록**.
  - 출처: _IAB Tech Lab Definition_

- **advertiser** (string)

  - 광고 XML에서 정의된 **광고주 이름**.
  - 광고 제공 주체(ad serving party)에 의해 정의됨.
  - 출처: _IAB Tech Lab Definition_

- **advertiserId** (string)

  - 광고 XML에서 제공된 **선택적 광고주 식별자**, 광고 서버가 제공.
  - 출처: _IAB Tech Lab Definition_

- **categories** (array)

  - 광고 XML에서 제공된 **광고 콘텐츠 카테고리 코드 또는 라벨 목록**.
  - 출처: _IAB Tech Lab Definition_

- **clickThroughUrl** (string)

  - 광고 XML에서 제공된 **광고 클릭 시 이동하는 광고주의 사이트 URI**.
  - 출처: _IAB Tech Lab Definition_

- **client** (string)

  - 현재 사용 중인 광고 클라이언트.
  - **가능한 값:**
    - `dai`
    - `freewheel`
    - `googima`
    - `vast`

- **conditionalAdOptOut** (boolean)

  - (**VPAID 전용**) VAST 응답 내 `conditionalAd` 속성이 포함된 광고를 **재생하지 않도록** 설정

- **creativeAdId** (string)

  - 광고 XML에서 정의된 **광고 서버의 고유 크리에이티브 식별자**.
  - 출처: _IAB Tech Lab Definition_

- **creativeId** (string)

  - 광고 XML에서 정의된 **크리에이티브를 제공하는 광고 서버 식별자**.
  - 출처: _IAB Tech Lab Definition_

- **creativetype** (string)

  - VAST XML에서 지정된 **현재 미디어 파일의 MIME 타입**

- **dealId** (string)

  - 광고 XML에서 **현재 광고의 래퍼(wrapper) 체인 상단부터 발견된 첫 번째 딜 ID** 반환.
  - 출처: _Google Definition_

- **description** (string)

  - 광고 XML에서 제공된 **광고의 상세 설명**.
  - 출처: _IAB Tech Lab Definition_

- **duration** (number)

  - 광고 XML에서 제공된 **선형 광고(linear ad)**의 재생 시간.
  - 형식: `HH:MM:SS.mmm`
  - 출처: _IAB Tech Lab Definition_

- **freewheel** (object)

  - `ad.adId` 속성 내에 **고유 광고 식별자**를 포함

- **id** (string)

  - 고유한 광고 식별자

- **ima** (object)

  - **IMA SDK의 현재 재생 광고 인스턴스** 및 JWP가 광고 요청 시 전달하는 **userRequestContext** 포함

- **linear** (string)

  - 광고 XML의 `linear` 속성 값.
  - **가능한 값:**
  - `linear`: 콘텐츠 재생을 중단하고 재생되는 **비디오 광고**
  - `nonlinear`: 플레이어 일부에 오버레이되는 **정적 디스플레이 광고**, 콘텐츠 재생은 중단되지 않음

- **mediafile** \| **mediaFile** (object)

  - 광고 XML에서 가져온 **선형 광고의 비디오 파일 정보**.
  - 출처: _IAB Tech Lab Definition_

- **mediaFileCompliance** (boolean)

  - 광고가 **mediaFile 규격 준수 여부**를 나타냅니다.
  - 다음 중 하나라도 충족하면 준수로 간주됩니다:
    - `.m3u8` 파일
    - `VPAID` 형식
    - MIME 타입당 최소 3개의 화질 레벨

- **nonComplianceReasons** (array)

  - `mediaFileCompliance` 검증 실패 이유 목록

- **offset** (number \| string)

  - 광고 위치.
  - **가능한 값:**
    - (Midroll) `{number in seconds}`
    - `post`
    - `pre`

- **placement** (number)

  - IAB 디지털 비디오 가이드라인에 따라 **플레이어의 위치를 식별하기 위해 입찰 요청에 포함되는 값**.
  - **가능한 값:**
    - `1`: Instream
    - `2`: Accompanying Content
    - `3`: Interstitials
    - `4`: No Content / Standalone<br>이 값은 `Object:Video` 내 `plcmt` 속성으로 전송됩니다. 자세한 내용은 _List: Plcmt Subtypes - Video_ 참조.

- **podcount** (number)

  - **광고 팟(pod)** 내 전체 광고 수

- **request** (object)

  - **광고 태그 URL로 전송된 XMLHttpRequest 객체**

- **response** (object)

  - **요청에 대한 XML 응답 객체**

- **sequence** (number)

  - 광고가 속한 **시퀀스 번호**

- **skipoffset** (number)

  - VAST 파일에 `skipoffset`이 없는 경우, **정적 VAST 광고에 추가되는 건너뛰기 오프셋 값**

- **tag** (string)

  - 광고 태그의 URL

- **type** (string)

  - 플레이어 이벤트 유형.
  - 이 값은 항상 **adComplete** 입니다.

- **universalAdId** (object)

  - 광고 XML에서 제공된 **시스템 간 광고 크리에이티브 추적용 고유 식별자**.
  - 출처: _IAB Tech Lab Definition_

- **vastversion** (number)

  - VAST XML 내 **VAST 버전 참조 값**

- **viewable** (boolean)

  - 플레이어의 **가시성(Viewability)**.
  - **가능한 값:**
    - `0`: 플레이어가 화면에 표시되지 않음
    - `1`: 플레이어가 화면에 표시됨

- **wcount** (number)

  - 워터폴(Waterfall) 카운트

- **witem** (number)

  - 워터폴(Waterfall) 인덱스

<br>
<br>

# .on('adError')

광고 재생을 **방해하는 오류가 발생할 때마다** 트리거됩니다.

> Google IMA가 사용 중인 경우, **단일 광고 태그(ad tag)** 에 대해 이 이벤트가 **여러 번 발생할 수 있습니다.**

### 반환 객체 (최소 포함 항목\*)

- **message** (string)

  - 광고 오류 메시지.
  - **가능한 값:** 아래의 표 참조.

- **tag** (string)

  - 오류를 발생시킨 광고 태그의 URL

- 추가 정보 가능 시 함께 반환될 수 있습니다.

### 가능한 오류 메시지 및 원인

| **Possible Error Messages** | **Causes**                                                                                        |
| --------------------------- | ------------------------------------------------------------------------------------------------- |
| `ad tag empty`              | 래핑된 광고 태그를 모두 검색한 후에도 **사용 가능한 광고가 없음**                                 |
| `error playing creative`    | **크리에이티브 파일 404 오류**                                                                    |
| `error loading ad tag`      | 기타 **광고 로드 중 발생한 모든 오류**                                                            |
| `invalid ad tag`            | **잘못된 XML** 또는 **형식이 올바르지 않은 VAST 구문**                                            |
| `no compatible creatives`   | **FLV 동영상 크리에이티브** 또는 **VPAID SWF** 파일이 **HTML5 플레이어에서 재생을 시도**하는 경우 |

<br>
<br>

# .on('adImpression')

<br>
<br>

# .on('adItem')

<br>
<br>

# .on('adLoaded')

<br>
<br>

# .on('adManager')

<br>
<br>

# .on('adMeta')

<br>
<br>

# .on('adPause')

<br>
<br>

# .on('adPlay')

<br>
<br>

# .on('adRequest')

<br>
<br>

# .on('adRequestedContentResume')

<br>
<br>

# .on('adSchedule')

<br>
<br>

# .on('adLoadedXML')

<br>
<br>

# .on('adSkipped')

<br>
<br>

# .on('adStarted')

<br>
<br>

# .on('adTime')

<br>
<br>

# .on('adViewableImpression')

<br>
<br>

# .on('adWarning')

<br>
<br>

# .on('adsManager')

<br>
<br>

# .on('beforeComplete')

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
<br>

# .on('beforePlay')

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
