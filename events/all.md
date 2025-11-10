# all Events

<br><br>

## .on('all')

이 단일 API 호출은 **플레이어의 모든 이벤트를 수집**하는 데 사용할 수 있습니다.

> 이 메서드는 **매우 많은 정보를 출력**하므로, 장시간 사용 시 **브라우저 성능이 저하될 수 있습니다.**

### 호출시점

- `jwplayer().on('all', callback)` 으로 등록하면
  플레이어 내부에서 발생하는 **모든 이벤트(ready, play, pause, buffer, complete, setupError 등)** 가
  **트리거될 때마다 한 번씩 호출**됩니다.
- 즉, **모든 이벤트의 “상위 디버그 훅”** 역할을 하며, 이벤트 발생 시마다 자동으로 실행됩니다.

### 이벤트 객체 구조 (콜백 파라미터)

```javascript
player.on('all', function (eventName, eventData) {
  console.log(eventName, eventData);
});
```

- **eventName** (string)

  - 발생한 이벤트 이름 (예: `play`, `pause`, `buffer`, `setupError`)

- **eventData** (object)

  - 해당 이벤트의 데이터 객체 (이벤트별로 내용 다름)

<br>

- `eventData.type` 은 항상 `eventName` 과 동일함.
- 전달되는 내용은 각 이벤트(`play`, `ready`, `bufferChange` 등)의 고유 구조를 그대로 포함합니다.

### 활용

#### 1. 디버깅/로깅

- 재생 과정 전체를 한눈에 추적 가능 (`setup` → `ready` → `play` → `buffer` → `complete` …)
- QA, 디버그, 분석용으로 사용.

#### 2. 커스텀 로깅/트래킹

- 특정 이벤트만 골라서 서버 로그 또는 데이터레이어에 전송

```javascript
player.on('all', (e, d) => {
  if (['play', 'pause', 'error'].includes(e)) sendLog(e, d);
});
```

#### 3. 개발 중 이벤트 흐름 분석

- 어떤 이벤트 순서로 발생하는지 확인해 이벤트 간 타이밍 조정 및 상태 관리 최적화.

### 주의사항

- **모든 이벤트**를 받기 때문에 호출 빈도가 매우 높음 →  
  장시간 실행하거나 무거운 로직을 넣으면 **_브라우저 성능 저하_** 및 콘솔 오버플로 발생 가능.

- 실서비스(프로덕션) 환경에서는 비활성화 권장, 디버그/개발 환경에서만 사용.

- 개별 이벤트 핸들러(`on('play')`, `on('pause')`)보다 먼저 실행 순서가 보장되지 않음.

- 이벤트 데이터 구조가 이벤트마다 다르므로, 공통 필드만 사용하거나 필터링 필요.

- `all`은 **이벤트 리스너 등록 순서를 고려하지 않음**, 내부 발생 순서대로 호출됨.

### 특징

- **JW Player의 모든 이벤트를 한 번에 수집**하는 유일한 API.
- **개발·디버깅용 툴** 성격이 강하며, 성능 부담이 큼.
- 이벤트명(`eventName`) + 데이터(`eventData`)의 **2인자 구조**를 갖는 **특수 이벤트**로,  
  다른 이벤트(`on('play', fn)`)와 달리 고유 파라미터 체계가 존재함.
