# Advertising Events

> **비디오 광고 삽입(Video Ad Insertion)** 기능은 **JWP 엔터프라이즈 라이선스(Enterprise License)** 가 필요합니다.  
> 계정 업그레이드를 원하시면 JWP 팀에 문의하십시오.

이 API는 개발자가 **JWP Advertising Edition**의 동작을 보다 세밀하게 제어할 수 있도록 합니다.  
VAST 및 IMA 플러그인의 경우, 이 API를 통해 **노출(impression) 검증, 맞춤형 광고 스케줄링, 다중 컴패니언 광고 지원** 등을 구현할 수 있습니다.

<br>
<br>

## .on('adBidRequest')

**헤더 비딩(Header Bidding)** 이 입찰 요청을 시작할 때 트리거됩니다.

- **bidders** (array)

  - 현재 입찰 요청에 포함된 모든 입찰자(bidder)의 배열

    - **bidders.id** (number)

      - 각 입찰자에 대해 헤더 비딩에 사용되는 퍼블리셔 ID

    - **bidders.name** (string)

      - 입찰자 이름
      - 참조: _advertising.bids.bidders[].name_

- **bidTimeout** (number)

  - 사용자가 재생을 클릭한 후 입찰 응답을 기다리는 시간 (밀리초 단위)

- **client** (string)

  - 현재 사용 중인 광고 클라이언트
  - 가능한 값:
    - `dai`
    - `freewheel`
    - `googima`
    - `vast`

- **floorPriceCents** (number)

  - 입찰이 재생되기 위해 초과해야 하는 최저가(센트 단위)
  - ⚠️ DFP 중재(mediation) 계층을 사용하는 경우 반환되지 않음

- **floorPriceCurrency** (string)

  - `floorPriceCents` 값의 통화 단위
  - Facebook 비딩 시 `usd` 여야 하며, 중재 계층이 `jwp`로 설정된 경우에만 사용됨

- **mediationLayerAdServer** (string)

  - 어떤 광고를 재생할지 결정하는 **중재 계층(Mediation Layer)**
  - 가능한 값:
    - `dfp`
    - `jwp`
    - `jwpdfp`
    - `jwpspotx`

- **offset** (string)

  - 광고의 오프셋 (재생 시작 시점)

- **type** (string)

  - 이벤트 유형<br>이 값은 항상 `"adBidRequest"`

- **viewable** (number)

  - 플레이어의 가시성 여부
  - 가능한 값:
    - `0`: 화면에 보이지 않음
    - `1`: 화면에 보임

<br>
<br>

## .on('adBidResponse')

**헤더 비딩(Header Bidding)** 응답이 반환될 때 트리거됩니다.

- **bidders** (array)

  - 현재 입찰 요청에 포함된 모든 입찰자(bidder)의 배열

    - **bidders.id** (number)

      - 각 입찰자에 대해 헤더 비딩에 사용되는 퍼블리셔 ID

    - **bidders.name** (string)

      - 입찰자 이름
      - 참조: _advertising.bids.bidders[].name_

    - **bidders.priceInCents** (number)

      - 입찰 금액 (센트 단위)
      - ⚠️ JWP가 중재 계층으로 설정된 경우에만 사용됨

    - **bidders.result** (string)

      - 입찰자의 응답 결과
      - 가능한 값:
        - `bid`
        - `noBid`
        - `timeout`

    - **bidders.tagKey** (number)

      - 반환된 입찰의 tagKey
      - ⚠️ SpotX 입찰자에 대해서만 사용됨

    - **bidders.timeForBidResponse** (number)

      - 입찰 응답이 반환되기까지 걸린 시간 (밀리초 단위)

    - **bidders.winner** (boolean)

      - 해당 입찰자가 낙찰자인 경우 `true`로 설정
      - 가능한 값:
        - `false`
        - `true`

- **bidTimeout** (number)

  - 사용자가 재생을 클릭한 후 입찰 응답을 기다리는 시간 (밀리초 단위)

- **client** (string)

  - 현재 사용 중인 광고 클라이언트
  - 가능한 값:
    - `dai`
    - `freewheel`
    - `googima`
    - `vast`

- **floorPriceCents** (number)

  - 입찰이 재생되기 위해 초과해야 하는 최저가 (센트 단위)
  - ⚠️ DFP 중재 계층 사용 시 반환되지 않음

- **floorPriceCurrency** (string)

  - `floorPriceCents` 값의 통화 단위
  - Facebook 입찰 시 `usd` 여야 하며, 중재 계층이 `jwp`인 경우에만 사용됨

- **mediationLayerAdServer** (string)

  - 어떤 광고를 재생할지 결정하는 **중재 계층(Mediation Layer)**
  - 가능한 값:
    - `dfp`
    - `jwp`
    - `jwpdfp`
    - `jwpspotx`

- **offset** (string)

  - 광고의 오프셋 (재생 시작 시점)

- **placement** (string)

  - 플레이어 위치를 식별하기 위해 입찰 요청 시 전송되는 값
  - 가능한 값:
    - `article`
    - `banner`
    - `feed`
    - `floating`
    - `instream`
    - `interstitial`
    - `slider`

- **type** (string)

  - 이벤트의 유형
  - 이 값은 항상 `"adBidResponse"`

- **viewable** (number)

  - 플레이어의 가시성 여부
  - 가능한 값:
    - `0`: 보이지 않음
    - `1`: 보임

<br>
<br>

## .on('adBlock')

이 이벤트는 **JWP 설정에 VAST 또는 Google IMA 광고 플러그인**이 구성되어 있을 때,  
**시청자의 브라우저에서 광고 차단기(Ad Blocker)** 가 감지되면 트리거됩니다.  
이 시점에서 사용자가 광고 차단기를 비활성화하도록 요청할 수 있습니다.

<br>
<br>

## .on('adBreakEnd')

광고 재생이 종료되고 **플레이어로 제어권이 다시 넘어올 때** 트리거됩니다.

- **adposition** (string)

  - 광고의 위치
  - 가능한 값:
    - `mid`
    - `post`
    - `pre`

- **client** (string)

  - 현재 사용 중인 광고 클라이언트
  - 가능한 값:
    - `dai`
    - `freewheel`
    - `googima`
    - `vast`

- **type** (string)

  - 이벤트 유형
  - 이 값은 항상 `"adBreakEnd"`

- **viewable** (number)

  - 플레이어의 가시성 여부
  - 가능한 값:
    - `0`: 플레이어가 50% 미만으로 보이거나 비활성 탭에 있음
    - `1`: 플레이어가 최소 50% 이상 보이고 활성 탭에 있음

<br>
<br>

## .on('adBreakIgnored')

(VAST 전용)  
이 이벤트는 **이전에 완전히 재생된 광고 구간(ad break)** 과 **현재 광고 구간** 사이의 경과 시간이  
`advertising.rules.timeBetweenAds` 에 정의된 값보다 **짧을 때** 트리거됩니다.

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

  - 광고 구간(ad break)의 설명용 이름

- **offset** (number \| string)

  - 광고의 위치
  - 가능한 값:
    - `{초 단위 숫자}` → 미드롤 광고 구간의 경우 발생
    - `post`
    - `pre`

- **tag** (string)

  - 광고 태그의 URL

- **timeSinceLastAd** (number)

  - 현재 광고 구간과 마지막 광고 구간 사이의 경과 시간(초 단위)
  - 이 값은 항상 `advertising.rules.timeBetweenAds` 값보다 작음

- **type** (string)

  - 이벤트 유형
  - 이 값은 항상 `"adBreakIgnored"`

<br>
<br>

## .on('adBreakStart')

광고 요청이 완료된 **직후**, 광고가 플레이어에 로드되기 **직전에** 트리거됩니다.  
이 이벤트는 광고 **구간(ad break)** 내의 **첫 번째 광고 이전에만** 발생합니다.

- **adposition** (string)

  - 광고의 위치
  - 가능한 값:
    - `mid`
    - `post`
    - `pre`

- **client** (string)

  - 현재 사용 중인 광고 클라이언트
  - 가능한 값:
    - `dai`
    - `freewheel`
    - `googima`
    - `vast`

- **type** (string)

  - 이벤트의 유형
  - 이 값은 항상 `"adBreakStart"`

- **viewable** (number)

  - 플레이어의 가시성 여부
  - 가능한 값:
    - `0`: 플레이어가 50% 미만으로 보이거나 비활성 탭에 있음
    - `1`: 플레이어가 최소 50% 이상 보이고 활성 탭에 있음

<br>
<br>

## .on('adClick')

광고를 클릭하여 **랜딩 페이지로 이동할 때마다** 트리거되는 이벤트입니다.  
해당 이벤트는 **DAI, FreeWheel, IMA, VAST** 클라이언트에서 모두 지원됩니다.

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

- **adBreakId** (string)

  - 각 광고 구간(ad break)의 고유 ID
  - 동일한 광고 구간 내 여러 광고는 동일한 `adBreakId`를 가짐

- **adId** (string)

  - 광고 XML에서 크리에이티브를 제공하는 광고 서버의 식별자
  - _(IAB Tech Lab 정의)_

- **adPlayId** (string)

  - 각 광고의 고유 ID
  - 동일한 광고 구간 내 각 광고는 서로 다른 `adPlayId`를 가짐

- **adposition** (string)

  - 광고의 위치
  - 가능한 값:
    - `mid`
    - `post`
    - `pre`

- **adschedule** (object)

  - 광고 구간 설정 정보

- **adsystem** (string)

  - 광고 XML에서 광고를 반환한 광고 서버의 이름
  - _(IAB Tech Lab 정의)_

- **adtitle** (string)

  - 광고 XML에 정의된 광고의 이름
  - _(IAB Tech Lab 정의)_

- **adVerifications** (object)

  - 광고 XML에서 가져온, 제3자 측정 코드 실행에 필요한 리소스 및 메타데이터 목록
  - _(IAB Tech Lab 정의)_

- **advertiser** (string)

  - 광고 XML에서 광고주 이름
  - _(IAB Tech Lab 정의)_

- **advertiserId** (string)

  - 광고 XML에서 광고 서버가 제공한 광고주의 선택적 식별자
  - _(IAB Tech Lab 정의)_

- **categories** (array)

  - 광고 XML에서 광고 콘텐츠 카테고리를 식별하는 코드 또는 라벨 목록
  - _(IAB Tech Lab 정의)_

- **clickThroughUrl** (string)

  - 광고 XML에 정의된 광고 클릭 시 이동하는 광고주의 사이트 URI
  - _(IAB Tech Lab 정의)_

- **client** (string)

  - 현재 사용 중인 광고 클라이언트
  - 가능한 값:
    - `dai`
    - `freewheel`
    - `googima`
    - `vast`

- **conditionalAdOptOut** (boolean)

  - _(VPAID 전용)_ — VAST 응답 내 `conditionalAd` 속성이 있는 광고를 재생하지 않도록 설정

- **creativeAdId** (string)

  - 광고 XML에서 크리에이티브의 고유 식별자
  - _(IAB Tech Lab 정의)_

- **creativeId** (string)

  - 광고 XML에서 크리에이티브를 제공하는 광고 서버의 식별자
  - _(IAB Tech Lab 정의)_

- **creativetype** (string)

  - VAST XML에 지정된 현재 미디어 파일의 MIME 타입

- **dealId** (string)

  - 광고 XML에서 반환된, 현재 광고의 래퍼 체인에서 첫 번째 거래 ID
  - _(Google 정의)_

- **description** (string)

  - 광고 XML에서 제공하는 긴 광고 설명
  - _(IAB Tech Lab 정의)_

- **duration** (number)

  - 광고 XML에서 정의된 선형 광고의 재생 시간(`HH:MM:SS.mmm` 형식)
  - _(IAB Tech Lab 정의)_

- **freewheel** (object)

  - 고유 광고 식별자를 포함하는 객체 (`ad.adId` 속성 내)

- **id** (string)

  - 고유 광고 식별자

- **ima** (object)

  - IMA SDK에서 현재 재생 중인 광고 인스턴스 및 JWP가 전달한 `userRequestContext` 포함

- **linear** (string)

  - 광고 XML의 `linear` 속성 값
  - 가능한 값:
    - `linear`: 콘텐츠 재생을 중단하는 비디오 광고
    - `nonlinear`: 재생을 중단하지 않고 플레이어 위에 표시되는 정적 오버레이 광고

- **mediafile / mediaFile** (object)

  - 광고 XML에서 선형 광고용 비디오 파일 정보
  - _(IAB Tech Lab 정의)_

- **mediaFileCompliance** (boolean)

  - 광고가 `mediaFile` 규격을 준수하는지 여부
  - 준수 조건 중 하나:
  - `.m3u8` 파일
  - `VPAID` 광고
  - MIME 타입별 최소 3개 이상의 품질 수준

- **nonComplianceReasons** (array)

  - `mediaFileCompliance` 실패 이유 목록

- **offset** (number \| string)

  - 광고 위치
  - 가능한 값:
    - 미드롤: `{초 단위 숫자}`
    - `post`
    - `pre`

- **placement** (number)

  - 플레이어의 위치를 식별하는 값
  - (IAB Digital Video Guidelines 준수)
  - 가능한 값:
    - `1`: Instream
    - `2`: Accompanying Content
    - `3`: Interstitials
    - `4`: No Content / Standalone
  - 이 값은 `Object:Video` 내 `plcmt` 속성으로 전달됨.
  - 자세한 내용은 **List: Plcmt Subtypes - Video** 참조.

- **podcount** (number)

  - 현재 광고 묶음(pod) 내 총 광고 개수

- **request** (object)

  - 광고 태그 URL에 대한 XMLHttpRequest 요청

- **response** (object)

  - 광고 요청에 대한 XML 응답

- **sequence** (number)

  - 광고가 속한 시퀀스 번호

- **skipoffset** (number)

  - VAST 파일에 존재하지 않는 경우, 정적 VAST 광고에 추가되는 스킵 오프셋 값

- **tag** (string)

  - 광고 태그 URL

- **type** (string)

  - 플레이어 이벤트의 유형
  - 이 값은 항상 `"adClick"`

- **universalAdId** (object)

  - 광고 XML의 **보편적 크리에이티브 ID** — 여러 시스템 간 추적을 위해 유지되는 고유 식별자
  - _(IAB Tech Lab 정의)_

- **vastversion** (number)

  - VAST XML 내 버전 참조 값

- **viewable** (boolean)

  - 플레이어의 가시성 상태
  - 가능한 값:
    - `0`: 플레이어가 화면에 보이지 않음
    - `1`: 플레이어가 화면에 보임

- **wcount** (number)

  - 워터폴 내 광고 요청 수 (Waterfall Count)

- **witem** (number)

  - 워터폴 인덱스 (Waterfall Index)

