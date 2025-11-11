\*\*# Related Events

<br>
<br>

## .on('relatedReady')

Related 플러그인이 준비되었을 때** 트리거되는 이벤트를 나타냅니다.  
이 시점이 **가장 먼저 API 호출을 수행할 수 있는 시점\*\*입니다.

```json
{
  "type": "relatedReady"
}
```

- **type** (string)

  - 플레이어 이벤트의 카테고리
  - 이 값은 항상 `"relatedReady"`

다음 페이지의 관련 이벤트들을 사용하려면, 아래 코드를 **사전에 구현해야 합니다.**

```javascript
.on('relatedReady', function(event) {
    relatedPlugin = jwplayer().getPlugin('related');
});
```

<br>
<br>

# ("related").on('open')

추천(related) 인터페이스가 **열릴 때** 트리거됩니다.

- **items** (object)

  - Related 플러그인에 로드된 모든 객체들의 집합

- **method** (object)

  - 플러그인을 연 데 사용된 메서드

- **url** (object)

  - 플레이어에 로드된 피드의 URL

<br>
<br>

# ("related").on('close')

추천(related) 인터페이스가 **닫힐 때** 트리거됩니다.

```json
{
  "visible": false,
  "method": "interaction_more",
  "relatedFile": "https://cdn.jwplayer.com/feed.json?feed_id=9qjYmZBC&related_media_id=Zb9M8K61",
  "onclick": "link"
}
```

- **method** (string)

  - 플러그인을 여는 데 사용된 메서드

- **onclick** (string)

  - 추천 영상이 선택되었을 때의 동작 방식
  - 참조: _onclick (Related)_

- **relatedFile** (string)

  - 플레이어에 로드된 피드의 URL

- **visible** (boolean)

  - Related 플러그인의 표시 여부
  - 이 값은 항상 `false`

<br>
<br>

# ("related").on('play')

사용자가 **추천 피드에서 항목을 선택했을 때** 트리거됩니다.

- **auto** (boolean)

  - 자동 재생으로 시작된 경우 `true`, 수동으로 시작된 경우 `false`

- **item** (object)

  - 피드에 지정된 선택된 항목의 메타데이터

- **position** (number)

  - 선택된 추천 영상의 순서 번호 (ordinal position)
