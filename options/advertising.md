# Advertising

> `advertising` 객체에서 변경한 내용은 대시보드에서 설정한 광고 설정을 모두 덮어씁니다.
> 따라서 `advertising` 객체를 사용할 때는 대시보드에서 이미 설정된 광고 구성을 반드시 포함해야 합니다.

<br>

이 객체는 **JW Player의 비디오 광고 기능을 구성**하며,  
대시보드에서 설정된 광고 구성을 **무시하고 재정의(overrides)** 합니다.  
광고 스케줄(`schedule`)이 지정되지 않은 경우, 기본적으로 **프리롤(preroll)** 광고가 재생됩니다.

<br>

- **client\*** (string)

  - (필수) 사용할 광고 클라이언트(ad client)를 지정합니다.
  - 가능한 값:
    - `dai`: Google IMA SDK를 사용한 동적 광고 삽입 (Dynamic Ad Insertion)
    - `freewheel`: FreeWheel 클라이언트 사용
    - `googima`: Google IMA SDK 사용 (일부 광고 태그에 필수)
    - `vast`: JWP VAST 클라이언트 사용

- **outstream\*** (boolean) <sup>8.6.0+</sup>

  - _(Outstream 전용 필수 속성)_ 아웃스트림 기능을 활성화합니다.
  - 가능한 값:
    - `true`: 아웃스트림 기능 활성화
    - `false`: 비활성화

- **adTagParameters** (object) <sup>8.18.3+</sup>

  - _(DAI)_ 광고 태그에 커스텀 매개변수(key-value 쌍)를 추가합니다.

- **adscheduleid** (string)

  - _(권장)_ 광고 스케줄(Ad Schedule)의 고유 식별자입니다.
  - 이 ID는 **Ad Schedule Detail 페이지의 Advanced 탭**에서 확인할 수 있습니다.
  - 대시보드에서 광고 스케줄을 생성하지 않은 경우, **임의의 8자리 영숫자 문자열**을 설정할 수 있습니다.

- **admessage** (string) <sup>< 8.6.0</sup>

  - 광고 재생 중 표시되는 문구입니다.
  - ⚠️주의: JWP 8.6.0 이상에서는 `intl` 객체를 사용해야 합니다.
  - 기본값: `The ad will end in xx seconds`

- **allowedOmidVendors** (array) <sup>8.20.0+</sup>

  - _(VAST)_ 허용할 OMID 벤더를 정의합니다.
  - 가능한 값:
    - 설정되지 않거나 빈 배열(`[]`)일 경우, 모든 벤더 허용
    - 설정된 경우, 지정된 벤더만 검증 실행
  - 실패 시 해당 검증 실패 내역이 광고 서버에 기록됩니다.

- **autoplayadsmuted** (boolean)

  - 모바일 환경에서 **음소거 상태로 자동 재생되는 인라인 플레이어**의 광고가 음소거된 상태로 재생되도록 허용합니다.

- **bids** (object)

  - _(IMA, VAST)_ Player Bidding 기능을 활성화하며, 설정된 입찰자(bidder)를 지정합니다.
  - 참고: `advertising.bids`

- **clearAdsOnComplete** (object)

  - 미디어 **재시작(replay)** 또는 **탐색(seek)** 시 광고 스케줄 블록을 유지할지 결정합니다.
  - 가능한 값:
    - `false`: (기본값) 광고 스케줄 유지
    - `true`: 재생 시 광고가 표시되지 않도록 광고 블록 제거

- **companiondiv** (object)

  - _(IMA, VAST)_ **컴패니언 광고(companion ad)** 가 표시될 div 정보를 지정합니다.
  - 참고: `advertising.companiondiv`

- **conditionaladoptout** (boolean)

  - _(VAST - VPAID 전용)_ `conditionalAd` 속성이 포함된 광고를 **재생하지 않도록** 설정합니다.
  - 기본값: `false`

- creativeTimeout (number)

  - _(VAST)_ VAST XML이 반환된 후 `adStart` 이벤트가 발생하기까지의 **최대 대기 시간(밀리초)**
  - 기본값: `15000`

- **cuetext** (string) <sup>< 8.6.0</sup>

  - 사용자가 광고 예약 마커 위에 마우스를 올렸을 때 표시되는 텍스트
  - ⚠️ 8.6.0 이상에서는 `intl` 객체 사용
  - 기본값: `Advertisement`

- **duration** (number)

  - _(FreeWheel)_ FreeWheel에서 제공하는 콘텐츠의 길이(초 단위)

- **enablePPS** (boolean \| object) <sup>8.34.3+</sup>

  - Google **Publisher Provided Signals (PPS)** 를 활성화하여,  
    광고 태그를 통해 IAB 컨텍스트 세그먼트를 전달함으로써 **프로그램매틱 수익 최적화**를 지원합니다.
  - 참고: `advertising.enablePPS`

- **enableVideoThumbnails** (boolean) <sup>8.38.4+</sup>

  - 대기 상태(idle)에서 **모션 썸네일(video thumbnails)** 을 표시할지 여부를 설정합니다.
  - 가능한 값:
    - `true`: (기본값) 자동 생성 또는 커스텀 모션 썸네일 표시
    - `false`: 정적 포스터 이미지만 표시

- **endstate** (string) <sup>8.6.0+</sup>

  - _(Outstream 전용)_ 광고가 모두 재생된 후의 플레이어 동작을 정의합니다.
  - 가능한 값:
    - `suspended`: (기본값) 회색 배경이 남고 컨트롤이 비활성화됨
    - `close`: 플레이어가 점차 닫힘

- **forceNonLinearFullSlot** (boolean)

  - _(IMA)_ 비선형 광고(non-linear ad)를 **오버레이 대신 전체 슬롯 광고(full slot)** 로 강제 표시합니다.

- **freewheel** (object)

  - _(FreeWheel)_ FreeWheel 광고 클라이언트 설정.
  - 참고: `advertising.freewheel`

- **fwassetid** (string)

  - _(FreeWheel)_ FreeWheel MRM에 구성된 콘텐츠의 식별자
  - 참고: `advertising.freewheel`

- **loadVideoTimeout** (number)

  - _(FreeWheel, IMA)_ VAST XML이 반환된 후 `adStart` 이벤트가 발생하기까지의 최대 대기 시간(밀리초)
  - 기본값: `15000`

- **locale** (string)

  - _(IMA)_ 건너뛰기 버튼(skip button) 텍스트의 언어 코드를 설정합니다. (2자리 언어 코드)

- **maxRedirects** (number)

  - _(IMA)_ 플레이어가 타임아웃 전에 따를 수 있는 **최대 리다이렉트 횟수**
  - 기본값: `4`

- **omidSupport** (string) <sup>8.20.2+</sup>

  - _(IMA, VAST)_ OMID(Open Measurement Interface Definition) 지원을 구성합니다.
  - 가능한 값:
    - `auto`: (VAST 기본값) VAST XML 내에 검증 노드가 있을 경우 OMID 스크립트를 로드
    - `disabled`: (IMA 기본값) OMID 스크립트를 절대 로드하지 않음
    - `enabled`: VAST 플러그인이 로드될 때 항상 OMID 스크립트를 로드

- **placement** (string) <sup>8.10.0+</sup>

  - _(IMA, VAST)_ 광고 입찰 요청(bid request)에 포함되는 **플레이어의 위치 정보 값**
  - 광고주가 비디오 광고 기회를 평가할 때 참고하는 데이터입니다.

  - 가능한 값:
    - `instream` (기본값) ｜ `article` (Outstream 기본값) ｜ `banner` ｜ `feed` ｜ `floating` ｜ `interstitial` ｜ `slider`

