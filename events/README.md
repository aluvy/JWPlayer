# Events Index

JWPlayer 이벤트

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

| Event                                                                       | Description                                 |
| :-------------------------------------------------------------------------- | :------------------------------------------ |
| [**adBidRequest**](./advertising.md#onadBidRequest)                         | 광고 요청(bid request)을 보내기 직전에 호출 |
| [**adBidResponse**](./advertising.md#onadBidResponse)                       |                                             |
| [**adBlock**](./advertising.md#onadBlock)                                   |                                             |
| [**adBreakEnd**](./advertising.md#onadBreakEnd)                             |                                             |
| [**adBreakIgnored**](./advertising.md#onadBreakIgnored)                     |                                             |
| [**adBreakStart**](./advertising.md#onadBreakStart)                         |                                             |
| [**adClick**](./advertising.md#onadClick)                                   |                                             |
| [**adCompanions**](./advertising.md#onadCompanions)                         |                                             |
| [**adComplete**](./advertising.md#onadComplete)                             |                                             |
| [**adError**](./advertising.md#onadError)                                   |                                             |
| [**adImpression**](./advertising.md#onadImpression)                         |                                             |
| [**adItem**](./advertising.md#onadItem)                                     |                                             |
| [**adLoaded**](./advertising.md#onadLoaded)                                 |                                             |
| [**adManager**](./advertising.md#onadManager)                               |                                             |
| [**adMeta**](./advertising.md#onadMeta)                                     |                                             |
| [**adPause**](./advertising.md#onadPause)                                   |                                             |
| [**adPlay**](./advertising.md#onadPlay)                                     |                                             |
| [**adRequest**](./advertising.md#onadRequest)                               |                                             |
| [**adRequestedContentResume**](./advertising.md#onadRequestedContentResume) |                                             |
| [**adSchedule**](./advertising.md#onadSchedule)                             |                                             |
| [**adLoadedXML**](./advertising.md#onadLoadedXML)                           |                                             |
| [**adSkipped**](./advertising.md#onadSkipped)                               |                                             |
| [**adStarted**](./advertising.md#onadStarted)                               |                                             |
| [**adTime**](./advertising.md#onadTime)                                     |                                             |
| [**adViewableImpression**](./advertising.md#onadViewableImpression)         |                                             |
| [**adWarning**](./advertising.md#onadWarning)                               |                                             |
| [**adsManager**](./advertising.md#onadsManager)                             |                                             |
| [**beforeComplete**](./advertising.md#onbeforeComplete)                     |                                             |
| [**beforePlay**](./advertising.md#onbeforePlay)                             |                                             |

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

| Event                                     | Description |
| :---------------------------------------- | :---------- |
| [**float**](./floating-player.md#onfloat) |             |

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

| Event                                                          | Description |
| :------------------------------------------------------------- | :---------- |
| [**autostartNotAllowed**](./playback.md#onautostartNotAllowed) |             |
| [**buffer**](./playback.md#onbuffer)                           |             |
| [**complete**](./playback.md#oncomplete)                       |             |
| [**error**](./playback.md#onerror)                             |             |
| [**firstFrame**](./playback.md#onfirstFrame)                   |             |
| [**idle**](./playback.md#onidle)                               |             |
| [**pause**](./playback.md#onpause)                             |             |
| [**play**](./playback.md#onplay)                               |             |
| [**playAttemptFailed**](./playback.md#onplayAttemptFailed)     |             |
| [**playbackRateChanged**](./playback.md#onplaybackRateChanged) |             |
| [**warning**](./playback.md#onwarning)                         |             |

<br>

## PLAYLIST

플레이리스트와 관련된 이벤트 그룹

| Event                                                    | Description |
| :------------------------------------------------------- | :---------- |
| [**nextClick**](./playlist.md#onnextClick)               |             |
| [**playlist**](./playlist.md#onplaylist)                 |             |
| [**playlistItem**](./playlist.md#onplaylistItem)         |             |
| [**playlistComplete**](./playlist.md#onplaylistComplete) |             |

<br>

## QUALITY

영상/오디오의 품질과 관련된 이벤트 그룹

| Event                                             | Description |
| :------------------------------------------------ | :---------- |
| [**levels**](./quality.md#onlevels)               |             |
| [**levelsChanged**](./quality.md#onlevelsChanged) |             |
| [**visualQuality**](./quality.md#onvisualQuality) |             |

<br>

## RELATED

동영상이 끝난 뒤 관련 콘텐츠 표시와 관련된 이벤트 그룹

| Event                                               | Description |
| :-------------------------------------------------- | :---------- |
| [**relatedReady**](./related.md#onrelatedReady)     |             |
| [**relatedOnclose**](./related.md#onrelatedOnclose) |             |
| [**relatedOnopen**](./related.md#onrelatedOnopen)   |             |
| [**relatedOnplay**](./related.md#onrelatedOnplay)   |             |

<br>

## RESIZE

플레이어 크기(가로·세로) 또는 전체화면(fullscreen)과 관련된 이벤트 그룹

| Event                                      | Description |
| :----------------------------------------- | :---------- |
| [**fullscreen**](./resize.md#onfullscreen) |             |
| [**resize**](./resize.md#onresize)         |             |

<br>

## SEEK

재생 위치를 이동(seek)과 관련된 이벤트 그룹

| Event                                                          | Description |
| :------------------------------------------------------------- | :---------- |
| [**absolutePositionReady**](./seek.md#onabsolutePositionReady) |             |
| [**seek**](./seek.md#onseek)                                   |             |
| [**seeked**](./seek.md#onseeked)                               |             |
| [**time**](./seek.md#ontime)                                   |             |

<br>

## SHARING

공유 기능(소셜 링크, 임베드 코드 등)과 관련된 이벤트 그룹

```javascript
player.on('ready', function (event) {
  sharingPlugin = jwplayer().getPlugin('sharing');
});
```

| Event                                               | Description |
| :-------------------------------------------------- | :---------- |
| [**sharingOnclick**](./sharing.md#onsharingOnclick) |             |
| [**sharingOnclose**](./sharing.md#onsharingOnclose) |             |
| [**sharingOnopen**](./sharing.md#onsharingOnopen)   |             |

<br>

## VIEWABILITY

플레이어가 화면에 얼마나 보여지는지(viewable)와 관련된 이벤트 그룹

| Event                                                         | Description |
| :------------------------------------------------------------ | :---------- |
| [**containerViewable**](./viewability.md#oncontainerViewable) |             |
| [**viewable**](./viewability.md#onviewable)                   |             |

<br>

## VOLUME

볼륨 변화(음량 변경, 음소거/음소거 해제)와 관련된 이벤트 그룹

| Event                              | Description |
| :--------------------------------- | :---------- |
| [**mute**](./volume.md#onmute)     |             |
| [**volume**](./volume.md#onvolume) |             |
