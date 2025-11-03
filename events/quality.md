# Quality Events

These API calls are used to listen to or update the video quality if multiple quality levels of a video are provided. Quality levels are sorted and given index numbers.

> An index of 0 will always be "Auto".

<br>
<br>

# .on('levels')

Fired when the list of available quality levels is updated. Happens e.g. shortly after a playlist item starts playing.

### 호출시점

- `levels` 이벤트는 **플레이어가 영상의 화질(quality) 정보를 로드하고, 선택 가능한 레벨 목록이 준비되었을 때** 발생합니다.

- 즉, 미디어 파일이 로드되며 **사용자가 선택할 수 있는 비디오 품질 옵션들이 파싱 완료된 시점**입니다.

#### 대표적인 발생 타이밍

1. `playlistItem` 이벤트 이후, 해당 영상의 소스 파일(`.m3u8`, `.mpd`, MP4 등)을 로드할 때

2. 플레이어가 HLS, DASH, progressive 등에서 **해상도·비트레이트 옵션 목록을 생성할 때**

3. Adaptive Streaming(HLS/DASH)일 경우, 초기 `manifest` 파싱 완료 시점

#### 일반 이벤트 흐름 예시

```
playlistItem
→ levels (화질 옵션 파싱 완료)
→ levelsChanged (선택된 화질 확정)
→ play
→ firstFrame
```

- 요약:
  - “플레이어가 선택 가능한 모든 화질 레벨 정보를 인식했을 때” 발생하는 초기 이벤트.

### 이벤트 객체 구조 (콜백 파라미터)

```json
{
  "levels": [
    { "label": "1080p", "bitrate": 3500000, "width": 1920, "height": 1080 },
    { "label": "720p", "bitrate": 2000000, "width": 1280, "height": 720 },
    { "label": "480p", "bitrate": 1000000, "width": 854, "height": 480 }
  ],
  "type": "levels"
}
```

| Value                         | Description                                 |
| :---------------------------- | :------------------------------------------ |
| **levels** (Array<Object>)    | 사용 가능한 화질 레벨 목록                  |
| **levels[].label** (string)   | 표시용 해상도 이름 (`"1080p"`, `"720p"` 등) |
| **levels[].bitrate** (number) | 해당 화질의 비트레이트(bps 단위)            |
| **levels[].width** (number)   | 비디오의 가로 해상도(px)                    |
| **levels[].height** (number)  | 비디오의 세로 해상도(px)                    |

### 활용

#### 1. 사용자 화질 옵션 UI 동기화

- 플레이어의 화질 옵션을 커스텀 셀렉트 박스에 반영.

```javascript
player.on('levels', (e) => {
  const options = e.levels.map((l, i) => `<option value="${i}">${l.label}</option>`).join('');
  document.querySelector('#qualitySelect').innerHTML = options;
});
```

#### 2. 자동 화질 선택 로직 분석

- Adaptive Bitrate Streaming(HLS/DASH) 환경에서  
  플레이어가 어떤 화질 레벨을 선택할 수 있는지 파악 가능:

```javascript
Adaptive Bitrate Streaming(HLS/DASH) 환경에서
플레이어가 어떤 화질 레벨을 선택할 수 있는지 파악 가능:
```

#### 3. 네트워크 품질 기반 초기 세팅

- 네트워크 상태를 측정하여 초기 화질 자동 선택:

```javascript
player.on('levels', (e) => {
  if (navigator.connection?.downlink < 2) {
    // 저속 네트워크면 480p 이하 선택
    const low = e.levels.findIndex((l) => l.height <= 480);
    if (low >= 0) player.setCurrentQuality(low);
  }
});
```

#### 4. 화질 로딩/비트레이트 로그

- CDN/스트리밍 최적화 및 통계 수집:

```javascript
player.on('levels', (e) => {
  sendAnalytics('quality_levels_loaded', {
    count: e.levels.length,
    available: e.levels.map((l) => l.label),
  });
});
```

### 주의사항

- `levels` 이벤트는 **각 playlist item마다 한 번만 발생**합니다.  
  (영상이 바뀌면 새롭게 트리거)

- **Adaptive Streaming(HLS/DASH)** 가 아닌 단일 MP4 소스인 경우,  
  `levels` 이벤트는 단일 항목(1개만)으로 발생하거나 생략될 수 있습니다.

