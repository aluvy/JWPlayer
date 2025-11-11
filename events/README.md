# Events Index

**JWPlayer 이벤트**

이벤트 핸들러 `.on`, `.once`, `.off` 을 사용하여 이벤트를 등록하거나 제거할 수 있으며,  
`.trigger` 를 통해 사용자 정의 이벤트를 발생시킬 수도 있습니다.

- `on`: 하나 또는 여러 개의 이벤트를 선택한 요소에 부착합니다. 이벤트가 발생할 때마다 지정된 함수(핸들러)를 실행합니다.
- `once`: 이벤트가 단 한 번만 발생하도록 등록합니다. 이벤트 핸들러가 한 번 실행된 후에는 자동으로 제거됩니다.
- `off`: 이전에 `on` 또는 `once`를 통해 등록했던 이벤트 핸들러를 제거합니다.
- `trigger`: 마우스 클릭과 같은 사용자의 실제 행동 없이도, 특정 요소에 연결된 이벤트를 프로그래밍 방식으로 강제 발생시킵니다. 또한, 사용자 정의 이벤트를 생성하여 원하는 시점에 실행하는 데에도 사용됩니다.

<br>

## SETUP

- [**ready**](./setup.md#onready)

  - 플레이어가 초기화되어 재생 준비가 완료되었을 때 발생합니다.

- [**remove**](./setup.md#onremove)

  - `jwplayer().remove()`를 통해 플레이어가 페이지에서 제거될 때 트리거됩니다.

- [**setupError**](./setup.md#onsetupError)

  - 플레이어를 정상적으로 설정할 수 없을 때 발생합니다.

<br>

## ALL

모든 이벤트를 수집하는 이벤트 그룹

- [**all**](./all.md#onall)

  - 플레이어 내부에서 발생하는 모든 이벤트가 트리거될 때마다 호출

<br>

## ADVERTISING

광고 기능과 관련된 이벤트 그룹

- [**adBidRequest**](./advertising.md#onadBidRequest)

  - 헤더 비딩(Header Bidding) 이 입찰 요청을 시작할 때 트리거됩니다.

- [**adBidResponse**](./advertising.md#onadBidResponse)

  - 헤더 비딩(Header Bidding) 응답이 반환될 때 트리거됩니다.

- [**adBlock**](./advertising.md#onadBlock)

  - 이 이벤트는 JWP 설정에 VAST 또는 Google IMA 광고 플러그인이 구성되어 있을 때, 시청자의 브라우저에서 광고 차단기(Ad Blocker) 가 감지되면 트리거됩니다.

- [**adBreakEnd**](./advertising.md#onadBreakEnd)

  - 광고 재생이 종료되고 플레이어로 제어권이 다시 넘어올 때 트리거됩니다.

- [**adBreakIgnored**](./advertising.md#onadBreakIgnored)

  - (VAST 전용) 이 이벤트는 이전에 완전히 재생된 광고 구간(ad break) 과 현재 광고 구간 사이의 경과 시간이 advertising.rules.timeBetweenAds 에 정의된 값보다 짧을 때 트리거됩니다.

- [**adBreakStart**](./advertising.md#onadBreakStart)

  - 광고 요청이 완료된 직후, 광고가 플레이어에 로드되기 직전에 트리거됩니다. 이 이벤트는 광고 구간(ad break) 내의 첫 번째 광고 이전에만 발생합니다.

- [**adClick**](./advertising.md#onadClick)

  - 광고를 클릭하여 랜딩 페이지로 이동할 때마다 트리거되는 이벤트입니다. 해당 이벤트는 DAI, FreeWheel, IMA, VAST 클라이언트에서 모두 지원됩니다.

- [**adCompanions**](./advertising.md#onadCompanions)

  - (VAST, IMA) 광고에 컴패니언(Companion) 이 포함되어 있을 때 트리거됩니다. IMA, VAST 두 광고 클라이언트 모두에서 지원됩니다.

- [**adComplete**](./advertising.md#onadComplete)

  - 광고가 완전히 재생을 마쳤을 때 트리거됩니다. 이 이벤트는 DAI, FreeWheel, IMA, VAST 클라이언트 모두에서 발생합니다.

- [**adError**](./advertising.md#onadError)

  - 광고 재생이 오류로 인해 중단될 때 트리거됩니다.

- [**adImpression**](./advertising.md#onadImpression)

  - IAB(Interactive Advertising Bureau) 정의에 따라 광고 노출(impression)이 발생할 때 트리거됩니다. 즉, 비디오 광고가 실제로 재생을 시작하는 순간 발생합니다.

- [**adItem**](./advertising.md#onadItem)

  - VAST XML이 파싱되고, 로드되며, 표시할 준비가 완료되었을 때 트리거됩니다. 적용 대상: IMA, VAST

- [**adLoaded**](./advertising.md#onadLoaded)

  - VAST XML이 파싱되어 로드되고, 표시할 준비가 완료되었을 때 트리거됩니다. 적용 대상: IMA, VAST

- [**adManager**](./advertising.md#onadManager)

  - FreeWheel 전용 이벤트로, 광고 매니저(ad manager) 가 플레이어에 로드될 때 트리거됩니다. 이 이벤트를 사용하면 광고 재생 전에 퍼블리셔가 FreeWheel 광고 매니저의 추가 기능을 통합할 수 있습니다.

- [**adMeta**](./advertising.md#onadMeta)

  - (VAST 전용) 플레이어가 광고로부터 새로운 메타데이터를 수신할 때마다 지속적으로 트리거되는 이벤트입니다. 값은 광고의 구성에 따라 달라질 수 있습니다.

- [**adPause**](./advertising.md#onadPause)

  - 광고가 일시정지(Pause) 될 때마다 트리거됩니다. 적용 대상: DAI, FreeWheel, IMA, VAST

- [**adPlay**](./advertising.md#onadPlay)

  - 광고가 재생을 시작하거나 또는 일시정지 상태에서 다시 재생될 때 트리거됩니다. 적용 대상: DAI, FreeWheel, IMA, VAST

- [**adRequest**](./advertising.md#onadRequest)

  - 플레이어가 광고를 요청할 때마다 트리거됩니다. 적용 대상: FreeWheel, IMA, VAST

- [**adRequestedContentResume**](./advertising.md#onadRequestedContentResume)

  - (IMA, VAST) 다음 중 하나의 이벤트가 발생할 때 트리거됩니다:
    - 광고가 완료되었을 때
    - 오류가 발생했을 때
    - 광고가 건너뛰기(skipped) 되었을 때
  - 이 이벤트는 광고 재생이 끝나고 콘텐츠 재생으로 전환되는 시점을 나타냅니다.

- [**adSchedule**](./advertising.md#onadSchedule)

  - (VAST) 광고 스케줄이 플러그인에 의해 로드되고 파싱될 때 트리거됩니다.

- [**adLoadedXML**](./advertising.md#onadLoadedXML)

  - VAST 전용 이벤트 VAST 광고 클라이언트가 광고 태그를 로드할 때 트리거됩니다.

- [**adSkipped**](./advertising.md#onadSkipped)

  - 광고가 건너뛰기(skipped) 되었을 때마다 트리거됩니다.

- [**adStarted**](./advertising.md#onadStarted)

  - (VAST [VPAID], FreeWheel [VPAID], IMA [모든 광고]) 광고 크리에이티브가 JWP 플레이어에 재생 시작을 알릴 때 트리거됩니다.

- [**adTime**](./advertising.md#onadTime)

  - 광고 재생이 진행 중일 때 주기적으로 발생하는 이벤트입니다. (DAI, FreeWheel, IMA, VAST 클라이언트에서 지원됨)

- [**adViewableImpression**](./advertising.md#onadViewableImpression)

  - VAST 및 IMA에서 사용됩니다. 다음 두 가지 조건이 모두 충족될 때만 트리거됩니다:
    - 광고가 연속 2초 이상 재생된 경우
    - 플레이어의 50% 이상이 뷰포트(viewport)에 표시된 경우

- [**adWarning**](./advertising.md#onadWarning)

  - VAST 전용 이벤트입니다.
  - 광고 재생에는 치명적이지 않지만 비정상적인 경고(non-fatal warning) 가 발생했음을 나타냅니다.
  - 광고의 재생은 계속되지만, 일부 트래킹 이벤트나 구성 요소가 누락되었을 때 발생할 수 있습니다.

- [**adsManager**](./advertising.md#onadsManager)

  - 플레이어에 광고 관리자(ad manager) 가 로드될 때 발생하는 이벤트입니다.
  - 이 이벤트는 광고 재생 전에 게시자가 추가적인 광고 관리자 기능을 통합할 수 있도록 합니다.

- [**beforeComplete**](./advertising.md#onbeforeComplete)

  - 플레이어가 재생을 완료하기 직전에 발생하는 이벤트입니다.

- [**beforePlay**](./advertising.md#onbeforePlay)

  - 다음 중 어느 하나가 발생하기 직전에 호출되는 이벤트입니다.
    - 단일 미디어 또는 재생목록(playlist)의 개별 항목이 처음 재생되기 전
    - 일시 정지된 상태에서 시청자 또는 다른 메커니즘에 의해 재생이 다시 시작되기 전

<br>

## AUDIO TRACKS

오디오 트랙과 관련된 이벤트 그룹

- [**audioTracks**](./audio-tracks.md#onaudiotracks)

  - 사용 가능한 오디오 트랙 목록이 업데이트될 때 트리거됩니다.

- [**audioTrackChanged**](./audio-tracks.md#onaudiotrackchanged)

  - 활성 오디오 트랙이 변경될 때 트리거됩니다.

<br>

## BUFFER

플레이어 버퍼링과 관련된 이벤트 그룹

- [**bufferChange**](./buffer.md#onbufferChange)

  - 현재 재생 중인 항목이 버퍼에 추가 데이터를 로드할 때 트리거됩니다.

<br>

## CAPTIONS

자막과 관련된 이벤트 그룹

- [**captionsList**](./captions.md#oncaptionsList)

  - 사용 가능한 자막 트랙 목록이 변경될 때 트리거됩니다.

- [**captionsChanged**](./captions.md#oncaptionsChanged)

  - 활성 자막 트랙이 수동으로 또는 API를 통해 변경될 때마다 트리거됩니다.

<br>

## CAST

크롬캐스트(Chromecast)나 AirPlay 같은 외부 기기로의 영상 송출(cast)과 관련된 이벤트 그룹

- [**cast**](./cast.md#oncast)

  - 캐스트 속성이 변경될 때 트리거됩니다.

- [**castIntercepted**](./cast.md#oncastIntercepted)

  - `interceptCast`가 활성화된 상태에서 사용자가 콘텐츠를 캐스트하려고 시도할 때 발생합니다.

<br>

## CONTROLS

플레이어 제어 UI(재생/일시정지/음량/전체화면/트랙변경 등)와 관련된 이벤트 그룹

- [**controls**](./controls.md#oncontrols)

  - 스크립트에 의해 컨트롤이 활성화되거나 비활성화될 때 트리거됩니다.

- [**displayClick**](./controls.md#ondisplayClick)

  - 사용자가 비디오 디스플레이 영역을 클릭할 때 트리거됩니다.

<br>

## FLOATING PLAYER

플레이어가 화면 위에 떠(floating)서 위치가 바뀔 때와 관련된 이벤트 그룹

- [**float**](./floating-player.md#onfloat)

  - 플로팅 플레이어가 떠오르거나(floating 시작) 원래 위치로 복귀할 때 트리거됩니다.

<br>

## METADATA

미디어 파일 내부에 포함된 메타데이터(e.g., ID3, timed-metadata, 트랙 길이·제작자 정보 등)와 관련된 이벤트 그룹

- [**meta**](./metadata.md#onmeta)

  - 재생이 새로운 메타데이터가 활성화되는 시간 구간에 진입할 때 트리거됩니다.

- [**metadataCueParsed**](./metadata.md#onmetadataCueParsed)

  - 메타데이터 큐 포인트가 버퍼링되면 트리거됩니다.

<br>

## PLAYBACK

실제 재생 상태 변화와 관련된 이벤트 그룹

- [**autostartNotAllowed**](./playback.md#onautostartNotAllowed)

  - 플레이어가 자동 재생(autostart) 으로 설정되어 있지만, 브라우저 설정으로 인해 자동 재생이 차단될 때 트리거됩니다.

- [**buffer**](./playback.md#onbuffer)

  - 다음 이벤트 중 하나가 발생할 때 트리거됩니다:
    - 플레이어가 재생을 시작할 때
    - 플레이어가 버퍼링 상태에 진입할 때

- [**complete**](./playback.md#oncomplete)

  - 비디오의 끝에 도달했을 때 트리거됩니다.

- [**error**](./playback.md#onerror)

  - 재생 과정에서 치명적인 오류가 발생했을 때 트리거됩니다.

- [**firstFrame**](./playback.md#onfirstFrame)

  - 비디오의 첫 번째 프레임이 표시되거나, 오디오 파일이 재생을 시작하는 순간 트리거됩니다.

- [**idle**](./playback.md#onidle)

  - 플레이어가 idle(대기) 상태로 전환될 때 트리거됩니다.

- [**pause**](./playback.md#onpause)

  - 플레이어 내의 광고가 아닌 미디어 콘텐츠가 일시정지될 때 트리거됩니다.

- [**play**](./playback.md#onplay)

  - 플레이어 내의 광고가 아닌 미디어 콘텐츠가 재생을 시작할 때 트리거됩니다.

- [**playAttemptFailed**](./playback.md#onplayAttemptFailed)

  - 재생이 중단되거나 차단될 때 트리거됩니다.

- [**playbackRateChanged**](./playback.md#onplaybackRateChanged)

  - 재생 속도가 변경되었을 때 트리거됩니다.

- [**warning**](./playback.md#onwarning)

  - 설정(setup) 또는 재생 과정(playback process) 에 치명적이지 않은 오류가 발생했을 때 트리거됩니다.

<br>

## PLAYLIST

플레이리스트와 관련된 이벤트 그룹

- [**nextClick**](./playlist.md#onnextClick)

  - 컨트롤 바의 다음(next) 버튼 또는 Next Up 오버레이가 클릭된 후 트리거됩니다.

- [**playlist**](./playlist.md#onplaylist)

  - 완전히 새로운 플레이리스트가 플레이어에 로드될 때 트리거됩니다.

- [**playlistItem**](./playlist.md#onplaylistItem)

  - 플레이리스트의 인덱스가 새 항목으로 변경될 때 트리거됩니다.

- [**playlistComplete**](./playlist.md#onplaylistComplete)

  - 다음 조건이 모두 충족될 때 트리거됩니다:
    - 플레이어가 플레이리스트의 모든 항목 재생을 완료했을 때
    - repeat 옵션이 false로 설정되어 있을 때

<br>

## QUALITY

영상/오디오의 품질과 관련된 이벤트 그룹

- [**levels**](./quality.md#onlevels)

  - 사용 가능한 화질 수준 목록이 업데이트될 때 트리거됩니다.

- [**levelsChanged**](./quality.md#onlevelsChanged)

  - 활성 화질 수준이 변경될 때 트리거됩니다.

- [**visualQuality**](./quality.md#onvisualQuality)

  - HLS에서 활성 화질 수준이 변경될 때 트리거됩니다.

<br>

## RELATED

동영상이 끝난 뒤 관련 콘텐츠 표시와 관련된 이벤트 그룹

- [**relatedReady**](./related.md#onrelatedReady)

  - Related 플러그인이 준비되었을 때\*\* 트리거되는 이벤트를 나타냅니다.

- [**related-on-open**](./related.md#onrelatedOnopen)

  - 추천(related) 인터페이스가 열릴 때 트리거됩니다.

- [**related-on-close**](./related.md#onrelatedOnclose)

  - 추천(related) 인터페이스가 닫힐 때 트리거됩니다.

- [**related-on-play**](./related.md#onrelatedOnplay)

  - 사용자가 추천 피드에서 항목을 선택했을 때 트리거됩니다.

<br>

## RESIZE

플레이어 크기(가로·세로) 또는 전체화면(fullscreen)과 관련된 이벤트 그룹

- [**fullscreen**](./resize.md#onfullscreen)

  - 플레이어가 전체화면 모드로 전환되거나 해제될 때 트리거됩니다.

- [**resize**](./resize.md#onresize)

  - 플레이어의 페이지 내 크기(dimension)가 변경되었을 때 트리거됩니다.

<br>

## SEEK

재생 위치를 이동(seek)과 관련된 이벤트 그룹

- [**absolutePositionReady**](./seek.md#onabsolutePositionReady)

  - .getAbsolutePosition() 메서드가 데이터를 반환할 준비가 되었을 때 트리거됩니다.

- [**seek**](./seek.md#onseek)

  - 컨트롤 바에서 스크럽(scrubbing) 하거나 API를 통해 탐색(seek) 이 요청되었을 때 트리거됩니다.

- [**seeked**](./seek.md#onseeked)

  - on('seek') 이벤트가 탐색 동작이 발생할 때 트리거되는 것과 달리, 이 이벤트는 탐색이 완료되어 비디오 위치가 실제로 변경된 후 트리거됩니다.

- [**time**](./seek.md#ontime)

  - 플레이어가 재생 중일 때 재생 위치가 업데이트될 때마다 트리거됩니다.

<br>

## SHARING

공유 기능(소셜 링크, 임베드 코드 등)과 관련된 이벤트 그룹

- [**sharing-on-click**](./sharing.md#onsharingOnclick)

  - 공유(Sharing) 플러그인이 열릴 때 감지합니다.

- [**sharing-on-close**](./sharing.md#onsharingOnclose)

  - 공유(Sharing) 플러그인이 닫힐 때 감지합니다.

- [**sharing-on-open**](./sharing.md#onsharingOnopen)

  - 공유(Sharing) 플러그인 내에서 사용자가 콘텐츠를 공유할 때마다 트리거됩니다.

<br>

## VIEWABILITY

플레이어가 화면에 얼마나 보여지는지(viewable)와 관련된 이벤트 그룹

- [**containerViewable**](./viewability.md#oncontainerViewable)

  - 플레이어 컨테이너(<div> 요소)의 가시성(viewability) 상태를 나타내는 이벤트가 트리거됩니다.

- [**viewable**](./viewability.md#onviewable)

  - 플레이어의 가시성(viewability) 상태를 나타내는 이벤트가 트리거됩니다.

<br>

## VOLUME

볼륨 변화(음량 변경, 음소거/음소거 해제)와 관련된 이벤트 그룹

- [**mute**](./volume.md#onmute)

  - 플레이어가 음소거 상태로 전환되거나 해제될 때 트리거됩니다.

- [**volume**](./volume.md#onvolume)

  - 플레이어의 볼륨이 변경될 때 트리거됩니다.