- **podmessage** (string) <sup>< 8.6.0</sup>

  - _(VAST)_ 광고 팟(pod) 재생 중 표시되는 문구.
  - `__AD_POD_CURRENT__` = 현재 광고 번호, `__AD_POD_LENGTH__` = 총 광고 수
  - ⚠️ 8.6.0 이상에서는 `intl` 객체 사용
  - 기본값: `Ad xx of yy.`

- **ppid** (string) <sup>8.13.3+</sup>

  - _(IMA)_ 퍼블리셔 제공 ID(Publisher Provided ID)를 IMA SDK로 전달합니다.

- **preloadAds** (boolean)

  - (IMA, VAST) 광고 브레이크(ad break)에 대한 광고 응답을 미리 요청합니다.
  - ⚠️주의: `true`로 설정하면 **프로그램매틱 광고의 충전율(fill rate)** 이 낮아질 수 있습니다.
  - 기본값: `false`

- **repeat** (boolean) <sup>8.6.0+</sup>

  - _(Outstream)_ 광고를 한 번만 재생할지, 계속 반복할지를 설정합니다.
  - 가능한 값:
    - `false`: (기본값) 모든 광고 재생 후 종료 상태로 전환
    - `true`: 광고를 계속 반복 재생 (사용자 또는 API 상호작용 시까지)

- **requestFilter** (function)

  - _(VAST)_ 광고 태그 요청에 사용되는 `XMLHttpRequest`를 **대체하거나 수정**할 수 있습니다.
  - `request` 매개변수는 다음 속성을 포함합니다:
    - `request.url`: 요청 URL
    - `request.xhr`: XMLHttpRequest 인스턴스
  - `xhr.onreadystatechange` 또는 `xhr.onerror` 콜백을 통해 요청 완료를 처리할 수 있으며,  
    `xhr.abort`를 호출하면 플레이어의 중단 요청을 수신할 수 있습니다.
  - **반드시 `xhr` 객체를 반환해야 합니다.**

- **requestTimeout** (number)

  - 광고 브레이크 시작 시점부터 **VAST 파일 또는 광고 노출(ad impression)** 이 반환될 때까지의 최대 대기 시간(밀리초).
  - **기본값:**
    - `5000` (VAST)
    - `10000` (IMA)
    - `15000` (FreeWheel)

- **rules** (object)

  - _(IMA, VAST)_ 광고 규칙(Ad Rules)을 활성화하며 관련 설정을 포함합니다.
  - 참고: `advertising.rules`

- **schedule** (array \| string)

  - 외부 **JSON 블록(array)** 또는 **VMAP XML(string)** 에서 광고 스케줄을 불러옵니다.
  - 참고: `advertising.schedule`

- **skipmessage** (string) <sup>< 8.6.0</sup>

  - _(FreeWheel, VAST)_ 사용자 정의 스킵 카운트다운 문구 설정
  - ⚠️ JWP 8.6.0 이상에서는 `intl` 객체 사용
  - 기본값: `Skip ad in xx`

- **skipoffset** (number)

  - _(FreeWheel, VAST)_ VAST 파일에 skip offset이 정의되어 있지 않은 경우, 수동으로 설정

- **skiptext** (string) <sup>< 8.6.0</sup>

  - _(FreeWheel, VAST)_ 카운트다운 후 “Skip” 버튼에 표시할 텍스트
  - ⚠️ JWP 8.6.0 이상에서는 `intl` 객체 사용
  - 기본값: `Skip`

- **tag** (array \| string)

  - _(필수)_ 광고 태그 URL을 지정합니다.  
    문자열일 경우 VAST/IMA/FreeWheel용 태그 URL이며,  
    배열일 경우 **VAST 태그의 백업 URL(fallback)** 로 사용됩니다.
  - Outstream 및 Player Bidding 사용 시 필수입니다.
  - `advertising.vastxml`과 동시에 사용할 수 없습니다.

- **trackingFilter** (function)

  - _(VAST)_ 특정 추적 픽셀 URL을 **필터링하여 전송되지 않게** 설정합니다.
  - 이 함수를 사용하려면, 필터링할 URL을 매개변수로 받고 `false`를 반환해야 합니다.

- **truncateMacros** (boolean)

  - _(IMA)_ 매크로 값의 길이를 **1000자**로 제한합니다.
  - 기본값: `true`

- **vastLoadTimeout** (number)

  - _(IMA)_ 광고 요청 후 **VAST 파일이 반환될 때까지의 최대 대기 시간(밀리초)**
  - 기본값: `10000`

- **vastxml** (string)

  - _(VAST)_ 광고 브레이크 중 요청되는 **VAST XML 광고 태그** 입니다.
  - `advertising.tag`와 함께 사용할 수 없습니다.

- **vpaidcontrols** (boolean)

  - _(IMA, VAST)_ VPAID 광고에 대해 **플레이어 컨트롤 표시 여부**를 설정합니다.
  - VPAID 광고가 자체 컨트롤을 가지고 있는 경우 중복될 수 있습니다.

- **vpaidmode** (string)

  - _(IMA - VPAID 전용)_ VPAID 기능 활성화 모드를 설정합니다.
  - 가능한 값:
    - `insecure`: (기본값) Friendly iframe 내에서 로드되어 사이트 접근 가능
    - `disabled`: VPAID 광고 비활성화 (요청 시 오류 반환)
    - `enabled`: Cross-domain iframe에서 VPAID 활성화 (사이트 접근 불가)

- **withCredentials** (boolean) <sup>8.13.0+</sup>

  - 광고 태그에 대한 **Access-Control 요청 시 자격 증명(credentials)** 을 사용할지 여부를 지정합니다.
  - 가능한 값:
    - `true`: (기본값) 쿠키, 인증 헤더, TLS 클라이언트 인증서 등을 포함하여 요청
    - `false`: 단일 요청만 보내며 자격 증명은 포함되지 않음

<br><br>

---

## advertising.bids

이 속성은 **지원되는 입찰자(bidder)** 와 함께 **Player Bidding** 기능을 활성화하고 구성할 때 사용됩니다.

JavaScript 예시

```javascript
jwplayer("myElement").setup({
  "playlist": "https://cdn.jwplayer.com/v2/playlists/a12bc3D4",
  "advertising": {
    ...
    "bids": {
      "bidOnBreaks": 3,
      "settings": {...},
      "bidders": [...],
      "ortbParams": {...}
    },
    ...
  }
});
```

<br>

- **bidders\*** (array)

  - _(필수)_ 각 입찰 파트너(bidding partner) 를 정의합니다.
  - 참고: advertising.bid.bidders

- **settings\*** (object)

  - _(필수)_ **중재 계층(mediation layer), 최저 입찰가(floor price), 타임아웃(timeout)** 을 정의합니다.
  - 참고: advertising.bid.settings

- **bidOnBreaks** (number)

  - 입찰 요청이 전송되는 **광고 브레이크(ad break)** 의 개수를 지정합니다.
  - 콘텐츠에 광고 브레이크가 3개 이상 있는 경우, 기본 설정값을 `3`으로 변경하고 성능에 따라 값을 조정할 수 있습니다.
  - 기본적으로 각 광고 브레이크마다 입찰 요청이 한 번씩 이루어집니다.

- **ortbParams** (object)

  - OpenRTB 입찰 요청에 포함되는 **OpenRTB 및 AdCOM (Advertising Common Object Model)** 데이터입니다.
  - 참고: advertising.bid.ortbParams

