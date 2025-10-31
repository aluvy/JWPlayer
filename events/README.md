# Events Index

JWPlayer 이벤트 리스트

<br>

## SETUP

플레이어 셋업과 관련된 이벤트 그룹

| Event                                     | Description                                             |
| :---------------------------------------- | :------------------------------------------------------ |
| [**ready**](./setup.md#onready)           | 플레이어 초기화가 완료되어 재생 준비가 끝난 시점에 호출 |
| [**remove**](./setup.md#onremove)         | 플레이어 인스턴스가 DOM에서 제거될 때 호출              |
| [**setupError**](./setup.md#onsetuperror) | 플레이어 초기화가 실패했을 때 호출                      |

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
| [**adBidResponse**](./advertising.md#onadbidresponse)                       |                                             |
| [**adBlock**](./advertising.md#onadblock)                                   |                                             |
| [**adBreakEnd**](./advertising.md#onadbreakend)                             |                                             |
| [**adBreakIgnored**](./advertising.md#onadbreakignored)                     |                                             |
| [**adBreakStart**](./advertising.md#onadbreakstart)                         |                                             |
| [**adClick**](./advertising.md#onadclick)                                   |                                             |
| [**adCompanions**](./advertising.md#onadcompanions)                         |                                             |
| [**adComplete**](./advertising.md#pmadcomplete)                             |                                             |
| [**adError**](./advertising.md#onaderror)                                   |                                             |
| [**adImpression**](./advertising.md#onadimpression)                         |                                             |
| [**adItem**](./advertising.md#onaditem)                                     |                                             |
| [**adLoaded**](./advertising.md#onadloaded)                                 |                                             |
| [**adManager**](./advertising.md#onadmanager)                               |                                             |
| [**adMeta**](./advertising.md#onadmeta)                                     |                                             |
| [**adPause**](./advertising.md#onadpause)                                   |                                             |
| [**adPlay**](./advertising.md#onadplay)                                     |                                             |
| [**adRequest**](./advertising.md#onadrequest)                               |                                             |
| [**adRequestedContentResume**](./advertising.md#onadrequestedcontentresume) |                                             |
| [**adSchedule**](./advertising.md#onadschedule)                             |                                             |
| [**adLoadedXML**](./advertising.md#onadloadedxml)                           |                                             |
| [**adSkipped**](./advertising.md#onadskipped)                               |                                             |
| [**adStarted**](./advertising.md#onadstarted)                               |                                             |
| [**adTime**](./advertising.md#onadtime)                                     |                                             |
| [**adViewableImpression**](./advertising.md#onadviewableimpression)         |                                             |
| [**adWarning**](./advertising.md#onadwarning)                               |                                             |
| [**adsManager**](./advertising.md#onadsmanager)                             |                                             |
| [**beforeComplete**](./advertising.md#onbeforecomplete)                     |                                             |
| [**beforePlay**](./advertising.md#onbeforeplay)                             |                                             |

<br>

## AUDIO TRACKS

오디오 트랙과 관련된 이벤트 그룹

| Event                                                          | Description |
| :------------------------------------------------------------- | :---------- |
| [**audioTrackChanged**](./audio-tracks.md#onaudiotrackchanged) |             |
| [**audioTracks**](./audio-tracks.md#onaudiotracks)             |             |

<br>

## BUFFER

플레이어 버퍼링과 관련된 이벤트 그룹

| Event                                          | Description                                                       |
| :--------------------------------------------- | :---------------------------------------------------------------- |
| [**bufferChange**](./buffer.md#onbufferchange) | 현재 재생 중인 미디어의 버퍼(로딩된 데이터 양)가 변할 때마다 호출 |

<br>

## CAPTIONS

자막과 관련된 이벤트 그룹

| Event                                                  | Description                                                              |
| :----------------------------------------------------- | :----------------------------------------------------------------------- |
| [**captionsChanged**](./captions.md#oncaptionschanged) | 자막 트랙을 변경했을 때 호출                                             |
| [**captionsList**](./captions.md#oncaptionslist)       | 사용 가능한 자막 트랙(caption tracks)을 처음 인식하거나 갱신했을 때 호출 |

<br>

## CAST

크롬캐스트(Chromecast)나 AirPlay 같은 외부 기기로의 영상 송출(cast)과 관련된 이벤트 그룹

| Event                                              | Description |
| :------------------------------------------------- | :---------- |
| [**cast**](./cast.md#oncast)                       |             |
| [**castIntercepted**](./cast.md#oncastintercepted) |             |

<br>

## CONTROLS

플레이어 제어 UI(재생/일시정지/음량/전체화면/트랙변경 등)와 관련된 이벤트 그룹

| Event                                            | Description |
| :----------------------------------------------- | :---------- |
| [**controls**](./controls.md#oncontrols)         |             |
| [**displayClick**](./controls.md#ondisplayclick) |             |

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
| [**metadataCueParsed**](./metadata.md#onmetadatacueparsed) |             |

<br>

## PLAYBACK

실제 재생 상태 변화와 관련된 이벤트 그룹

| Event                                                          | Description |
| :------------------------------------------------------------- | :---------- |
| [**autostartNotAllowed**](./playback.md#onautostartnotallowed) |             |
| [**buffer**](./playback.md#onbuffer)                           |             |
| [**complete**](./playback.md#oncomplete)                       |             |
| [**error**](./playback.md#onerror)                             |             |
| [**firstFrame**](./playback.md#onfirstframe)                   |             |
| [**idle**](./playback.md#onidle)                               |             |
| [**pause**](./playback.md#onpause)                             |             |
| [**play**](./playback.md#onplay)                               |             |
| [**playAttemptFailed**](./playback.md#onplayattemptfailed)     |             |
| [**playbackRateChanged**](./playback.md#onplaybackratechanged) |             |
| [**warning**](./playback.md#onwarning)                         |             |

<br>

## PLAYLIST

플레이리스트와 관련된 이벤트 그룹

| Event                                                    | Description |
| :------------------------------------------------------- | :---------- |
| [**nextClick**](./playlist.md#onnextclick)               |             |
| [**playlist**](./playlist.md#onplaylist)                 |             |
| [**playlistItem**](./playlist.md#onplaylistitem)         |             |
| [**playlistComplete**](./playlist.md#onplaylistcomplete) |             |

<br>

## QUALITY

영상/오디오의 품질과 관련된 이벤트 그룹

| Event                                             | Description |
| :------------------------------------------------ | :---------- |
| [**levels**](./quality.md#onlevels)               |             |
| [**levelsChanged**](./quality.md#onlevelschanged) |             |
| [**visualQuality**](./quality.md#onvisualquality) |             |

<br>

## RELATED

동영상이 끝난 뒤 관련 콘텐츠 표시와 관련된 이벤트 그룹

| Event                                               | Description |
| :-------------------------------------------------- | :---------- |
| [**relatedReady**](./related.md#onrelatedready)     |             |
| [**relatedOnclose**](./related.md#onrelatedonclose) |             |
| [**relatedOnopen**](./related.md#onrelatedonopen)   |             |
| [**relatedOnplay**](./related.md#onrelatedonplay)   |             |

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
| [**absolutePositionReady**](./seek.md#onabsolutepositionready) |             |
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
