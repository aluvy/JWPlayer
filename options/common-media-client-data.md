# Common Media Client Data

<sup>8.36.2+</sup>

이 속성은 모든 HLS.js 또는 Shaka 요청에 공통 미디어 클라이언트 데이터(CMCD) 정보를 추가합니다.

> `"cmcd": {}`을(를) 추가하여 각 CMCD 속성에 대한 기본값을 보낼 수 있습니다.
> 그러나 각 CMCD ID에 대해 의미 있는 값을 추가하는 것이 좋습니다.

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

- **contentId** (string)

  - 현재 미디어 항목의 식별자
  - Default: 재생 목록 항목의 미디어 ID

- **sessionId** (string)

  - 세션 식별자
  - Default: 무작위로 생성된 UUID

- **useHeaders** (boolean)

  - 요청에 CMCD 헤더를 추가합니다
  - 가능한 값
    - `false` (Default): CMCD 정보는 쿼리 문자열을 통해 전송됩니다.
    - `true`: CMCD 정보는 헤더를 통해 전송됩니다. 서버가 이러한 추가 헤더를 기대하고 허용하지 않는 한 헤더를 추가하는 것은 CORS를 위반할 수 있다는 점을 유의하세요.