<br><br>

---

## advertising.bids.bidders[]

아래 확장 가능한 섹션에서는 **각 입찰자(bidder)** 에 대한 코드 예시와 속성을 확인할 수 있습니다.
입찰자의 이름을 클릭하면 해당 파트너에 대한 세부 정보를 볼 수 있습니다.

제공된 설정값에 따라 각 속성을 구성하십시오.

<br>

<details>
<summary><strong>Azerion</strong></summary>

```javascript
jwplayer("myElement").setup({
  "playlist": "https://cdn.jwplayer.com/v2/playlists/a12bc3D4",
  "advertising": {
    "bids": {
      ...
      "bidders": [{
        "name": "improvedigital",
        "placementId": 1111
      }]
    }
    ...
  }
});
```

- **name\*** (string)

  - _(필수)_ 입찰이 수신되는 광고 파트너 이름 (`improvedigital`)

- **placementId\*** (integer)

  - _(필수)_ Azerion Placement ID

</details>

<br>

<details>
<summary><strong>Connatix</strong></summary>

```javascript
jwplayer("myElement").setup({
  "playlist": "https://cdn.jwplayer.com/v2/playlists/a12bc3D4",
  "advertising": {
    "bids": {
      ...
      "bidders": [{
        "name": "connatix",
        "placementId": "{placementId}"
      }]
    }
    ...
  }
});
```

- **name\*** (string)

  - _(필수)_ 입찰이 수신되는 광고 파트너 이름 (`connatix`)

- **placementId\*** (integer)

  - _(필수)_ Connatix Placement ID

</details>

<br>

<details>
<summary><strong>Criteo</strong></summary>

```javascript
jwplayer("myElement").setup({
  "playlist": "https://cdn.jwplayer.com/v2/playlists/a12bc3D4",
  "advertising": {
    "bids": {
      ...
      "bidders": [{
        "name": "criteo",
        "networkId": 1111,
        "zoneId": 12345
      }]
    }
    ...
  }
});
```

- **name\*** (string)

  - _(필수)_ 입찰이 수신되는 광고 파트너 이름 (`criteo`)

- **networkId\*** (integer)

  - _(필수)_ Criteo Network ID

- **zoneId\*** (integer)

  - _(필수)_ Criteo Zone ID

</details>

<br>

<details>
<summary><strong>Emodo</strong></summary>

```javascript
jwplayer("myElement").setup({
  "playlist": "https://cdn.jwplayer.com/v2/playlists/a12bc3D4",
  "advertising": {
    "bids": {
      ...
      "bidders": [{
        "name": "Axonix",
        "supplyId": "1234567"
      }]
    },
    ...
  }
});
```

- **name\*** (string)

  - _(필수)_ 입찰이 수신되는 광고 파트너 이름 (`Amonix`)

- **supplyId\*** (string)

  - _(필수)_ Emodo에서 제공하는 **공급 UUID(Supply UUID)**

</details>

<br>

<details>
<summary><strong>EMX</strong></summary>

```javascript
jwplayer("myElement").setup({
  "playlist": "https://cdn.jwplayer.com/v2/playlists/a12bc3D4",
  "advertising": {
    "bids": {
        ...
      "bidders": [{
        "id": "{section_id}",
        "name": "EMX",
        "pubid": "{publisher_id}"
      }]
    }
    ...
  }
});
```

- **id\*** (string)

  - _(필수)_ EMX Digital Tag ID

- **name\*** (string)

  - _(필수)_ 입찰이 수신되는 광고 파트너 이름 (`EMX`)

</details>

<br>

<details>
<summary><strong>Equativ</strong></summary>

```javascript
jwplayer("myElement").setup({
  "playlist": "https://cdn.jwplayer.com/v2/playlists/a12bc3D4",
  "advertising": {
    "bids": {
      ...
      "bidders": [{
        "formatId": 1234,
        "name": "SmartAdServer",
        "networkId": 12345,
        "pageId": 1234,
        "siteId": "temp1234"
      }]
    },
    ...
  }
});
```

- **formatId\*** (integer)

  - _(필수)_ Equativ Placement Format ID<br>Equativ에서 제공되지 않은 경우 임의의 숫자를 사용 가능합니다.

- **name\*** (string)

  - _(필수)_ 입찰이 수신되는 광고 파트너 이름 (`SmartAdServer`)

- **networkId\*** (integer)

  - _(필수)_ Equativ 네트워크 식별자

- **pageId\*** (integer)

  - _(필수)_ Equativ Placement Page ID<br>Equativ에서 제공되지 않은 경우 임의의 숫자 사용 가능

- **siteId\*** (string)

  - _(필수)_ Equativ Placement Site ID<br>Equativ에서 제공되지 않은 경우 임의의 문자열 사용 가능

</details>

<br>

<details>
<summary><strong>iMDS</strong></summary>

```javascript
jwplayer("myElement").setup({
  "playlist": "https://cdn.jwplayer.com/v2/playlists/a12bc3D4",
  "advertising": {
    "bids": {
      ...
      "bidders": [{
        "id": "{placement_id}",
        "name": "SynacorMedia",
        "pubid": "{seat_id}"
      }]
    },
    ...
  }
});
```

- **id\*** (string)

  - _(필수)_ iMDS Placement ID 또는 Tag ID

- **name\*** (string)

  - _(필수)_ 입찰이 수신되는 광고 파트너 이름 (`SynacorMedia`)

- **pubid\*** (string)

  - _(필수)_ iMDS Seat ID<br>모든 광고 유닛에 동일하게 적용됩니다.

</details>

<br>

<details>
<summary><strong>Index Exchange</strong></summary>

```javascript
jwplayer("myElement").setup({
  "playlist": "https://cdn.jwplayer.com/v2/playlists/a12bc3D4",
  "advertising": {
    "bids": {
      ...
      "bidders": [{
        "id": "{section_id}",
        "name": "IndexExchange"
      }]
    },
    ...
  }
});
```

- **id\*** (string)

  - _(필수)_ Index Exchange Site ID<br>이 광고 유닛에 연결된 IX 고유 식별자

- **name\*** (string)

  - _(필수)_ 입찰이 수신되는 광고 파트너 이름 (`IndexExchange`)

</details>

<br>

<details>
<summary><strong>Kargo</strong></summary>

```javascript
jwplayer("myElement").setup({
  "playlist": "https://cdn.jwplayer.com/v2/playlists/a12bc3D4",
  "advertising": {
    "bids": {
      ...
      "bidders": [{
        "name": "kargo",
        "placementId": "a12345"
      }]
    },
    ...
  }
});
```

- **name\*** (string)

  - _(필수)_ 입찰이 수신되는 광고 파트너 이름 (`kargo`)

- **placementId\*** (string)

  - _(필수)_ Kargo Placement ID

</details>

<br>

<details>
<summary><strong>Magnite</strong></summary>

```javascript
jwplayer("myElement").setup({
  "playlist": "https://cdn.jwplayer.com/v2/playlists/a12bc3D4",
  "advertising": {
    "bids": {
      ...
      "bidders": [{
        "name": "Rubicon",
        "pubid": "{account_id}",
        "siteId": "{site_id}",
        "zoneId": "{zone_id}"
      }]
    },
    ...
  }
});
```

- **name\*** (string)

  - _(필수)_ 입찰이 수신되는 광고 파트너 이름 (`Rubicon`)

- **pubid\*** (string)

  - _(필수)_ Magnite 퍼블리셔 계정 ID

