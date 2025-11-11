# Floating Player Events

플레이어는 사용자가 **플로팅 상태를 동적으로 업데이트**할 수 있도록 합니다.

<br>
<br>

## .on('float')

플로팅 플레이어가 **떠오르거나(floating 시작) 원래 위치로 복귀할 때** 트리거됩니다.

```json
{
  "floating": true,
  "type": "float"
}
```

- **floating** (boolean)

  - 플레이어의 플로팅 상태를 나타냅니다.
  - 가능한 값:
    - `true`: 플로팅 플레이어 상태
    - `false`: 플레이어가 원래 위치로 복귀된 상태

- **type** (string)

  - 플레이어 이벤트의 카테고리
  - 이 이벤트의 경우 항상 `"float"`