<br>
<br>

## .on('adCompanions')

**(VAST, IMA)**  
광고에 **컴패니언(Companion)** 이 포함되어 있을 때 트리거됩니다.  
IMA, VAST 두 광고 클라이언트 모두에서 지원됩니다.

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

- **adBreakId** (string)

  - 각 광고 구간(ad break)의 고유 ID.
  - 동일한 광고 구간 내 여러 광고는 동일한 `adBreakId`를 가짐.

- **adId** (string)

  - 광고 XML에서 크리에이티브를 제공하는 광고 서버의 식별자 _(IAB Tech Lab 정의)_

- **adPlayId** (string)

  - 각 광고의 고유 ID.
  - 같은 광고 구간 내 광고들은 서로 다른 `adPlayId`를 가짐.

- **adposition** (string)

  - 광고 위치
  - 가능한 값:
    - `mid`
    - `post`
    - `pre`

- **adschedule** (object)

  - 광고 구간의 설정

- **adsystem** (string)

  - 광고 XML에서 광고를 반환한 광고 서버의 이름 _(IAB Tech Lab 정의)_

- **adtitle** (string)

  - 광고 XML에서 정의된 광고 이름 _(IAB Tech Lab 정의)_

- **adVerifications** (object)

  - 광고 XML에서 정의된 제3자 검증용 리소스 및 메타데이터 목록 _(IAB Tech Lab 정의)_

- **advertiser** (string)

  - 광고 XML에서 정의된 광고주의 이름 _(IAB Tech Lab 정의)_

- **advertiserId** (string)

  - 광고 서버가 제공한 광고주의 선택적 식별자 _(IAB Tech Lab 정의)_

- **clickThroughUrl** (string)

  - 광고 XML에서 정의된 클릭 시 광고주의 사이트로 이동하는 URI _(IAB Tech Lab 정의)_

- **client** (string)

  - 현재 사용 중인 광고 클라이언트
  - 가능한 값:
    - `googima`
    - `vast`

- **companions** (array)

  - 사용 가능한 컴패니언 광고 정보
  - 참조: `companions[]`

  - **companions.click** (string)

    - 컴패니언 클릭 시 이동할 URL _(type이 `static`일 때만 사용 가능)_

  - **companions.creativeview** (array)

    - `creativeview` 이벤트 트래킹 픽셀 배열

  - **companions.height** (number)

    - 컴패니언의 높이 (픽셀 단위)

  - **companions.resource** (string)

    - 정적/iframe 리소스의 URL 또는 HTML 콘텐츠

  - **companions.type** (string)

    - 크리에이티브 유형
    - 가능한 값:
      - `html`
      - `iframe`
      - `static`

  - **companions.width** (number)

    - 컴패니언의 너비 (픽셀 단위)

- **conditionalAdOptOut** (boolean)

  - _(VPAID 전용)_
  - `conditionalAd` 속성을 포함한 광고를 재생하지 않도록 설정

- **creativeAdId** (string)

  - 광고 XML에서 크리에이티브의 고유 식별자
  - _(IAB Tech Lab 정의)_

- **creativeId** (string)

  - 광고 XML에서 크리에이티브를 제공하는 광고 서버의 식별자
  - _(IAB Tech Lab 정의)_

- **creativetype** (string)

  - VAST XML에 정의된 현재 미디어 파일의 MIME 타입

- **dealId** (string)

  - 광고 XML의 래퍼 체인 내 첫 번째 거래 ID
  - _(Google 정의)_

- **description** (string)

  - 광고 XML에서 제공하는 긴 설명
  - _(IAB Tech Lab 정의)_

- **duration** (number)

  - 광고 XML에서 정의된 선형 광고의 재생 시간(`HH:MM:SS.mmm` 형식)
  - _(IAB Tech Lab 정의)_

- **id** (string)

  - 고유 광고 식별자

- **ima** (object)

  - IMA SDK에서 재생 중인 광고 인스턴스 및 `userRequestContext` 포함

- **linear** (string)

  - 광고 XML의 `linear` 속성 값
  - 가능한 값:
    - `linear`: 영상 재생을 중단하는 비디오 광고
    - `nonlinear`: 영상 재생을 중단하지 않고 플레이어에 오버레이되는 정적 광고

- **mediafile \| mediaFile** (object)

  - 광고 XML에서 선형 광고의 비디오 파일
  - _(IAB Tech Lab 정의)_

- **mediaFileCompliance** (boolean)

  - 광고가 `mediaFile` 규격을 준수하는지 여부
  - 다음 조건 중 하나 이상을 충족해야 함:
    - `.m3u8` 파일
    - `VPAID` 광고
    - MIME 타입별 최소 3개 품질 수준

- **nonComplianceReasons** (array)

  - `mediaFileCompliance` 실패 사유 목록

- **offset** (number \| string)

  - 광고 위치
  - 가능한 값:
    - 미드롤: `{초 단위 숫자}`
    - `post`
    - `pre` |

- **placement** (number)

  - 플레이어의 위치를 식별하는 값 (IAB Digital Video Guidelines 기반)
  - 가능한 값:
    - `1`: Instream
    - `2`: Accompanying Content
    - `3`: Interstitials
    - `4`: No Content / Standalone
  - `Object:Video` 내 `plcmt` 속성으로 전달됨.
  - 자세한 내용은 **List: Plcmt Subtypes - Video** 참조.

- **request** (object)

  - 광고 태그 URL로 보낸 XMLHttpRequest 요청

- **response** (object)

  - 요청에 대한 XML 응답

- **sequence** (number)

  - 해당 광고의 시퀀스 번호

- **skipoffset** (number)

  - VAST 파일에 없을 경우, 정적 VAST 광고에 추가된 스킵 오프셋 값

- **tag** (string)

  - 광고 태그 URL

- **type** (string)

  - 플레이어 이벤트의 유형
  - 항상 `"adCompanions"`

- **universalAdId** (object)

  - 광고 XML에서 정의된 시스템 간 크리에이티브 추적용 고유 식별자
  - _(IAB Tech Lab 정의)_

- **vastversion** (number)

  - VAST XML 내 버전 참조

- **viewable** (boolean)

  - 플레이어의 가시성 여부
  - 가능한 값:
    - `0`: 플레이어가 화면에 보이지 않음
    - `1`: 플레이어가 화면에 보임

- **wcount** (number)

  - 워터폴(Waterfall) 내 광고 요청 수

- **witem** (number)

  - 워터폴 인덱스

<br>
<br>

## .on('adComplete')

광고가 **완전히 재생을 마쳤을 때** 트리거됩니다.  
이 이벤트는 **DAI, FreeWheel, IMA, VAST** 클라이언트 모두에서 발생합니다.

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

  - 각 광고 구간(ad break)의 고유 ID
  - 동일한 광고 구간에 여러 광고가 있을 경우, 동일한 `adBreakId`를 가짐

- **adId** (string)

  - 광고 XML에서 크리에이티브를 제공하는 광고 서버의 식별자
  - _(IAB Tech Lab 정의)_

- **adPlayId** (string)

  - 각 광고의 고유 ID
  - 동일한 광고 구간 내에서는 각 광고마다 서로 다른 `adPlayId`를 가짐

- **adposition** (string)

  - 광고의 위치
  - 가능한 값:
    - `mid`
    - `post`
    - `pre`

- **adschedule** (object)

  - 광고 구간의 설정 정보

- **adsystem** (string)

  - 광고 XML에서 광고를 반환한 광고 서버의 이름
  - _(IAB Tech Lab 정의)_

- **adtitle** (string)

  - 광고 XML에서 정의된 광고의 일반 이름
  - _(IAB Tech Lab 정의)_

- **adVerifications** (object)

  - 광고 XML에서 정의된 제3자 검증용 리소스 및 메타데이터 목록
  - _(IAB Tech Lab 정의)_

- **advertiser** (string)

  - 광고 XML에서 광고 서버에 의해 정의된 광고주의 이름
  - _(IAB Tech Lab 정의)_

- **advertiserId** (string)

  - 광고 XML에서 광고 서버가 제공한 광고주의 선택적 식별자
  - _(IAB Tech Lab 정의)_

- **categories** (array)

  - 광고 XML에서 광고 콘텐츠 카테고리를 나타내는 코드 또는 라벨 목록
  - _(IAB Tech Lab 정의)_

- **clickThroughUrl** (string)

  - 광고 XML에서 광고 클릭 시 열리는 광고주의 사이트 URI
  - _(IAB Tech Lab 정의)_

- **client** (string)

  - 현재 사용 중인 광고 클라이언트
  - 가능한 값:
    - `dai`
    - `freewheel`
    - `googima`
    - `vast`

- **conditionalAdOptOut** (boolean)

  - _(VPAID 전용)_ — VAST 응답에 포함된 `conditionalAd` 속성이 있는 광고를 재생하지 않도록 지정

- **creativeAdId** (string)

  - 광고 XML에서 광고 서버가 부여한 크리에이티브의 고유 식별자
  - _(IAB Tech Lab 정의)_

- **creativeId** (string)

  - 광고 XML에서 크리에이티브를 제공하는 광고 서버의 식별자
  - _(IAB Tech Lab 정의)_

- **creativetype** (string)

  - VAST XML에 지정된 현재 미디어 파일의 MIME 타입

- **dealId** (string)

  - 광고 XML에서 현재 광고의 래퍼 체인 상단부터 존재하는 첫 번째 거래 ID
  - _(Google 정의)_

- **description** (string)

  - 광고 XML에서 제공하는 긴 광고 설명
  - _(IAB Tech Lab 정의)_

- **duration** (number)

  - 광고 XML에서 정의된 선형 광고의 재생 시간 (`HH:MM:SS.mmm` 형식)
  - _(IAB Tech Lab 정의)_

- **freewheel** (object)

  - `ad.adId` 속성 내 고유 광고 식별자를 포함

- **id** (string)

  - 고유 광고 식별자

- **ima** (object)

  - IMA SDK에서 현재 재생 중인 광고 인스턴스 및 JWP가 IMA SDK에 전달한 `userRequestContext` 포함

- **linear** (string)

  - 광고 XML의 `linear` 속성 값
  - 가능한 값:
    - `linear`: 콘텐츠 재생을 중단하는 비디오 광고
    - `nonlinear`: 콘텐츠 재생을 중단하지 않고 플레이어 위에 표시되는 정적 광고

- **mediafile \| mediaFile** (object)

  - 광고 XML에서 선형 광고용 비디오 파일
  - _(IAB Tech Lab 정의)_

- **mediaFileCompliance** (boolean)

  - 광고가 `mediaFile` 규격을 준수하는지 여부
  - 다음 조건 중 하나 이상을 충족해야 함:
    - `.m3u8` 파일 사용
    - `VPAID` 광고
    - MIME 타입별 최소 3개의 품질 레벨 존재

- **nonComplianceReasons** (array)

  - `mediaFileCompliance` 검증 실패 사유 목록

- **offset** (number \| string)

  - 광고의 위치
  - 가능한 값:
    - 미드롤:`{초 단위 숫자}`
    - `post`
    - `pre`

- **placement** (number)

  - 플레이어의 위치를 식별하기 위한 입찰 요청 시 전송되는 값  
    (IAB Digital Video Guidelines 준수)
  - 가능한 값:
    - `1`: Instream
    - `2`: Accompanying Content
    - `3`: Interstitials
    - `4`: No Content / Standalone
  - 이 값은 `Object:Video` 내 `plcmt` 속성으로 전달됨.
  - 자세한 내용은 _List: Plcmt Subtypes - Video_ 참조. | |

- **podcount** (number)

  - 현재 광고 묶음(pod)에 포함된 총 광고 개수

- **request** (object)

  - 광고 태그 URL에 대한 XMLHttpRequest 요청

- **response** (object)

  - 요청에 대한 XML 응답

- **sequence** (number)

  - 광고가 속한 시퀀스 번호

- **skipoffset** (number)

  - VAST 파일에 존재하지 않는 경우, 정적 VAST 광고에 추가된 스킵 오프셋 값

- **tag** (string)

  - 광고 태그 URL

- **type** (string)

  - 플레이어 이벤트의 카테고리
  - 이 값은 항상 `"adComplete"`

- **universalAdId** (object)

  - 광고 XML에서 정의된, 시스템 간 크리에이티브 추적을 위한 고유 식별자
  - _(IAB Tech Lab 정의)_

- **vastversion** (number)

  - VAST XML 내 버전 참조 값

- **viewable** (boolean)

  - 플레이어의 가시성 여부
  - 가능한 값:
    - `0`: 플레이어가 보이지 않음
    - `1`: 플레이어가 보임

- **wcount** (number)

  - 워터폴(Waterfall) 광고 요청 수

- **witem** (number)

  - 워터폴 인덱스

<br>
<br>

## .on('adError')

광고 재생이 **오류로 인해 중단될 때** 트리거됩니다.

> Google IMA를 사용하는 경우, **하나의 광고 태그에 대해 여러 번 발생할 수 있습니다.**

- **message** (string)

  - 광고 오류 메시지
  - 가능한 값은 아래 표 참조
  - 가능한 오류 메시지 및 원인

    - **ad tag empty**

      - 래핑된(ad wrapped) 광고 태그를 모두 확인했으나 사용할 광고가 없음

    - **error playing creative**

      - 크리에이티브 파일(creative file)에서 404 오류 발생

    - **error loading ad tag**

      - 그 외 모든 광고 로드 관련 오류

    - **invalid ad tag**

      - XML이 유효하지 않거나 VAST 구문이 잘못된 형식으로 작성됨

    - **no compatible creatives**

      - FLV 비디오 크리에이티브 또는 VPAID SWF가 HTML5 플레이어에서 재생을 시도함

- **tag** (string)

  - 오류가 발생한 광고 태그의 URL

<br>

필요 시 추가적인 `adError` 정보가 함께 반환될 수 있습니다.

<br>
<br>

## .on('adImpression')

**IAB(Interactive Advertising Bureau)** 정의에 따라 광고 노출(impression)이 발생할 때 트리거됩니다.  
즉, **비디오 광고가 실제로 재생을 시작하는 순간** 발생합니다.

적용 대상: **DAI, FreeWheel, IMA, VAST**

DAI

```json
{
  "client": "dai",
  "viewable": 1,
  "id": "z5bbhua8yw00",
  "adPlayId": "7e8jal5j9y80",
  "adtitle": "External NCA1C1L1 Preroll",
  "adsystem": "GDFP",
  "creativetype": "application/x-mpegURL",
  "linear": "linear",
  "adposition": "pre",
  "type": "adImpression"
}
```

FreeWheel