- **siteId\*** (string)

  - _(필수)_ Magnite 사이트 ID

- **zoneId\*** (string)

  - _(필수)_ Magnite 존(zone) ID

</details>

<br>

<details>
<summary><strong>Media.net</strong></summary>

```javascript
jwplayer("myElement").setup({
  "playlist": "https://cdn.jwplayer.com/v2/playlists/a12bc3D4",
  "advertising": {
    "bids": {
      ...
      "bidders": [{
        "id": "{crid}",
        "name": "MediaNet",
        "pubid": "{cid}"
      }]
    },
    ...
  }
});
```

- **id\*** (string)

  - _(필수)_ Media.net `crid` Placement ID

- **name\*** (string)

  - _(필수)_ 입찰이 수신되는 광고 파트너 이름 (`MediaNet`)

- **pubid\*** (string)

  - _(필수)_ Media.net `cid` Customer ID

</details>

<br>

<details>
<summary><strong>OpenX</strong></summary>

```javascript
jwplayer("myElement").setup({
  "playlist": "https://cdn.jwplayer.com/v2/playlists/a12bc3D4",
  "advertising": {
    "bids": {
      ...
      "bidders": [{
        "id": "{unit}",
        "name": "OpenX",
        "delDomain": "{del_domain}"
      }]
    },
    ...
  }
});
```

- **delDomain\*** (string)

  - _(필수)_ OpenX 전달 도메인(Delivery Domain)

- **id\*** (string)

  - _(필수)_ OpenX 광고 유닛 ID

- **name\*** (string)

  - _(필수)_ 입찰이 수신되는 광고 파트너 이름 (`OpenX`)

</details>

<br>

<details>
<summary><strong>PubMatic</strong></summary>

```javascript
jwplayer("myElement").setup({
  "playlist": "https://cdn.jwplayer.com/v2/playlists/a12bc3D4",
  "advertising": {
    "bids": {
      ...
      "bidders": [{
        "id": "{section_id}",
        "name": "PubMatic",
        "pubid": "{publisher_id}"
      }]
    },
    ...
  }
});
```

- **name\*** (string)

  - _(필수)_ 입찰이 수신되는 광고 파트너 이름 (`PubMatic`)

- **pubid\*** (string)

  - _(필수)_ PubMatic 퍼블리셔 ID

- **id** (string)

  - PubMatic 광고 슬롯 ID

</details>

<br>

<details>
<summary><strong>Sonobi</strong></summary>

```javascript
jwplayer("myElement").setup({
  "playlist": "https://cdn.jwplayer.com/v2/playlists/a12bc3D4",
  "advertising": {
    "bids": {
      ...
      "bidders": [{
        "id": "{placement_id}",
        "name": "Sonobi"
      }]
    }
    ...
  },
});
```

- **id\*** (string)

  - _(필수)_ Sonobi Placement ID

- **name\*** (string)

  - _(필수)_ 입찰이 수신되는 광고 파트너 이름 (`Sonobi`)

</details>

<br>

<details>
<summary><strong>Sovrn</strong></summary>

```javascript
jwplayer("myElement").setup({
  "playlist": "https://cdn.jwplayer.com/v2/playlists/a12bc3D4",
  "advertising": {
    "bids": {
      ...
      "bidders": [{
        "name": "Sovrn",
        "tagid": "1234567"
      }]
    },
    ...
  }
});
```

- **name\*** (string)

  - _(필수)_ 입찰이 수신되는 광고 파트너 이름 (`Sovrn`)

- **tagid\*** (string)

  - _(필수)_ Sovrn이 제공하는 광고 태그 ID

</details>

<br>

<details>
<summary><strong>SpotX</strong></summary>

```javascript
jwplayer("myElement").setup({
  "playlist": "https://cdn.jwplayer.com/v2/playlists/a12bc3D4",
  "advertising": {
    "bids": {
      ...
      "bidders": [{
        "id": "{channel_id}",
        "name": "SpotX"
      }]
    },
    ...
  }
});
```

- **id\*** (string)

  - _(필수)_ SpotX 채널 ID<br>SpotX 퍼블리셔 플랫폼에서 채널 생성 시 생성되는 고유한 5자리 ID

- **name\*** (string)

  - _(필수)_ 입찰이 수신되는 광고 파트너 이름 (`SpotX`)

</details>

<br>

<details>
<summary><strong>The MediaGrid</strong></summary>

```javascript
jwplayer("myElement").setup({
  "playlist": "https://cdn.jwplayer.com/v2/playlists/a12bc3D4",
  "advertising": {
    "bids": {
      ...
      "bidders": [{
        "id": "{section_id}",
        "name": "MediaGrid",
        "pubid": "{publisher_id}"
      }]
    },
    ...
  }
});
```

- **id\*** (string)

  - _(필수)_ MediaGrid 광고 슬롯 ID (`uid`), 사이트 페이지의 `<div>` ID와 연결됨

- **name\*** (string)

  - _(필수)_ 입찰이 수신되는 광고 파트너 이름 (`MediaGrid`)

- **pubid\*** (string)

  - 광고 파트너가 발급한 퍼블리셔 식별자

</details>

<br>

<details>
<summary><strong>The Trade Desk</strong></summary>

```javascript
jwplayer("myElement").setup({
  "playlist": "https://cdn.jwplayer.com/v2/playlists/a12bc3D4",
  "advertising": {
    "bids": {
      ...
      "bidders": [{
        "name": "TheTradeDesk",
        "supplySourceId": "{supplySourceId}",
        "publisherId": "{publisherId}",
        "placementId": "{placementId}"
      }]
    }
    ...
  }
});
```

- **name\*** (string)

  - _(필수)_ 입찰이 수신되는 광고 파트너 이름 (`TheTradeDesk`)

- **publisherId\*** (string)

  - _(필수)_ 퍼블리셔 ID<br>`sellers.json`이 있는 경우, 해당 사이트의 `seller_id`와 동일해야 합니다. 없을 경우 `1`로 설정합니다.

- **supplySourceId\*** (string)

  - _(필수)_ TTD에서 제공한 공급 소스 이름

- **placementId** (string)

  - 광고 유닛 이름

</details>

<br>

<details>
<summary><strong>Unruly</strong></summary>

```javascript
jwplayer("myElement").setup({
  "playlist": "https://cdn.jwplayer.com/v2/playlists/a12bc3D4",
  "advertising": {
    "bids": {
      ...
      "bidders": [{
        "name": "Unruly",
        "pubid": "{siteId}"
      }]
    },
    ...
  }
});
```

- **name\*** (string)

  - _(필수)_ 입찰이 수신되는 광고 파트너 이름 (`Unruly`)

- **pubid\*** (string)

  - _(필수)_ Unruly 사이트 ID

</details>

<br>

<details>
<summary><strong>VideoByte</strong></summary>

```javascript
jwplayer("myElement").setup({
  "playlist": "https://cdn.jwplayer.com/v2/playlists/a12bc3D4",
  "advertising": {
    "bids": {
      ...
      "bidders": [{
          "name": "VideoByte",
          "nid": "temp_222",
          "placementId": "temp_222",
          "pubId": "temp_222"
      }]
    },
    ...
  }
});
```

- **name\*** (string)

  - _(필수)_ 입찰이 수신되는 광고 파트너 이름 (`VideoByte`)

- **pubId\*** (string)

  - _(필수)_ VideoByte 퍼블리셔 ID

- **nid** (string)

  - VideoByte 네트워크 ID

- **placementId** (string)

  - VideoByte Placement ID

