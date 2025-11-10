# Related

이 객체는 **추천(related) 플레이리스트의 표시 방식과 동작(behavior)** 을 제어합니다.

<br>

- **autoplaymessage** (string) <sup>< 8.6.0</sup>

  - 자동 재생(autoplay) 시 표시되는 **사용자 지정 메시지**를 설정합니다.
  - **매크로:**
    - `xx` → 카운트다운 타이머로 대체됩니다.
    - `__title__` → 다음 재생 예정 영상의 제목으로 대체됩니다.
  - 기본값: `__title__ will play in xx seconds`
  - 주의: JWP 8.6.0 이상에서는 이 속성을 설정할 때 `intl` **객체**를 사용해야 합니다.

- **autoplaytimer** (number)

  - 다음 관련 영상을 재생하기 전 대기할 **초 단위 시간**을 지정합니다.
  - 0으로 설정하면 다음 영상이 **즉시 재생**됩니다.
  - 기본값: `10`

- **displayMode** (string) <sup>8.1.9+</sup>

  - **추천 플레이리스트 UI 표시 방식**을 설정합니다.
  - 가능한 값:
    - `overlay`: (수동 및 동적 플레이리스트의 기본값)  
      컨트롤 바에 “더 많은 동영상” 아이콘을 추가합니다.  
      아이콘 클릭 시 오버레이가 플레이어를 덮으며 재생이 일시정지됩니다.
    - `shelf`: (추천(Recommendations)의 기본값)  
      컨트롤 바 위에 썸네일이 나열된 가로형 썸네일 바를 추가하여 시청자가 재생 중에도 추천 영상을 탐색할 수 있습니다.  
      이 썸네일 바는 접을 수 있으며, 접히면 “More Videos” 버튼이 컨트롤 바 위에 표시됩니다.  
      플레이어 크기가 작을 경우 자동으로 overlay 모드로 전환됩니다.
    - `none` (8.9.0+): 컨트롤 바의 다음 영상 버튼(next up) 을 제외하고 모든 관련 UI를 제거합니다.  
      외부에서 구현한 사용자 정의 플레이리스트 UI를 사용할 때 이 값을 설정합니다.
    - `shelfWidget` (8.5.0+): 플레이어 아래쪽, 외부 영역에 지속적으로 표시되는 썸네일 바를 추가합니다.  
      시청자가 재생 중에도 추천 영상을 탐색할 수 있습니다.  
      `selector` 속성을 사용해 위치를 지정할 수 있습니다.  
      [셀렉터](https://docs.jwplayer.com/players/reference/related-config-ref#relatedselector)

- **file** (string)

  - 추천(recommendations) 플레이리스트가 포함된 **JSON 또는 RSS 파일의 경로(URL)** 를 지정합니다.
  - 이 속성을 사용하면 모든 플레이리스트에 추천 목록을 연결할 수 있습니다.
  - 특정 플레이리스트 또는 항목에만 추천 목록을 연결하려면 [playlist[].recommendations](https://docs.jwplayer.com/players/reference/related-config-ref#playlist-recommendations) 를 사용하세요.

- **onclick** (string)

  - **관련 영상 클릭 시 동작 방식**을 정의합니다.
  - 가능한 값:
    - `link`: (기본값) 관련 항목의 link 필드에 지정된 **URL로 리디렉션**
    - `play`: 현재 플레이어 내에서 **다음 영상을 바로 재생**
  - `oncomplete` 가 `autoplay` 로 설정된 경우, `onclick` 값은 항상 `play` 로 동작합니다.

- **oncomplete** (string)

  - 단일 영상 또는 플레이리스트 재생이 완료된 후 **추천 오버레이의 동작 방식**을 정의합니다.
  - 가능한 값:
    - `none` (8.9.0+, 기본값): 오버레이를 표시하지 않으며, 다음 플레이리스트 항목으로 자동 전환합니다.  
      만약 다음 항목이 없으면 **다시 재생(Replay)** 버튼이 표시됩니다.
    - `show` (< 8.9.0 기본값): 추천 오버레이를 표시합니다.
    - `autoplay`: 추천 오버레이를 표시한 뒤, 10초 후 관련 피드의 다음 영상을 자동으로 재생합니다.  
      이 옵션을 사용하면 `onclick` 동작이 자동으로 `play` 로 설정됩니다.
    - `hide`: **재생 버튼(Replay)** 과 **추천 아이콘(related icon)** 만 표시됩니다.

- **selector** (string) <sup>8.6.0+</sup>

  - `displayMode` 가 `shelfWidget` 인 경우, 위젯을 삽입할 **HTML 요소를 가리키는 CSS 선택자(selector)** 를 지정합니다.
  - 구성 방법:
    - **정의되지 않은 HTML 요소 및 선택자:**
      - `id="{playerID}-shelf-widget"` 인 요소가 자동으로 생성됩니다.  
        기본적으로 위젯은 `<div id="{playerID}-shelf-widget">` 안에 플레이어 바로 아래에 표시되며,  
        크기는 플레이어에 맞게 반응형으로 조정됩니다.
      - 이 ID를 페이지 내 다른 HTML 요소에 직접 지정할 수도 있습니다.  
        이 경우 새 선택자를 정의하지 않아도 되며, 위젯은 해당 요소의 크기에 맞게 표시됩니다.
    - **정의된 HTML 요소 및 선택자:**
      - HTML 요소가 `id="myDefinedID"` 를 가지고 있고 `"selector": "#myDefinedID"` 로 설정된 경우,  
        추천 위젯은 해당 요소 내부에 삽입됩니다.
      - 위젯의 크기는 HTML 요소의 크기에 따라 반응형으로 조정됩니다.
