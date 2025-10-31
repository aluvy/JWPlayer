# Events Index

그룹별로 정리 된 JWPlayer 이벤트 리스트

<br>

## SETUP

이 API 호출은 플레이어를 생성하고 설정 정보를 제공하는 데 사용됩니다.

| Event                                     | Description        |
| :---------------------------------------- | :----------------- |
| [**ready**](./setup.md#onready)           | 재생 준비 완료     |
| [**remove**](./setup.md#onremove)         | 플레이어 삭제      |
| [**setupError**](./setup.md#onsetuperror) | 플레이어 셋업 에러 |

<br>

## ALL

이 단일 API 호출은 플레이어의 API에서 모든 이벤트를 수집하는 데 사용할 수 있습니다.

| Event                     | Description          |
| :------------------------ | :------------------- |
| [**all**](./all.md#onall) | 모든 이벤트에 트리거 |

<br>

## ADVERTISING

이 API는 개발자에게 JWP의 광고판 기능에 대한 더 많은 제어 권한을 제공합니다.  
VAST 및 IMA 플러그인의 경우, 이 API는 인상 검증, 사용자 지정 스케줄링, 여러 동반자와 같은 작업을 가능하게 합니다.

| Event                                                    | Description |
| :------------------------------------------------------- | :---------- |
| [**adBidRequest**](./advertising.md#onEvent)             |             |
| [**adBidResponse**](./advertising.md#onEvent)            |             |
| [**adBlock**](./advertising.md#onEvent)                  |             |
| [**adBreakEnd**](./advertising.md#onEvent)               |             |
| [**adBreakIgnored**](./advertising.md#onEvent)           |             |
| [**adBreakStart**](./advertising.md#onEvent)             |             |
| [**adClick**](./advertising.md#onEvent)                  |             |
| [**adCompanions**](./advertising.md#onEvent)             |             |
| [**adComplete**](./advertising.md#onEvent)               |             |
| [**adError**](./advertising.md#onEvent)                  |             |
| [**adImpression**](./advertising.md#onEvent)             |             |
| [**adItem**](./advertising.md#onEvent)                   |             |
| [**adLoaded**](./advertising.md#onEvent)                 |             |
| [**adManager**](./advertising.md#onEvent)                |             |
| [**adMeta**](./advertising.md#onEvent)                   |             |
| [**adPause**](./advertising.md#onEvent)                  |             |
| [**adPlay**](./advertising.md#onEvent)                   |             |
| [**adRequest**](./advertising.md#onEvent)                |             |
| [**adRequestedContentResume**](./advertising.md#onEvent) |             |
| [**adSchedule**](./advertising.md#onEvent)               |             |
| [**adLoadedXML**](./advertising.md#onEvent)              |             |
| [**adSkipped**](./advertising.md#onEvent)                |             |
| [**adStarted**](./advertising.md#onEvent)                |             |
| [**adTime**](./advertising.md#onEvent)                   |             |
| [**adViewableImpression**](./advertising.md#onEvent)     |             |
| [**adWarning**](./advertising.md#onEvent)                |             |
| [**adsManager**](./advertising.md#onEvent)               |             |
| [**beforeComplete**](./advertising.md#onEvent)           |             |
| [**beforePlay**](./advertising.md#onEvent)               |             |

<br>

## AUDIO TRACKS

이 API 호출은 비디오의 여러 오디오 트랙이 제공되는 경우 오디오 트랙을 듣거나 업데이트하는 데 사용됩니다.

| Event                                              | Description |
| :------------------------------------------------- | :---------- |
| [**audioTrackChanged**](./audio-tracks.md#onEvent) |             |
| [**audioTracks**](./audio-tracks.md#onEvent)       |             |

<br>

## BUFFER

이 API 호출은 플레이어에 버퍼링된 파일의 비율로 클라이언트를 업데이트하는 데 사용됩니다.

| Event                                   | Description |
| :-------------------------------------- | :---------- |
| [**bufferChange**](./buffer.md#onEvent) |             |

<br>

## CAPTIONS

이 API 호출은 자막 관련 트랙이 로드되거나 업데이트하는 데 사용됩니다.

| Event                                        | Description |
| :------------------------------------------- | :---------- |
| [**captionsChanged**](./captions.md#onEvent) |             |
| [**captionsList**](./captions.md#onEvent)    |             |

<br>

## CAST

크롬캐스트(Chromecast)나 AirPlay 같은 외부 기기로 영상을 송출(cast)할 때 관련된 이벤트 그룹입니다.

| Event                                    | Description |
| :--------------------------------------- | :---------- |
| [**cast**](./cast.md#onEvent)            |             |
| [**castIntercepted**](./cast.md#onEvent) |             |

<br>

## CONTROLS

플레이어 제어 UI(재생/일시정지/음량/전체화면/트랙변경 등)와 관련된 이벤트입니다.  
예컨대 “컨트롤바가 보이거나 사라질 때”, “컨트롤 버튼이 눌렸을 때” 등이 이 그룹에 속할 수 있습니다.

이 API 호출을 통해 개발자는 내장된 플레이어 컨트롤(컨트롤 바 및 디스플레이 아이콘)과 상호 작용할 수 있습니다.

| Event                                     | Description |
| :---------------------------------------- | :---------- |
| [**controls**](./controls.md#onEvent)     |             |
| [**displayClick**](./controls.md#onEvent) |             |

<br>

## FLOATING PLAYER

페이지 스크롤 등으로 플레이어가 화면 위에 떠(floating)서 위치가 바뀔 때 관련된 이벤트입니다.  
플레이어가 고정 위치에서 떠서 다른 위치로 이동했거나 다시 원래 위치로 복귀할 때 발생합니다.

| Event                                     | Description |
| :---------------------------------------- | :---------- |
| [**float**](./floating-player.md#onEvent) |             |

<br>

## METADATA

미디어 파일 내부에 포함된 메타데이터(e.g., ID3, timed-metadata, 트랙 길이·제작자 정보 등)가 재생 중 특정 시점에 활성화되거나 탐지되었을 때 발생하는 이벤트입니다.

| Event                                          | Description |
| :--------------------------------------------- | :---------- |
| [**meta**](./metadata.md#onEvent)              |             |
| [**metadataCueParsed**](./metadata.md#onEvent) |             |

<br>

## PLAYBACK

실제 재생 상태 변화와 관련된 이벤트 그룹입니다.  
이 API 호출은 플레이어의 현재 재생 상태를 검색하고 변경하는 데 사용됩니다.

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

플레이리스트 아이템이 변경되거나 새로운 플레이리스트가 로드되었을 때 발생하는 이벤트 그룹입니다.

이러한 API 호출은 현재 재생 목록(하나 이상의 항목)을 로드하고 검색하는 데 사용되며, 재생 목록 항목 간을 탐색하는 데도 사용됩니다.  
API를 통해 액세스할 때 재생 목록은 하나 이상의 객체가 포함된 배열입니다.

| Event                                         | Description |
| :-------------------------------------------- | :---------- |
| [**nextClick**](./playlist.md#onEvent)        |             |
| [**playlist**](./playlist.md#onEvent)         |             |
| [**playlistItem**](./playlist.md#onEvent)     |             |
| [**playlistComplete**](./playlist.md#onEvent) |             |

<br>

## QUALITY

영상/오디오의 품질(예: 720p, 1080p, 비트레이트) 변경이나 사용자/자동으로 품질이 바뀔 때 관련된 이벤트입니다. 예컨대 .on('levelsChanged'), .on('visualQuality') 등이 포함됩니다.

이러한 API 호출은 동영상의 여러 품질 수준이 제공되는 경우 동영상 품질을 청취하거나 업데이트하는 데 사용됩니다. 품질 수준은 정렬되어 인덱스 번호가 부여됩니다.

| Event                                     | Description |
| :---------------------------------------- | :---------- |
| [**levels**](./quality.md#onEvent)        |             |
| [**levelsChanged**](./quality.md#onEvent) |             |
| [**visualQuality**](./quality.md#onEvent) |             |

<br>

## RELATED

동영상이 끝난 뒤 다음 추천 동영상 또는 관련 콘텐츠가 표시될 때, 또는 관련 콘텐츠 영역이 열리거나 닫힐 때 관련된 이벤트입니다. 이 그룹은 relatedPlugin을 사용하는 경우에 해당됩니다.

| Event                                      | Description |
| :----------------------------------------- | :---------- |
| [**relatedReady**](./related.md#onEvent)   |             |
| [**relatedOnclose**](./related.md#onEvent) |             |
| [**relatedOnopen**](./related.md#onEvent)  |             |
| [**relatedOnplay**](./related.md#onEvent)  |             |

<br>

## RESIZE

플레이어 크기(가로·세로) 또는 전체화면(fullscreen) 상태가 바뀌는 경우 발생하는 이벤트 그룹입니다. 예컨대 .on('resize'), .on('fullscreen') 등이 있습니다.

이 API 호출은 현재 플레이어의 치수와 전체 화면 상태를 검색하고 업데이트하는 데 사용됩니다.

| Event                                 | Description |
| :------------------------------------ | :---------- |
| [**fullscreen**](./resize.md#onEvent) |             |
| [**resize**](./resize.md#onEvent)     |             |

<br>

## SEEK

재생 위치를 이동(seek)했을 때 발생하는 이벤트 그룹입니다. 예컨대 .on('seek') 등이 포함됩니다.

이 API 호출은 현재 미디어 재생 위치를 검색하고 업데이트하는 데 사용됩니다.

| Event                                          | Description |
| :--------------------------------------------- | :---------- |
| [**absolutePositionReady**](./seek.md#onEvent) |             |
| [**seek**](./seek.md#onEvent)                  |             |
| [**seeked**](./seek.md#onEvent)                |             |
| [**time**](./seek.md#onEvent)                  |             |

<br>

## SHARING

공유 기능(소셜 링크, 임베드 코드 등)을 통해 공유 UI가 열리거나 닫힐 때 발생하는 이벤트 그룹입니다.
API 호출을 공유하는 것은 우리의 getPlugin() 메서드와 함께 작동합니다. 예를 들어, 모든 공유 인스턴스는 이 특정 플러그인을 참조하기 위해 getPlugin('공유') API 호출을 사용하고 있습니다. 다음은 우리의 공유 플러그인을 대상으로 할 것입니다:

```javascript
.on('ready', function(event){
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

플레이어가 화면에 얼마나 보여지는지(viewable) 또는 몇 %가 보이는지 등의 뷰어빌리티 상태가 바뀌었을 때 관련된 이벤트입니다. 예컨대 .on('viewable'), .on('containerViewable') 등이 해당됩니다.

| Event                                             | Description |
| :------------------------------------------------ | :---------- |
| [**containerViewable**](./viewability.md#onEvent) |             |
| [**viewable**](./viewability.md#onEvent)          |             |

<br>

## VOLUME

볼륨 변화(음량 변경, 음소거/음소거 해제)와 관련된 이벤트 그룹입니다. 예컨대 .on('volume'), .on('mute') 등이 포함됩니다.

| Event                             | Description |
| :-------------------------------- | :---------- |
| [**mute**](./volume.md#onEvent)   |             |
| [**volume**](./volume.md#onEvent) |             |