</details>

<br>

<details>
<summary><strong>Xandr</strong></summary>

```javascript
jwplayer("myElement").setup({
  "playlist": "https://cdn.jwplayer.com/v2/playlists/a12bc3D4",
  "advertising": {
    "bids": {
      ...
    "bidders": [{
        "id": "{placementId}",
        "name": "AppNexus",
        "member": "{member}",
        "invCode": "{invCode}",
        "publisherId": "{publisherId}"
      }]
    },
    ...
  }
});
```

- **id\*** (string)

  - _(필수)_ Xandr Placement ID<br>`invCode`와 `member` 조합으로 대체 식별 가능

- **name\*** (string)

  - _(필수)_ 입찰이 수신되는 광고 파트너 이름 (`AppNexus`)

- **invCode** (string)

  - Xandr 인벤토리 코드 (`member`와 함께 사용해야 함)

- **member** (string)

  - Xandr 멤버 ID (`invCode`와 함께 사용해야 함)

- **publisherId** (string)

  - Xandr 퍼블리셔 ID<br>Placement ID가 없거나 `invCode` 오류 시 퍼블리셔 식별용으로 사용됩니다.

</details>

<br>

<details>
<summary><strong>YahooSSP</strong></summary>

```javascript
jwplayer("myElement").setup({
  "playlist": "https://cdn.jwplayer.com/v2/playlists/a12bc3D4",
  "advertising": {
    "bids": {
      ...
      "bidders": [{
        "name": "YahooSSP",
        "pubid": "{pub_id}",
        "siteId": "{site_id}",
        "id": "{placement_id}"
      }]
    },
    ...
  }
});
```

- **name\*** (string)

  - _(필수)_ 입찰이 수신되는 광고 파트너 이름 (`YahooSSP`)

- **pubid\*** (string)

  - _(필수)_ YahooSSP 퍼블리셔 외부 ID

- **id** (string)

  - YahooSSP Placement ID

- **siteId** (string)

  - YahooSSP 사이트 ID

</details>

<br><br>

---

## advertising.bids.bidders[].optionalParams

> 이 객체는 **IndexExchange, PubMatic, SpotX**, 또는 **Xandr (AppNexus)** 가 광고 파트너일 때만 사용해야 합니다.

General

```javascript
jwplayer("myElement").setup({
  "playlist": "https://cdn.jwplayer.com/v2/playlists/a12bc3D4",
  "advertising": {
    "bids": {
      ...
      "bidders": [{
        "name": "SpotX",
        "id": "85395",
        "optionalParams": {
          "price_floor": 5.30,
          "custom": {
            "name": "{custom_param_name_goes_here}",
            "value": "{custom_param_value_goes_here}"
          }
        }
      }]
    },
    ...
  }
});
```

Xandr with Keywords

```javascript
jwplayer("myElement").setup({
  "playlist": "https://cdn.jwplayer.com/v2/playlists/a12bc3D4",
  "advertising": {
    "bids": {
      ...
      "bidders": [{
        "id": "12345",
        "name": "AppNexus",
        "optionalParams": {
          "keywords": {
            "segment1": "12346",
            "segment2": "12347"
          }
        }
      }]
    },
    ...
  }
});
```

<br>

- ⚠️ **content** (object)

  - _(더 이상 사용되지 않음 — Deprecated)_ OpenRTB 콘텐츠 객체.
  - 참고: [OpenRTB 2.5 명세서]의 “3.2.16 Object: Content” 항목.

- **custom** (object)

  - 퍼블리셔가 정의한 **커스텀 키-값 쌍**으로, 입찰 요청에 포함됩니다.

- **keywords** (object)

  - _(Xandr 전용)_ 페이지의 모든 광고 슬롯에 적용되는 **키-값 쌍 집합**입니다.

- **no_vpaid_ads** (boolean)

  - 사용 가능한 광고 미디어 유형에서 **VPAID 크리에이티브를 제거**합니다.

- **passFloorPrice** (boolean)

  - 설정된 `floorPriceCents` 값을 **입찰 요청에 전달**합니다.

- ⚠️ **token** (object) <sup>< 8.13.0</sup>

  - _(더 이상 사용되지 않음 — Deprecated)_ 퍼블리셔가 정의한 **커스텀 패스스루 매크로(pass-through macros)**

<br><br>

---

## advertising.bids.ortbParams

```javascript
jwplayer("myElement").setup({
  "playlist": "https://cdn.jwplayer.com/v2/playlists/a12bc3D4",
  "advertising": {
    ...
    "bids": {
      ...
      "ortbParams": {
        "plcmt": 1,
        "pos": 1,
        "gpid": "my-pub-defined-gpid"
      }
    },
    ...
  }
});
```

<br>

- **gpid** (string)

  - 퍼블리셔가 정의한 문자열로, **플랫폼 및 SSP 간 광고 게재 위치를 고유하게 식별**합니다.
  - 이 값은 `imp.ext.gpid` 속성을 통해 전송됩니다.
  - 자세한 내용은 **[3.2.4 Object: Imp]** 항목을 참조하십시오.

- **plcmt** (number)

  - **IAB 디지털 비디오 가이드라인**에 따라 정의된 비디오의 게재 유형입니다.
  - 가능한 값:
    • `1`: 인스트림(Instream)
    • `2`: 동반 콘텐츠(Accompanying Content)
    • `3`: 인터스티셜(Interstitial)
    • `4`: 독립형 / 비콘텐츠(No Content / Standalone)
  - 이 값은 `Object: Video` 내의 `plcmt` 속성을 통해 전송됩니다.
  - 자세한 내용은 **List: Plcmt Subtypes - Video** 를 참조하십시오.

- **pos** (number)

  - IAB에서 정의한 광고 위치 유형으로, **콘텐츠 내에서 광고가 표시되는 위치**를 나타냅니다.
  - 가능한 값:
    • `0`: 알 수 없음(Unknown)
    • `1`: 화면 상단(Above the Fold)
    • `2`: (사용 중단됨 — DEPRECATED)
    • `3`: 화면 하단(Below the Fold)
    • `4`: 헤더(Header)
    • `5`: 푸터(Footer)
    • `6`: 사이드바(Sidebar)
    • `7`: 전체화면(Full Screen)
  - 이 값은 `pos` 속성을 통해 전송됩니다.
  - 자세한 내용은 **[5.4 Ad Position]** 항목을 참조하십시오.

<br><br>

---

## advertising.bids.settings

```javascript
jwplayer("myElement").setup({
  "playlist": "https://cdn.jwplayer.com/v2/playlists/a12bc3D4",
  "advertising": {
    ...
    "bids": {
      ...
      "settings": {
        "mediationLayerAdServer": "jwp",
        "floorPriceCents": 10,
        "floorPriceCurrency": "usd",
        "bidTimeout": 4000,
        "sendAllBids": true
      }
    }
    ...
  }
});
```

<br>

