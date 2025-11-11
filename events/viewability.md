# Viewability Events

> iframe 환경에서는 viewability API 사용을 권장하지 않습니다.

<br>
<br>

## .on('containerViewable')

플레이어 컨테이너(`<div>` 요소)의 **가시성(viewability)** 상태를 나타내는 이벤트가 트리거됩니다.

```json
{
  "viewable": 0,
  "type": "containerViewable"
}
```

- **type** (string)

  - 이벤트의 유형
  - 이 값은 항상 `"containerViewable"`

- **viewable** (boolean)

  - 플레이어 컨테이너의 가시성 상태
  - 가능한 값:
    - `0`: 플레이어 컨테이너가 화면에 보이지 않음
    - `1`: 플레이어 컨테이너가 화면에 보임

<br>
<br>

## .on('viewable')

플레이어의 **가시성(viewability)** 상태를 나타내는 이벤트가 트리거됩니다.

```json
{
  "viewable": 1,
  "type": "viewable"
}
```

- **type** (string)

  - 이벤트의 유형
  - 이 값은 항상 `"viewable"`

- **viewable** (boolean)

  - 플레이어의 가시성 상태
  - 가능한 값:
    - `0`: 플레이어가 화면에 보이지 않음
    - `1`: 플레이어가 화면에 보임
