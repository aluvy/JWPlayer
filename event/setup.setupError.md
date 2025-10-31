# .on('setupError')

Fires when neither the player could be set up

## 호출시점

- `.setup()` 실행 중 **플레이어 초기화가 실패했을 때** 호출됨.
- 예를 들어 파일 URL 없음, 잘못된 playlist 포맷, 라이선스 키 누락, 네트워크 에러 등으로 초기 설정을 완료하지 못했을 때 트리거됨.
- 이 경우 `ready` 이벤트는 절대 발생하지 않음.

## 이벤트 객체 구조 (콜백 파라미터)

```json
{
  "code": 101101,
  "sourceError": null,
  "message": "Sorry, the video player failed to load.",
  "type": "setupError"
}
```

| Value                           | Description                                                 |
| :------------------------------ | :---------------------------------------------------------- |
| **code** (number)               | JW Player 내부 에러 코드 (원인 식별용)                      |
| **sourceError** (object / null) | 하위 에러 객체 (브라우저 또는 네트워크 스택 오류 포함 가능) |
| **message** (string)            | 오류 메시지 (사용자 또는 콘솔에 표시 가능)                  |

## 활용

- **에러 로그 수집**: `code`, `message` 기반으로 문제 원인 추적.
- **UI 대체 처리**: 오류 발생 시 플레이어 대신 에러 화면 또는 재시도 버튼 표시.
- **자동 복구 시도**: 특정 코드 (예: 네트워크 타임아웃) 발생 시 재요청 또는 fallback URL 로딩.

## 주의사항

- `setupError` 발생 후 해당 인스턴스는 정상 작동하지 않음 → 새로 `setup()` 해야 함.
- 에러 발생 전까지 등록한 이벤트 리스너(`ready`, `play` 등)는 호출되지 않음.
- 로드 도중 네트워크 지연으로 `setupError` 가 뜨는 경우 재시도 전에 `remove()` 로 정리 필요.
- 코드 101xxx ~ 102xxx 등은 공식 에러 테이블에서 의미가 정의되어 있음 (예: 101001 = playlist load failure, 102001 = missing license key).

## 특징

- **플레이어 초기화 단계의 유일한 실패 알림 이벤트.**
- `ready`**와 짝을 이루는 반대 이벤트** → 성공(`ready`) vs 실패(`setupError`).
- 디버깅, 로그 분석, 에러 UI 대체 처리를 위해 필수적으로 구독해야 함.
