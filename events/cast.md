# Cast Events

이 API 호출은 개발자가 **캐스트 관련 이벤트를 감지**할 수 있도록 합니다.

<br>
<br>

## .on('cast')

캐스트 속성이 변경될 때 트리거됩니다.

```json
{
  "available": true,
  "active": true,
  "deviceName": "ViewerTV",
  "type": "cast"
}
```

- **active** (boolean)

  - 캐스팅이 시작되었는지(`true`) 또는 중지되었는지(`false`)를 나타냅니다.

- **available** (boolean)

  - 캐스트 가능한 디바이스가 존재하면 `true`, 없으면 `false`를 반환합니다.
  - 캐스트 가능한 기기가 탐지되면 **플레이어 컨트롤 바에 캐스트 아이콘이 표시됩니다.**

- **deviceName** (string)

  - 시청자가 기기에 지정한 이름(수신기 이름)을 나타냅니다.
  - 이 속성은 `"cast.active": "true"`일 때만 채워집니다.

- **type** (string)

  - 플레이어 이벤트의 카테고리 (이 이벤트의 경우 항상 `"cast"`)

<br>
<br>

## .on('castIntercepted')

[`interceptCast`](<(https://docs.jwplayer.com/players/reference/casting)>)가 활성화된 상태에서 사용자가 콘텐츠를 캐스트하려고 시도할 때 발생합니다.

이 이벤트는 또한 **현재 캐스트 세션이 활성 상태인지 여부**를 나타냅니다.

```Javascript
{
  "isCasting": false,
  "type": "castIntercepted"
}
```

- **isCasting** (boolean)

  - Chromecast 기기가 미디어를 캐스트 중(`true`)인지, 아닌지(`false`)를 나타냅니다.

- **type** (string)

  - 수신 중인 이벤트의 유형을 식별합니다. (이 이벤트의 경우 항상 `"castIntercepted"`)
