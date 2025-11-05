# Google Analytics (ga)

Google 애널리틱스와의 내장 통합 구성

Google 애널리틱스 계정에서 플레이어 이벤트를 캡처하려면 플레이어와 동일한 페이지에서 Google 애널리틱스를 구현해야 합니다.

> JWP는 Google 태그 관리자(GTM)를 통해 GA4를 지원하지 않습니다.

```json
"ga":  {
    "label": "mediaid",
    "sendEnhancedEvents": true
}
```

- **label** (string)

  - Google 애널리틱스에서 이벤트 레이블로 사용할 재생 목록 속성(예: title 또는 mediaid)을 정의합니다
  - Default: `file`

- **sendEnhancedEvents** (boolean) <sup>8.27.0+</sup>

  - (Google Analytics 4) 이벤트에 대해 향상된 측정 값과 이름이 반환되는지 여부를 결정합니다
  - 가능한 값
    - `false` (Default): JWP 이벤트는 오래된 이벤트 이름과 오래된 매개변수로 실행됩니다.
    - `true`: 모든 JWP 이벤트는 video\_가 추가된 새로운 이벤트 이름으로 시작되며 다음과 같은 새로운 G4 비디오 매개변수를 포함합니다.
      - video_current_time
      - video_duration
      - video_percent
      - video_pro- vider
      - video_title
      - video_url
      - visible

<br>

> 빈 ga 객체`("ga":{})`도 정의할 수 있습니다. 모든 기본값이 설정됩니다.
