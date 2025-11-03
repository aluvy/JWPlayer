# Playlist Events

These API calls are used for loading and retrieving the current playlist (of one or more items), as well as for navigating between playlist items. When accessed via the API, a playlist is an Array, containing one or more objects. Each of these objects contains the following:

| Value       | Description                                                                                  | Type   |
| :---------- | :------------------------------------------------------------------------------------------- | :----- |
| description | A description specified inside of the playlist                                               | String |
| mediaid     | A unique media identifier for a particular piece of content, regardless of the format used   | String |
| file        | Provides a shortcut to sources[0].file. Alternative files are listed in the allSources array | String |
| image       | The poster image file loaded inside of the player                                            | String |
| preload     | Preload status for current item. Can be: metadata \| auto \| none                            | String |
| title       | The title of the playlist item                                                               | String |
| tracks      | The full array of tracks included with the playlist item, similar to getCaptionsList()       | Array  |
| sources     | An array that contains a single object with information about the currently utilized source  | Array  |
| allSources  | An array of all configured sources for the current playlist item                             | Array  |

> Each playlist item has a file property at the highest level, which acts as a shortcut to the file of the first entry in the sources object.

### The `sources` array contains a single object with information about the currently utilized source:

| Value   | Description                                                     | Type    |
| :------ | :-------------------------------------------------------------- | :------ |
| file    | The file used in the source array item                          | String  |
| label   | A label assigned to a particular quality                        | String  |
| type    | The media type                                                  | String  |
| default | If this media source has been defined as a default for playback | Boolean |

### The `tracks` array contains a list of objects based on any configured tracks:

| Value | Description                                                                          | Type   |
| :---- | :----------------------------------------------------------------------------------- | :----- |
| file  | The file used containing any utilized tracks                                         | String |
| label | If using captions, the configured label assigned to a particular quality             | String |
| kind  | The type of tracks assigned to the video. Can be: captions \| chapters \| thumbnails | String |

<br>
<br>

# .on('nextClick')

Fires after the next button (in the control bar) or the next up overlay is clicked.

### 호출시점

- `nextClick` 이벤트는 **사용자가 플레이어 UI의 “다음(next)” 버튼을 클릭했을 때** 발생합니다.

- 즉, 플레이리스트(playlist)가 구성되어 있을 때,  
  사용자가 수동으로 **다음 영상으로 이동하려는 의도(intent)** 를 표현하면 호출됩니다.

#### 발생 조건 요약

1. 플레이어에 2개 이상의 playlist item이 존재해야 함 (`playlist: [ {file:...}, {file:...} ]`)

2. 플레이어 컨트롤 바에 **“다음(next)” 버튼이 표시된 상태**

3. 사용자가 해당 버튼을 클릭 (or 탭)

4. 클릭 직후 `nextClick` 이벤트가 발생 → 그 다음 자동으로 `playlistItem` 이벤트로 전환

#### 일반적인 이벤트 흐름

```
play (현재 영상 재생 중)
→ nextClick (다음 버튼 클릭 감지)
→ playlistItem (다음 영상 로드 시작)
→ play / firstFrame (재생 개시)
```

> 즉, `nextClick`은 **"다음 콘텐츠 이동 요청"** 시점이며,
> 실제 재생은 그 직후 `playlistItem` 이벤트에서 시작됩니다.

### 이벤트 객체 구조 (콜백 파라미터)

```json
{
  "type": "nextClick"
}
```

> 이 이벤트는 단순 사용자 입력 감지를 목적으로 하므로,  
> 별도의 `item`, `index` 등의 속성은 포함하지 않습니다.  
> (다음 항목의 인덱스는 `player.getPlaylistIndex() + 1` 로 계산 가능)

### 활용

#### 1. 사용자 행동(다음 재생 의도) 추적

- 사용자의 “다음 재생 시도” 이벤트를 수집하여 시청 행동 분석에 활용.

