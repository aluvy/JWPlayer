# Setup Options

아래 옵션들은 **플레이어의 레이아웃과 재생 동작(playback behavior)** 을 설정하는 항목입니다.  
각 옵션은 플레이어의 **setup 구성 객체**에 직접 포함됩니다.

<br>

## Appearance

외관 관련 옵션

<br>

### aspectratio (string)

플레이어의 `width`가 백분율(%)로 지정된 경우, **비율(aspect ratio)** 을 유지합니다.
플레이어가 **고정 크기(static size)** 인 경우에는 적용되지 않습니다.
비율은 **x:y 형식**으로 입력해야 합니다.

- 예: `"16:9"`, `"4:3"`

<br>

### **controls** (boolean)

**비디오 컨트롤(재생바 및 표시 아이콘)** 을 표시할지 여부를 설정합니다.

- 기본값: `true`

<br>

### **displaydescription** (boolean)

미디어 파일의 **설명(description)** 을 표시할지 여부를 설정합니다.

- 기본값: `true`

<br>

### **displayHeading** (boolean) <sup>8.6.0+</sup>

**(Outstream 전용)**
Outstream 플레이어 상단에 표시되는 **헤딩(heading)** 을 제어합니다.

가능한 값:

- `false`: (기본값) 헤딩이 표시되지 않음
- `true`: “Advertisement”라는 기본 텍스트와 함께 헤딩이 표시됨