- `levels` 이벤트 이후 선택된 화질 변경(`setCurrentQuality`)이 일어나면  
  별도의 `levelsChanged` 이벤트가 발생합니다.  
  → 즉, `levels`는 “화질 목록 준비”, `levelsChanged`는 “선택 변경”.

- 일부 iOS Safari 버전에서는 HLS manifest가 단일 해상도만 포함된 경우  
  `levels` 이벤트가 발생하지 않습니다.

- 다국어 자막(track)과 혼동 금지: `levels`는 화질, `audioTracks`는 오디오 트랙, `captionsList`는 자막입니다.

### 특징

- **“플레이어의 화질 옵션이 준비된 시점”을 감지하는 유일한 이벤트.**

- 화질 선택 UI, 네트워크 최적화, CDN 로깅 등에 필수적으로 사용됨.

- `levels` → `levelsChanged`는 **화질 관련 이벤트 흐름의 기본 구조.**

- Adaptive 환경에서는 플레이어가 자동으로 선택하는 **초기 품질 결정 직전**에 호출됩니다.

- OTT/뉴스/교육 플랫폼 등에서 “화질 선택 커스터마이징”을 구현할 때 반드시 후킹해야 하는 이벤트입니다.

<br>
<br>

# .on('levelsChanged')

Fired when the active quality level is changed. Happens in response to e.g. a user clicking an option in the quality menu or a script calling setCurrentQuality.

### 호출시점

- `levelsChanged` 이벤트는 **플레이어의 재생 화질(quality level)이 변경될 때마다** 발생합니다.

- 즉, **사용자 또는 플레이어가 자동으로 화질을 바꿨을 때** 모두 트리거됩니다.

#### 대표적인 트리거 상황

1. 사용자가 화질 메뉴(예: “720p → 1080p”)를 직접 변경했을 때

2. `player.setCurrentQuality(index)` API로 화질 변경 시

3. Adaptive Streaming(HLS/DASH) 환경에서 네트워크 상태에 따라  
   JW Player가 **자동으로 화질을 전환(adaptive bitrate switching)** 했을 때

#### 이벤트 흐름 예시

```
levels (화질 옵션 목록 준비)
→ levelsChanged (선택된 화질 변경)
→ play / buffer / firstFrame

```

### 이벤트 객체 구조 (콜백 파라미터)

```json
{
  "currentQuality": 1,
  "levels": [
    { "label": "1080p", "bitrate": 3500000 },
    { "label": "720p", "bitrate": 2000000 },
    { "label": "480p", "bitrate": 1000000 }
  ],
  "type": "levelsChanged"
}
```

| Value                         | Description                                    |
| :---------------------------- | :--------------------------------------------- |
| **currentQuality** (number)   | 현재 선택된 화질의 인덱스 (`levels` 배열 기준) |
| **levels** (Array<Object>)    | 사용 가능한 화질 레벨 목록                     |
| **levels[].label** (string)   | 해당 화질의 표시명 (`"1080p"`, `"720p"` 등)    |
| **levels[].bitrate** (number) | 비트레이트(bps)                                |

> `levels` 배열은 `levels` 이벤트 때 전달된 구조와 동일하지만,
> `currentQuality` 속성이 추가되어 “현재 선택된 화질”을 알려줍니다.

### 활용

#### 1. UI 동기화 (화질 메뉴 표시 갱신)

- 사용자가 화질을 바꾸면, 현재 선택된 해상도를 실시간 반영.

```javascript
player.on('levelsChanged', (e) => {
  const selected = e.levels[e.currentQuality];
  document.querySelector('#currentQuality').textContent = selected.label;
});
```

#### 2. 자동 화질 전환 감지 (ABR 모니터링)

- 네트워크 상황에 따른 JW Player의 **자동 품질 변경(Adaptive Bitrate)** 감지:  
  네트워크 품질 기반 동적 품질 전환 로깅에 활용.

```javascript
player.on('levelsChanged', (e) => {
  const level = e.levels[e.currentQuality];
  console.log(`자동 화질 변경 → ${level.label} (${Math.round(level.bitrate / 1000)} kbps)`);
});
```

#### 3. 사용자 선택 화질 로그 분석

- 사용자 주도(수동) vs 플레이어 주도(자동) 변경 구분 가능.