```json
{
  "client": "freewheel",
  "tag": "placeholder_preroll",
  "freewheel": {
    "ad": {
      "adId": "20747694"
    }
  },
  "adposition": "pre",
  "id": "20747694",
  "linear": "linear",
  "creativetype": "video/mp4",
  "viewable": 1,
  "timeLoading": 752,
  "type": "adImpression"
}
```

IMA

```json
{
  "client": "googima",
  "placement": 1,
  "viewable": 1,
  "adposition": "pre",
  "tag": "//playertest-cdn.longtailvideo.com/vast-30s-ad.xml?vid_t=sintel",
  "adBreakId": "m26lcvab5a1o",
  "adPlayId": "m26lcvab5a1o",
  "id": "m26lcvab5a1o",
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
  "timeLoading": 48,
  "type": "adImpression"
}
```

VAST

```json
{
  "client": "vast",
  "placement": 1,
  "adBreakId": "u73yp6nm93y1",
  "adPlayId": "u73yp6nm93y1",
  "offset": "pre",
  "id": "1bfd53080g7t",
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
  "duration": 60,
  "linear": "linear",
  "mediaFileCompliance": false,
  "nonComplianceReasons": ["video/mp4 has only 2 qualities"],
  "mediafile": {
    "file": "//content.jwplatform.com/videos/zz4Abp0Z-bPwArWA4.mp4"
  },
  "viewable": 1,
  "creativetype": "video/mp4",
  "categories": [],
  "timeLoading": 154,
  "type": "adImpression"
}
```

- **adBreakId** (string)

  - 각 광고 구간(ad break)의 고유 ID.
  - 동일한 광고 구간 내 여러 광고는 동일한 `adBreakId`를 가짐.

- **adId** (string)

  - 광고 XML에서 크리에이티브를 제공하는 광고 서버의 식별자 _(IAB Tech Lab 정의)_

- **adPlayId** (string)

  - 각 광고의 고유 ID.
  - 동일한 광고 구간 내 각 광고는 서로 다른 `adPlayId`를 가짐.

- **adposition** (string)

  - 광고 위치.
  - 가능한 값:
    - `mid`
    - `post`
    - `pre`

- **adschedule** (object)

  - 광고 구간의 설정 정보.

- **adsystem** (string)

  - 광고 XML에서 광고를 반환한 광고 서버의 이름
  - _(IAB Tech Lab 정의)_

- **adtitle** (string)

  - 광고 XML에서 정의된 광고의 공통 이름
  - _(IAB Tech Lab 정의)_

- **adVerifications** (object)

  - 광고 XML에서 제3자 검증용 리소스 및 메타데이터 목록
  - _(IAB Tech Lab 정의)_

- **advertiser** (string)

  - 광고 XML에서 광고 서버에 의해 정의된 광고주의 이름
  - _(IAB Tech Lab 정의)_

- **advertiserId** (string)

  - 광고 서버에서 제공하는 광고주의 선택적 식별자
  - _(IAB Tech Lab 정의)_

- **bidders** (array)

  - _(IMA 전용)_
  - 해당 광고 슬롯에 입찰한 입찰자 목록.
  - 헤더 비딩(header bidding)이 발생한 경우에만 포함됩니다.
  - 각 입찰자 속성은 **bidder object** 참조.

- **categories** (array)

  - 광고 XML에서 정의된 광고 콘텐츠 카테고리 코드 또는 라벨 목록
  - _(IAB Tech Lab 정의)_

- **clickThroughUrl** (string)

  - 광고 XML에서 광고 클릭 시 열리는 광고주 사이트의 URI
  - _(IAB Tech Lab 정의)_

- **client** (string)

  - 현재 사용 중인 광고 클라이언트.
  - 가능한 값:
    - `dai`
    - `freewheel`
    - `googima`
    - `vast`

- **conditionalAdOptOut** (boolean)

  - _(VPAID 전용)_
  - VAST 응답에 `conditionalAd` 속성이 포함된 광고를 재생하지 않도록 지정.

- **creativeAdId** (string)

  - 광고 XML에서 광고 서버가 부여한 크리에이티브의 고유 식별자
  - _(IAB Tech Lab 정의)_

- **creativeId** (string)

  - 광고 XML에서 크리에이티브를 제공하는 광고 서버의 식별자
  - _(IAB Tech Lab 정의)_

- **creativetype** (string)

  - VAST XML에서 지정된 현재 미디어 파일의 MIME 타입

- **dealId** (string)

  - 광고 XML의 래퍼 체인 중 최상단에서부터 발견된 첫 번째 거래 ID
  - _(Google 정의)_

- **description** (string)

  - 광고 XML에서 제공하는 긴 광고 설명
  - _(IAB Tech Lab 정의)_

- **duration** (number)

  - 광고 XML에서 정의된 선형 광고의 재생 시간 (`HH:MM:SS.mmm` 형식)
  - _(IAB Tech Lab 정의)_

- **freewheel** (object)

  - `ad.adId` 속성 내 고유 광고 식별자를 포함

- **id** (string)

  - 고유 광고 식별자

- **ima** (object)

  - IMA SDK에서 현재 재생 중인 광고 인스턴스 및 `userRequestContext` 포함

- **linear** (string)

  - 광고 XML의 `linear` 속성 값.
  - 가능한 값:
    - `linear`: 콘텐츠 재생을 중단하는 비디오 광고
    - `nonlinear`: 콘텐츠 재생을 중단하지 않고 플레이어 위에 오버레이되는 정적 광고

- **mediafile \| mediaFile** (object)

  - 광고 XML에서 선형 광고의 비디오 파일
  - _(IAB Tech Lab 정의)_

- **mediaFileCompliance** (boolean)

  - 광고가 `mediaFile` 규격을 준수하는지 여부.
  - 다음 중 하나 이상을 충족해야 함:
    - `.m3u8` 파일 사용
    - `VPAID` 광고
    - MIME 타입별 최소 3개의 품질 수준 존재

- **nonComplianceReasons** (array)

  - `mediaFileCompliance` 검증 실패 사유 목록

- **offset** (number \| string)

  - 광고의 위치.
  - 가능한 값:
    - 미드롤: `{초 단위 숫자}`
    - `post`
    - `pre` |

- **placement** (number)

  - IAB Digital Video Guidelines에 따라 플레이어 위치를 식별하기 위한 입찰 요청 값.
  - 가능한 값:
    - `1`: Instream
    - `2`: Accompanying Content
    - `3`: Interstitials
    - `4`: No Content / Standalone
  - 이 값은 `Object:Video`의 `plcmt` 속성으로 전송됨.
  - 자세한 내용은 _List: Plcmt Subtypes - Video_ 참조.

- **request** (object)

  - 광고 태그 URL로 전송된 XMLHttpRequest 요청 객체

- **response** (object)

  - 광고 태그 요청에 대한 XML 응답 객체

- **sequence** (number)

  - 광고가 속한 시퀀스 번호

- **tag** (string)

  - 광고 태그의 URL

- **timeLoading** (number)

  - 광고가 로드되는 데 걸린 시간 (밀리초 단위)

- **type** (string)

  - 플레이어 이벤트의 종류.
  - 이 이벤트의 경우 항상 `"adImpression"`

- **universalAdId** (object)

  - 광고 XML에서 정의된 시스템 간 크리에이티브 추적용 고유 식별자
  - _(IAB Tech Lab 정의)_

- **vastversion** (number)

  - VAST XML 내 버전 참조 값

- **viewable** (boolean)

  - 플레이어의 가시성 여부.
  - 가능한 값:
    - `0`: 플레이어가 화면에 보이지 않음
    - `1`: 플레이어가 화면에 보임

- **wcount** (number)

  - 워터폴(Waterfall) 광고 요청 수

- **witem** (number)

  - 워터폴 인덱스

- **wrapper** (array)

  - _(VAST 전용)_
  - 사용된 광고 래퍼에 지정된 광고 시스템 목록.
  - 인덱스 값은 래퍼의 중첩 레벨을 나타냄.

<br>
<br>

## .on('adItem')

VAST XML이 **파싱되고, 로드되며, 표시할 준비가 완료되었을 때** 트리거됩니다.  
적용 대상: **IMA, VAST**

IMA

```json
{
  "client": "googima",
  "placement": 1,
  "viewable": 1,
  "adposition": "pre",
  "tag": "{ad_tag_url}",
  "adBreakId": "1a2b3c4d5e6f",
  "adPlayId": "1a2b3c4d5e6f",
  "id": "1a2b3c4d5e6f",
  "ima": {
    ...
  },
  "adtitle": "LinearInlineSkippable",
  "adsystem": "GDFP",
  "creativetype": "video/mp4",
  "duration": 10,
  "linear": "linear",
  "description": "LinearInlineSkippable ad",
  "creativeAdId": "",
  "adId": "123456789",
  "universalAdId": [{
    ...
  }],
  "advertiser": "",
  "dealId": "",
  "mediaFile": {
    "file": "{linear_ad_video_file_url}"
  },
  "type": "adItem"
}
```

VAST

```json
{
  "client": "vast",
  "placement": 1,
  "adBreakId": "6f5e4d3c2b1a",
  "adPlayId": "6f5e4d3c2b1a",
  "offset": "pre",
  "item": {
    ...
  },
  "type": "adItem"
}
```

- **adBreakId** (string)

  - 각 광고 구간(ad break)의 고유 ID.
  - 동일한 광고 구간 내 여러 광고는 동일한 `adBreakId`를 가짐.

- **adId** (string)

  - 광고 XML에서 크리에이티브를 제공하는 광고 서버의 식별자.
  - _(IAB Tech Lab 정의)_

- **adPlayId** (string)

  - 각 광고의 고유 ID.
  - 동일한 광고 구간 내 각 광고는 서로 다른 `adPlayId`를 가짐.

- **adposition** (string)

  - 광고의 위치.
  - 가능한 값:
    - `mid`
    - `post`
    - `pre`

- **adsystem** (string)

  - 광고 XML에서 광고를 반환한 광고 서버의 이름.
  - _(IAB Tech Lab 정의)_

- **adtitle** (string)

  - 광고 XML에서 정의된 광고의 일반 이름.
  - _(IAB Tech Lab 정의)_

- **advertiser** (string)

  - 광고 XML에서 광고 서버에 의해 정의된 광고주의 이름.
  - _(IAB Tech Lab 정의)_

- **client** (string)

  - 현재 사용 중인 광고 클라이언트.
  - 가능한 값:
    - `dai`
    - `freewheel`
    - `googima`
    - `vast`

- **creativeAdId** (string)

  - 광고 XML에서 광고 서버가 부여한 크리에이티브의 고유 식별자.
  - _(IAB Tech Lab 정의)_

- **creativetype** (string)

  - VAST XML에서 지정된 현재 미디어 파일의 MIME 타입.

- **dealId** (string)

  - 광고 XML의 래퍼 체인 상단부터 존재하는 첫 번째 거래 ID.
  - _(Google 정의)_

- **description** (string)

  - 광고 XML에서 제공하는 긴 광고 설명.
  - _(IAB Tech Lab 정의)_

- **duration** (number)

  - 광고 XML에서 정의된 선형 광고의 재생 시간 (`HH:MM:SS.mmm` 형식).
  - _(IAB Tech Lab 정의)_

- **id** (string)

  - 고유 광고 식별자.

- **ima** (object)

  - IMA SDK에서 현재 재생 중인 광고 인스턴스와, JWP가 IMA SDK에 광고 요청 시 전달한 `userRequestContext`를 포함.

- **item** (object)

  - 현재 콘텐츠로 재생 중인 플레이리스트 항목.

- **linear** (string)

  - 광고 XML의 `linear` 속성 값.
  - 가능한 값:
    - `linear`: 콘텐츠 재생을 중단하는 비디오 광고
    - `nonlinear`: 콘텐츠 재생을 중단하지 않고 플레이어 위에 표시되는 디스플레이 광고

- **mediaFile** (object)

  - 광고 XML에서 선형 광고용 비디오 파일 정보를 포함.
  - _(IAB Tech Lab 정의)_

| **offset** (number \| string) 광고의 위치.

- 가능한 값:

  - (미드롤) `{초 단위 숫자}`
  - `post`
  - `pre` |

- **placement** (number)

  - IAB Digital Video Guidelines에 따라 플레이어의 위치를 식별하기 위한 입찰 요청 값.
  - 가능한 값:
    - `1`: Instream
    - `2`: Accompanying Content
    - `3`: Interstitials
    - `4`: No Content / Standalone
  - 이 값은 `Object:Video`의 `plcmt` 속성으로 전송됨.
  - 자세한 내용은 _List: Plcmt Subtypes - Video_ 참조.

- **sequence** (number)

  - 광고가 속한 시퀀스 번호를 반환.

- **tag** (string)

  - 광고 태그의 URL.

- **type** (string)

  - 플레이어 이벤트의 카테고리.
  - 이 이벤트의 경우 항상 `"adItem"`.

- **universalAdId** (object)

  - 광고 XML에서 정의된 시스템 간 크리에이티브 추적용 고유 식별자.
  - _(IAB Tech Lab 정의)_

- **viewable** (boolean)

  - 플레이어의 가시성 여부.
  - 가능한 값:
    - `0`: 플레이어가 보이지 않음.
    - `1`: 플레이어가 보임.

<br>
<br>

## .on('adLoaded')

VAST XML이 **파싱되어 로드되고, 표시할 준비가 완료되었을 때** 트리거됩니다.  
적용 대상: **IMA, VAST**

IMA

```json
{
  "client": "googima",
  "placement": 1,
  "viewable": 1,
  "adposition": "pre",
  "tag": "{ad_tag_url}",
  "adBreakId": "1a3c2b4d5e6f",
  "adPlayId": "1a3c2b4d5e6f",
  "id": "1a3c2b4d5e6f",
  "ima": {
    ...
  },
  "adtitle": "",
  "adsystem": "GDFP",
  "creativetype": "video/mp4",
  "duration": 10,
  "linear": "linear",
  "description": "",
  "creativeAdId": "",
  "adId": "232859236",
  "universalAdId": [],
  "advertiser": "",
  "dealId": "",
  "mediaFile": {
    "file": "{linear_ad_video_file_url}"
  },
  "type": "adLoaded"
}
```

VAST

```json
{
  "client": "vast",
  "placement": 1,
  "adBreakId": "1a2b3c4d5e6f",
  "adPlayId": "1a2b3c4d5e6f",
  "offset": "pre",
  "id": "dlsjb0-test",
  "tag": "{ad_tag_url}",
  "adposition": "pre",
  "sequence": 1,
  "witem": 1,
  "wcount": 1,
  "adsystem": "",
  "adschedule": {
    ...
  },
  "item": {
    ...
  },
  "timeLoading": 57,
  "type": "adLoaded"
}
```

