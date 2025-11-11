# Resize Events

이 API 호출은 **플레이어의 현재 크기(dimension)** 및 **전체화면(fullscreen) 상태를 조회하거나 업데이트**하는 데 사용됩니다.

<br>
<br>

## .on('fullscreen')

플레이어가 **전체화면 모드로 전환되거나 해제될 때** 트리거됩니다.

```json
{
  "fullscreen": false,
  "type": "fullscreen"
}
```

- **fullscreen** (boolean)

  - 플레이어 화면이 전체화면 모드인지 여부
  - 가능한 값:
    - `true`
    - `false`

- **type** (string)

  - 리사이즈 이벤트의 카테고리
  - 이 값은 항상 `"fullscreen"`

<br>
<br>

## .on('resize')

플레이어의 **페이지 내 크기(dimension)가 변경되었을 때** 트리거됩니다.

> `.on('resize')` 이벤트는 **전체화면(fullscreen) 전환 시**에는 트리거되지 않습니다.

```json
{
  "width": 1920,
  "height": 1080,
  "type": "resize"
}
```

- **height** (number)

  - 플레이어의 새로운 높이

- **type** (string)

  - 리사이즈 이벤트의 카테고리
  - 이 값은 항상 `"resize"`

- **width** (number)

  - 플레이어의 새로운 너비
