# Setup Events

These API calls are used to create players and provide setup information.

<br><br><br>

## .on('ready')

Signifies when the player has been initialized and is ready for playback  
This is the earliest point at which any API calls should be made.

### 호출시점

- `.setup()` 실행 후 **플레이어 초기화가 완료되어 재생 준비가 끝난 시점**에 호출됨.
- 이 시점부터 **모든 JW Player API를 안전하게 호출 가능**함.
- `setupError`가 발생하면 `ready`는 호출되지 않음.

### 이벤트 객체 구조 (콜백 파라미터)

```json
{
  "setupTime": 240,
  "viewable": 1,
  "type": "ready"
}
```

| Value                  | Description                                      |
| :--------------------- | :----------------------------------------------- |
| **setupTime** (number) | setup() → ready까지 걸린 시간 (ms)               |
| **viewable** (number)  | 플레이어가 화면에 보이는 상태 (1=보임, 0=안보임) |

### 활용

- `ready` 안에서 버튼 추가, 자막 설정, 자동 재생, 트래킹 초기화 등 **초기 동작을 구성**함.
- 초기화 지연 측정(`setupTime`)이나 플레이어 표시 상태(`viewable`) 확인에 사용.
- 여러 플레이어를 관리할 때, 각 인스턴스별 초기화 완료 시점을 구분해 로직 실행 가능.

### 주의사항

- `ready` 이후에만 다른 이벤트(`play`, `pause`, `buffer`)가 발생함.
- `setup()` 후 즉시 이벤트를 등록해야 놓치지 않음.
- `remove()` 호출로 플레이어를 삭제하면 `ready`는 다시 발생하지 않음(새로 setup 시 다시 발생).
- 자동재생 설정 시, 브라우저 정책에 따라 `ready`는 오지만 재생이 안 될 수 있음(`autostartNotAllowed` 별도 처리 필요).

### 특징

- **JW Player** 초기화 완료의 유일한 보장 이벤트.
- 모든 **API 호출의 시작점**이며, `setupError`와 짝지어 사용해야 안정적인 로딩 관리 가능.

<br><br><br>

## .on('remove')

Triggered when the player is taken off of a page via `jwplayer().remove();`  
No value returned

### 호출시점

- `jwplayer(id).remove()` 메서드가 실행되어 **플레이어 인스턴스가 DOM에서 제거될 때** 호출됨.
- 내부적으로 모든 미디어 로드 중단·이벤트 리스너 해제·UI 요소 삭제가 완료된 직후 발생.
- 플레이어가 완전히 해제된 이후(`destroyed` 상태)로 전환되는 마지막 단계.

### 이벤트 객체 구조 (콜백 파라미터)

```json
{
  "type": "remove"
}
```

### 활용

- 플레이어 제거 시 연결된 UI 또는 전역 데이터 정리
- 싱글페이지 앱(SPA) 또는 동적 페이지 환경에서 메모리 누수를 막기 위해 정리 루틴 추가.
- 새로운 플레이어를 같은 DOM 요소에 `setup()` 하기 전에 정상적으로 정리되었는지 확인용으로 활용.

### 주의사항

- `remove()` 호출 이후에는 해당 인스턴스의 모든 API (`play()`, `pause()`, `getState()` 등) 호출이 불가.
- 같은 ID DOM 요소에 다시 `setup()` 할 경우 새 인스턴스로 취급됨(기존 리스너 무효).
- 이벤트가 발생한다고 해서 DOM 노드가 자동으로 없어지는 것은 아님. 플레이어가 내부적으로 삽입한 요소만 삭제함.
- 광고나 트래킹 모듈이 동작 중인 상태에서 `remove()`를 호출하면 해당 세션도 즉시 종료됨.

### 특징

- **플레이어 수명 주기의 마지막 이벤트.**
- **리소스 정리·메모리 관리 트리거**로 사용하기 적합.
- 다른 이벤트(`ready`, `complete`, `setupError`)와 달리, `remove()` 명시 호출 없이는 발생하지 않음.

<br><br><br>

## .on('setupError')

Fires when neither the player could be set up

### 호출시점

- `.setup()` 실행 중 **플레이어 초기화가 실패했을 때** 호출됨.
- 예를 들어 파일 URL 없음, 잘못된 playlist 포맷, 라이선스 키 누락, 네트워크 에러 등으로 초기 설정을 완료하지 못했을 때 트리거됨.
- 이 경우 `ready` 이벤트는 절대 발생하지 않음.

### 이벤트 객체 구조 (콜백 파라미터)

```json
{
  "code": 101101,
  "sourceError": null,
  "message": "Sorry, the video player failed to load.",
  "type": "setupError"
}
```

| Value                            | Description                                                 |
| :------------------------------- | :---------------------------------------------------------- |
| **code** (number)                | JW Player 내부 에러 코드 (원인 식별용)                      |
| **sourceError** (object \| null) | 하위 에러 객체 (브라우저 또는 네트워크 스택 오류 포함 가능) |
| **message** (string)             | 오류 메시지 (사용자 또는 콘솔에 표시 가능)                  |

### 활용

- **에러 로그 수집**: `code`, `message` 기반으로 문제 원인 추적.
- **UI 대체 처리**: 오류 발생 시 플레이어 대신 에러 화면 또는 재시도 버튼 표시.
- **자동 복구 시도**: 특정 코드 (예: 네트워크 타임아웃) 발생 시 재요청 또는 fallback URL 로딩.

### 주의사항

- `setupError` 발생 후 해당 인스턴스는 정상 작동하지 않음 → 새로 `setup()` 해야 함.
- 에러 발생 전까지 등록한 이벤트 리스너(`ready`, `play` 등)는 호출되지 않음.
- 로드 도중 네트워크 지연으로 `setupError` 가 뜨는 경우 재시도 전에 `remove()` 로 정리 필요.
- 코드 101xxx ~ 102xxx 등은 공식 에러 테이블에서 의미가 정의되어 있음 (예: 101001 = playlist load failure, 102001 = missing license key).

### 특징

- **플레이어 초기화 단계의 유일한 실패 알림 이벤트.**
- `ready`**와 짝을 이루는 반대 이벤트** → 성공(`ready`) vs 실패(`setupError`).
- 디버깅, 로그 분석, 에러 UI 대체 처리를 위해 필수적으로 구독해야 함.