- **adBreakId** (string)

  - 각 광고 구간(ad break)의 고유 ID.
  - 동일한 광고 구간 내 여러 광고는 동일한 `adBreakId`를 가짐.

- **adId** (string)

  - 광고 XML에서 크리에이티브를 제공하는 광고 서버의 식별자.
  - _(IAB Tech Lab에서 정의됨)_

- **adPlayId** (string)

  - 각 광고의 고유 ID.
  - 동일한 광고 구간 내 각 광고는 서로 다른 `adPlayId`를 가짐.

- **adposition** (string)

  - 광고의 위치.
  - 가능한 값:
    - `mid`
    - `post`
    - `pre`

- **adschedule** (object)

  - 광고 구간 정보.

- **adsystem** (string)

  - 광고 XML에서 광고를 반환한 광고 서버의 이름.
  - _(IAB Tech Lab에서 정의됨)_

- **adtitle** (string)

  - 광고 XML에서 정의된 광고의 공통 이름.
  - _(IAB Tech Lab에서 정의됨)_

- **advertiser** (string)

  - 광고 XML에서 광고 서버에 의해 정의된 광고주의 이름.
  - _(IAB Tech Lab에서 정의됨)_

- **client** (string)

  - 현재 사용 중인 광고 클라이언트.
  - 가능한 값:
    - `dai`
    - `freewheel`
    - `googima`
    - `vast`

- **creativeAdId** (string)

  - 광고 XML에서 광고 서버가 부여한 크리에이티브의 고유 식별자.
  - _(IAB Tech Lab에서 정의됨)_

- **creativetype** (string)

  - 광고 XML에서 지정된 현재 미디어 파일의 MIME 타입.

- **dealId** (string)

  - 광고 XML의 래퍼 체인 중 최상단부터 발견된 첫 번째 거래 ID.
  - _(Google에서 정의됨)_

- **description** (string)

  - 광고 XML에서 제공하는 광고 설명.
  - _(IAB Tech Lab에서 정의됨)_

- **duration** (number)

  - 광고 XML에서 정의된 선형 광고의 재생 시간 (`HH:MM:SS.mmm` 형식).
  - _(IAB Tech Lab에서 정의됨)_

- **id** (string)

  - 고유 광고 식별자.

- **ima** (object)

  - IMA SDK에서 현재 재생 중인 광고 인스턴스와, JWP가 IMA SDK에 광고 요청 시 전달한 `userRequestContext` 포함.

- **item** (object)

  - 현재 콘텐츠로 재생 중인 플레이리스트 항목.

- **linear** (string)

  - 광고 XML의 `linear` 속성 값.
  - 가능한 값:
    - `linear`: 콘텐츠 재생을 중단하는 비디오 광고
    - `nonlinear`: 콘텐츠 재생을 중단하지 않고 오버레이로 표시되는 정적 광고

- **mediaFile** (object)

  - 광고 XML에서 선형 광고용 비디오 파일 정보 포함.
  - _(IAB Tech Lab에서 정의됨)_

- **offset** (number`\|`string)

  - 광고의 위치.
  - 가능한 값:
    - `{초 단위 숫자}` (미드롤 광고의 경우)
    - `post`
    - `pre`

- **placement** (string)

  - 입찰 요청에서 플레이어의 위치를 식별하는 값.
  - 가능한 값:
    - `article`
    - `banner`
    - `feed`
    - `floating`
    - `instream`
    - `interstitial`
    - `slider`

- **sequence** (number)

  - 광고가 속한 시퀀스 번호.

- **tag** (string)

  - 광고 태그의 URL.

- **timeLoading** (integer)

  - 광고가 로드되는 데 걸린 시간(밀리초 단위).

- **type** (string)

  - 플레이어 이벤트의 종류.
  - 이 이벤트의 경우 항상 `"adLoaded"`.

- **universalAdId** (object)

  - 광고 XML에서 정의된 시스템 간 크리에이티브 추적용 고유 식별자.
  - _(IAB Tech Lab에서 정의됨)_

- **viewable** (number)

  - 플레이어의 가시성 여부.
  - 가능한 값:
    - `0`: 플레이어가 화면에 보이지 않음
    - `1`: 플레이어가 화면에 보임

- **wcount** (number)

  - 워터폴(Waterfall) 카운트.

- **witem** (number)

  - 워터폴 인덱스.

<br>
<br>

## .on('adManager')

JW Player `8.8.0` 이상부터는 `.on('adsManager')` 를 사용하십시오.

**FreeWheel 전용 이벤트**로, **광고 매니저(ad manager)** 가 플레이어에 로드될 때 트리거됩니다.  
이 이벤트를 사용하면 **광고 재생** 전에 퍼블리셔가 FreeWheel 광고 매니저의 추가 기능을 통합할 수 있습니다.

```json
{
  "adManager": {...},
  "type": "adManager"
}
```

- **adManager** (object)

  - 광고 매니저 설정 정보를 포함하는 객체

- **type** (string)

  - 플레이어 이벤트의 종류.
  - 이 이벤트의 경우 항상 `"adManager"`

<br>
<br>

## .on('adMeta')

**(VAST 전용)**  
플레이어가 광고로부터 **새로운 메타데이터를 수신할 때마다 지속적으로 트리거되는 이벤트**입니다.  
값은 광고의 구성에 따라 달라질 수 있습니다.

```json
{
  "client": "vast",
  "placement": 1,
  "adBreakId": "vy8zfpt58kw4",
  "adPlayId": "vy8zfpt58kw4",
  "offset": "pre",
  "id": "jmxfy711x9xh",
  "tag": "//playertest-cdn.longtailvideo.com/pre.xml",
  "adposition": "pre",
  "sequence": 1,
  "witem": 1,
  "wcount": 1,
  "adsystem": "Alex_Vast",
  "skipoffset": 3,
  "adschedule": {
    "item": 1,
    "breakid": "myAds",
    "tags": ["//playertest-cdn.longtailvideo.com/pre.xml"],
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
  "linear": "linear",
  "mediaFileCompliance": false,
  "nonComplianceReasons": ["video/mp4 has only 1 qualities", "video/webm has only 1 qualities"],
  "mediafile": {
    "file": "//content.bitsontherun.com/videos/1EI2jHpo-52qL9xLP.mp4"
  },
  "viewable": 1,
  "creativetype": "video/mp4",
  "categories": [],
  "adMessage": "This ad will end in xx",
  "companions": [
    {
      "width": 728,
      "height": 90,
      "type": "image/jpeg",
      "source": "//s3.amazonaws.com/qa.jwplayer.com/~alex/pre_728X90.jpg",
      "trackers": {
        "creativeView": ["http://myTrackingURL/firstCompanion"]
      },
      "clickthrough": "http://jwplayer.com/"
    }
  ],
  "skipMessage": "Skip in xx",
  "skipText": "Skip",
  "type": "adMeta"
}
```

- **adBreakId** (string)

  - 각 광고 구간(ad break)의 고유 ID.
  - 동일한 구간 내 모든 광고는 동일한 `adBreakId`를 가짐.

- **adId** (string)

  - 광고 XML에서 크리에이티브를 제공하는 광고 서버의 식별자.
  - _(IAB Tech Lab 정의)_

- **adMessage** (string)

  - 광고 재생 중 표시되는 문구.
  - _(IAB Tech Lab 정의)_

- **adPlayId** (string)

  - 각 광고의 고유 ID.
  - 동일한 구간 내 광고마다 다른 `adPlayId`를 가짐.

- **adposition** (string)

  - 광고의 위치.
  - 가능한 값:
    - `mid`
    - `post`
    - `pre`

- **adschedule** (object)

  - 광고 구간 설정 정보.

- **adsystem** (string)

  - 광고 XML에서 광고를 반환한 광고 서버의 이름.
  - _(IAB Tech Lab 정의)_

- **adtitle** (string)

  - 광고 XML에서 정의된 광고 이름.
  - _(IAB Tech Lab 정의)_

- **adVerifications** (object)

  - 광고 XML에서 제3자 측정 코드를 실행하기 위해 필요한 리소스 및 메타데이터 목록.
  - _(IAB Tech Lab 정의)_

- **advertiser** (string)

  - 광고 XML에서 광고 서버에 의해 정의된 광고주 이름.
  - _(IAB Tech Lab 정의)_

- **advertiserId** (string)

  - 광고 XML에서 제공된 광고주의 선택적 식별자.
  - _(IAB Tech Lab 정의)_

- **categories** (array)

  - 광고 XML에서 광고 콘텐츠 카테고리를 식별하는 코드 또는 라벨 목록.
  - _(IAB Tech Lab 정의)_

- **clickThroughUrl** (string)

  - 광고 XML에서 광고 클릭 시 열리는 광고주 사이트의 URI.
  - _(IAB Tech Lab 정의)_

- **client** (string)

  - 현재 사용 중인 광고 클라이언트.
  - 항상 `"vast"`.

- **companions** (array)

  - 이용 가능한 컴패니언 광고 정보.
  - 자세한 내용은 _adMeta companions[]_ 참조.

    - **companions.clickthrough** (string)

      - 사용자가 컴패니언 광고를 클릭할 때 이동할 링크 URL.
      - 단, `type`이 `static`일 때만 사용 가능.

    - **companions.height** (number)

      - 컴패니언 광고의 높이 (px 단위).

    - **companions.source** (string)

      - 크리에이티브의 리소스 URL.

    - **companions.trackers** (object)

      - 컴패니언 유닛의 제3자 추적 URL 목록.

    - **companions.type** (string)

      - 크리에이티브 유형.
      - 가능한 값:
        - `html`
        - `iframe`
        - `static`

    - **companions.width** (number)

      - 컴패니언 광고의 너비 (px 단위).

- **conditionalAdOptOut** (boolean)

  - _(VPAID 전용)_ VAST 응답 내 `conditionalAd` 속성을 가진 광고를 재생하지 않도록 지시.

- **creativeAdId** (string)

  - 광고 XML에서 광고 서버가 부여한 크리에이티브의 고유 식별자.
  - _(IAB Tech Lab 정의)_

- **creativeId** (string)

  - 광고 XML에서 크리에이티브를 제공하는 광고 서버의 식별자.
  - _(IAB Tech Lab 정의)_

- **creativetype** (string)

  - VAST XML에 지정된 현재 미디어 파일의 MIME 타입.

- **dealId** (string)

  - 광고 XML의 래퍼 체인 상단부터 존재하는 첫 번째 거래 ID.
  - _(Google 정의)_

- **description** (string)

  - 광고 XML에서 제공하는 긴 광고 설명.
  - _(IAB Tech Lab 정의)_

- **id** (string)

  - 고유 광고 식별자.

- **linear** (string)

  - 광고 XML의 `linear` 속성 값.
  - 가능한 값:
    - `linear`: 콘텐츠 재생을 중단하는 비디오 광고
    - `nonlinear`: 콘텐츠 위에 표시되는 정적 광고

- **mediafile** (object)

  - 광고 XML에서 선형 광고용 비디오 파일 포함.
  - _(IAB Tech Lab 정의)_

- **mediaFileCompliance** (boolean)

  - 광고가 미디어 파일 규격을 준수하는지 여부.
  - 다음 중 하나를 충족해야 함:
  - `.m3u8`
  - `VPAID`
  - MIME 타입당 최소 3개 품질 수준

- **offset** (number \| string)

  - 광고의 위치.
  - 가능한 값:
    - (미드롤) `{초 단위 숫자}`
    - `post`
    - `pre`

- **placement** (number)

  - IAB Digital Video Guidelines에 따른 플레이어 위치 값.
  - 가능한 값:
    - `1`: Instream
    - `2`: Accompanying Content
    - `3`: Interstitials
    - `4`: Content / Standalone

- **request** (object)

  - 광고 태그 URL에 대한 HTTP 요청 객체.

- **response** (object)

  - 요청에 대한 XML 응답 객체.

- **sequence** (number)

  - 광고가 속한 시퀀스 번호.

- **skipMessage** (string)

  - 사용자에게 표시되는 맞춤 카운트다운 문구.

- **skipoffset** (number)

  - VAST 파일에 존재하지 않을 경우, 정적 VAST 광고에 추가되는 스킵 오프셋 값.

- **skipText** (string)

  - 카운트다운이 끝난 후 ‘Skip’ 버튼에 표시되는 텍스트.

- **tag** (string)

  - 광고 태그의 URL.

- **type** (string)

  - 플레이어 이벤트의 종류.
  - 항상 `"adMeta"`.

- **vastversion** (number)

  - 광고 XML의 VAST 버전.

- **viewable** (boolean)

  - 플레이어의 가시성 여부.
  - 가능한 값:
    - `0`: 플레이어가 보이지 않음.
    - `1`: 플레이어가 보임.

- **wcount** (number)

  - 워터폴(Waterfall) 카운트.

- **witem** (number)

  - 워터폴 인덱스.

<br>
<br>

## .on('adPause')

광고가 **일시정지(Pause)** 될 때마다 트리거됩니다.  
적용 대상: **DAI, FreeWheel, IMA, VAST**

DAI

```json
{
  "newstate": "paused",
  "type": "adPause",
  "oldstate": "playing"
}
```

FreeWheel

```json
{
  "oldstate": "playing",
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
  "newstate": "paused",
  "pauseReason": "interaction",
  "type": "adPause"
}
```

IMA

```json
{
  "oldstate": "playing",
  "client": "googima",
  "placement": 1,
  "viewable": 1,
  "adposition": "pre",
  "tag": "//playertest-cdn.longtailvideo.com/vast-30s-ad.xml",
  "adBreakId": "1wskqlynd4xz",
  "adPlayId": "1wskqlynd4xz",
  "id": "1wskqlynd4xz",
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
  "newstate": "paused",
  "pauseReason": "interaction",
  "type": "adPause"
}
```

VAST

```json
{
  "oldstate": "playing",
  "client": "vast",
  "placement": 1,
  "adBreakId": "rtcr7l1md0og",
  "adPlayId": "rtcr7l1md0og",
  "offset": "pre",
  "id": "1njnx9fbk02y",
  "tag": "//playertest-cdn.longtailvideo.com/pre-60s.xml",
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
    "video/mp4 has only 2 qualities"
  ],
  "mediafile": {
    "file": "//content.jwplatform.com/videos/zz4Abp0Z-bPwArWA4.mp4"
  },
  "viewable": 1,
  "creativetype": "video/mp4",
  "categories": [],
  "newstate": "paused",
  "pauseReason": "interaction",
  "type": "adPause"
}
```

- **adBreakId** (string)

  - 각 광고 구간(ad break)의 고유 ID.
  - 같은 광고 구간에 여러 광고가 있을 경우 모두 동일한 `adBreakId`를 가짐.

