# Setup Events

이 API 호출은 **플레이어를 생성하고 설정 정보를 제공**하는 데 사용됩니다.

<br><br>

## .on('ready')

플레이어가 초기화되어 **재생 준비가 완료되었을 때** 발생합니다.

이 시점이 **가장 빠르게 API 호출을 수행할 수 있는 시점**입니다.

```json
{
  "setupTime": 240,
  "viewable": 1
}
```

- **setupTime** (number)

  - `setup()` 호출부터 `ready` 상태까지 걸린 시간(밀리초 단위)

- **viewable** (number)

  - 플레이어가 현재 **화면에 보이는 상태인지 여부**

<br>
<br>

## .on('remove')

`jwplayer().remove()`를 통해 **플레이어가 페이지에서 제거될 때** 트리거됩니다.

반환되는 값은 없습니다.

<br>
<br>

## .on('setupError')

플레이어를 **정상적으로 설정할 수 없을 때** 발생합니다.

```json
{
  "code": 101101,
  "sourceError": null,
  "message": "Sorry, the video player failed to load.",
  "type": "setupError"
}
```

- **code** (number)

  - 오류 식별자(ID)

- **message** (string)

  - 사용자에게 표시되는 오류 메시지
  - 이 속성은 **지역화(다국어 변환)** 가능합니다.

- **sourceError** (object \| null)

  - 플레이어 내부에서 발생한 **하위 수준 오류나 이벤트 객체**로, 이 오류의 원인이 된 요소입니다.

- **type** (string)

  - 오류 또는 경고의 카테고리 (이 이벤트의 경우 항상 `"setupError"`)
