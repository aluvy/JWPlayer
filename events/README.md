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

플레이어 셋업과 관련된 이벤트 그룹

| Event                                     | Description                                             |
| :---------------------------------------- | :------------------------------------------------------ |
| [**ready**](./setup.md#onready)           | 플레이어 초기화가 완료되어 재생 준비가 끝난 시점에 호출 |
| [**remove**](./setup.md#onremove)         | 플레이어 인스턴스가 DOM에서 제거될 때 호출              |
| [**setupError**](./setup.md#onsetupError) | 플레이어 초기화가 실패했을 때 호출                      |

<br>

## ALL

모든 이벤트를 수집하는 이벤트 그룹

| Event                     | Description                                                   |
| :------------------------ | :------------------------------------------------------------ |
| [**all**](./all.md#onall) | 플레이어 내부에서 발생하는 모든 이벤트가 트리거될 때마다 호출 |

<br>

## ADVERTISING

광고 기능과 관련된 이벤트 그룹

| Event                                                                       | Description                                                        |
| :-------------------------------------------------------------------------- | :----------------------------------------------------------------- |
| [**adBidRequest**](./advertising.md#onadBidRequest)                         | 광고 요청(bid request)을 보내기 직전에 호출                        |
| [**adBidResponse**](./advertising.md#onadBidResponse)                       |                                                                    |
| [**adBlock**](./advertising.md#onadBlock)                                   |                                                                    |
| [**adBreakEnd**](./advertising.md#onadBreakEnd)                             |                                                                    |
| [**adBreakIgnored**](./advertising.md#onadBreakIgnored)                     |                                                                    |
| [**adBreakStart**](./advertising.md#onadBreakStart)                         |                                                                    |
| [**adClick**](./advertising.md#onadClick)                                   |                                                                    |
| [**adCompanions**](./advertising.md#onadCompanions)                         |                                                                    |
| [**adComplete**](./advertising.md#onadComplete)                             |                                                                    |
| [**adError**](./advertising.md#onadError)                                   |                                                                    |
| [**adImpression**](./advertising.md#onadImpression)                         |                                                                    |
| [**adItem**](./advertising.md#onadItem)                                     |                                                                    |
| [**adLoaded**](./advertising.md#onadLoaded)                                 |                                                                    |
| [**adManager**](./advertising.md#onadManager)                               |                                                                    |
| [**adMeta**](./advertising.md#onadMeta)                                     |                                                                    |
| [**adPause**](./advertising.md#onadPause)                                   |                                                                    |
| [**adPlay**](./advertising.md#onadPlay)                                     |                                                                    |
| [**adRequest**](./advertising.md#onadRequest)                               |                                                                    |
| [**adRequestedContentResume**](./advertising.md#onadRequestedContentResume) |                                                                    |
| [**adSchedule**](./advertising.md#onadSchedule)                             |                                                                    |
| [**adLoadedXML**](./advertising.md#onadLoadedXML)                           |                                                                    |
| [**adSkipped**](./advertising.md#onadSkipped)                               |                                                                    |
| [**adStarted**](./advertising.md#onadStarted)                               |                                                                    |
| [**adTime**](./advertising.md#onadTime)                                     |                                                                    |
| [**adViewableImpression**](./advertising.md#onadViewableImpression)         |                                                                    |
| [**adWarning**](./advertising.md#onadWarning)                               |                                                                    |
| [**adsManager**](./advertising.md#onadsManager)                             |                                                                    |
| [**beforeComplete**](./advertising.md#onbeforeComplete)                     | 현재 재생 중인 영상이 끝나기 직전(마지막 프레임 재생 직전) 에 호출 |
| [**beforePlay**](./advertising.md#onbeforePlay)                             | 영상이 재생되기 직전, 즉 실제 `play()` 명령이 실행되기 직전에 호출 |

<br>

## AUDIO TRACKS

오디오 트랙과 관련된 이벤트 그룹

| Event                                                          | Description |
| :------------------------------------------------------------- | :---------- |
| [**audioTrackChanged**](./audio-tracks.md#onaudioTrackChanged) |             |
| [**audioTracks**](./audio-tracks.md#onaudioTracks)             |             |

<br>

## BUFFER

플레이어 버퍼링과 관련된 이벤트 그룹

| Event                                          | Description                                                       |
| :--------------------------------------------- | :---------------------------------------------------------------- |
| [**bufferChange**](./buffer.md#onbufferChange) | 현재 재생 중인 미디어의 버퍼(로딩된 데이터 양)가 변할 때마다 호출 |

<br>

## CAPTIONS

자막과 관련된 이벤트 그룹

| Event                                                  | Description                                                              |
| :----------------------------------------------------- | :----------------------------------------------------------------------- |
| [**captionsChanged**](./captions.md#oncaptionsChanged) | 자막 트랙을 변경했을 때 호출                                             |
| [**captionsList**](./captions.md#oncaptionsList)       | 사용 가능한 자막 트랙(caption tracks)을 처음 인식하거나 갱신했을 때 호출 |

<br>

## CAST

크롬캐스트(Chromecast)나 AirPlay 같은 외부 기기로의 영상 송출(cast)과 관련된 이벤트 그룹

| Event                                              | Description |
| :------------------------------------------------- | :---------- |
| [**cast**](./cast.md#oncast)                       |             |
| [**castIntercepted**](./cast.md#oncastIntercepted) |             |

<br>

## CONTROLS

플레이어 제어 UI(재생/일시정지/음량/전체화면/트랙변경 등)와 관련된 이벤트 그룹

| Event                                            | Description |
| :----------------------------------------------- | :---------- |
| [**controls**](./controls.md#oncontrols)         |             |
| [**displayClick**](./controls.md#ondisplayClick) |             |

<br>

## FLOATING PLAYER

플레이어가 화면 위에 떠(floating)서 위치가 바뀔 때와 관련된 이벤트 그룹

| Event                                     | Description                                                  |
| :---------------------------------------- | :----------------------------------------------------------- |
| [**float**](./floating-player.md#onfloat) | 플레이어가 플로팅(floating) 상태로 전환되거나 해제될 때 호출 |

<br>

## METADATA

미디어 파일 내부에 포함된 메타데이터(e.g., ID3, timed-metadata, 트랙 길이·제작자 정보 등)와 관련된 이벤트 그룹

| Event                                                      | Description |
| :--------------------------------------------------------- | :---------- |
| [**meta**](./metadata.md#onmeta)                           |             |
| [**metadataCueParsed**](./metadata.md#onmetadataCueParsed) |             |

<br>

## PLAYBACK

실제 재생 상태 변화와 관련된 이벤트 그룹

| Event                                                          | Description                                                                                                                                        |
| :------------------------------------------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------- |
| [**autostartNotAllowed**](./playback.md#onautostartNotAllowed) | 플레이어 설정(`autostart: true`) 또는 `player.play()` 호출로 자동재생을 시도했으나, 브라우저나 OS의 미디어 자동재생 정책에 의해 차단되었을 때 호출 |
| [**buffer**](./playback.md#onbuffer)                           | 플레이어가 재생 도중 데이터 로딩이 부족하여 일시적으로 멈출 때 호출                                                                                |
| [**complete**](./playback.md#oncomplete)                       | 플레이어가 현재 재생 중인 영상의 마지막 프레임을 모두 재생 완료했을 때 호출                                                                        |
| [**error**](./playback.md#onerror)                             | 플레이어가 재생 중 오류를 감지했을 때 호출                                                                                                         |
| [**firstFrame**](./playback.md#onfirstFrame)                   | 영상이 실제로 화면에 첫 번째 프레임을 렌더링했을 때 호출                                                                                           |
| [**idle**](./playback.md#onidle)                               | 플레이어가 재생 가능한 미디어가 없거나, 현재 재생이 완전히 종료되어 대기 상태로 전환될 때 호출                                                     |
| [**pause**](./playback.md#onpause)                             | 재생 중이던 영상이 일시정지(Pause) 상태로 전환될 때 호출                                                                                           |
| [**play**](./playback.md#onplay)                               | 플레이어가 실제 재생 상태(playing)로 전환될 때 호출                                                                                                |
| [**playAttemptFailed**](./playback.md#onplayAttemptFailed)     | 플레이어가 재생을 시도했으나(play() 실행됨) 브라우저 또는 플랫폼 정책, 미디어 문제 등으로 인해 재생이 시작되지 못했을 때 호출                      |
| [**playbackRateChanged**](./playback.md#onplaybackRateChanged) | 플레이어의 재생 속도(playbackRate) 가 변경될 때마다 호출                                                                                           |
| [**warning**](./playback.md#onwarning)                         | 재생은 계속 진행되지만 경고성 문제(Warning)가 발생했을 때 호출                                                                                     |

<br>

## PLAYLIST

플레이리스트와 관련된 이벤트 그룹

| Event                                                    | Description                                                    |
| :------------------------------------------------------- | :------------------------------------------------------------- |
| [**nextClick**](./playlist.md#onnextClick)               | 사용자가 플레이어 UI의 “다음(next)” 버튼을 클릭했을 때 호출    |
| [**playlist**](./playlist.md#onplaylist)                 | 완전히 새로운 플레이리스트가 플레이어에 로드되었을 때 1회 호출 |
| [**playlistItem**](./playlist.md#onplaylistItem)         | 플레이리스트 내에서 현재 재생 항목이 바뀌는 순간 호출          |
| [**playlistComplete**](./playlist.md#onplaylistComplete) |                                                                |

<br>

## QUALITY

영상/오디오의 품질과 관련된 이벤트 그룹

| Event                                             | Description                                                                                 |
| :------------------------------------------------ | :------------------------------------------------------------------------------------------ |
| [**levels**](./quality.md#onlevels)               | 플레이어가 영상의 화질(quality) 정보를 로드하고, 선택 가능한 레벨 목록이 준비되었을 때 호출 |
| [**levelsChanged**](./quality.md#onlevelsChanged) | 플레이어의 재생 화질(quality level)이 변경될 때마다 호출                                    |
| [**visualQuality**](./quality.md#onvisualQuality) | 플레이어의 실제 비디오 렌더링 품질(Visual Quality) 이 실시간으로 변경될 때 호출             |

<br>

## RELATED

동영상이 끝난 뒤 관련 콘텐츠 표시와 관련된 이벤트 그룹

| Event                                                 | Description                                                                                                    |
| :---------------------------------------------------- | :------------------------------------------------------------------------------------------------------------- |
| [**relatedReady**](./related.md#onrelatedReady)       | Related 플러그인(추천 영상 기능) 이 추천 콘텐츠(related playlist) 를 불러오고 DOM 렌더링 준비가 끝났을 때 호출 |
| [**related-on-close**](./related.md#onrelatedOnclose) | 추천(related) 콘텐츠 오버레이가 닫힐 때 호출                                                                   |
| [**related-on-open**](./related.md#onrelatedOnopen)   | 추천(related) 콘텐츠 오버레이가 실제로 열릴 때 호출                                                            |
| [**related-on-play**](./related.md#onrelatedOnplay)   | 사용자가 추천 콘텐츠(related item)를 클릭하여 재생이 시작될 때 호출                                            |

<br>

## RESIZE

플레이어 크기(가로·세로) 또는 전체화면(fullscreen)과 관련된 이벤트 그룹

| Event                                      | Description                                                            |
| :----------------------------------------- | :--------------------------------------------------------------------- |
| [**fullscreen**](./resize.md#onfullscreen) | 플레이어가 전체화면 모드(Fullscreen Mode) 로 진입하거나 해제될 때 호출 |
| [**resize**](./resize.md#onresize)         | 플레이어 컨테이너의 크기(width/height)가 변경될 때마다 호출            |

<br>

## SEEK

재생 위치를 이동(seek)과 관련된 이벤트 그룹

| Event                                                          | Description                                                                                         |
| :------------------------------------------------------------- | :-------------------------------------------------------------------------------------------------- |
| [**absolutePositionReady**](./seek.md#onabsolutePositionReady) | 플레이어가 타임라인 상의 “절대 위치(absolute position)” 정보를 계산할 준비가 완료된 시점에 호출     |
| [**seek**](./seek.md#onseek)                                   | 사용자가 재생 위치를 변경(Seek) 했을 때 호출                                                        |
| [**seeked**](./seek.md#onseeked)                               | 시킹(Seek) 동작이 완료된 직후, 즉 플레이어가 새 위치로 이동하고 재생 가능한 상태로 복귀했을 때 호출 |
| [**time**](./seek.md#ontime)                                   | 플레이어가 재생 중일 때, 매 프레임(또는 일정 주기) 마다 호출                                        |

<br>

## SHARING

공유 기능(소셜 링크, 임베드 코드 등)과 관련된 이벤트 그룹

```javascript
player.on('ready', function (event) {
  sharingPlugin = jwplayer().getPlugin('sharing');
});
```

| Event                                                 | Description                                                   |
| :---------------------------------------------------- | :------------------------------------------------------------ |
| [**sharing-on-click**](./sharing.md#onsharingOnclick) | 사용자가 공유 패널 내의 특정 플랫폼 아이콘을 클릭했을 때 호출 |
| [**sharing-on-close**](./sharing.md#onsharingOnclose) | 사용자가 공유 패널(Sharing Overlay) 을 열었을 때 호출         |
| [**sharing-on-open**](./sharing.md#onsharingOnopen)   | 사용자가 공유 패널을 닫았을 때 호출                           |

<br>

## VIEWABILITY

플레이어가 화면에 얼마나 보여지는지(viewable)와 관련된 이벤트 그룹

| Event                                                         | Description                                                                             |
| :------------------------------------------------------------ | :-------------------------------------------------------------------------------------- |
| [**containerViewable**](./viewability.md#oncontainerViewable) | 플레이어가 브라우저 뷰포트(Viewport) 내에서 보이는 상태인지 감지될 때마다 호출          |
| [**viewable**](./viewability.md#onviewable)                   | 플레이어가 브라우저 탭 또는 OS 수준에서 “실제로 보이는 상태(Viewable)”로 전환될 때 호출 |

<br>

## VOLUME

볼륨 변화(음량 변경, 음소거/음소거 해제)와 관련된 이벤트 그룹

| Event                              | Description                                    |
| :--------------------------------- | :--------------------------------------------- |
| [**mute**](./volume.md#onmute)     | 플레이어의 음소거 상태(Muted)가 변경될 때 호출 |
| [**volume**](./volume.md#onvolume) | 플레이어의 볼륨(Volume) 크기가 변경될 때 호출  |