- **adId** (string)

  - 광고 XML에서 크리에이티브를 제공하는 광고 서버의 식별자.
  - _(IAB Tech Lab 정의)_

- **adPlayId** (string)

  - 각 광고의 고유 ID.
  - 같은 광고 구간 내 각 광고는 서로 다른 `adPlayId`를 가짐.

- **adposition** (string)

  - 광고의 위치.
  - 가능한 값:
    - `mid`
    - `post`
    - `pre`

- **adschedule** (object)

  - 광고 구간 설정 정보.

- **adsystem** (string)

  - 광고 XML에서 광고를 반환한 광고 서버의 이름.
  - _(IAB Tech Lab 정의)_

- **adtitle** (string)

  - 광고 XML에서 정의된 광고 이름.
  - _(IAB Tech Lab 정의)_

- **adVerifications** (object)

  - 광고 XML에 포함된, 제3자 측정 코드를 실행하기 위한 리소스 및 메타데이터 목록.
  - _(IAB Tech Lab 정의)_

- **advertiser** (string)

  - 광고 XML에서 광고 서버에 의해 정의된 광고주 이름.
  - _(IAB Tech Lab 정의)_

- **advertiserId** (string)

  - 광고 XML에서 광고 서버가 제공한 광고주의 선택적 식별자.
  - _(IAB Tech Lab 정의)_

- **categories** (array)

  - 광고 XML에 정의된 광고 콘텐츠 카테고리 코드 또는 라벨 목록.
  - _(IAB Tech Lab 정의)_

- **clickThroughUrl** (string)

  - 광고 XML에서 광고 클릭 시 열리는 광고주 사이트 URI.
  - _(IAB Tech Lab 정의)_

- **client** (string)

  - 현재 사용 중인 광고 클라이언트.
  - 가능한 값:
    - `dai`
    - `freewheel`
    - `googima`
    - `vast`

- **conditionalAdOptOut** (boolean)

  - _(VPAID 전용)_ VAST 응답 내 `conditionalAd` 속성을 가진 광고를 재생하지 않도록 지시.

- **creativeAdId** (string)

  - 광고 XML에서 광고 서버가 정의한 크리에이티브의 고유 식별자.
  - _(IAB Tech Lab 정의)_

- **creativeId** (string)

  - 광고 XML에서 크리에이티브를 제공하는 광고 서버의 식별자.
  - _(IAB Tech Lab 정의)_

- **creativetype** (string)

  - VAST XML에 지정된 현재 미디어 파일의 MIME 타입.

- **dealId** (string)

  - 광고 XML의 래퍼 체인 상단에서 발견된 첫 번째 거래 ID.
  - _(Google 정의)_

- **description** (string)

  - 광고 XML에서 제공하는 상세 설명.
  - _(IAB Tech Lab 정의)_

- **duration** (number)

  - 광고 XML에서 정의된 선형 광고의 재생 시간 (`HH:MM:SS.mmm` 형식).
  - _(IAB Tech Lab 정의)_

- **freewheel** (object)

  - `ad.adId` 속성에 포함된 고유 광고 식별자.

- **id** (string)

  - 고유 광고 식별자.

- **ima** (object)

  - IMA SDK의 현재 재생 광고 인스턴스 및 JWP가 IMA SDK로 광고 요청 시 전달한 `userRequestContext`.

- **linear** (string)

  - 광고 XML의 `linear` 속성 값.
  - 가능한 값:
    - `linear`: 콘텐츠 재생을 중단하는 비디오 광고
    - `nonlinear`: 콘텐츠 재생을 중단하지 않고 오버레이로 표시되는 광고

- **mediafile** \| **mediaFile** (`object`)

  - 광고 XML에서 선형 광고의 비디오 파일 정보 포함.
  - _(IAB Tech Lab 정의)_

- **mediaFileCompliance** (boolean)

  - 광고의 미디어 파일 준수 여부.
  - 다음 중 하나를 만족해야 함:
    - `.m3u8` 파일 사용
    - `VPAID` 광고
    - MIME 타입당 최소 3개 품질 수준 존재

- **newstate** (string)

  - 플레이어의 새로운 상태.
  - 이 이벤트에서는 항상 `"paused"`.

- **offset** (`number` \| `string`)

  - 광고의 위치.
  - 가능한 값:
    - (미드롤) `{초 단위 숫자}`
    - `post`
    - `pre`

- **oldstate** (string)

  - 광고 일시정지 전의 플레이어 상태.
  - 항상 `"playing"`.

- **pauseReason** (string) <sup>8.7.0+</sup>

  - 광고 재생이 일시정지된 이유.
  - 가능한 값:
    - `clickthrough`: 사용자가 광고를 클릭함
    - `external`: VPAID 네이티브 컨트롤 또는 `jwplayer().pauseAd()` 호출
    - `interaction`: 사용자가 플레이어의 일시정지 버튼을 클릭함

- **placement** (number)

  - IAB Digital Video Guidelines에 따른 플레이어 위치 값.
  - 가능한 값:
    - `1`: Instream
    - `2`: Accompanying Content
    - `3`: Interstitials
    - `4`: NContent \| Standalone

- **podcount** (number)

  - 광고 묶음(pod)의 총 광고 개수.

- **request** (object)

  - 광고 태그 URL로 보낸 XML HTTP 요청 객체.

- **response** (object)

  - 요청에 대한 XML 응답 객체.

- **sequence** (number)

  - 광고가 속한 시퀀스 번호.

- **skipoffset** (number)

  - VAST 파일에 명시되지 않은 경우 추가되는 스킵 오프셋 값.

- **tag** (string)

  - 광고 태그 URL.

- **type** (string)

  - 플레이어 이벤트의 종류.
  - 이 이벤트에서는 항상 `"adPause"`.

- **universalAdId** (object)

  - 광고 XML에 정의된 시스템 간 크리에이티브 추적용 고유 식별자.
  - _(IAB Tech Lab 정의)_

- **vastversion** (number)

  - VAST XML 버전 번호.

- **viewable** (boolean)

  - 플레이어의 가시성 여부.
  - 가능한 값:
    - `0`: 플레이어가 보이지 않음
    - `1`: 플레이어가 화면에 보임

- **wcount** (number)

  - 워터폴(Waterfall) 카운트.

- **witem** (number)

  - 워터폴 인덱스.

<br>
<br>

## .on('adPlay')

광고가 **재생을 시작하거나** 또는 **일시정지 상태에서 다시 재생될 때** 트리거됩니다.  
적용 대상: **DAI, FreeWheel, IMA, VAST**

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
  "newstate": "playing",
  "oldstate": "playing",
  "type": "adPlay"
}
```

FreeWheel

```json
{
  "oldstate": "buffering",
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
  "skipoffset": 3,
  "newstate": "playing",
  "playReason": "interaction",
  "type": "adPlay"
}
```

IMA

```json
{
  "oldstate": "buffering",
  "client": "googima",
  "placement": 1,
  "viewable": 1,
  "adposition": "pre",
  "tag": "//playertest-cdn.longtailvideo.com/vast-30s-ad.xml",
  "adBreakId": "lbvj8f1bk67a",
  "adPlayId": "lbvj8f1bk67a",
  "id": "lbvj8f1bk67a",
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
  "newstate": "playing",
  "playReason": "interaction",
  "type": "adPlay"
}
```

VAST

```json
{
  "oldstate": "buffering",
  "client": "vast",
  "placement": 1,
  "adBreakId": "rtcr7l1md0og",
  "adPlayId": "rtcr7l1md0og",
  "offset": "pre",
  "id": "1njnx9fbk02y",
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
  "newstate": "playing",
  "playReason": "interaction",
  "type": "adPlay"
}
```

- **adBreakId** (string)

  - 각 광고 구간(ad break)의 고유 ID.
  - 동일한 광고 구간 내 여러 광고는 같은 `adBreakId`를 가짐.

- **adId** (string)

  - 광고 XML에서 크리에이티브를 제공하는 광고 서버의 식별자.
  - _(IAB Tech Lab 정의)_

- **adPlayId** (string)

  - 각 광고의 고유 ID.
  - 광고 구간 내 각 광고는 서로 다른 `adPlayId`를 가짐.

- **adposition** (string)

  - 광고의 위치.
  - 가능한 값:
    - `mid`
    - `post`
    - `pre`

- **adschedule** (object)

  - 광고 구간 설정 정보.

- **adsystem** (string)

  - 광고 XML에서 광고를 반환한 광고 서버의 이름.
  - _(IAB Tech Lab 정의)_

- **adtitle** (string)

  - 광고 XML에서 정의된 광고 이름.
  - _(IAB Tech Lab 정의)_

- **adVerifications** (object)

  - 광고 XML에서 제3자 검증용 코드 실행에 필요한 리소스 및 메타데이터 목록.
  - _(IAB Tech Lab 정의)_

- **advertiser** (string)

  - 광고 XML에서 광고 서버에 의해 정의된 광고주 이름.
  - _(IAB Tech Lab 정의)_

- **advertiserId** (string)

  - 광고 XML에서 광고 서버가 제공한 광고주의 선택적 식별자.
  - _(IAB Tech Lab 정의)_

- **categories** (array)

  - 광고 XML에서 광고 콘텐츠 카테고리를 식별하는 코드 또는 라벨 목록.
  - _(IAB Tech Lab 정의)_

- **clickThroughUrl** (string)

  - 광고 XML에서 광고 클릭 시 열리는 광고주의 웹사이트 URI.
  - _(IAB Tech Lab 정의)_

- **client** (string)

  - 현재 사용 중인 광고 클라이언트.
  - 가능한 값:
    - `dai`
    - `freewheel`
    - `googima`
    - `vast`

- **conditionalAdOptOut** (boolean)

  - _(VPAID 전용)_ VAST 응답 내 `conditionalAd` 속성이 포함된 광고 재생을 방지.

- **creativeAdId** (string)

  - 광고 XML에서 광고 서버가 정의한 크리에이티브의 고유 식별자.
  - _(IAB Tech Lab 정의)_

- **creativeId** (string)

  - 광고 XML에서 크리에이티브를 제공하는 광고 서버의 식별자.
  - _(IAB Tech Lab 정의)_

- **creativetype** (string)

  - VAST XML에서 지정된 현재 미디어 파일의 MIME 타입.

- **dealId** (string)

  - 광고 XML의 래퍼 체인 상단에서 발견된 첫 번째 거래 ID.
  - _(Google 정의)_

- **description** (string)

  - 광고 XML에서 제공하는 긴 광고 설명.
  - _(IAB Tech Lab 정의)_

- **duration** (number)

  - 광고 XML에서 정의된 선형 광고의 재생 시간 (`HH:MM:SS.mmm` 형식).
  - _(IAB Tech Lab 정의)_

- **freewheel** (object)

  - FreeWheel 광고의 경우 `ad.adId` 속성에 포함된 고유 식별자.

- **id** (string)

  - 고유 광고 식별자.

- **ima** (object)

  - 현재 재생 중인 광고 인스턴스 (IMA SDK 기준) 및 JWP가 광고 요청 시 전달한 `userRequestContext`.

- **linear** (string)

  - 광고 XML의 `linear` 속성 값.
  - 가능한 값:
    - `linear`: 콘텐츠 재생을 중단하는 비디오 광고
    - `nonlinear`: 콘텐츠 위에 겹쳐 표시되며 재생을 중단하지 않음

- **mediafile** \| **mediaFile** (object)

  - 광고 XML에서 선형 광고용 비디오 파일 포함.
  - _(IAB Tech Lab 정의)_

- **newstate** (string)

  - 광고 재생 후 플레이어의 새로운 상태.
  - 항상 `"playing"`.

- **offset** (number \| string)

  - 광고의 위치.
  - 가능한 값:
    - (미드롤) `{초 단위 숫자}`
    - `post`
    - `pre`

- **oldstate** (string)

  - 광고 재생 전 플레이어의 상태.

- **placement** (number)

  - IAB Digital Video Guidelines에 따른 플레이어 위치.
  - 가능한 값:
    - `1`: Instream
    - `2`: Accompanying Content
    - `3`: Interstitials
    - `4`: No Content \| Standalone

- **playReason** (string)

  - 광고 재생이 시작된 이유.
  - 가능한 값:
    - `autostart`: 자동 재생
    - `external`: API 호출(`jwplayer().playAd()` 등)
    - `interaction`: 클릭, 터치, 키보드 등 사용자 입력
    - `playlist`: 자동 다음 재생
    - `related-audio`: 추천 오디오 플레이리스트 자동 재생
    - `related-interaction`: 추천 항목으로 이동 시 사용자 상호작용

- **request** (object)

  - 광고 태그 URL에 대한 XML HTTP 요청 객체.

- **response** (object)

  - 요청에 대한 XML 응답 객체.

- **sequence** (number)

  - 광고가 속한 시퀀스 번호.

- **skipoffset** (number)

  - VAST 파일에 명시되지 않은 경우 추가되는 스킵 오프셋 값.

- **tag** (string)

  - 광고 태그 URL.

- **type** (string)

  - 플레이어 이벤트의 종류.
  - 이 이벤트에서는 항상 `"adPlay"`.

- **universalAdId** (object)

  - 광고 XML에서 시스템 간 추적을 위한 크리에이티브의 고유 식별자.
  - _(IAB Tech Lab 정의)_

- **vastversion** (number)

  - VAST XML 버전 번호.

- **viewable** (boolean)

  - 플레이어의 가시성 여부.
  - 가능한 값:
    - `0`: 플레이어가 화면에 표시되지 않음
    - `1`: 플레이어가 화면에 표시됨

- **wcount** (number)

  - 워터폴(Waterfall) 카운트.

- **witem** (number)

  - 워터폴 인덱스.

<br>
<br>

## .on('adRequest')

플레이어가 **광고를 요청할 때마다** 트리거됩니다.  
적용 대상: **FreeWheel, IMA, VAST**

FreeWheel

```json
{
  "client": "freewheel",
  "networkid": "90750",
  "type": "adRequest"
}
```

IMA

```json
{
  "client": "googima",
  "placement": 1,
  "viewable": 1,
  "tag": "//playertest-cdn.longtailvideo.com/vast-30s-ad.xml",
  "adBreakId": "lbvj8f1bk67a",
  "adPlayId": "lbvj8f1bk67a",
  "id": "lbvj8f1bk67a",
  "adposition": "pre",
  "type": "adRequest"
}
```

VAST

```json
{
  "client": "vast",
  "placement": 1,
  "adBreakId": "rtcr7l1md0og",
  "adPlayId": "rtcr7l1md0og",
  "offset": "pre",
  "id": "1njnx9fbk02y",
  "tag": "//playertest-cdn.longtailvideo.com/pre-60s.xml",
  "adposition": "pre",
  "sequence": 1,
  "witem": 1,
  "wcount": 1,
  "adsystem": "",
  "skipoffset": 5,
  "adschedule": {
    "item": 1,
    "breakid": "adbreak1",
    "tags": [
      "//playertest-cdn.longtailvideo.com/pre-60s.xml"
    ],
    "offset": "pre"
  },
  "item": {...},
  "type": "adRequest"
}
```

- **adBreakId** (string)

  - 각 광고 구간(ad break)의 고유 ID.
  - 동일한 광고 구간 내 여러 광고는 같은 `adBreakId`를 가짐.

- **adPlayId** (string)

  - 각 광고의 고유 ID.
  - 동일한 광고 구간 내 각 광고는 서로 다른 `adPlayId`를 가짐.

- **adposition** (string)

  - 광고의 위치.
  - 가능한 값:
    - `mid`
    - `post`
    - `pre`

- **adschedule** (object)

  - 광고 구간 설정 정보.

- **adsystem** (string)

  - 광고 XML에서 광고를 반환한 광고 서버의 이름.
  - _(IAB Tech Lab 정의)_

- **client** (string)

  - 현재 사용 중인 광고 클라이언트.
  - 가능한 값:
    - `dai`
    - `freewheel`
    - `googima`
    - `vast`

- **id** (string)

  - 고유 광고 식별자.

- **item** (object)

  - 현재 콘텐츠로 재생 중인 플레이리스트 항목.

- **networkid** (string)

  - FreeWheel 네트워크 식별자.
  - 예시: `90750`

- **offset** (number \| string)

  - 광고의 위치.
  - 가능한 값:
    - (미드롤) `{초 단위 숫자}`
    - `post`
    - `pre`

- **placement** (number)

  - IAB Digital Video Guidelines에 따른 플레이어의 위치 값.
  - 가능한 값:
    - `1`: Instream
    - `2`: Accompanying Content
    - `3`: Interstitials
    - `4`: No Content / Standalone
  - 이 값은 Object:Video의 `plcmt` 속성을 통해 전송됩니다.
  - 자세한 내용은 _List: Plcmt Subtypes - Video_ 참고.

- **sequence** (number)

  - 광고가 속한 시퀀스 번호.

- **skipoffset** (number)

  - VAST 파일에 명시되지 않은 경우 정적 VAST 광고에 추가되는 스킵 오프셋 값.

- **tag** (string)

  - 광고 태그 URL.

- **type** (string)

  - 플레이어 이벤트의 종류.
  - 이 이벤트에서는 항상 `"adRequest"`.

- **viewable** (boolean)

  - 플레이어의 가시성 여부.
  - 가능한 값:
    - `0`: 플레이어가 화면에 표시되지 않음
    - `1`: 플레이어가 화면에 표시됨

- **wcount** (number)

  - 워터폴(Waterfall) 카운트.

- **witem** (number)

  - 워터폴 인덱스.

<br>
<br>

## .on('adRequestedContentResume')

**(IMA, VAST)**
다음 중 하나의 이벤트가 발생할 때 트리거됩니다:

- 광고가 완료되었을 때
- 오류가 발생했을 때
- 광고가 건너뛰기(skipped) 되었을 때

이 이벤트는 **광고 재생이 끝나고 콘텐츠 재생으로 전환되는 시점**을 나타냅니다.

```json
{
  "type": "adRequestedContentResume"
}
```

- **type** (string)

  - 이벤트의 유형.
  - 이 이벤트에서는 항상 `"adRequestedContentResume"`.

<br>
<br>

## .on('adSchedule')

**(VAST)**  
광고 스케줄이 **플러그인에 의해 로드되고 파싱될 때** 트리거됩니다.

```json
{
  "client": "vast",
  "placement": 1,
  "item": {...},
  "tag": null,
  "adbreaks": [
    {
      "offset": "pre",
      "adbreak": {
        "item": 1,
        "breakid": "adbreak1",
        "tags": [
          "//playertest-cdn.longtailvideo.com/pre-60s.xml"
        ]
      }
    },
    {
      "offset": 15,
      "adbreak": {
        "item": 2,
        "breakid": "adbreak2",
        "tags": [
          "//playertest-cdn.longtailvideo.com/pre-60s.xml"
        ]
      }
    },
    {
      "offset": "post",
      "adbreak": {
        "item": 3,
        "breakid": "adbreak3",
        "tags": [
          "//playertest-cdn.longtailvideo.com/vast-30s-ad.xml"
        ]
      }
    }
  ],
  "type": "adSchedule"
}
```

- **adbreaks** (array)

  - 각 광고 구간(ad break)에 대한 정보를 포함하는 객체 배열.

- **client** (string)

  - 현재 사용 중인 광고 클라이언트.
  - 이 이벤트에서는 항상 `"vast"`.

- **item** (object)

  - 현재 콘텐츠로 재생 중인 플레이리스트 항목.

- **placement** (number)

  - IAB Digital Video Guidelines에 따라 광고 요청 시 전송되는 플레이어의 위치 값.
  - 가능한 값:
    - `1`: Instream
    - `2`: Accompanying Content
    - `3`: Interstitials
    - `4`: No Content / Standalone
  - 이 값은 Object:Video의 `plcmt` 속성을 통해 전송됩니다.
  - 자세한 내용은 _List: Plcmt Subtypes - Video_ 참고.

- **tag** (string)

  - 광고 태그의 URL.

- **type** (string)

  - 플레이어 이벤트의 유형.
  - 이 이벤트에서는 항상 `"adSchedule"`.

<br>
<br>

## .on('adLoadedXML')

**VAST 전용 이벤트**  
VAST 광고 클라이언트가 **광고 태그를 로드할 때** 트리거됩니다.

응답 객체에는 `XML` 매개변수가 포함되어 있으며,  
이 매개변수는 태그에서 다운로드된 XML 데이터를 노출합니다.

또한 다른 광고 이벤트(`adBreakId`, `adPlayId`, `adPosition`, `client`, `tag` 등)과 동일한 객체 속성들을 함께 포함합니다.

<br>
<br>

## .on('adSkipped')

광고가 **건너뛰기(skipped)** 되었을 때마다 트리거됩니다.

지원 클라이언트: **FreeWheel, IMA, VAST**

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
  "type": "adSkipped"
}
```

