# Setup Options

These are the options for configuring the layout and playback behavior of a player. Each is placed directly into the setup of the player.

<br>
<br>

## Appearance

- **aspectratio** (string)

  - 너비가 % 등 가변일 때 높이를 비율로 유지하도록 `"가로:세로"` 형식의 문자열로 지정합니다.
  - 플레이어가 고정 크기인 경우 이 값은 사용되지 않습니다.
  - (예) "16:9"

- **controls** (boolean)

  - 플레이어의 제어바(재생, 일시정지, 볼륨 등)를 표시할지 여부
  - Default: `true`

- **displaydescription** (boolean)

  - 미디어 파일의 설명 제목을 표시할지 여부를 구성합니다.
  - Default: `true`

- **displayHeading** (boolean) <sup>8.6.0+</sup>

  - (아웃스트림 전용) 아웃스트림 플레이어 위에 헤딩이 표시되는 경우 컨트롤
  - 가능한 값:
    - `false` (Default): 제목이 표시되지 않습니다
    - `true`: 제목은 기본 텍스트인 광고와 함께 표시됩니다
  - [intl.{lang}.displayheading](https://docs.jwplayer.com/players/reference/internationalization)을 사용하여 텍스트를 사용자 지정하거나 위치 지정합니다.

- **displayPlaybackLabel** (boolean) <sup>8.6.0+</sup>

  - 플레이어 유휴 화면의 재생 버튼 아래에서 실행 호출 텍스트를 활성화합니다
  - true로 설정하면 시청자가 재생 버튼을 클릭하여 동영상을 시청하는 횟수가 최대 5%까지 증가할 수 있습니다.
  - 기본 실행 호출 텍스트는 재생입니다. 이 메시지를 시청자를 위해 현지화할 수도 있습니다.
  - Default: `false`

- **displaytitle** (boolean)

  - 미디어의 설명(description) 텍스트를 표시할지 여부
  - Default: `true`

- **height** (number)

  - 플레이어의 높이를 픽셀 단위로 지정합니다.
  - `aspectratio`를 사용하는 경우 생략 가능성이 높습니다.
  - Default: `360`

- **horizontalVolumeSlider** (boolean) <sup>8.18.4+</sup>

  - 볼륨 슬라이더를 컨트롤바 내부에서 **가로형 슬라이더**로 표시할지 여부
  - 플레이어가 오디오 모드에 있으면 항상 수평 슬라이더가 표시됩니다.
  - 기본적으로 볼륨 슬라이더는 컨트롤 바 위에 세로 슬라이더로 표시됩니다.
  - Default: `false`

- **livePause** (boolean) <sup>8.38.1+</sup>

  - 라이브 스트림 재생 중 “일시정지” 기능을 제공할지 여부.
  - 일시정지 상태에서도 세그먼트 로드는 계속될 수 있음.
  - Default: `false`

- **nextUpDisplay** (boolean)

  - 다음 재생할 항목(Next Up) 카드 형태 미리보기 표시 여부.

- **qualityLabels** (array)

  - (HLS/DASH 등) 기본 화질 레벨 대신 사용자 정의 라벨을 지정할 수 있는 객체 값
  - 예) `{ "2500":"High", "1000":"Medium" }`