> 텍스트를 사용자 정의하거나 현지화하려면 [`intl.{lang}.displayHeading`](https://docs.jwplayer.com/players/reference/internationalization) 옵션을 사용하세요.

<br>

### **displayPlaybackLabel** (boolean) <sup>8.6.0+</sup>

플레이어 **대기 화면(idle screen)** 의 재생 버튼 아래에 **콜투액션(call-to-action)** 문구를 표시합니다.

`true`로 설정 시, **재생 버튼 클릭률이 최대 약 5% 향상**될 수 있습니다.  
기본 문구는 `"Play"`이며, 시청자에게 맞게 현지화할 수도 있습니다.

- 기본값: `false`

<br>

### **displaytitle** (boolean)

미디어 파일의 **제목(title)** 을 표시할지 여부를 설정합니다.

- 기본값: `true`

<br>

### **height** (number)

비디오 플레이어의 **세로 크기(px)** 를 지정합니다.
`aspectratio`가 정의되어 있을 경우에는 이 속성을 생략해야 합니다.

- 기본값: `360`

<br>

### **horizontalVolumeSlider** (boolean) <sup>8.18.4+</sup>

컨트롤 바 안에 **볼륨 슬라이더를 가로 방향으로 표시**합니다.  
오디오 모드일 경우, 항상 가로 슬라이더가 표시됩니다.  
기본적으로 볼륨 슬라이더는 **컨트롤 바 위쪽에 세로형으로 표시**됩니다.

- 기본값: `false`

<br>

### **livePause** (boolean) <sup>8.38.1+</sup>

**DASH 및 HLS 스트림**에서 실시간 재생 중 **일시정지 버튼**을 표시합니다.

일시정지 중에도 **라이브 세그먼트는 백그라운드에서 계속 로드**됩니다.  
DASH 스트림의 경우, 재생이 마지막 프레임에서 멈춥니다.

- 기본값: `false`

<br>

### **nextUpDisplay** (boolean)

"Next Up" 모달 창을 표시할지 여부를 설정합니다.

<br>

### **qualityLabels** (array)

기본적으로 JW Player는 매니페스트 파일의 정보를 기반으로 화질 단계를 설정합니다.  
이 옵션을 사용하면 특정 **대역폭(kbps)** 에 사용자 지정 **화질 라벨(quality label)** 을 적용할 수 있습니다.  
(HLS 및 DASH에서 모두 동작합니다.)

```javascript
qualityLabels: {
  "2500": "High",
  "1000": "Medium"
}
```

<br>

### **renderCaptionsNatively** (boolean) <sup>8.0.1+</sup>

**(Chrome 및 Safari 전용)**  
자막(captions)을 **브라우저 렌더러** 또는 **플레이어 렌더러** 중 어떤 것으로 표시할지를 결정합니다.

가능한 값:

- `true`: (Safari 기본값) 브라우저 렌더러를 사용하여 자막을 표시
- `false`: (Chrome 기본값) JW Player 렌더러를 사용하여 자막을 표시

> 관련 항목: [captions](https://docs.jwplayer.com/players/reference/captions-config-ref)

<br>

### **stretching** (string)

이미지 및 비디오를 **플레이어 크기에 맞게 조정하는 방식**을 설정합니다.

가능한 값:

- `uniform`: (기본값) 비율을 유지하며 플레이어 크기에 맞춤
- `exactfit`: 비율을 무시하고 플레이어 크기에 강제로 맞춤
- `fill`: 비율을 유지하면서 화면을 채우도록 확대/잘라내기
- `none`: 원본 크기로 표시하며 여백(검은 테두리)이 생김

<br>

### **width** (number \| string)

비디오 플레이어의 **가로 크기(px 또는 %)** 를 지정합니다.

- 기본값: `640`

<br><br>

## Behavior

동작 관련 옵션

<br>

### **aboutlink** (string)

플레이어의 **우클릭(right-click) 메뉴**에서 클릭 시 연결될 **사용자 지정 URL**을 설정합니다.

- 기본값: `https://www.jwplayer.com/learn-more`

<br>

### **abouttext** (string)

우클릭 메뉴에 표시될 **사용자 지정 텍스트**를 설정합니다.

<br>

### **allowFullscreen** (boolean) <sup>8.22.0+</sup>

false로 설정하면 **플레이어의 전체화면 기능**을 완전히 비활성화합니다.  
(탭, 클릭, 키보드 단축키, API 접근 포함)

다시 활성화하려면 `setAllowFullscreen(allowFullscreen)` 메서드를 사용하세요.

- 기본값: `true`

<br>

### **autostart** (boolean \| string)

페이지가 로드될 때 플레이어가 자동으로 재생을 시도할지 여부를 설정합니다.

가능한 값:

- `false`: (기본값) 자동재생 비활성화
- `true`: 자동재생 활성화
- `viewable`: 플레이어의 50% 이상이 뷰포트에 노출되면 자동재생

<br>

### **defaultBandwidthEstimate** (number) <sup>8.3.0+</sup>

새 방문자에게 사용할 초기 **대역폭 추정치(bps)** 를 지정하여 플레이어의 기본 추정치를 재정의합니다.

**사용 사례**: 신규 사용자가 사이트를 처음 방문할 때, 네트워크 속도에 적응하기 전까지 몇 초간 저화질로 표시될 수 있습니다. 대부분의 시청자가 빠른 네트워크 환경이라면 이 속성으로 초기 화질을 개선할 수 있습니다.

**⚠️ 주의**: 이 속성은 **재생 성능을 희생하고 화질을 우선시**하므로, 기본적으로 설정하지 않는 것을 권장합니다. 플레이어는 수 초 내에 최적의 대역폭을 자동으로 선택합니다.

- 기본값: `50000`

<br>

### **fullscreenOrientationLock** (string) <sup>8.28.0+</sup>

**(Android 전용)**  
사용자가 모바일 기기를 회전할 때, **모바일 웹뷰 내에서 플레이어 회전을 제어**합니다.

가능한 값:

- `landscape`: (기본값) 가로 방향으로 고정되며, 180º 회전 시 자동 재조정
- `none`: 기기의 방향에 따라 전체화면 회전 자동 조정
- `portrait`: 세로 방향으로 고정되며, 180º 회전 시 자동 재조정

<br>

### **generateSEOMetadata** (boolean) <sup>8.26.1+</sup>

**Google SEO 최적화** 메타데이터 생성을 활성화합니다.

- 기본값: `false`

<br>

### **liveSyncDuration** (number) <sup>8.12.0+</sup>

라이브 스트림의 **라이브 엣지(live edge)** 와의 시간 간격(초 단위)을 정의합니다.  
다음 상황에서 적용됩니다:

1. 플레이어가 라이브 또는 DVR 스트림을 시작할 때
2. 사용자가 “Live” 버튼을 클릭해 실시간 콘텐츠로 복귀할 때

허용 범위: **5–30초**  
버전 **8.19.0+** 에서는 비워두면 플레이어가 자동으로 최적 지연 시간을 결정합니다.

- 기본값: `25` <sup>(<8.19.0)</sup>

<br>

### **mute** (boolean)

재생 중 **플레이어 음소거 여부**를 정의합니다.

사용자가 이 설정을 변경하면, **해당 사용자 세션 동안 지속적으로 유지**됩니다.
예: 모든 플레이어에 `mute: true`가 설정되어 있어도 사용자가 음소거를 해제하면, 이후의 모든 플레이어는 음소거 해제 상태로 시작합니다.

- 기본값: `false`

<br>

### **nextupoffset** (number \| string) <sup>8.9.0+</sup>

"Next Up" 카드가 재생 중 표시되는 **시점을 설정**합니다.

양수 값은 영상 시작 시점으로부터의 오프셋, 음수 값은 영상 종료 시점으로부터의 오프셋입니다.  
숫자(`-10`) 또는 문자열 퍼센트(`"-2%"`)로 지정 가능합니다.

- 기본값: `-10`

<br>

### **pipIcon** (string) <sup>8.21.0+</sup>

**Picture-in-Picture(PiP)** 아이콘을 제어합니다.

가능한 값:

- `enabled`: (기본값) 지원되는 데스크톱 브라우저에 PiP 아이콘 및 우클릭 메뉴 항목 표시
- `disabled`: 아이콘 비활성화

CSS를 통해 아이콘을 숨길 수도 있습니다.

- 기본값: `enabled`

<br>

### **playbackRateControls** (boolean)

**재생 속도 조절 메뉴** 표시 여부를 설정합니다.  
true로 설정 시, 기본 메뉴 옵션으로 `0.5x`, `1x`, `1.25x`, `2x`가 표시됩니다.  
`playbackRates` 배열을 전달해 사용자 정의 옵션을 지정할 수도 있습니다.  
참고: Android의 HLS 스트림에서는 현재 지원되지 않습니다.

- 기본값: `false`

<br>

### **playbackRates** (array)

**설정 메뉴에 표시될 재생 속도 옵션 배열**을 지정합니다.

- 기본값: `[0.25, 0.75, 1, 1.25]`

<br>

### **playlist** (array \| string)

플레이어에서 재생할 **미디어 리스트(playlist)** 를 지정합니다.  
참고: [Playlists](https://docs.jwplayer.com/players/reference/playlists)

<br>

### **playlistIndex** (number) <sup>8.15.0+</sup>

재생을 시작할 **플레이리스트 항목의 인덱스**를 지정합니다.  
플레이리스트의 첫 번째 인덱스는 0입니다.  
음수 값은 끝에서부터 카운트하며, 항목 수보다 큰 절대값은 사용할 수 없습니다.  
예: 항목이 5개일 경우 `"playlistIndex": -5` 가 최솟값입니다.  
이 속성은 JSON, RSS, 배열 형식의 플레이리스트 모두에 적용됩니다.

<br>

### **repeat** (boolean)

플레이리스트가 완료된 후 콘텐츠를 **반복 재생(loop)** 할지 여부를 결정합니다.

- 기본값: `false`

<br><br>

## Rendering and Loading

렌더링 및 로딩 관련 옵션

<br>

### **base** (string)

스킨(skins)과 프로바이더(providers)에 사용할 **대체 기본 경로(base path)** 를 설정합니다.

- 기본값: `/`

<br>

### **flashplayer** (string) <sup>< 8.19.0</sup>

**Adobe의 공지에 따라 Flash Player는 더 이상 지원되지 않습니다.**  
이 옵션은 `jwplayer.flash.swf`의 **대체 디렉터리 경로**를 지정합니다.

- 기본값: `/`

<br>

### **hlsjsdefault** (boolean)

**Chrome for Android** 환경에서 **HLS.js를 기본 프로바이더**로 사용하도록 설정합니다.  
이 속성을 비활성화하면 **브라우저의 기본 HLS 프로바이더**를 사용합니다.

- 기본값: `true`

<br>

### **liveTimeout** (number) <sup>8.1.9+</sup>

**라이브 스트림(manifest) 갱신이 중단(stalled)** 되었을 때의 처리 방식을 설정합니다.

이 속성은 초 단위의 양의 숫자를 허용하지만, **1~10초 사이의 값은 무시됩니다.**  
`0`으로 설정하면 스트림이 **절대 타임아웃되지 않도록** 설정할 수 있습니다.

플레이어는 스트림이 타임아웃될 때까지 **지속적으로 매니페스트 요청**을 시도합니다.  
요청 후 `liveTimeout` 초 동안 매니페스트가 업데이트되지 않으면, **오류 이벤트(error event)** 와 함께 스트림이 종료됩니다.  
즉시 종료를 원할 경우, 매니페스트에 **`<END>` 태그**를 포함하세요.

⚠️ 이 옵션은 **매니페스트 정지 문제**만 제어하며, 세그먼트(segment) 로딩 오류(예: 404 오류)는 그대로 발생합니다.

기본값:

- `liveTimeout` = 매니페스트의 `targetDuration` 값 × 3
- Safari의 경우 기본값은 **30초**입니다.

> 참고: `targetDuration`에 대한 자세한 내용은 [HLS 사양](https://datatracker.ietf.org/doc/html/draft-pantos-hls-rfc8216bis#section-4.4.3.1)을 참조하세요.

<br>

### **loadAndParseHlsMetadata** (boolean)

Safari에서도 Chrome과 동일한 HLS 메타데이터 이벤트를 트리거하기 위해 사용합니다.

이 속성이 활성화되면, JW Player는 `PROGRAM-DATE-TIME` 태그가 포함된 HLS 플레이리스트에 대해 **추가 요청을 수행**하여 **시간 동기화 메타데이터(timed metadata)** 를 임베드합니다.

가능한 값:

- `true`: (기본값) Safari에서 매니페스트를 로드 및 파싱하고, 그 결과를 미디어 재생과 동기화합니다. 여러 번의 매니페스트 요청이 발생할 수 있습니다.
- `false`: Safari가 노출하지 않는 `DATERANGE`, `CUE-OUT`, `CUE-IN`, `DISCONTINUITY` 등의 **매니페스트 외부(out-of-band) 메타데이터**를 파싱하기 위해 추가 HLS 요청을 하지 않습니다.

<br>

### **preload** (string)

재생 전에 콘텐츠를 미리 로드할지 여부를 지정합니다.  
빠른 재생 시작이나, 재생 전 메타데이터 로딩이 필요한 경우 유용합니다.

가능한 값:

- `metadata`: (기본값) 매니페스트를 로드하고 HLS/DASH 스트림의 **최대 1개 세그먼트**를 버퍼링
- `auto`: 매니페스트를 로드하고 **약 30초 분량의 세그먼트**를 버퍼링
- `none`: 콘텐츠를 사전 로드하지 않음 (데이터 사용량이 우려되는 경우 권장)

<br>

### **showUIWhen** (string) <sup>8.32.0+</sup>

플레이어 UI를 **언제 표시할지 시점**을 설정합니다.

가능한 값:

- `onReady`: (기본값) 플레이어가 로드될 때 UI 표시
- `onContent`: 광고 또는 영상 콘텐츠가 준비될 때까지 UI 숨김