- **mediationLayerAdServer\*** (string)

  - (필수) 어떤 광고가 재생될지를 결정하는 **중재 계층(Mediation Layer)** 을 지정합니다.
  - 가능한 값:
    - `jwp` (VAST / IMA): 플레이어가 자체 경매를 수행합니다. 낙찰자가 선택되면 해당 광고를 호출하고, 낙찰자가 없으면 **대체 태그(fallback tag)** 가 호출됩니다. `floorPrice`를 반드시 지정해야 합니다.
    - `jwpspotx` (VAST): 플레이어가 경매를 수행하지 않습니다. SpotX에 입찰 요청을 보내고, 가격에 상관없이 반환된 광고 응답을 재생합니다. 이는 JW Player 중재에서 **$0.01 floor price** 를 사용하는 것과 동일합니다. SpotX 라인 아이템을 구성해야 합니다.
    - `dfp` (IMA): 플레이어가 경매를 수행하지 않습니다. 모든 입찰이 **Google Ad Manager(GAM)** 으로 전송되며, 라인 아이템으로 렌더링되어 다른 라인 아이템과 경쟁합니다. GAM이 낙찰된 라인 아이템을 제공합니다. GAM에서 생성해야 할 라인 아이템 수를 줄이기 위해 **가격 버킷(buckets)** 설정을 권장합니다.
    - `jwpdfp` (IMA): 플레이어가 1차 경매를 수행합니다. 낙찰자가 선택되면 해당 광고가 재생되고, 낙찰자가 없으면 fallback 태그가 호출됩니다. 낙찰자가 없을 경우, 모든 유효한 입찰이 **Google Ad Manager(GAM)** 으로 전송되어 입찰가가 라인 아이템으로 렌더링되고, GAM이 최종 낙찰 아이템을 제공합니다. `floorPriceCents`를 반드시 설정해야 하며, GAM의 라인 아이템 수를 줄이기 위해 **버킷 설정(buckets)** 을 권장합니다.
  - 기본값: `jwp`

- **bidTimeout** (number)

  - 입찰 요청 발생 시점부터 입찰 응답을 기다리는 **최대 시간(밀리초)** 입니다.
  - 버전별 기본값:
    • **8.17.0+** → 기본값: `3000` / 최소값: `2000`
    • **8.13.0+** → 기본값: `2000`
    • **8.13.0 미만** → 사용자가 재생 버튼을 클릭한 후 입찰 응답을 기다리는 시간입니다. 기본값: `2000`
  - SpotX를 광고 파트너로 사용하는 경우, **SpotX SDK 로딩 시간**을 고려해야 합니다.

- **buckets** (array)

  - _(권장)_ **입찰가 범위(Bid Price Range)** 를 정의합니다.
  - 버킷을 사용할 경우, GAM에 전송되는 입찰가는 지정된 증분 단위로 **내림(round down)** 처리됩니다. 이 방법은 GAM의 라인 아이템 수를 줄이는 데 유용합니다. 버킷을 사용하지 않으면 **1센트 단위마다 별도의 라인 아이템**이 필요합니다.
  - 이 속성은 `mediationLayerAdServer` 값이 `dfp` 또는 `jwpdfp`일 때(즉, GAM 중재 사용 시)만 적용됩니다.
  - 참고: [advertising.bids.settings.buckets object]

- **consentManagement** (object)

  - **GDPR(유럽 일반 데이터 보호 규정)** 및 **CCPA(캘리포니아 소비자 프라이버시법)** 관리를 위한 리소스.
  - 참고: [advertising.bids.settings.consentManagement object]

- **disableConsentManagementOnNoCmp** (boolean)

  - **TCF API 함수** 존재 여부에 따라 Prebid GDPR 설정을 조정합니다.
  - `tcfapi()` 함수는 CMP(동의 관리 플랫폼)가 사용자의 동의 상태를 알리기 위해 사용하는 **표준화된 인터페이스**입니다. 이 함수는 사용자가 데이터 처리 및 광고 목적에 동의했는지를 확인하여 **GDPR 준**수를 보장합니다.
  - 가능한 값:
    • `false` (기본값): JWP가 퍼블리셔의 동의 관리 설정을 전달함
    • `true`:
    • `tcfapi()` 함수가 존재하면, `consentManagement` 설정은 퍼블리셔 정의대로 유지됨
    • `tcfapi()` 함수가 존재하지 않으면, `consentManagement가` 제거됨
  - 퍼블리셔는 **JWP 웹 플레이어가 초기화될 때 CMP의 `tcfapi()` 함수가 로드되어 있어야 함**을 반드시 보장해야 합니다.

- **floorPriceCents** (number)

  - 입찰이 이겨야 하는 **최소 CPM 단위 가격(센트 단위)** 을 지정합니다.
  - 이 속성은 `mediationLayerAdServer가` `jwp` 또는 `jwpdfp일` 때 반드시 설정해야 합니다.
  - 적절한 floor price는 여러 요인에 따라 달라집니다. 최적의 값을 결정하려면 JW Player 담당자 또는 SSP 파트너에게 문의하십시오.

- **floorPriceCurrency** (string)

  - `floorPriceCents의` 통화 단위 입니다.
  - `mediationLayerAdServer`가 `jwp`로 설정된 경우 반드시 `"usd"`로 지정해야 합니다.

- **sendAllBids** (boolean)

  - 모든 입찰 데이터를 전송할지, 아니면 **최상위 입찰만 전송할지**를 결정합니다.
  - 가능한 값:
    • `true` (기본값): 모든 입찰자에 대한 키-값 쌍(KVP)을 전송하여 광고 서버가 낙찰자를 선택하고 과거 입찰가를 분석할 수 있도록 함.
    • `false`: 상위 입찰자 한 명의 KVP만 전송하여 광고 서버로의 데이터 전송량을 줄임.

<br><br>

---

## advertising.bids.settings.buckets[]

- **increment** (number)

  - 입찰 통화 단위에서 **입찰가가 내림(round down)** 처리되는 **가장 가까운 증분 단위**입니다.
  - 기본값: `0.01`

- **max** (number)

  - **입찰가 버킷의 최대값** (입찰 통화 단위 기준)

- **min** (number)

  - **입찰가 버킷의 최소값** (입찰 통화 단위 기준)
  - 기본값: `0`

<br><br>

---

## advertising.bids.settings.consentManagement

- **gdpr** (object)

  - **GDPR(일반 데이터 보호 규정)** 지원을 위한 리소스
  - 참고: [advertising.bids.settings.consentManagement.gdpr]

- **usp** (object)

  - **CCPA(캘리포니아 소비자 프라이버시법)** 지원을 위한 리소스
  - 참고: [advertising.bids.settings.consentManagement.usp]

<br><br>

---

## advertising.bids.settings.consentManagement.gdpr

GDPR 동의 관리 모듈(GDPR Consent Management Module)의 다음 매개변수에 대한 자세한 내용은 해당 문서를 참고하세요.

<br>

- **allowAuctionWithoutConsent** (boolean)

  - _(TCF v1.1 전용)_ CMP(Consent Management Platform)에서 동의 정보를 가져오지 못했을 때 어떻게 처리할지를 결정합니다.

- **cmpApi** (string)

  - 사용 중인 **CMP 인터페이스** 유형을 지정합니다.

- **defaultGdprScope** (boolean)

  - CMP가 제시간에 응답하지 않거나 정적 데이터에 `gdprApplies` 플래그가 포함되지 않은 경우, 해당 플래그의 기본값을 정의합니다.

- **rules** (array of objects)

  - 퍼블리셔가 기본 동작을 **재정의(override)** 할 수 있도록 허용합니다.
  - 참고: [advertising.bids.settings.consentManagement.gdpr.rules]

- **timeout** (integer)

  - CMP가 **GDPR 동의 문자열(consent string)** 을 가져올 수 있도록 허용하는 **최대 시간(밀리초 단위)** 을 지정합니다.

<br><br>

---

## advertising.bids.settings.consentManagement.gdpr.rules

- **enforcePurpose** (boolean)

  - **목적 동의(Purpose Consent)** 를 강제할지 여부를 결정합니다.

