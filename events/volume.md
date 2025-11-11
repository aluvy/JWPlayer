# Volume Events

<br>
<br>

## .on('mute')

플레이어가 **음소거 상태로 전환되거나 해제될 때** 트리거됩니다.

```json
{
  "mute": true,
  "type": "mute"
}
```

- **event** (string)

  - 이벤트의 유형
  - 이 속성의 값은 항상 `"volume"`

- **mute** (boolean)

  - 플레이어의 새로운 음소거 상태
  - 가능한 값:
    - `true`
    - `false`

<br>
<br>

## .on('volume')

플레이어의 **볼륨이 변경될 때** 트리거됩니다.

```json
{
  "volume": 73,
  "type": "volume"
}
```

- **type** (string)

  - 이벤트의 유형
  - 이 속성의 값은 항상 `"volume"`

- **volume** (number)

  - 새로운 볼륨 비율 (0–100)