```javascript
player.on('nextClick', () => {
  const current = player.getPlaylistIndex();
  const next = current + 1;
  console.log(`사용자가 다음(${next}) 영상을 요청함`);
  sendAnalytics('next_click', { from: current, to: next });
});
```

#### 2. 다음 콘텐츠 미리 로딩

- 사용자가 클릭하자마자 **사전 로드 로직(prefetch)** 실행:

```javascript
player.on('nextClick', () => {
  preloadNextThumbnail(player.getPlaylistIndex() + 1);
});
```

#### 3. UI/UX 효과 제어

- “다음 버튼” 클릭 시 화면 전환 효과, 페이드아웃, 로딩 표시:

```javascript
player.on('nextClick', () => {
  document.body.classList.add('transition-next');
  showLoadingSpinner(true);
});
```

#### 4. 추천/자동재생 로직 통합

`relatedPlay`(추천 클릭)과 동일하게 취급하여 다음 재생 흐름 제어:

```javascript
player.on('nextClick', () => {
  console.log('사용자 수동 다음 재생 요청');
  cancelAutoplayCountdown();
});
```

### 주의사항

- `nextClick`은 **“사용자 수동 조작”에만 발생**합니다.  
  자동으로 다음 콘텐츠로 넘어가는 경우(`autoplay: true`)에는 호출되지 않습니다.  
  → 자동 재생 시에는 `complete` → `playlistItem` 흐름만 발생.

- `nextClick` 직후 JW Player 내부에서 자동으로 `playlistItem` 이벤트가 발생하며,  
  **플레이어가 즉시 다음 영상으로 전환**됩니다.  
  따라서 `nextClick`에서 `preventDefault()` 같은 중단 제어는 불가능합니다.

- **이전(prev) 버튼 클릭**은 별도의 이벤트(`prevClick`)로 구분됩니다.

- iOS Safari 전체화면 모드에서는 **“다음” 버튼이 표시되지 않을 수도** 있어  
  이벤트 자체가 발생하지 않을 수 있습니다.

- next`Click 이벤트는 JW Player **UI 컨트롤을 사용한 경우에만** 발생하며,  
외부 커스텀 버튼(`player.next()`) 호출 시에는 이벤트가 트리거되지 않습니다.

### 특징

- `nextClick`은 **“플레이리스트 네비게이션 중 사용자 입력을 감지하는 유일한 이벤트”** 입니다.

- `playlistItem`이 “재생된 결과(event after change)”라면,  
  `nextClick`은 “사용자 의도(event before change)”에 해당합니다.

- 광고, 자동재생, 추천 알고리즘과 무관하게  
  **순수 사용자 상호작용만 구분**할 수 있다는 점에서 UX·분석 측면에서 매우 중요합니다.

- OTT, 뉴스, 강의 플랫폼 등에서는  
  “사용자가 스스로 다음 영상으로 넘어갔는가?”를 확인하는 지표로 활용됩니다.

<br>
<br>

# .on('playlist')

Fired when an entirely new playlist has been loaded into the player.

> This event is not fired as part of the initial playlist load. Please use on('ready') in these cases.

### 호출시점

### 이벤트 객체 구조 (콜백 파라미터)

```json

```

| Value | Description |
| :---- | :---------- |
|       |             |

### 활용

### 주의사항

### 특징

<br>
<br>

# .on('playlistItem')

Fired when the playlist index changes to a new playlist item. This event occurs before the player begins playing the new playlist item.

### 호출시점

### 이벤트 객체 구조 (콜백 파라미터)

```json

```

| Value | Description |
| :---- | :---------- |
|       |             |

### 활용

### 주의사항

### 특징

<br>
<br>

# .on('playlistComplete')

Fires when the following occurs:

### 호출시점

### 이벤트 객체 구조 (콜백 파라미터)

```json

```

| Value | Description |
| :---- | :---------- |
|       |             |

### 활용

### 주의사항

### 특징
