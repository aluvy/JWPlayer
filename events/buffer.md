# Buffer Events

These API calls are used to update clients with the percentage of a file that is buffered into the player.

> This only applies to VOD media. Live streaming media (HLS/DASH) does not expose this behavior.

<br>
<br>

# .on('bufferChange')

Fires when the currently playing item loads additional data into its buffer

### 호출시점

- 플레이어가 **현재 재생 중인 미디어의 버퍼(로딩된 데이터 양)가 변할 때마다** 호출됩니다.
- 즉, 미디어 데이터가 새로 다운로드되거나 시킹(seek) 후 버퍼링 진행 중일 때마다 주기적으로 트리거됨.
- 주로 **VOD(Progressive MP4 또는 HLS)** 콘텐츠에서 작동하며, 라이브 스트림에서는 빈도가 줄거나 비활성일 수 있습니다.

### 이벤트 객체 구조 (콜백 파라미터)

```json
{
  "bufferPercent": 37.5,
  "position": 120.31,
  "duration": 640,
  "currentTime": 120.31,
  "seekRange": {
    "start": 0,
    "end": 640
  },
  "absolutePosition": null,
  "type": "bufferChange"
}
```

| Value                                 | Description                                                                                         |
| :------------------------------------ | :-------------------------------------------------------------------------------------------------- |
| **bufferPercent** (number)            | 현재 미디어 전체 중 버퍼된 비율(%) (0–100 범위 아님: 0–100 대신 0–1 또는 퍼센트값 사용 버전에 따름) |
| **position** (number)                 | 현재 재생 위치 (초)                                                                                 |
| **duration** (number)                 | 전체 영상 길이(초)                                                                                  |
| **currentTime** (number)              | 현재 시간(일부 환경에서는 `position`과 동일)                                                        |
| **seekRange** (object)                | 탐색 가능한 시작·끝 범위(`start`, `end`)                                                            |
| **absolutePosition** (number \| null) | 라이브 스트림일 경우 절대 시간 정보, VOD 에서는 `null`                                              |

### 활용

#### 1. UI 버퍼 바 표시

→ 버퍼링 진행 정도를 시각화하여 사용자에게 진행 상황 표시.

```javascript
player.on('bufferChange', (e) => {
  updateBufferBar(e.bufferPercent);
});
```

#### 2. 로딩 상태 모니터링

- 버퍼 비율이 낮을 경우 “로딩 중” 스피너 표시,  
  일정 이상 채워지면 숨김 처리.

#### 3. 품질/네트워크 분석

- 재생 중 버퍼링 비율과 시점을 로깅하여 네트워크 품질 분석 데이터로 활용.

#### 4. 시킹(탐색) 후 완충 상태 감지

- 시크 바를 이동한 후 버퍼링 진행 상황을 `bufferChange`로 감시하여  
  “이동한 구간이 얼마나 로딩 되었는가” 파악.

### 주의사항

- **발생 빈도 높음** → 프레임당 또는 초당 여러 번 호출될 수 있으므로 무거운 로직(네트워크 요청 등)을 직접 넣지 말 것.
- **라이브 스트림(HLS/DASH)** 에서는 버퍼가 짧거나 고정되어 이벤트가 거의 안 나올 수 있음.
- `bufferPercent` 값은 브라우저·스트림 형식에 따라 0–100 혹은 0–1 형태로 달라질 수 있으므로 정규화 필요.
- `seekRange` 및 `absolutePosition` 은 환경에 따라 `null` 또는 부분 데이터일 수 있음 → 존재 체크 후 사용.
- `buffer` 이벤트(상태 변화)와 혼동 주의:
  - `buffer` → “재생 상태가 버퍼링으로 바뀜”
  - `bufferChange` → “버퍼 진행 률이 바뀜”

### 특징

- **실시간 버퍼 진행률을 제공하는 유일한 이벤트.**
- 재생 품질 지표 (예: 시청 중 로딩 빈도 및 완충 시간) 측정에 적합.
- `buffer` 이벤트보다 빈번히 발생하고 정량적 데이터(`bufferPercent`)를 제공함.
- 개발자 도구 나 분석 시스템에서 **네트워크 상태 피드백용**으로 활용 하기 좋음.
