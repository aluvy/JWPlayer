# Events Index

JWPlayer 이벤트 리스트

<br>

## SETUP

플레이어 셋업과 관련된 이벤트 그룹

| Event                                     | Description                                      |
| :---------------------------------------- | :----------------------------------------------- |
| [**ready**](./setup.md#onready)           | 플레이어 초기화가 완료되어 재생 준비가 끝난 시점 |
| [**remove**](./setup.md#onremove)         | 플레이어 인스턴스가 DOM에서 제거될 때            |
| [**setupError**](./setup.md#onsetuperror) | 플레이어 초기화가 실패했을 때                    |

<br>

## ALL

모든 이벤트를 수집하는 이벤트 그룹

| Event                     | Description                                                   |
| :------------------------ | :------------------------------------------------------------ |
| [**all**](./all.md#onall) | 플레이어 내부에서 발생하는 모든 이벤트가 트리거될 때마다 호출 |

<br>

## ADVERTISING

광고 기능과 관련된 이벤트 그룹

| Event                                                    | Description                                 |
| :------------------------------------------------------- | :------------------------------------------ |
| [**adBidRequest**](./advertising.md#onEvent)             | 광고 요청(bid request)을 보내기 직전에 발생 |
| [**adBidResponse**](./advertising.md#onEvent)            |                                             |
| [**adBlock**](./advertising.md#onEvent)                  |                                             |
| [**adBreakEnd**](./advertising.md#onEvent)               |                                             |
| [**adBreakIgnored**](./advertising.md#onEvent)           |                                             |
| [**adBreakStart**](./advertising.md#onEvent)             |                                             |
| [**adClick**](./advertising.md#onEvent)                  |                                             |
| [**adCompanions**](./advertising.md#onEvent)             |                                             |
| [**adComplete**](./advertising.md#onEvent)               |                                             |
| [**adError**](./advertising.md#onEvent)                  |                                             |
| [**adImpression**](./advertising.md#onEvent)             |                                             |
| [**adItem**](./advertising.md#onEvent)                   |                                             |
| [**adLoaded**](./advertising.md#onEvent)                 |                                             |
| [**adManager**](./advertising.md#onEvent)                |                                             |
| [**adMeta**](./advertising.md#onEvent)                   |                                             |
| [**adPause**](./advertising.md#onEvent)                  |                                             |
| [**adPlay**](./advertising.md#onEvent)                   |                                             |
| [**adRequest**](./advertising.md#onEvent)                |                                             |
| [**adRequestedContentResume**](./advertising.md#onEvent) |                                             |
| [**adSchedule**](./advertising.md#onEvent)               |                                             |
| [**adLoadedXML**](./advertising.md#onEvent)              |                                             |
| [**adSkipped**](./advertising.md#onEvent)                |                                             |
| [**adStarted**](./advertising.md#onEvent)                |                                             |
| [**adTime**](./advertising.md#onEvent)                   |                                             |
| [**adViewableImpression**](./advertising.md#onEvent)     |                                             |
| [**adWarning**](./advertising.md#onEvent)                |                                             |
| [**adsManager**](./advertising.md#onEvent)               |                                             |
| [**beforeComplete**](./advertising.md#onEvent)           |                                             |
| [**beforePlay**](./advertising.md#onEvent)               |                                             |

<br>

## AUDIO TRACKS

오디오 트랙과 관련된 이벤트 그룹

| Event                                              | Description |
| :------------------------------------------------- | :---------- |
| [**audioTrackChanged**](./audio-tracks.md#onEvent) |             |
| [**audioTracks**](./audio-tracks.md#onEvent)       |             |

<br>

## BUFFER

플레이어 버퍼링과 관련된 이벤트 그룹

| Event                                   | Description                                                       |
| :-------------------------------------- | :---------------------------------------------------------------- |
| [**bufferChange**](./buffer.md#onEvent) | 현재 재생 중인 미디어의 버퍼(로딩된 데이터 양)가 변할 때마다 호출 |

<br>

## CAPTIONS

자막과 관련된 이벤트 그룹

| Event                                        | Description                                                              |
| :------------------------------------------- | :----------------------------------------------------------------------- |
| [**captionsChanged**](./captions.md#onEvent) | 자막 트랙을 변경했을 때 발생                                             |
| [**captionsList**](./captions.md#onEvent)    | 사용 가능한 자막 트랙(caption tracks)을 처음 인식하거나 갱신했을 때 발생 |

<br>

## CAST

크롬캐스트(Chromecast)나 AirPlay 같은 외부 기기로의 영상 송출(cast)과 관련된 이벤트 그룹

| Event                                    | Description |
| :--------------------------------------- | :---------- |
| [**cast**](./cast.md#onEvent)            |             |
| [**castIntercepted**](./cast.md#onEvent) |             |

<br>

## CONTROLS

플레이어 제어 UI(재생/일시정지/음량/전체화면/트랙변경 등)와 관련된 이벤트 그룹

| Event                                     | Description |
| :---------------------------------------- | :---------- |
| [**controls**](./controls.md#onEvent)     |             |
| [**displayClick**](./controls.md#onEvent) |             |

<br>

## FLOATING PLAYER

플레이어가 화면 위에 떠(floating)서 위치가 바뀔 때와 관련된 이벤트 그룹

| Event                                     | Description |
| :---------------------------------------- | :---------- |
| [**float**](./floating-player.md#onEvent) |             |

<br>

## METADATA

미디어 파일 내부에 포함된 메타데이터(e.g., ID3, timed-metadata, 트랙 길이·제작자 정보 등)와 관련된 이벤트 그룹

| Event                                          | Description |
| :--------------------------------------------- | :---------- |
| [**meta**](./metadata.md#onEvent)              |             |
| [**metadataCueParsed**](./metadata.md#onEvent) |             |

<br>

## PLAYBACK

실제 재생 상태 변화와 관련된 이벤트 그룹

| Event                                            | Description |
| :----------------------------------------------- | :---------- |
| [**autostartNotAllowed**](./playback.md#onEvent) |             |
| [**buffer**](./playback.md#onEvent)              |             |
| [**complete**](./playback.md#onEvent)            |             |
| [**error**](./playback.md#onEvent)               |             |
| [**firstFrame**](./playback.md#onEvent)          |             |
| [**idle**](./playback.md#onEvent)                |             |
| [**pause**](./playback.md#onEvent)               |             |
| [**play**](./playback.md#onEvent)                |             |
| [**playAttemptFailed**](./playback.md#onEvent)   |             |
| [**playbackRateChanged**](./playback.md#onEvent) |             |
| [**warning**](./playback.md#onEvent)             |             |

<br>

## PLAYLIST

플레이리스트와 관련된 이벤트 그룹

| Event                                         | Description |
| :-------------------------------------------- | :---------- |
| [**nextClick**](./playlist.md#onEvent)        |             |
| [**playlist**](./playlist.md#onEvent)         |             |
| [**playlistItem**](./playlist.md#onEvent)     |             |
| [**playlistComplete**](./playlist.md#onEvent) |             |

<br>

## QUALITY

영상/오디오의 품질과 관련된 이벤트 그룹

| Event                                     | Description |
| :---------------------------------------- | :---------- |
| [**levels**](./quality.md#onEvent)        |             |
| [**levelsChanged**](./quality.md#onEvent) |             |
| [**visualQuality**](./quality.md#onEvent) |             |

<br>

## RELATED

동영상이 끝난 뒤 관련 콘텐츠 표시와 관련된 이벤트 그룹

| Event                                      | Description |
| :----------------------------------------- | :---------- |
| [**relatedReady**](./related.md#onEvent)   |             |
| [**relatedOnclose**](./related.md#onEvent) |             |
| [**relatedOnopen**](./related.md#onEvent)  |             |
| [**relatedOnplay**](./related.md#onEvent)  |             |

<br>

## RESIZE

플레이어 크기(가로·세로) 또는 전체화면(fullscreen)과 관련된 이벤트 그룹

| Event                                 | Description |
| :------------------------------------ | :---------- |
| [**fullscreen**](./resize.md#onEvent) |             |
| [**resize**](./resize.md#onEvent)     |             |

<br>

## SEEK

재생 위치를 이동(seek)과 관련된 이벤트 그룹

| Event                                          | Description |
| :--------------------------------------------- | :---------- |
| [**absolutePositionReady**](./seek.md#onEvent) |             |
| [**seek**](./seek.md#onEvent)                  |             |
| [**seeked**](./seek.md#onEvent)                |             |
| [**time**](./seek.md#onEvent)                  |             |

<br>

## SHARING

공유 기능(소셜 링크, 임베드 코드 등)과 관련된 이벤트 그룹

```javascript
jwplayer().on('ready', function (event) {
  sharingPlugin = jwplayer().getPlugin('sharing');
});
```

| Event                                      | Description |
| :----------------------------------------- | :---------- |
| [**sharingOnclick**](./sharing.md#onEvent) |             |
| [**sharingOnclose**](./sharing.md#onEvent) |             |
| [**sharingOnopen**](./sharing.md#onEvent)  |             |

<br>

## VIEWABILITY

플레이어가 화면에 얼마나 보여지는지(viewable)와 관련된 이벤트 그룹

| Event                                             | Description |
| :------------------------------------------------ | :---------- |
| [**containerViewable**](./viewability.md#onEvent) |             |
| [**viewable**](./viewability.md#onEvent)          |             |

<br>

## VOLUME

볼륨 변화(음량 변경, 음소거/음소거 해제)와 관련된 이벤트 그룹

| Event                             | Description |
| :-------------------------------- | :---------- |
| [**mute**](./volume.md#onEvent)   |             |
| [**volume**](./volume.md#onEvent) |             |