- **enforceVendor** (boolean)

  - 이 목적(Purpose)에 대해 **벤더 신호(Vendor Signals)** 를 강제할지 여부를 결정합니다.

- **purpose** (string)

  - 퍼블리셔가 재정의(override)할 수 있는 **특정 기본 동작**을 정의합니다.
  - 가능한 값:
    • `storage` (Purpose 1)
    • `basicAds` (Purpose 2)
    • `measurement` (Purpose 7)

- **vendorExceptions** (array)

  - 해당 목적(Purpose)의 강제 적용에서 **제외될 입찰자 코드(bidder codes)** 또는 **모듈 이름(module names)** 의 목록을 정의합니다.

<br><br>

---

## advertising.bids.settings.consentManagement.usp

다음 매개변수에 대한 자세한 내용은 **US Privacy Consent Management Module** 문서를 참고하세요.

<br>

- **cmpApi** (string)

  - 사용 중인 **USP-API 인터페이스**를 지정합니다.

- **timeout** (integer)

  - **USP-API**가 **CCPA 문자열(CCPA string)** 을 가져올 수 있도록 허용하는 **최대 시간(밀리초 단위)** 을 지정합니다.

<br><br>

---

## advertising.companiondiv

이 객체는 **id, width, height**의 세 가지 속성을 포함합니다.  
이 속성들을 설정하면, JW Player가 **VAST/IMA 태그**로부터 **companion 광고**를 불러와 페이지 내의 특정 `<div>` 요소에 로드합니다.  
자세한 내용은 _Additional Options_ 표의 **Companion Ads** 항목을 참고하세요.

<br>

- **height** (number)

  - VAST 광고에 포함된 **companion 광고의 목표 높이**를 지정합니다.

- **id** (string)

  - **companion 광고로 대체할 `<div>` 요소의 ID**를 지정합니다.

- **width** (number)

  - VAST 광고에 포함된 **companion 광고의 목표 너비**를 지정합니다.

<br>

> JW Player의 전체 광고 기능 개요는 **[Video Ads 전용 섹션]**을 참고하세요.

<br><br>

---

## advertising.enablePPS

이 속성은 **IAB 컨텍스트 세그먼트(IAB contextual segments)** 를 광고 태그(ad tag)를 통해 자동으로 전달하여 **프로그래매틱 수익화(programmatic monetization)** 를 개선하는 데 사용됩니다.

> 이 기능을 사용할 때는 다음 사항에 유의하세요:
>
> - **Google Publisher Provided Signals (PPS)** 는 **JWP 측에서 활성화해야** 합니다. 참여를 원할 경우 JWP 담당자에게 문의하세요.
> - JWP는 영상을 자동으로 컨텍스추얼화(contextualize)하려 시도하지만, **모든 영상에 IAB 컨텍스트 세그먼트가 PPS를 통해 전달되지는 않습니다.**
> - 광고 태그(ad tag)에 `&ppsj` 쿼리 매개변수가 정의되어 있는 경우, **JWP는 해당 값을 덮어쓰지 않습니다.**
> - `"enablePPS": true` 로 설정하면, **객체의 모든 기본값(default values)** 이 자동으로 적용됩니다.

<br>

object

```javascript
jwplayer("myElement").setup({
  "playlist": "https://cdn.jwplayer.com/v2/playlists/a12bc3D4",
  "advertising": {
    "client": "googima",
    "enablePPS": {
      "encodePPS": false,
      "includeIABTaxonomyVersion": true
    },
  ...
  }
});
```

object

```javascript
jwplayer("myElement").setup({
  "playlist": "https://cdn.jwplayer.com/v2/playlists/a12bc3D4",
  "advertising": {
    "client": "googima",
    "enablePPS": true,
  ...
  }
});
```

<br>

- **encodePPS** (boolean) <sup>8.38.4+</sup>

  - **Publisher Provided Signal (PPS)** 값을 광고 요청에 추가하기 전에 **인코딩할지 여부**를 결정합니다.
  - 기본값: `true`

<br>

- **includeIABTaxonomyVersion** (boolean) <sup>8.38.4+</sup>

  - Google Ad Manager의 **IAB Taxonomy Version** 을 광고 요청에 `iabt_ver` 매개변수로 **추가할지 여부**를 결정합니다.
  - 기본값: `false`

<br><br>

---

## advertising.freewheel

```javascript
jwplayer("myElement").setup({
  "playlist": "https://cdn.jwplayer.com/v2/playlists/a12bc3D4",
  "fwassetid": "test_asset",
  "duration": 600,
  "advertising": {
    ...
    "freewheel": {
      "networkid": 12345,
      "adManagerURL": "https://mssl.fwmrm.net/libs/adm/6.24.0/AdManager.js",
      "serverid": "http://demo.v.fwmrm.net/ad/g/1",
      "profileid": "12345:html5_test",
      "sectionid": "test_site_section",
      "streamtype": "live"
    }
  }
});
```

<br>

- **fwassetid\*** (string)

  - **(필수)** 특정 미디어 항목의 **FreeWheel 식별자**입니다.
  - 예시: `DemoVideoGroup.01`
  - 중요: 특정 플레이리스트 항목에 광고 콘텐츠를 예약할 때는, **각 플레이리스트 항목마다 고유한** `fwassetid` 를 가져야 합니다.
  - 예시 구성은 _Recipe_ 문서를 참고하세요.

- **adManagerURL** (string)

  - **FreeWheel Ad Manager의 URL** 입니다.
  - 예시: `https://mssl.fwmrm.net/libs/adm/6.24.0/AdManager.js`
  - 참고: FreeWheel 솔루션 엔지니어 또는 계정 담당자가 **버전이 지정된 AdManager URL** 을 제공할 수 있습니다.

- **custom** (object)

  - 사용자 정의 **key-value 쌍**을 FreeWheel SDK에 전달합니다.

- **networkid** (number)

  - FreeWheel 네트워크의 **식별자**입니다.
  - 예시: `96749`

- **profileid** (string)

  - 특정 애플리케이션 환경의 **FreeWheel 식별자**입니다.
  - 예시: `global-js`

- **sectionid** (string)

  - 동영상 콘텐츠가 재생되는 **섹션의 FreeWheel 식별자**입니다.
  - 예시: `DemoSiteGroup.01`

- **serverid** (string)

  - **FreeWheel 광고 서버의 URL** 입니다.
  - 예시: `http://demo.v.fwmrm.net/ad/g/1`

- **streamtype** (string)

  - 미디어 항목이 **라이브 스트리밍 자산(live streaming asset)** 인지를 나타냅니다.
  - 이 속성을 사용할 때는 값을 `"live"` 로 설정하세요.

<br>

> 위 표의 **FreeWheel 계정 값(account values)** 을 어디서 확인해야 할지 모를 경우, **FreeWheel 담당자에게 문의하세요.**

<br><br>

---

## advertising.rules

이 속성은 **광고 재생 빈도(ad playback frequency)** 를 제어하는 데 사용됩니다.  
자세한 내용은 Ad Rules Reference 지원 문서를 참고하세요.

<br>

```javascript
jwplayer("myElement").setup({
  "playlist": [...],
  "advertising": {
    ...
    "rules": {
      "startOn": 2,
      "frequency": 1,
      "timeBetweenAds": 300,
      "startOnSeek": "pre",
      "deferAds": {}
    }
  }
});
```

<br>

