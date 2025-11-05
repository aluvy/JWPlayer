# Related

이 객체는 재생 목록의 모양과 동작을 제어합니다.

- **autoplaymessage** (string) <sup>< 8.6.0</sup>

  - 자동 재생 중에 나타나는 사용자 지정 메시지
  - 매크로:
    - xx는 카운트다운 타이머로 대체됩니다.
    - \_\__title_\_는 관련 피드의 다음 제목으로 대체됩니다.
  - Default: `__title__는 xx초 후에 재생됩니다`
  - 경고: JWP 8.6.0부터 [intl 객체를 사용하여 이 속성을 설정하십시오.](https://docs.jwplayer.com/players/reference/internationalization#intllangrelated-object)

- **autoplaytimer** (number)

  - 콘텐츠 목록에서 다음 관련 동영상을 재생하기까지 기다리는 시간(초)
  - 다음 관련 콘텐츠를 즉시 재생하려면 0으로 설정하세요.
  - Default: `10`

- **displayMode** (string) <sup>8.1.9+</sup>

  - 재생 목록 사용자 인터페이스를 구성합니다
  - 가능한 값

    - `overlay`: (수동 및 동적 재생 목록의 기본값) 컨트롤 바에 "더 많은 동영상" 아이콘을 추가합니다.  
      클릭하면 오버레이가 플레이어를 차지하고 재생이 일시 중지됩니다.

    - `shelf`: (추천 사항의 기본값) 재생 경험 중에 시청자가 추천 동영상을 탐색할 수 있도록 제어 막대 위에 썸네일의 가로 막대를 추가합니다  
      선반은 컨트롤 바 위에 표시되는 "더 많은 동영상" 버튼으로 축소할 수 있습니다. 크기 제약으로 인해 작은 플레이어는 다시 "오버레이" 모드로 돌아갑니다.

    - `none`<sup>8.9.0+</sup>: 컨트롤 바의 다음 위쪽 버튼을 제외하고 재생 목록 인터페이스의 모든 측면을 제거합니다.  
      사용자 지정 외부 재생 목록 사용자 인터페이스를 사용할 때 이 값을 설정합니다.

    - `shelfWidget`<sup>8.5.0+</sup>: 플레이어 외부와 아래에 지속적인 썸네일 가로 막대를 추가하여 재생 중에 추천 동영상을 볼 수 있도록 합니다.  
      [셀렉터](https://docs.jwplayer.com/players/reference/related-config-ref#relatedselector)를 사용하여 선반 위치를 구성합니다.

- **file** (string)

  - 추천 재생 목록이 포함된 JSON 또는 RSS 파일의 위치
  - 이 속성을 사용하여 추천 재생 목록을 플레이어의 모든 재생 목록과 연결합니다. 추천 재생 목록을 특정 재생 목록 또는 재생 목록 항목과 연결하려면 [playlist[].recommendations](https://docs.jwplayer.com/players/reference/related-config-ref#playlist-recommendations)를 사용합니다.

- **onclick** (string)

  - 관련 동영상이 선택될 때의 동작
  - 가능한 값:
    - `link` (Default): 관련 항목의 링크 필드에 지정된 URL로 페이지를 리디렉션합니다
    - `play`: 현재 플레이어 내에서 다음 비디오를 재생합니다  
      `oncomplete` 이 `autoplay` 로 설정되면, `onclick` 시 항상 값이 재생됩니다.

- **oncomplete** (string)

  - 단일 비디오 또는 재생 목록이 완료될 때 관련 비디오 오버레이의 동작

  - 가능한 값:

    - `none`<sup>8.9.0+</sup>: (Default) 오버레이가 나타나지 않고 플레이어가 자동으로 다음 재생 목록 항목으로 이동합니다.  
      진행할 미디어 항목이 없으면 재생 버튼이 나타납니다.

    - `show `<sup>< 8.9.0</sup>: (Default, 8.9.0+ 제외) 관련 오버레이 표시

    - `autoplay`: 관련 오버레이를 표시하고 10초 후에 관련 피드의 다음 동영상을 자동으로 재생합니다.  
      이 옵션은 또한 클릭 동작을 자동으로 설정하여 재생합니다.

    - `hide`: 재생 버튼과 관련 아이콘이 나타납니다

- **selector** (string) <sup>8.6.0+</sup>

  - displayMode가 [shelfWidget](https://docs.jwplayer.com/players/reference/related-config-ref#shelfwidget)으로 설정될 때 컨테이너로 사용되는 HTML 요소를 가리키는 CSS 선택기
  - 이 속성은 다음과 같은 방식으로 구성할 수 있습니다:
    - **정의되지 않은 HTML 요소 및 선택기** ID="{playerID}-shelf-widget" 요소가 생성됩니다. 기본적으로 선반 위젯은 플레이어 바로 아래에 <div id="{playerID}-shelf-widget">로 표시됩니다. 선반 위젯 크기는 플레이어에 따라 달라집니다.  
      이 ID를 페이지의 다른 HTML 요소에 할당할 수도 있습니다. 이렇게 하면 새 선택기를 정의하지 않고도 위젯 위치를 설정할 수 있습니다. 이 ID를 다른 HTML 요소에 할당하면 선반 위젯 크기가 HTML 요소에 반응합니다.
    - **정의된 HTML 요소 및 선택기**: HTML 요소에 ID(myDefinedID)와 "selector"가 있는 경우, "#myDefinedID"는 HTML 요소 내부에 ID="myDefinedId"로 배치됩니다. 선반 위젯 크기는 HTML 요소에 반응합니다.
