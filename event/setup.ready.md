# .on('ready')

Signifies when the player has been initialized and is ready for playback  
This is the earliest point at which any API calls should be made.

## 호출시점

- `.setup()` 실행 후 **플레이어 초기화가 완료되어 재생 준비가 끝난 시점**에 호출됨.
- 이 시점부터 **모든 JW Player API를 안전하게 호출 가능**함.
- `setupError`가 발생하면 `ready`는 호출되지 않음.

## 이벤트 객체 구조 (콜백 파라미터)

```json
{
  "setupTime": 240,
  "viewable": 1,
  "type": "ready"
}
```

| Value              | Description                                      |
| :----------------- | :----------------------------------------------- |
| setupTime (number) | setup() → ready까지 걸린 시간 (ms)               |
| viewable (number)  | 플레이어가 화면에 보이는 상태 (1=보임, 0=안보임) |

## 활용

- `ready` 안에서 버튼 추가, 자막 설정, 자동 재생, 트래킹 초기화 등 **초기 동작을 구성**함.
- 초기화 지연 측정(`setupTime`)이나 플레이어 표시 상태(`viewable`) 확인에 사용.
- 여러 플레이어를 관리할 때, 각 인스턴스별 초기화 완료 시점을 구분해 로직 실행 가능.

## 주의사항

- `ready` 이후에만 다른 이벤트(`play`, `pause`, `buffer`)가 발생함.
- `setup()` 후 즉시 이벤트를 등록해야 놓치지 않음.
- `remove()` 호출로 플레이어를 삭제하면 `ready`는 다시 발생하지 않음(새로 setup 시 다시 발생).
- 자동재생 설정 시, 브라우저 정책에 따라 `ready`는 오지만 재생이 안 될 수 있음(`autostartNotAllowed` 별도 처리 필요).

## 특징

- **JW Player** 초기화 완료의 유일한 보장 이벤트.
- 모든 **API 호출의 시작점**이며, `setupError`와 짝지어 사용해야 안정적인 로딩 관리 가능.