- **deferAds** (object) <sup>8.12.0+</sup>

  - **플레이어가 비활성 탭(inactive tab)** 에 있을 때 **광고 재생을 지연(defer)** 시키는 빈 객체 설정입니다.
  - 플레이어의 탭이 다시 활성화되면, **마지막으로 지연된 광고 구간(ad break)** 이 재생됩니다.
  - 광고가 시청 불가능한 상태에서 재생되는 것을 중단하려면, `autoPause` 속성을 함께 구성하세요.
  - 이 기능은 JW Player **대시보드에서 활성화할 수 없습니다.**
    - Ad client: VAST
    - Default: `-`

- **frequency** (number)

  - **플레이리스트 내 광고 재생 주기**를 설정합니다.
  - 예를 들어 `frequency: 3` 으로 설정하면, **3번째마다 광고가 재생**됩니다.  
    `0`으로 설정하면 첫 번째 항목에서만 광고가 재생됩니다.
  - 이 설정은 대시보드의 **Ad Frequency Rules** 중 하나입니다.
    - Ad client: IMA, VAST
    - Default: `1`

- **startOn** (number)

  - 광고 재생이 **허용되는 첫 번째 플레이리스트 항목 번호**를 지정합니다.
  - 이 설정은 대시보드의 **Ad Frequency Rules** 중 하나입니다.
    - Ad client: IMA, VAST
    - Default: `1`

- **startOnSeek** (string) <sup>8.5.0+</sup>

  - **이전에 시청하던 콘텐츠를 다시 재생할 때(pre-roll) 광고를 보여줄지 여부**를 정의합니다.
  - 가능한 값:
    • `pre`: 이전에 시청하던 시점으로 돌아가기 전에 프리롤 광고를 재생합니다.
    • `none`: 광고 없이 바로 콘텐츠 재생을 재개합니다.
  - 이 설정은 대시보드의 **Long-form Engagement Rules** 중 하나입니다.
  - 참고: 아래 정보를 추적해야 합니다.
    – 고유 시청자 ID
    – 시청 중이던 고유 콘텐츠 ID
    – 시청자가 콘텐츠를 떠난 시점
  - 플레이어 설정 시 이 정보를 전달해야 하며, `starttime` 속성을 사용해 재생 재개 시점을 지정합니다.
    - Ad client: VAST
    - Default: `-`

- **timeBetweenAds** (number)

  - 광고가 재생된 후 **다음 광고를 재생하기 전까지 필요한 최소 시간(초)** 을 설정합니다.
  - 이 설정은 대시보드의 **Long-form Engagement Rules** 중 하나입니다.
    - Ad client: VAST
    - Default: `0`

<br><br>

---

## advertising.schedule

이 속성은 **여러 광고 구간(ad break)** 을 포함하는 전체 **광고 스케줄(ad schedule)** 을 JW Player에 로드하는 데 사용됩니다.  
속성 값은 **VMAP 스케줄(.xml)** 의 URL이거나, **광고 정보를 포함한 인라인 JSON 블록**일 수 있습니다.  
이 스케줄은 모든 플레이리스트 항목에 적용됩니다.  
개별 플레이리스트 항목별 광고 스케줄링 방법은 playlist 항목별 광고 스케줄링을 참고하세요.

<br>

### Ad Schedules with VMAP Files

VMAP 파일을 사용하는 경우, `schedule` 속성에 **VMAP `.xml` 파일의 링크**를 추가합니다.

이 VMAP 스케줄은 각 플레이리스트 항목에 자동으로 적용됩니다.  
자세한 내용은 VMAP 스케줄 관련 문서를 참고하세요.

```javascript
jwplayer('myElement').setup({
  playlist: 'https://cdn.jwplayer.com/v2/playlists/a12bc3D4',
  advertising: {
    client: 'vast',
    adscheduleid: 't4Xk5tsF',
    schedule: 'myvmap.xml',
  },
});
```

### Embedded Ad Schedules with JSON

JSON 형식의 스케줄을 사용하려면, `schedule` 속성 내부에 **최소 한 개 이상의 광고 구간(ad break)** 을 정의해야 합니다.  
각 광고 구간에는 반드시 `offset` 과 `tag` 또는 `vastxml` 속성이 포함되어야 합니다.

```javascript
jwplayer('myElement').setup({
  playlist: 'https://cdn.jwplayer.com/v2/playlists/a12bc3D4',
  advertising: {
    client: 'vast',
    adscheduleid: 'p4Xk5lsZ',
    schedule: [
      {
        tag: 'myPreroll.xml',
        offset: 'pre',
        custParams: {
          testkey1: 'testval1',
          testkey2: 'testval2',
        },
      },
      {
        vastxml: "<VAST version='2.0'> ... </VAST>",
        offset: '50%',
      },
    ],
  },
});
```

<br>

- **custParams** (object)

  - 광고 서버에 요청되는 URL에 전달될 **사용자 정의 매개변수(custom parameters)** 를 지정합니다.

- **offset** (string \| number)

  - 광고 태그가 재생될 **시점**을 정의합니다.
  - 가능한 값:
    • `pre`: (기본값, string) 프리롤 광고로 재생
    • `post`: 포스트롤 광고로 재생
    • `xx%`: (string - VAST 전용) 콘텐츠의 xx% 진행 시점에 광고 재생
    • _(number)_: 지정된 초(Seconds) 후 광고 재생
    • _(timecode)_: `HH:MM:SS:sss` 형식으로 특정 시점에 광고 재생

- **pod** (array)

  - _(VAST 전용)_ 하나의 광고 구간에서 **2개 이상의 VAST 광고를 연속 재생**하도록 구성합니다.
  - `advertising.schedule[].tag` 또는 `advertising.schedule[].vastxml` 과 **같이 사용하지 마세요.**
  - 이 속성 대신, **VAST 3.0 템플릿**을 사용하는 것을 강력히 권장합니다.

- **tag** (string \| array)

  - 단일 문자열일 경우, **VAST 또는 IMA 플러그인용 광고 태그 URL** 또는 **FreeWheel용 문자열 플레이스홀더**로 사용됩니다.
  - _(VAST 전용)_ 배열일 경우, 여러 VAST 광고 태그를 지정하여 **하나 이상의 광고 태그 로드 실패 시 대체(fallback)** 로 사용됩니다.
  - VAST 태그를 사용하는 경우, **GDPR 동의(GDPR consent)** 등과 같은 기능을 정의하기 위해 **광고 태그 타겟팅 매크로(ad tag targeting macros)** 를 추가할 수 있습니다.
  - 이 속성과 `advertising.schedule[].vastxml` 은 **동일한 광고 구간에서 함께 사용할 수 없습니다.**

- **type** (string)

  - 광고 구간 내에서 제공되는 광고의 **형식(format)** 을 지정합니다.
  - 가능한 값:
    • `linear`: (기본값) 영상 재생을 중단하고 표시되는 **비디오 광고**
    • `nonlinear`: 영상 재생을 중단하지 않고 **플레이어 일부 위에 오버레이되는 광고**.
  - (이 경우 광고 큐 포인트가 표시되지 않음)
  - 하나의 광고 구간에서 선형(linear)과 비선형(nonlinear) 광고가 혼합되어 제공되는 경우, 이 속성을 **정의하지 마세요.**
  - 플레이어는 선형 광고 시 재생을 중단하고, 비선형 광고 시 재생을 계속 유지합니다.

- **vastxml** (string)

  - _(VAST 전용)_ 설정된 광고 구간에서 요청되는 **VAST XML 광고 태그** 를 정의합니다.
  - 이 속성과 `advertising.schedule[].tag` 는 **동시에 사용할 수 없습니다.**