IMA

```json
{
  "client": "googima",
  "placement": 1,
  "viewable": 1,
  "adposition": "pre",
  "tag": "//pubads.g.doubleclick.net/gampad/ads...etc",
  "adBreakId": "wdglp2rmb9qe",
  "adPlayId": "wdglp2rmb9qe",
  "id": "wdglp2rmb9qe",
  "ima": {...},
  "adtitle": "External NCA1C1L1 LinearInlineSkippable",
  "adsystem": "GDFP",
  "creativetype": "video/mp4",
  "duration": 10,
  "linear": "linear",
  "description": "External NCA1C1L1 LinearInlineSkippable ad",
  "creativeAdId": "",
  "adId": "697200496",
  "universalAdId": [
    {
      "universalAdIdRegistry": "GDFP",
      "universalAdIdValue": "57860459056"
    }
  ],
  "advertiser": "",
  "dealId": "",
  "mediaFile": {
    "file": ""
  },
  "type": "adSkipped"
}
```

VAST

```json
{
  "client": "vast",
  "placement": 1,
  "adBreakId": "mlhd5f15zf6p",
  "adPlayId": "mlhd5f15zf6p",
  "offset": "pre",
  "id": "tfdpe61g5v1f",
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
  "duration": 60,
  "mediaFileCompliance": false,
  "nonComplianceReasons": ["video/mp4 has only 2 qualities"],
  "mediafile": {
    "file": "//content.jwplatform.com/videos/zz4Abp0Z-bPwArWA4.mp4"
  },
  "viewable": 1,
  "creativetype": "video/mp4",
  "categories": [],
  "position": 5.777861,
  "watchedPastSkipPoint": 0.7778609999999997,
  "type": "adSkipped"
}
```

- **adBreakId** (string)

  - 각 광고 구간(ad break)의 고유 ID.
  - 하나의 광고 구간에 여러 광고가 포함된 경우, 해당 광고들은 모두 동일한 `adBreakId`를 가짐.

- **adId** (string)

  - 광고 XML에서 정의된, 크리에이티브를 제공하는 광고 서버의 식별자.
  - _출처: IAB Tech Lab 정의_

- **adPlayId** (string)

  - 각 광고의 고유 ID.
  - 하나의 광고 구간에 여러 광고가 포함된 경우, 각 광고는 서로 다른 `adPlayId`를 가짐.

- **adposition** (string)

  - 광고의 위치.
  - 가능한 값:
    - `mid`
    - `post`
    - `pre`

- **adschedule** (object)

  - 광고 구간(ad break)의 설정 정보.

- **adsystem** (string)

  - 광고 XML에서 반환된 광고 서버의 이름.
  - _출처: IAB Tech Lab 정의_

- **adtitle** (string)

  - 광고 XML에서 정의된 광고의 일반 이름.
  - _출처: IAB Tech Lab 정의_

- **adVerifications** (object)

  - 광고 XML에서 정의된, 제3자 측정 코드 실행을 위해 필요한 리소스 및 메타데이터 목록.
  - _출처: IAB Tech Lab 정의_

- **advertiser** (string)

  - 광고 XML에서 정의된 광고주 이름.
  - _출처: IAB Tech Lab 정의_

- **advertiserId** (string)

  - 광고 XML에서 광고 서버가 제공한 광고주의 선택적 식별자.
  - _출처: IAB Tech Lab 정의_

- **categories** (array)

  - 광고 XML에서 정의된 광고 콘텐츠의 카테고리 코드 또는 라벨 목록.
  - _출처: IAB Tech Lab 정의_

- **clickThroughUrl** (string)

  - 광고 XML에서 정의된 클릭 시 열리는 광고주의 사이트 URL.
  - _출처: IAB Tech Lab 정의_

- **client** (string)

  - 현재 사용 중인 광고 클라이언트.
  - 가능한 값:
    - `freewheel`
    - `googima`
    - `vast`

- **conditionalAdOptOut** (boolean)

  - (VPAID 전용) VAST 응답 내 `conditionalAd` 속성이 포함된 광고를 재생하지 않도록 플레이어에 지시함.

- **creativeAdId** (string)

  - 광고 XML에서 정의된 광고 서버의 크리에이티브 고유 식별자.
  - _출처: IAB Tech Lab 정의_

- **creativeId** (string)

  - 광고 XML에서 정의된, 크리에이티브를 제공하는 광고 서버의 식별자.
  - _출처: IAB Tech Lab 정의_

- **creativetype** (string)

  - 크리에이티브의 MIME 타입.

- **dealId** (string)

  - 광고 XML에서 정의된 거래 ID 중, 가장 상단의 래퍼 체인에 있는 첫 번째 ID를 반환.
  - _출처: Google 정의_

- **description** (string)

  - 광고 XML에서 정의된 광고 설명.
  - _출처: IAB Tech Lab 정의_

- **duration** (number)

  - 광고 XML에서 정의된 선형 광고의 재생 시간(HH:MM:SS.mmm 형식).
  - _출처: IAB Tech Lab 정의_

- **freewheel** (object)

  - 고유 광고 식별자(`ad.adId`)를 포함.

- **id** (string)

  - 고유 광고 식별자.

- **ima** (object)

  - IMA SDK에서 현재 재생 중인 광고 인스턴스 및 JWP가 IMA SDK에 전달한 `userRequestContext` 포함.

- **linear** (string)

  - 광고 XML의 `linear` 속성 값.
  - 가능한 값:
    - `linear`: 콘텐츠 재생을 중단시키는 동영상 광고
    - `nonlinear`: 재생을 중단시키지 않고 플레이어 일부를 덮는 정적 광고

- **mediafile \| mediaFile** (object)

  - 광고 XML에서 정의된 선형 광고의 비디오 파일.
  - _출처: IAB Tech Lab 정의_

- **mediaFileCompliance** (boolean)

  - 광고가 미디어 파일 규격(mediaFile compliant)을 충족하는지 여부.
  - 다음 조건 중 하나를 만족해야 함:
    - `.m3u8` 파일
    - `VPAID` 사용
    - MIME 타입별 최소 3개의 품질 수준 보유

- **nonComplianceReasons** (array)

  - `mediaFileCompliance` 검사 실패의 원인 목록.

- **offset** (number \| string)

  - 광고의 위치.
  - 가능한 값:
    - `(Midroll)` 초 단위 숫자
    - `post`
    - `pre`

- **placement** (number)

  - IAB Digital Video Guidelines에 따른 광고 요청 시 플레이어 위치 식별 값.
  - 가능한 값:
    - `1`: Instream
    - `2`: Accompanying Content
    - `3`: Interstitials
    - `4`: No Content / Standalone
  - 이 값은 Object:Video의 `plcmt` 속성을 통해 전송됨.
  - 자세한 내용은 _List: Plcmt Subtypes - Video_ 참고.

- **podcount** (number)

  - 현재 광고 묶음(pod)에 포함된 총 광고 수.

- **position** (number)

  - 광고 크리에이티브 내 현재 재생 위치(초 단위).

- **request** (object)

  - 광고 태그 URL로 보낸 XML HTTP 요청 객체.

- **response** (object)

  - 요청에 대한 XML 응답 객체.

- **sequence** (number)

  - 광고가 속한 시퀀스 번호를 반환.

- **skipoffset** (number)

  - VAST 파일에 skip offset이 존재하지 않을 경우, 정적 광고에 추가된 기본 skip offset 값.

- **tag** (string)

  - 광고 태그의 URL.

- **type** (string)

  - 플레이어 이벤트 유형.
  - 이 이벤트에서는 항상 `"adSkipped"`.

- **universalAdId** (object)

  - 광고 XML에서 정의된, 시스템 간 추적용 고유 크리에이티브 식별자.
  - _출처: IAB Tech Lab 정의_

- **vastversion** (number)

  - VAST XML에서 정의된 VAST 버전.

- **viewable** (boolean)

  - 플레이어의 가시 여부.
  - 가능한 값:
    - `0`: 플레이어가 화면에 보이지 않음
    - `1`: 플레이어가 화면에 보임

- **watchedPastSkipPoint** (number)

  - 광고가 스킵 가능해진 이후, 사용자가 실제로 스킵하기까지 경과된 시간(초 단위).

- **wcount** (number)

  - 워터폴(waterfall) 수.

- **witem** (number)

  - 워터폴(waterfall) 인덱스.

<br>
<br>

## .on('adStarted')

**(VAST [VPAID], FreeWheel [VPAID], IMA [모든 광고])**  
광고 크리에이티브가 **JWP 플레이어에 재생 시작을 알릴 때** 트리거됩니다.

지원 클라이언트: **IMA, VAST**