```javascript
player.on('levelsChanged', (e) => {
  const l = e.levels[e.currentQuality];
  sendAnalytics('quality_changed', {
    label: l.label,
    bitrate: l.bitrate,
    manual: !player.getConfig().autoQuality,
  });
});
```

#### 4. UI 상태 전환 또는 버퍼 최적화

- 화질 전환 시 잠깐의 버퍼링이 발생하므로 로딩 표시를 추가:

```javascript
player.on('levelsChanged', () => showSpinner(true));
player.on('firstFrame', () => showSpinner(false));
```

### 주의사항

- `levelsChanged`는 **“현재 화질이 실제로 변경된 후”** 에 발생합니다.  
  즉, 선택한 직후가 아니라 JW Player 내부 스트림이 새 세그먼트를 로드한 뒤 호출됩니다.

- **Adaptive (자동 화질 전환)** 시에는 사용자 조작 없이도 여러 번 발생할 수 있습니다.  
  → 이벤트 발생 빈도가 높을 수 있으므로 **로깅 시 샘플링 필요.**

- 동일한 화질을 재선택해도(`setCurrentQuality(1)` → `1` 유지) 이벤트는 발생하지 않습니다.

- 일부 HLS/DASH 스트림의 경우, 초기화 중 내부적으로 화질이 자동 전환되어  
  재생 초반에 여러 번 호출될 수 있습니다.

- iOS Safari는 OS 네이티브 HLS 플레이어를 사용하기 때문에,  
  자동 화질 변경이 내부적으로 일어나도 `levelsChanged`가 **보고되지 않을 수도 있습니다.**

- 단일 화질 소스(MP4 1개)에서는 이벤트가 발생하지 않습니다.

### 특징

- `levels` 이벤트가 “화질 목록 준비 완료”라면,  
  `levelsChanged`는 **“현재 화질이 바뀐 결과 통보”** 에 해당합니다.

- JW Player의 **Adaptive Bitrate(ABR)** 엔진과 직접 연결된 이벤트로,  
  화질 전환 시점을 감지하는 **품질 모니터링 핵심 이벤트**입니다.

- `currentQuality` 인덱스를 통해 `levels[]` 배열 내 선택 상태를 직관적으로 파악할 수 있습니다.

- `qualityLabels` UI 커스터마이징, 사용자 인터랙션 추적,  
  네트워크 적응성(ABR) 분석 등에서 필수적으로 활용됩니다.

<br>
<br>

# .on('visualQuality')

Fires when the active quality level is changed for HLS

