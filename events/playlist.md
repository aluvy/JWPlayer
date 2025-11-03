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

- **완전히 새로운 플레이리스트가 플레이어에 로드되었을 때** 1회 발생합니다.

- 초기 `setup()` **시 로드된 첫 플레이리스트에 대해서는 호출되지 않습니다.** 초기에는 ready를 사용하세요.

### 이벤트 객체 구조 (콜백 파라미터)

```json
{
  "playlist": [
    /* getPlaylist()와 동일한 배열 */
  ]
}
```

| Value                              | Description                                                                                       |
| :--------------------------------- | :------------------------------------------------------------------------------------------------ |
| **playlist** (Array<PlaylistItem>) | 방금 로드된 새 플레이리스트(각 아이템은 `title`, `image`, `sources`, `tracks` 등 표준 필드 포함). |

### 활용

#### 1. UI/메타데이터 갱신

- 새 플레이리스트 카드/썸네일 영역을 재그리기:

```javascript
player.on('playlist', (e) => {
  renderPlaylist(e.playlist); // 커스텀 목록 구성
});
```

#### 2. 프리패치/썸네일 선로딩

- 다음 아이템의 포스터·스프라이트를 미리 불러 안정성 향상.

#### 3. 분석/로깅

- 플레이리스트 교체 시점(콘텐츠 세트 변경)을 이벤트로 기록해 추천 전환률이나 세션 흐름을 분석.

### 주의사항

- **초기 로드에는 트리거되지 않음** → 초기 세팅 로직은 `ready`에서 처리하고, 그 이후 동적 교체(`load()` 등) 시에만 `playlist`를 후킹.

- `playlistItem`과 혼동 금지:

  - `playlist` = **목록 자체가 바뀜**

  - `playlistItem` = **재생 중인 인덱스가 바뀜**(아이템 전환)

광고/관련(related)·추천 로직으로 목록을 교체할 때 비동기 지연이 있을 수 있으므로, UI 갱신은 **이 이벤트 이후**에 안전하게 수행.

### 특징

- **목록 교체 감지 전용**으로, `getPlaylist()`와 동일한 구조를 즉시 제공해 **한 번에 전체 UI 재구성**이 가능합니다.

- `nextClick`/`playlistItem` 같은 네비게이션 이벤트와 달리, **콘텐츠 세트 수준 변화**를 포착합니다.

<br>
<br>

# .on('playlistItem')

Fired when the playlist index changes to a new playlist item. This event occurs before the player begins playing the new playlist item.

### 호출시점

- `playlistItem` 이벤트는 **플레이리스트 내에서 현재 재생 항목이 바뀌는 순간** 호출됩니다.

- 즉, 플레이어가 **새로운 아이템(playlist index)** 을 로드하기 시작할 때 트리거됩니다.

#### 대표적 발생 상황

1. 사용자가 “다음/이전” 버튼 클릭 (`nextClick`, `prevClick`)

2. `autoplay` 에 의해 다음 영상 자동 재생

3. `player.load()` 또는 `player.playlistItem(index)` 로 프로그래밍 변경

4. `relatedPlay` (추천 영상 클릭) 으로 새 항목 재생 시작

5. `relatedClose` → 다음 영상 전환 시점

#### 일반적인 이벤트 흐름

```
complete (이전 영상 종료)
→ playlistItem (새 영상 로드 시작)
→ levels (화질 옵션 파싱)
→ play / firstFrame (재생 시작)
```

> 즉, **플레이어가 다음 미디어 소스를 로드하기 시작한 직후** 발생하는 이벤트입니다.

### 이벤트 객체 구조 (콜백 파라미터)

```json
{
  "index": 2,
  "item": {
    "mediaid": "abc123",
    "title": "새 영상 제목",
    "image": "https://example.com/thumb.jpg",
    "file": "https://cdn.example.com/video.mp4"
  },
  "type": "playlistItem"
}
```