<br>

IMA

```json
{
  "client": "googima",
  "placement": 1,
  "viewable": 1,
  "adposition": "pre",
  "tag": "//playertest-cdn.longtailvideo.com/vpaid2-jwp-30s.xml?vid_t=Bunny",
  "adBreakId": "179y6cu228me",
  "adPlayId": "179y6cu228me",
  "id": "179y6cu228me",
  "ima": {...},
  "adtitle": "VPAID 2 Linear",
  "adsystem": "Ad System",
  "creativetype": "application/javascript",
  "duration": 30,
  "linear": "linear",
  "description": "VPAID 2 Linear Video Ad",
  "creativeAdId": "",
  "adId": "1234567",
  "universalAdId": [],
  "advertiser": "",
  "dealId": "",
  "mediaFile": {
    "file": "//playertest.longtailvideo.com/vpaid-2-player-test.js"
  },
  "type": "adStarted"
}
```

VAST

```json
{
  "client": "vast",
  "placement": 1,
  "adBreakId": "hd7kj31r2jpu",
  "adPlayId": "hd7kj31r2jpu",
  "offset": "pre",
  "id": "1020ed21wj9r",
  "tag": "//playertest-cdn.longtailvideo.com/vast/adpod-first-vpaid.xml",
  "adposition": "pre",
  "sequence": 1,
  "witem": 1,
  "wcount": 1,
  "adsystem": "Ad System",
  "wrapperAdSystem": ["Alex_Vast", "Alex_Vast"],
  "wrappedTags": ["https://playertest.longtailvideo.com/vpaid2-jwp-30s.xml", "//playertest.longtailvideo.com/pre.xml", "//playertest.longtailvideo.com/mid.xml"],
  "wrapperAdIds": ["lr3"],
  "adschedule": {
    "item": 1,
    "tags": ["//playertest-cdn.longtailvideo.com/vast/adpod-first-vpaid.xml"],
    "offset": "pre"
  },
  "adtitle": "VPAID 2 Linear",
  "description": "VPAID 2 Linear Video Ad",
  "adId": "1234567",
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
  "vastversion": 3,
  "clickThroughUrl": "http://google.com",
  "mediaFileCompliance": true,
  "mediafile": {
    "file": "//playertest.longtailvideo.com/vpaid-2-player-test.js"
  },
  "viewable": 1,
  "podcount": 3,
  "creativetype": "application/javascript",
  "categories": [],
  "type": "adStarted"
}
```

- **adBreakId** (string)

  - 각 광고 구간(ad break)의 고유 ID.
  - 여러 광고가 동일한 구간에 포함된 경우, 동일한 `adBreakId` 값을 가집니다.

- **adId** (string)

  - 광고 XML에서 가져온 크리에이티브를 제공하는 광고 서버의 식별자.
  - _출처: IAB Tech Lab_

- **adPlayId** (string)

  - 각 광고의 고유 ID.
  - 여러 광고가 동일한 구간에 포함된 경우, 각 광고는 고유한 `adPlayId`를 가집니다.

- **adposition** (string)

  - 광고의 위치.
  - 가능한 값:
    - `mid`
    - `post`
    - `pre`

- **adschedule** (object)

  - 광고 구간(ad break)에 대한 설정 정보.

- **adsystem** (string)

  - 광고 XML에서 반환된 광고 서버의 이름.
  - _출처: IAB Tech Lab_

- **adtitle** (string)

  - 광고 XML에서 정의된 광고의 이름.
  - _출처: IAB Tech Lab_

- **adVerifications** (object)

  - 광고 XML에서 정의된 제3자 측정 코드 실행을 위한 리소스 및 메타데이터 목록.
  - _출처: IAB Tech Lab_

- **advertiser** (string)

  - 광고 XML에서 정의된 광고주 이름.
  - _출처: IAB Tech Lab_

- **advertiserId** (string)

  - 광고 XML에서 광고 서버가 제공한 광고주의 선택적 식별자.
  - _출처: IAB Tech Lab_

- **categories** (array)

  - 광고 XML에서 정의된 광고 콘텐츠의 카테고리 코드 또는 라벨 목록.
  - _출처: IAB Tech Lab_

- **clickThroughUrl** (string)

  - 광고 XML에서 정의된 클릭 시 열리는 광고주의 웹사이트 URI.
  - _출처: IAB Tech Lab_

- **client** (string)

  - 현재 사용 중인 광고 클라이언트.
  - 가능한 값:
    - `freewheel`
    - `googima`
    - `vast`

- **conditionalAdOptOut** (boolean)

  - _(VPAID 전용)_ VAST 응답에 포함된 `conditionalAd` 속성이 있는 광고를 재생하지 않도록 플레이어에 지시합니다.

- **creativeAdId** (string)

  - 광고 XML에서 정의된 광고 서버의 크리에이티브 고유 식별자.
  - _출처: IAB Tech Lab_

- **creativeId** (string)

  - 광고 XML에서 정의된, 크리에이티브를 제공하는 광고 서버의 식별자.
  - _출처: IAB Tech Lab_

- **creativetype** (string)

  - VPAID 크리에이티브의 MIME 타입.

- **dealId** (string)

  - 광고 XML의 래퍼 체인 중 최상단에 위치한 첫 번째 거래 ID를 반환.
  - _출처: Google_

- **description** (string)

  - 광고 XML에서 정의된 광고 설명.
  - _출처: IAB Tech Lab_

- **duration** (number)

  - 광고 XML에서 정의된 선형 광고의 재생 시간(`HH:MM:SS.mmm` 형식).
  - _출처: IAB Tech Lab_

- **freewheel** (object)

  - `ad.adId` 속성 내에 고유 광고 식별자를 포함합니다.

- **id** (string)

  - 고유 광고 식별자.

- **ima** (object)

  - IMA SDK의 현재 광고 인스턴스 및 JWP가 전달한 `userRequestContext`를 포함합니다.

- **linear** (string)

  - 광고 XML의 `linear` 속성 값.
  - 가능한 값:
    - `linear`: 콘텐츠 재생을 중단하는 동영상 광고
    - `nonlinear`: 재생을 중단하지 않고 플레이어 일부에 표시되는 정적 광고

- **mediafile \| mediaFile** (object)

  - 광고 XML에서 정의된 선형 광고의 비디오 파일.
  - _출처: IAB Tech Lab_

- **mediaFileCompliance** (boolean)

  - 광고가 mediaFile 규격을 충족하는지 여부.
  - 아래 조건 중 하나 이상을 만족해야 함:
    - `.m3u8` 파일 형식
    - `VPAID` 형식
    - MIME 타입당 최소 3개의 화질(quality level) 존재

- **offset** (number \| string)

  - 광고의 위치.
  - 가능한 값:
    - `(Midroll)` 초 단위 숫자
    - `post`
    - `pre`

- **placement** (number)

  - IAB Digital Video Guidelines에 따라 광고 요청 시 플레이어 위치를 나타내는 값.
  - 가능한 값:
    - `1`: Instream
    - `2`: Accompanying Content
    - `3`: Interstitials
    - `4`: No Content / Standalone
  - 이 값은 Object:Video의 `plcmt` 속성으로 전송됩니다.
  - 자세한 내용은 _List: Plcmt Subtypes - Video_ 참고.

- **podcount** (number)

  - 광고 묶음(pod)에 포함된 총 광고 수.

- **request** (object)

  - 광고 태그 URL로 보낸 XML HTTP 요청 객체.

- **response** (object)

  - 광고 태그 요청에 대한 XML 응답 객체.

- **sequence** (number)

  - 해당 광고가 속한 시퀀스 번호.

- **tag** (string)

  - 광고 태그의 URL.

- **type** (string)

  - 플레이어 이벤트 유형.
  - 이 이벤트의 값은 항상 `"adStarted"`.

- **universalAdId** (object)

  - 시스템 간 광고 크리에이티브 추적을 위한 고유 식별자.
  - _출처: IAB Tech Lab_

- **vastversion** (number)

  - VAST XML에서 정의된 VAST 버전.

- **viewable** (boolean)

  - 플레이어의 가시 여부.
  - 가능한 값:
    - `0`: 화면에 표시되지 않음
    - `1`: 화면에 표시됨

- **wcount** (number)

  - 워터폴(waterfall) 광고 수.

- **witem** (number)

  - 워터폴(waterfall) 인덱스.

<br>
<br>

## .on('adTime')

광고 재생이 진행 중일 때 주기적으로 발생하는 이벤트입니다.  
(DAI, FreeWheel, IMA, VAST 클라이언트에서 지원됨)

<br>

DAI

```json
{
  "client": "dai",
  "viewable": 1,
  "id": "cz1a5nqne800",
  "adPlayId": "6xszr0hh2v90",
  "adtitle": "External NCA1C1L3 Midroll",
  "adsystem": "GDFP",
  "creativetype": "application/x-mpegURL",
  "linear": "linear",
  "adposition": "mid",
  "position": 0.48821499999999673,
  "duration": 10.01,
  "type": "adTime"
}
```

FreeWheel

```json
{
  "client": "freewheel",
  "tag": "placeholder_midroll",
  "freewheel": {
    "ad": {
      "adId": "17302933"
    }
  },
  "adposition": "mid",
  "id": "17302933",
  "linear": "linear",
  "creativetype": "video/mp4",
  "viewable": 1,
  "sequence": 1,
  "podcount": 2,
  "skipoffset": 3,
  "position": 3.393394,
  "duration": 30,
  "type": "adTime"
}
```

IMA

```json
{
  "client": "googima",
  "placement": 1,
  "viewable": 1,
  "adposition": "pre",
  "tag": "//pubads.g.doubleclick.net/gampad/ads?sz=640x480...etc",
  "adBreakId": "1n822171mob5",
  "adPlayId": "1n822171mob5",
  "id": "1n822171mob5",
  "ima": {...},
  "adtitle": "External NCA1C1L1 LinearInlineSkippable",
  "adsystem": "GDFP",
  "creativetype": "video/mp4",
  "duration": 10,
  "linear": "linear",
  "description": "External NCA1C1L1 LinearInlineSkippable ad",
  "creativeAdId": "",
  "adId": "697200496",
  "universalAdId": [
    {
      "universalAdIdRegistry": "GDFP",
      "universalAdIdValue": "57860459056"
    }
  ],
  "advertiser": "",
  "dealId": "",
  "mediaFile": {
    "file": ""
  },
  "position": 0,
  "type": "adTime"
}
```

VAST

```json
{
  "client": "vast",
  "placement": 1,
  "adBreakId": "mlhd5f15zf6p",
  "adPlayId": "mlhd5f15zf6p",
  "offset": "pre",
  "id": "tfdpe61g5v1f",
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
  "duration": 60,
  "mediaFileCompliance": false,
  "nonComplianceReasons": ["video/mp4 has only 2 qualities"],
  "mediafile": {
    "file": "//content.jwplatform.com/videos/zz4Abp0Z-bPwArWA4.mp4"
  },
  "viewable": 1,
  "creativetype": "video/mp4",
  "position": 5.777861,
  "type": "adTime"
}
```

- **adBreakId** (string)

  - 각 광고 구간(ad break)의 고유 ID.
  - 동일한 광고 구간 내 여러 광고는 같은 `adBreakId` 값을 가집니다.

- **adId** (string)

  - 광고 XML에서 가져온, 크리에이티브를 제공하는 광고 서버의 식별자.
  - _출처: IAB Tech Lab_

- **adPlayId** (string)

  - 각 광고의 고유 ID.
  - 하나의 광고 구간 내 여러 광고가 있을 경우, 각 광고는 고유한 `adPlayId`를 가집니다.

- **adposition** (string)

  - 광고의 위치.
  - 가능한 값:
    - `mid`
    - `post`
    - `pre`

- **adschedule** (object)

  - 광고 구간(ad break)에 대한 설정 정보.

- **adsystem** (string)

  - 광고 XML에서 반환된 광고 서버의 이름.
  - _출처: IAB Tech Lab_

- **adtitle** (string)

  - 광고 XML에서 정의된 광고 이름.
  - _출처: IAB Tech Lab_

- **adVerifications** (object)

  - 광고 XML에 정의된 제3자 측정 코드를 실행하기 위한 리소스 및 메타데이터 목록.
  - _출처: IAB Tech Lab_

- **advertiser** (string)

  - 광고 XML에서 정의된 광고주 이름.
  - _출처: IAB Tech Lab_

- **advertiserId** (string)

  - 광고 XML에서 광고 서버가 제공한 광고주의 선택적 식별자.
  - _출처: IAB Tech Lab_

- **clickThroughUrl** (string)

  - 광고 XML에서 정의된 클릭 시 열리는 광고주의 사이트 URI.
  - _출처: IAB Tech Lab_

- **client** (string)

  - 현재 사용 중인 광고 클라이언트.
  - 가능한 값:
    - `dai`
    - `freewheel`
    - `googima`
    - `vast`

- **conditionalAdOptOut** (boolean)

  - _(VPAID 전용)_ VAST 응답 내 `conditionalAd` 속성이 포함된 광고를 재생하지 않도록 플레이어에 지시.

- **creativeAdId** (string)

  - 광고 XML에서 정의된 광고 서버의 크리에이티브 고유 식별자.
  - _출처: IAB Tech Lab_

- **creativeId** (string)

  - 광고 XML에서 정의된 크리에이티브 제공 광고 서버의 식별자.
  - _출처: IAB Tech Lab_

- **creativetype** (string)

  - 광고 XML에 명시된 현재 미디어 파일의 MIME 타입.

- **dealId** (string)

  - 광고 XML의 래퍼 체인 상단부터 첫 번째로 발견된 거래 ID.
  - _출처: Google_

- **description** (string)

  - 광고 XML에서 정의된 광고 설명.
  - _출처: IAB Tech Lab_

- **duration** (number)

  - 광고 XML에서 정의된 선형 광고의 총 재생 시간 (`HH:MM:SS.mmm` 형식).
  - _출처: IAB Tech Lab_

- **freewheel** (object)

  - `ad.adId` 속성 내에 고유 광고 식별자를 포함합니다.

- **id** (string)

  - 고유 광고 식별자.

- **ima** (object)

  - IMA SDK에서 현재 재생 중인 광고 인스턴스 및 JWP가 전달한 `userRequestContext` 포함.

- **linear** (string)

  - 광고 XML의 `linear` 속성 값.
  - 가능한 값:
    - `linear`: 콘텐츠 재생을 중단하는 동영상 광고
    - `nonlinear`: 콘텐츠 재생을 중단하지 않는 정적 오버레이 광고

- **mediafile \| mediaFile** (object)

  - 광고 XML에서 정의된 선형 광고의 비디오 파일.
  - _출처: IAB Tech Lab_