This is different than [levelsChanged](https://docs.jwplayer.com/players/reference/quality-events#onlevelschanged) since this will trigger when adaptive streaming automatically shifts quality.

### 호출시점

- `visualQuality` 이벤트는 **플레이어의 실제 비디오 렌더링 품질(Visual Quality)** 이 **실시간으로 변경될 때** 발생합니다.

- 즉, **화질 수준(levels)** 뿐만 아니라 **비트레이트, 해상도, 프레임레이트, 렌더링 모드(DRM/HLS/DASH 등)** 이 변할 때마다 트리거됩니다.

#### 발생 타이밍 요약

1. 재생 시작 직후 (첫 프레임이 렌더링될 때)

2. **Adaptive Bitrate (ABR)** 스트리밍 중, 플레이어가 더 높거나 낮은 화질로 전환될 때

3. 사용자가 직접 화질(level)을 변경했을 때 (`setCurrentQuality()`)

4. 네트워크 품질 변화, 버퍼 부족 등으로 JWPlayer 내부 **렌더링 파이프라인이 다른 스트림으로 교체될 때**

#### 일반적인 이벤트 흐름

```
levels (사용 가능한 화질 목록 준비)
→ levelsChanged (선택된 화질 변경)
→ visualQuality (실제 렌더링 품질이 적용됨)
```

> 즉, `levelsChanged`가 “논리적 선택 변경”이라면  
> `visualQuality`는 “실제 재생 중 품질이 적용된 시점”입니다.

### 이벤트 객체 구조 (콜백 파라미터)

```json
{
  "reason": "auto",
  "mode": "dynamic",
  "level": {
    "bitrate": 3500000,
    "width": 1920,
    "height": 1080,
    "label": "1080p"
  },
  "type": "visualQuality"
}
```

| Value                                 | Description                                                    |
| :------------------------------------ | :------------------------------------------------------------- |
| **reason** (string)                   | 화질 변경 이유 (`"auto"`, `"manual"`, `"initial"`)             |
| **mode** (string)                     | 스트리밍 모드 (`"dynamic"` = Adaptive, `"static"` = 고정 화질) |
| **level** (object)                    | 현재 적용된 화질 레벨 정보                                     |
| **level.label** (string)              | 표시용 해상도명 (`"1080p"`, `"720p"`)                          |
| **level.bitrate** (number)            | 비트레이트(bps)                                                |
| **level.width** / **height** (number) | 실제 렌더링 해상도(px)                                         |

### 활용

#### 1. 실제 재생 중 화질 상태 모니터링

- 사용자에게 현재 화질 상태를 표시하거나 품질 HUD 구성.

```javascript
player.on('visualQuality', (e) => {
  const { label, bitrate } = e.level;
  console.log(`현재 시각적 품질: ${label} (${Math.round(bitrate / 1000)} kbps)`);
});
```

#### 2. Adaptive Bitrate 동작 감시 (ABR 로깅)

- 플레이어의 ABR 동작을 추적해 네트워크 최적화 분석 가능.

```javascript
player.on('visualQuality', (e) => {
  sendAnalytics('visual_quality_change', {
    label: e.level.label,
    bitrate: e.level.bitrate,
    mode: e.mode,
    reason: e.reason,
  });
});
```

#### 3. 품질 변화에 따른 UI/UX 동기화

- 화질 전환 시 일시적으로 로딩 스피너 표시:

```javascript
player.on('visualQuality', () => showSpinner(true));
player.once('firstFrame', () => showSpinner(false));
```

- 고해상도 전환 시 UI 강조 효과:

```javascript
player.on('visualQuality', (e) => {
  if (e.level.height >= 1080) highlightHDIcon(true);
  else highlightHDIcon(false);
});
```

#### 4. 품질 전환 사유(reason) 기반 정책 제어

- ABR과 수동 변경을 구분하여 로그/UX 분리.

```javascript
player.on('visualQuality', (e) => {
  if (e.reason === 'auto') console.log('네트워크 기반 자동 화질 변경');
  if (e.reason === 'manual') console.log('사용자 직접 변경');
});
```

### 주의사항

- `visualQuality`는 **실제 렌더링 품질이 적용된 후** 호출됩니다.  
  즉, `levelsChanged`보다 늦게 발생합니다.

- 같은 화질이라도 비트레이트나 인코딩 세그먼트가 바뀌면 이벤트가 재차 호출될 수 있습니다.  
  → **Adaptive 모드에서는 호출 빈도가 높을 수 있음** (1~2분 간격으로 자주 바뀜)

- `reason: "auto"` 인 경우는 ABR 엔진에 의해 자동 전환된 것이며,  
  네트워크나 버퍼 상황에 따라 매우 빈번하게 바뀔 수 있습니다.

- **MP4 단일 화질 파일**을 사용하는 경우에는 발생하지 않습니다.

- **iOS Safari (HLS 네이티브 재생)** 에서는  
  `visualQuality` 이벤트가 발생하지 않거나,  
  불완전한 정보(`label`, `bitrate` 없음)가 전달될 수 있습니다.

- `visualQuality`는 시각적 품질 전환만을 의미하며,  
  오디오 트랙 변경(`audioTrackChanged`)과는 무관합니다.

### 특징

- **JW Player의 실제 “렌더링 품질” 변화를 감지하는 가장 세밀한 이벤트.**

- `levelsChanged`가 “선택된 품질”, `visualQuality`는 “적용된 품질”을 의미.

- Adaptive Bitrate Streaming(HLS, DASH) 환경에서  
  플레이어의 **ABR 알고리즘 동작**을 실시간으로 모니터링 가능.

- OTT, 뉴스, 교육 플랫폼에서 **“실제 시청 화질” 기반 품질 리포트**를 만들 때 필수.

- 네트워크 품질, 사용자 설정, 장치 성능(예: GPU 디코딩 한계)까지 모두 반영된 결과치라  
  **“실제 사용자 시청 품질(Experience Quality)”** 지표로 활용됩니다.
