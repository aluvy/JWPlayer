# Google Analytics (ga)

이 옵션은 **Google Analytics(GA)** 와의 **내장 통합 기능(built-in integration)** 을 구성합니다.

플레이어 이벤트를 Google Analytics 계정에서 추적하려면,  
**플레이어가 포함된 동일한 페이지에 Google Analytics가 구현되어 있어야 합니다.**

<br>

> 주의: JW Player는 **Google Tag Manager(GTM)** 를 통한 **GA4 통합을 지원하지 않습니다.**

<br>

```json
"ga":  {
    "label": "mediaid",
    "sendEnhancedEvents": true
}
```

---

<br>

### label (string)

Google Analytics 이벤트의 **라벨(label)** 로 사용할 **플레이리스트 속성**을 정의합니다.  
예: `title`, `mediaid`

- 기본값: `file`

<br>

### sendEnhancedEvents (boolean) <sup>8.27.0+</sup>

**(Google Analytics 4 전용)**
이벤트에 대해 **향상된 측정(enhanced measurement)** 값과 **새로운 이벤트 네이밍 규칙**을 사용할지 여부를 지정합니다.

가능한 값:

- `false`: (기본값) 기존 이벤트 이름과 기존 매개변수로 JWP 이벤트 전송
- `true`: 모든 JWP 이벤트가 `video_` 접두사로 시작하는 **새 이벤트 이름**으로 전송되며, 다음 **GA4 비디오 관련 매개변수**를 포함합니다:
   • video_current_time
   • video_duration
   • video_percent
   • video_provider
   • video_title
   • video_url
   • visible

<br>

> 팁:  
> 빈 GA 객체`("ga": {})`를 정의할 수도 있습니다.  
> 이 경우 **모든 기본값이 자동으로 적용**됩니다.