- **mediaFileCompliance** (boolean)

  - 광고가 mediaFile 규격을 충족하는지 여부.
  - 다음 조건 중 하나 이상을 만족해야 함:
    - `.m3u8` 파일
    - `VPAID` 형식
    - MIME 타입당 최소 3개의 품질 수준 보유

- **nonComplianceReasons** (array)

  - `mediaFileCompliance` 검증 실패의 이유 목록.

- **offset** (number \| string)

  - 광고의 위치.
  - 가능한 값:
    - `(Midroll)` 초 단위 숫자
    - `post`
    - `pre`

- **placement** (number)

  - IAB Digital Video Guidelines에 따라 광고 요청 시 플레이어의 위치를 나타내는 값.
  - 가능한 값:
    - `1`: Instream
    - `2`: Accompanying Content
    - `3`: Interstitials
    - `4`: No Content / Standalone
  - 이 값은 Object:Video의 `plcmt` 속성을 통해 전송됩니다.
  - 자세한 내용은 _List: Plcmt Subtypes - Video_ 참고.

- **position** (number)

  - 광고 크리에이티브 내 현재 재생 위치 (초 단위).

- **request** (object)

  - 광고 태그 URL로 보낸 XML HTTP 요청 객체.

- **response** (object)

  - 광고 태그 요청에 대한 XML 응답 객체.

- **sequence** (number)

  - 해당 광고가 속한 시퀀스 번호.

- **skipoffset** (number)

  - VAST 파일에 `skipoffset`이 없을 경우 정적 광고에 적용된 기본 스킵 오프셋 값.

- **tag** (string)

  - 광고 태그의 URL.

- **type** (string)

  - 플레이어 이벤트 유형.
  - 이 이벤트에서는 항상 `"adTime"`.

- **universalAdId** (object)

  - 광고 XML에서 정의된, 시스템 간 광고 크리에이티브 추적용 고유 식별자.
  - _출처: IAB Tech Lab_

- **vastversion** (number)

  - 광고 XML에서 정의된 VAST 버전 참조.

- **viewable** (boolean)

  - 플레이어의 가시 여부.
  - 가능한 값:
    - `0`: 화면에 표시되지 않음
    - `1`: 화면에 표시됨

- **wcount** (number)

  - 워터폴(waterfall) 수.

- **witem** (number)

  - 워터폴(waterfall) 인덱스.

<br>
<br>

## .on('adViewableImpression')

**VAST** 및 **IMA**에서 사용됩니다.  
다음 두 가지 조건이 모두 충족될 때만 트리거됩니다:

1. 광고가 **연속 2초 이상 재생된 경우**
2. 플레이어의 **50% 이상이 뷰포트(viewport)에 표시된 경우**

`adViewableImpression` 이벤트는 광고 노출의 **뷰어빌리티(viewability)** 를 추적하기 위해 사용됩니다.  
이 메트릭은 **Google의 TrueView viewable impression** 기준과 유사합니다.

지원 클라이언트: **IMA, VAST**

<br>

IMA

```json
{
  "client": "googima",
  "placement": 1,
  "viewable": 1,
  "adposition": "pre",
  "tag": "{google_ad_tag}",
  "adBreakId": "1a2b3c4d5e6f",
  "adPlayId": "1a2b3c4d5e6f",
  "id": "1a2b3c4d5e6f",
  "ima": {
    ...
  },
  "adtitle": "External Linear Inline",
  "adsystem": "GDFP",
  "creativetype": "video/mp4",
  "duration": 10,
  "linear": "linear",
  "description": "External Linear Inline ad",
  "creativeAdId": "",
  "adId": "697x312-p",
  "universalAdId": [{
    ...
  }],
  "advertiser": "",
  "dealId": "",
  "mediaFile": {
    ...
  },
  "type": "adViewableImpression"
}
```

VAST

```json
{
  "client": "vast",
  "placement": 1,
  "adBreakId": "f61a2b3c4d5e",
  "adPlayId": "f61a2b3c4d5e",
  "offset": "pre",
  "id": "f61a2b3c4d5e",
  "tag": "{ad_tag_url}",
  "adposition": "pre",
  "sequence": 1,
  "witem": 1,
  "wcount": 1,
  "adsystem": "Ad System",
  "adschedule": {
    ...
  },
  "adtitle": "VPAID 2 Linear",
  "description": "VPAID 2 Linear Video Ad",
  "adId": "1234567",
  "advertiser": "",
  "advertiserId": "",
  "creativeId": "",
  "creativeAdId": "",
  "dealId": "",
  "request": {},
  "response": {
    ...
  },
  "conditionalAdOptOut": false,
  "vastversion": 3,
  "clickThroughUrl": "https://click-through-url.com",
  "mediaFileCompliance": true,
  "mediafile": {
    ...
  },
  "viewable": 1,
  "creativetype": "application/javascript",
  "type": "adViewableImpression"
}
```

- **adBreakId** (string)

  - 각 광고 구간(ad break)의 고유 ID.
  - 여러 광고가 동일한 구간에 포함된 경우, 동일한 `adBreakId` 값을 가집니다.

- **adId** (string)

  - 광고 XML에서 가져온 크리에이티브를 제공하는 광고 서버의 식별자.
  - _출처: IAB Tech Lab_

- **adPlayId** (string)

  - 각 광고의 고유 ID.
  - 같은 광고 구간 내 여러 광고가 있을 경우, 각 광고는 다른 `adPlayId` 값을 가집니다.

- **adposition** (string)

  - 광고의 위치.
  - 가능한 값:
    - `mid`
    - `post`
    - `pre`

- **adschedule** (object)

  - 광고 구간(ad break)에 대한 정보.

- **adsystem** (string)

  - 광고 XML에서 반환된 광고 서버의 이름.
  - _출처: IAB Tech Lab_

- **adtitle** (string)

  - 광고 XML에서 정의된 광고의 이름.
  - _출처: IAB Tech Lab_

- **advertiser** (string)

  - 광고 XML에서 정의된 광고주의 이름.
  - _출처: IAB Tech Lab_

- **advertiserId** (string)

  - 광고 XML에서 광고 서버가 제공한 광고주의 선택적 식별자.
  - _출처: IAB Tech Lab_

- **clickThroughUrl** (string)

  - 광고 XML에서 정의된 클릭 시 열리는 광고주의 웹사이트 URI.
  - _출처: IAB Tech Lab_

- **client** (string)

  - 현재 사용 중인 광고 클라이언트.
  - 가능한 값:
    - `dai`
    - `freewheel`
    - `googima`
    - `vast`

- **conditionalAdOptOut** (boolean)

  - _(VPAID 전용)_ VAST 응답에 포함된 `conditionalAd` 속성을 가진 광고를 재생하지 않도록 지정.

- **creativeId** (string)

  - 광고 XML에서 크리에이티브를 제공하는 광고 서버의 식별자.
  - _출처: IAB Tech Lab_

- **creativeAdId** (string)

  - 광고 XML에서 정의된 크리에이티브의 고유 식별자.
  - _출처: IAB Tech Lab_

- **creativetype** (string)

  - 광고 XML에 명시된 현재 미디어 파일의 MIME 타입.

- **dealId** (string)

  - 광고 XML의 래퍼 체인에서 최상단부터 첫 번째로 발견된 거래 ID.
  - _출처: Google_

- **description** (string)

  - 광고 XML에서 정의된 광고 설명.
  - _출처: IAB Tech Lab_

- **duration** (number)

  - 광고 XML에서 정의된 선형 광고의 재생 시간 (`HH:MM:SS.mmm` 형식).
  - _출처: IAB Tech Lab_

- **id** (string)

  - 고유 광고 식별자.

- **ima** (object)

  - IMA SDK에서 현재 재생 중인 광고 인스턴스 및 JWP가 전달한 `userRequestContext` 포함.

- **linear** (string)

  - 광고 XML의 `linear` 속성 값.
  - 가능한 값:
    - `linear`: 콘텐츠 재생을 중단시키는 동영상 광고
    - `nonlinear`: 재생을 중단시키지 않는 오버레이형 광고

- **mediafile \| mediaFile** (object)

  - 광고 XML에서 정의된 선형 광고의 비디오 파일.
  - _출처: IAB Tech Lab_

- **mediaFileCompliance** (boolean)

  - 광고가 mediaFile 규격을 준수하는지 여부.
  - 다음 조건 중 하나 이상을 충족해야 함:
    - `.m3u8` 파일
    - `VPAID` 형식
    - MIME 타입당 최소 3개의 화질 수준

- **offset** (number \| string)

  - 광고의 위치.
  - 가능한 값:
    - `(Mid-roll ads)` 초 단위 숫자
    - `post`
    - `pre`

- **placement** (string)

  - IAB Digital Video Guidelines에 따라 광고 요청 시 플레이어의 위치를 나타내는 값.
  - 가능한 값:
    - `article`
    - `banner`
    - `feed`
    - `floating`
    - `instream`
    - `interstitial`
    - `slider`

- **request** (object)

  - 광고 태그 URL로 보낸 XML HTTP 요청 객체.

- **response** (object)

  - 광고 태그 요청에 대한 XML 응답 객체.

- **sequence** (number)

  - 광고가 속한 시퀀스 번호.

- **tag** (string)

  - 광고 태그의 URL.

- **type** (string)

  - 플레이어 이벤트의 카테고리.
  - 이 이벤트에서는 항상 `"adViewableImpression"`.

- **universalAdId** (object)

  - 광고 XML에서 정의된, 시스템 간 광고 크리에이티브 추적용 고유 식별자.
  - _출처: IAB Tech Lab_

- **vastversion** (number)

  - 광고 XML에서 정의된 VAST 버전 준수 정보.
  - _출처: IAB Tech Lab_

- **viewable** (number)

  - 플레이어의 가시 여부.
  - 가능한 값:
    - `0`: 화면에 표시되지 않음
    - `1`: 화면에 표시됨

- **wcount** (number)

  - 워터폴(waterfall) 광고 수.

- **witem** (number)

  - 워터폴(waterfall) 인덱스.

<br>
<br>

## .on('adWarning') <sup>8.13.0+</sup>

**VAST 전용 이벤트**입니다.  
광고 재생에는 치명적이지 않지만 **비정상적인 경고(non-fatal warning)** 가 발생했음을 나타냅니다.  
광고의 재생은 계속되지만, 일부 트래킹 이벤트나 구성 요소가 누락되었을 때 발생할 수 있습니다.

```json
{
  "message": "Tracking events are missing breakStart, breakEnd, or error for AdBreak",
  "code": 1002,
  "adErrorCode": 70001,
  "type": "adWarning",
  "tag": "{ad_tag_url}"
}
```

- **adErrorCode** (number)

  - JWP 광고 경고 코드입니다.

- **code** (number)

  - VAST 경고 코드입니다.

- **message** (string)

  - 광고 경고 메시지입니다.
  - 예: `"Tracking events are missing breakStart, breakEnd, or error for AdBreak"`

- **tag** (string)

  - 경고를 발생시킨 광고 태그의 URL입니다.

- **type** (string)

  - 플레이어 이벤트의 카테고리입니다.
  - 이 이벤트의 값은 항상 `"adWarning"` 입니다.

<br>
<br>

## .on('adsManager') <sup>8.5.2+ (IMA)</sup> <sup>8.8.0+ (FreeWheel, IMA)</sup>

플레이어에 **광고 관리자(ad manager)** 가 로드될 때 발생하는 이벤트입니다.  
이 이벤트는 광고 재생 전에 게시자가 **추가적인 광고 관리자 기능을 통합**할 수 있도록 합니다.

> 주의:  
> 이 이벤트를 통해 추가 기능을 통합할 경우 **광고 재생(ad playback)에 영향을 미칠 수 있습니다.**

```json
{
  "adsManager": {...},
  "type": "adsManager",
  "videoElement": {}
}
```

- **adsManager** (object)

  - 광고 관리자 설정을 포함하는 객체입니다.
  - 각 SDK(Google IMA 또는 FreeWheel)가 반환하는 속성에 대한 자세한 내용은 해당 문서를 참조하십시오.

- **type** (string)

  - 플레이어 이벤트의 유형.
  - 이 이벤트에서는 항상 `"adsManager"` 입니다.

- **videoElement** (object)

  - _(IMA 전용)_ 재생에 사용되는 HTML `<video>` 요소 객체입니다.

<br>
<br>

## .on('beforeComplete')

플레이어가 **재생을 완료하기 직전**에 발생하는 이벤트입니다.  
onComplete 이벤트와 달리, 이 시점에서는 플레이어가 **리플레이 화면을 표시하거나 다음 재생 항목(playlistItem)으로 이동하기 전**입니다.  
따라서 이 이벤트는 `playAd()`를 사용하여 **포스트롤(post-roll) 광고를 삽입하기에 적절한 시점**입니다.

- 반환값 없음

<br>
<br>

## .on('beforePlay')

다음 중 **어느 하나가 발생하기 직전**에 호출되는 이벤트입니다.

- 단일 미디어 또는 재생목록(playlist)의 개별 항목이 처음 재생되기 전
- 일시 정지된 상태에서 시청자 또는 다른 메커니즘에 의해 재생이 다시 시작되기 전

> 이 이벤트는 `.on('play')`보다 먼저 발생하므로,  
> `state`가 `"idle"`일 때 `loadAdTag(tag)` 또는 `loadAdTag(xml)`을 호출하여 **프리롤(preroll) 광고를 재생**하기 위한 트리거로 사용할 수 있습니다.

```json
{
  "playReason": "interaction",
  "state": "idle",
  "viewable": 1,
  "type": "beforePlay"
}
```

- **playReason** (string)

  - 재생이 시작된 이유를 나타냅니다.
  - 가능한 값:
    • `autostart` - 자동 재생
    • `external` - API를 통한 재생
    • `interaction` - 클릭, 터치, 키보드 입력 등 사용자 상호작용
    • `playlist` - 재생목록의 자동 전환
    • `related-audio` - 추천 오디오 재생목록의 자동 전환
    • `related-interaction` - 추천 재생목록 항목으로 이동 시 사용자 상호작용

- **state** (string)

  - 이벤트가 발생할 때의 재생 상태를 나타냅니다.
  - 가능한 값:
    • `idle` - 초기 재생 전 또는 재생목록 항목 전환 시 발생
    • `paused` - 단일 미디어 항목에서 재생이 재개될 때 발생

- **type** (string)

  - 이벤트의 종류입니다.
  - 이 값은 항상 `"beforePlay"`입니다.

- **viewable** (boolean)

  - 플레이어 컨테이너의 가시성을 나타냅니다.
  - 가능한 값:
    • `0` - 플레이어 컨테이너가 보이지 않음
    • `1` - 플레이어 컨테이너가 보임
