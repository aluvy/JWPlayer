# Common Media Client Data

<sup>8.36.2+</sup>

공통 미디어 클라이언트 데이터, CMCD

이 속성은 **모든 HLS.js 또는 Shaka 요청에 CMCD(Common Media Client Data) 정보를 추가**합니다.

> 참고: `"cmcd": {}`를 추가하면 각 CMCD 속성에 대해 **기본값(default values)** 이 전송됩니다.  
> 그러나 각 **CMCD ID에 의미 있는 값**을 직접 지정하는 것을 권장합니다.

<br>

```javascript
player.setup({
    "playlist": "https://cdn.jwplayer.com/v2/media/hWF9vG66",
    ...
    "cmcd": {
        "sessionId": "6e2fb550-c457-11e9-bb97-0800200c9a66",
        "contentId": "hWF9vG66",
        "useHeaders": false
    }
});

```

<br>

- **contentId** (string)

  - 현재 미디어 항목의 **식별자(identifier)** 를 지정합니다.
  - 기본값: 플레이리스트 항목의 `media ID`

<br>

- **sessionId** (string)

  - **세션(session)** 의 고유 식별자입니다.
  - 기본값: 무작위로 생성된 `UUID`

<br>

- **useHeaders** (boolean)

  - 요청에 **CMCD 헤더를 추가할지 여부**를 설정합니다.
  - 가능한 값:
    - `false`: (기본값) CMCD 정보가 **쿼리 스트링(query string)** 을 통해 전송됩니다.
    - `true`: CMCD 정보가 **HTTP 헤더(headers)** 를 통해 전송됩니다.
  - ⚠️ 단, 서버가 이러한 추가 헤더를 **예상하고 허용하지 않는 경우, CORS 오류**가 발생할 수 있습니다.