| Value                     | Description                             |
| :------------------------ | :-------------------------------------- |
| **index** (number)        | 새로 재생될 아이템의 인덱스(0부터 시작) |
| **item** (object)         | 해당 플레이리스트 아이템의 전체 정보    |
| **item.title** (string)   | 콘텐츠 제목                             |
| **item.image** (string)   | 썸네일 이미지 URL                       |
| **item.file** (string)    | 실제 미디어 파일 URL                    |
| **item.mediaid** (string) | JW Player playlist item ID              |

> `item` 구조는 `player.getPlaylistItem()` 결과와 동일합니다.

### 활용

#### 1. 현재 재생 콘텐츠 UI 갱신

- 플레이어 외부의 제목, 썸네일, 설명 영역을 실시간 업데이트.

```javascript
player.on('playlistItem', (e) => {
  document.querySelector('.video-title').textContent = e.item.title;
  document.querySelector('.video-thumb').src = e.item.image;
});
```

#### 2. 재생 로그 / 분석 트래킹

- → 사용자가 어떤 순서로 영상을 재생하는지,  
  또는 자동재생/수동재생 전환을 분석할 수 있음.

```javascript
player.on('playlistItem', (e) => {
  sendAnalytics('playlist_item_start', {
    index: e.index,
    title: e.item.title,
    mediaid: e.item.mediaid,
  });
});
```

#### 3. 자동재생 UX 제어

- 새 항목 재생 시 배너나 안내 메시지 표시:

```javascript
player.on('playlistItem', (e) => {
  showToast(`다음 영상: ${e.item.title}`);
});
```

#### 4. 광고 or 오버레이 초기화

- 새 영상이 시작될 때 광고 오버레이, 챕터, 자막 등을 리셋:

```javascript
player.on('playlistItem', () => {
  resetOverlay();
  resetCaptionState();
});
```

#### 5. 맞춤 자동재생 정책

- 특정 조건일 때 다음 영상 자동 차단:

```javascript
player.on('playlistItem', (e) => {
  if (e.index >= 5) player.pause();
});
```

### 주의사항

- **이전 영상이 완전히 종료되기 전**(`complete` 직후)  
  **다음 아이템이 로드되는 시점**에서 호출됩니다.  
  따라서 오버레이나 타이머 초기화는 반드시 이 이벤트 이후에 처리해야 함.

- `playlistItem` 이벤트는 **현재 인덱스 기준으로만 발생**합니다.  
  같은 아이템을 다시 재생(`playlistItem(currentIndex)`)하면 이벤트가 재호출되지 않습니다.

- `playlist` 자체가 교체될 경우(`load()` 호출 시)  
  새 리스트 로드 후 **첫 번째 아이템 로드 시점에만** 다시 발생합니다.

- 광고(IMA, FreeWheel) 삽입 시에는 **광고 슬롯 이후** 본편이 시작될 때  
  다시 `playlistItem` 이 발생하므로, 본편 전용 초기화 로직으로 활용하기 좋습니다.

- iOS Safari에서 관련 오버레이를 전환하는 경우  
  `playlistItem` 이후 `ready` 이벤트가 **지연 호출될 수 있음** — 타이밍 동기화 유의.

### 특징

- **JW Player의 콘텐츠 전환을 감지하는 핵심 이벤트.**

- `playlist` 이벤트가 “목록 교체”라면,  
  `playlistItem`은 “현재 재생 항목 교체”에 해당.

- OTT, 뉴스, 교육 플랫폼 등에서  
  **시청 세션 구간(Log segmentation)** 추적의 기준 이벤트로 가장 많이 사용됨.

- 다른 이벤트(`time`, `levels`, `relatedPlay`, `nextClick`)의 **상위 트리거 지점**으로도 자주 활용됨.

- `item` 객체는 재생 관련 메타데이터 접근의 기준이 되므로,  
  API 기반 콘텐츠 로드 시 필수적으로 참조해야 함.

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