- **renderCaptionsNatively** (boolean) <sup>8.0.1+</sup>

  - 자막 렌더링을 브라우저 기본 렌더러(native)로 할지, 또는 플레이어 내장 렌더러로 할지 지정합니다.
  - 가능한 값
    - `true` (사파리 기본 값): 캡션은 브라우저의 렌더러를 사용하여 렌더링합니다.
    - `false` (크롬 기본 값): 캡션은 플레이어의 렌더러를 사용하여 렌더링됩니다.
  - See also: [captions](https://docs.jwplayer.com/players/reference/captions-config-ref)

- **stretching** (string)

  - 영상이나 이미지가 플레이어 영역에 맞춰 어떻게 확대·축소 또는 채워질지를 지정하는 문자열 값
  - 가능한 값:
    - `uniform`(Default) : 가로 세로 비율을 유지하면서 플레이어의 크기에 맞춥니다.
    - `fill`: 가로 세로 비율을 유지하지 않고 플레이어의 크기에 맞춥니다.
    - `exactfit`: 화면 비율을 유지하면서 크기를 채우기 위해 비디오를 확대하고 자릅니다.
    - `none`: 검은색 테두리가 있는 비디오 파일의 실제 크기를 표시합니다.

- **width** (number \| string)

  - 플레이어의 너비를 픽셀(px) 또는 백분율(%)로 지정합니다.
  - Default: `640`

<br><br>

## Behavior

- **aboutlink** (string)

  - 오른쪽 클릭 메뉴를 클릭할 때 연결할 사용자 지정 URL
  - Default: `"https://www.jwplayer.com/learn-more"`

- **abouttext** (string)

  - 오른쪽 클릭 메뉴에 표시할 사용자 지정 텍스트

- **allowFullscreen** (boolean) <sup>8.22.0+</sup>

  - false로 설정하면 탭, 클릭, 바로가기 키, API 액세스를 포함한 플레이어의 전체 화면 기능을 비활성화합니다.
  - 다시 활성화하려면 `setAllowFullscreen(allowFullscreen)` 을 사용합니다.
  - Default: `true`

- **autostart** (boolean \| string)

  - 페이지가 로드될 때 플레이어가 자동으로 재생을 시작할지 여부를 결정합니다.
  - 가능한 값
    - `false` (Default): 자동 시작 비활성화
    - `true`: 자동 시작 활성화
    - `viewable`: 플레이어의 50%가 보여질 때 자동 시작

- **defaultBandwidthEstimate** (number) <sup>8.3.0+</sup>

  - 설정되면, 초기 대역폭 추정치(초당 비트 수)를 제안합니다. 이는 사이트에 새로 온 시청자를 위한 플레이어의 기본 대역폭 추정치보다 우선합니다
  - 사용 사례: 사이트에 새로 접속하는 사용자는 콘텐츠가 전달되는 연결 속도에 따라 품질이 조정되기 전에 몇 초 동안 저화질 동영상을 경험할 수 있습니다. 이 속도는 연결 속도에 따라 제한될 수 있습니다. 이 경우 대부분의 시청자가 빠른 연결을 가지고 있는지 확인해야 합니다.
  - > 이 속성은 비디오 품질을 위해 플레이어 성능을 희생시키므로 이 속성을 설정하지 않는 것이 좋습니다. 플레이어는 몇 초 후에 최적의 대역폭을 선택하도록 최적화되어 있습니다.

- **fullscreenOrientationLock** (string) <sup>8.28.0+</sup>

  - (안드로이드) 뷰어가 모바일 장치를 회전할 때 모바일 웹 보기에서 플레이어가 회전할 수 있도록 합니다.
  - 가능한 값
    - `landscape` (Default): 장치를 180도 º 회전 시 재조정하여 가로 방향으로 고정합니다
    - `none`: 장치의 위치에 따라 전체 화면에 방향을 조정합니다
    - `portrait`: 세로 방향으로 고정하고 장치를 180도 º 회전 시 재조정합니다

- **generateSEOMetadata** (boolean) <sup>8.26.1+</sup>

  - Google SEO 최적화 지원
  - Default: `false`

- **liveSyncDuration** (number) <sup>8.12.0+</sup>

  - 다음 시나리오에서 라이브 에지로부터의 거리를 초 단위로 정의합니다:
    - 플레이어가 라이브 또는 DVR 스트림을 시작하려고 시도합니다.
    - 사용자가 라이브 버튼을 클릭하면 스트림의 라이브 콘텐츠로 돌아갑니다.
  - 이 속성은 5-30 사이의 모든 값을 허용할 수 있습니다.
  - 플레이어가 스트림을 기반으로 목표 지연 시간을 결정하려면 8.19.0+를 공백으로 두십시오.
  - Default: 25 <8.19.0

- **mute** (boolean)

  - 재생 중에 플레이어를 음소거해야 하는지 여부를 정의합니다
  - 사용자가 이 속성을 재정의하면 사용자의 작업은 사용자 세션 동안 지속됩니다.  
    예를 들어, 모든 플레이어가 음소거: true로 구성되어 있고 사용자가 플레이어를 음소거 해제하면  
    사용자가 만나는 각 후속 플레이어는 음소거 해제 상태로 시작됩니다.
  - 브라우저 정책상 자동재생 시 음소거가 강제되는 경우가 많습니다.
  - Default: `false`

- **nextupoffset** (number \| string) <sup>8.9.0+</sup>

  - 재생 중에 Next Up 카드가 표시되는 시기를 구성합니다.
  - 양수 값은 동영상 시작 시점의 오프셋입니다.
  - 음수 값은 동영상 끝 시점의 오프셋입니다.
  - 이 속성은 숫자(-10) 또는 문자열("-2%")로 백분율로 정의할 수 있습니다
  - Default: `-10`

- **pipIcon** (string) <sup>8.21.0+</sup>

  - pip mode를 제어합니다.
  - 가능한 값:
    - `enabled` (Default): 지원되는 데스크톱 브라우저에만 그림 아이콘을 추가하고 마우스 오른쪽 버튼 메뉴 옵션을 클릭합니다.
    - `disabled`: 아이콘을 제거합니다
  - 아이콘은 CSS로도 숨길 수 있습니다.

- **playbackRateControls** (boolean)

  - 재생 속도를 조정하기 위한 설정 메뉴 표시 여부
  - 참일 경우 메뉴는 0.5배, 1배, 1.25배, 2배의 미리 정의된 옵션으로 표시됩니다.
  - 배열을 전달하여 재생 속도를 사용하여 메뉴 옵션을 사용자 지정할 수 있습니다.
  - > 이 기능은 현재 HLS 스트림이 있는 안드로이드에서는 지원되지 않습니다.

- **playbackRates** (array)

  - 설정 메뉴에 표시할 사용자 지정 재생률 옵션
  - Default: `[0.25, 0.75, 1, 1.25]`

- **playlist** (array \| string)

  - 플레이어에서 플레이 할 미디어
  - See: [Playlists](https://docs.jwplayer.com/players/reference/playlists)

- **playlistIndex** (number) <sup>8.15.0+</sup>

  - 재생을 시작하기 위한 재생 목록 내 항목의 인덱스
  - 재생목록에서 첫 번째 인덱스는 0입니다.
  - playlistIndex 값이 음수이면 재생목록이 끝날 때부터 인덱스가 계산되기 시작합니다.
  - 음수 값은 재생목록의 절대 수를 초과할 수 없습니다.  
    예를 들어. 5개의 항목의 재생목록에서 `playlistIndex: -5` 는 허용 가능한 음수 중 가장 낮은 수입니다.
  - 이 속성은 JSON, RSS 및 배열 재생 목록에 적용됩니다.

- **repeat** (boolean)

  - 재생 목록이 완료된 후 플레이어가 콘텐츠를 반복하는지 여부를 결정합니다
  - Default: `false`

<br><br>

## Rendering and Loading

- **base** (string)

  - 스킨 및 제공자를 위한 대체 기본 경로를 구성합니다.
  - Default: `/`

- **flashplayer** (string) <sup>< 8.19.0</sup>

  - As per Adobe, Flash Player is no longer supported.
  - jwplayer.flash.swf의 대체 디렉토리를 지정합니다.
  - Default: `/`

- **hlsjsdefault** (boolean)

  - 안드로이드용 크롬을 사용할 때 HLSjs를 기본 제공자로 만듭니다
  - 브라우저의 기본 제공자를 사용하려면 이 속성을 비활성화하세요.
  - Default: `true`

- **liveTimeout** (number) <sup>8.1.9+</sup>

  - 정지된 라이브 매니페스트 처리 방법을 구성합니다
  - 이 속성은 초 단위로 양수를 허용하지만 1~10 사이의 값은 무시됩니다. 스트림을 시간 초과하지 않도록 구성하려면 이 속성을 0으로 설정합니다.
  - 플레이어는 타임아웃될 때까지 매니페스트를 계속 요청합니다.  
    라이브 매니페스트가 라이브타임아웃보다 더 오래 요청된 후에도 업데이트되지 않으면 스트림은 오류 이벤트로 종료됩니다.  
    스트림을 즉시 종료하려면 매니페스트에 종료 태그를 포함하세요.
  - 이 구성 옵션은 세그먼트 로드 문제 없이 정지된 매니페스트만 처리합니다.  
    예를 들어 404가 되는 청크는 여전히 오류가 발생합니다.
  - **Default**
    - liveTimeout 은 매니페스트에 정의된 targetDuration 값의 3배(3배)에 해당하는 값으로 기본 설정됩니다.
    - > targetDuration 에 대한 자세한 내용은 [HLS 사양](https://datatracker.ietf.org/doc/html/draft-pantos-hls-rfc8216bis#section-4.4.3.1)을 참조하세요.

- **loadAndParseHlsMetadata** (boolean)

  - Safari에서 HLS 재생을 활성화하여 Chrome에서 HLS 재생 중에 트리거되는 동일한 메타데이터 이벤트를 트리거하려면,  
    레이어가 시간 메타데이터를 삽입하는 데 사용할 수 있는 PROGRAM-DATE-TIME 태그가 포함된 경우 HLS 재생 목록에 추가 요청을 하게 됩니다.
  - 가능한 값:
    - `true` (Default): 플레이어는 Safari에서 로드할 때 재생 목록 매니페스트를 로드하고 파싱하며 결과를 미디어 재생과 동기화합니다.  
      이렇게 하면 여러 매니페스트 요청이 발생할 수도 있습니다.
    - `false` : 플레이어가 라이브 HLS 매니페스트를 사이드로드하여  
      DATERANGE, CUE-OUT, CUE-IN, DISUTIONITY 태그와 같이 Safari가 노출하지 않는 대역 외 매니페스트 메타데이터를 분석하는 것을 방지합니다.

- **preload** (string)

  - 재생하기 전에 콘텐츠를 로드해야 하는지 플레이어에게 알려줍니다
  - 이것은 더 빠른 재생 속도나 특정 메타데이터를 재생하기 전에 로드해야 하는 경우에 유용합니다.
  - 가능한 값:
    - `metadata` (Default): 매니페스트를 로드하고 HLS 및 대시 스트림에 대해 최대 한 세그먼트의 미디어를 버퍼링합니다
    - `auto`: 매니페스트를 로드하고 약 30초 분량의 미디어 세그먼트를 버퍼링합니다
    - `none`: 콘텐츠가 미리 로드되지 않았습니다. 과도한 콘텐츠 사용이 우려되는 경우 이 옵션을 사용하는 것이 좋습니다.

- **showUIWhen** (string) <sup>8.32.0+</sup>

  - 플레이어에게 플레이어 UI를 표시할 시기를 알려줍니다
  - 가능한 값:
    - `onReady` (Default): 플레이어가 로드될 때 UI 표시
    - `onContent`: 광고 또는 비디오 콘텐츠가 재생될 준비가 될 때까지 플레이어 UI 숨기기
