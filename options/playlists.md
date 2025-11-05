# Playlists

재생 목록은 여러 비디오 또는 오디오 파일을 재생하는 데 사용되는 JWP 플레이어의 강력한 기능입니다.

재생 목록은 다음 데이터 유형 중 하나로 정의할 수 있습니다:

- RSS 피드 또는 JSON 파일의 URL을 참조하는 문자열
- 여러 미디어 객체가 포함된 배열

## playlist

`playlist`의 값은 각 미디어 항목의 기본 파일, 관련 자산 및 메타데이터를 포함하는 RSS 피드 또는 JSON 파일을 참조하는 단일 URL입니다.

```javascript
playlist.setup({
  playlist: 'http://example.com/myPlaylist.json',
});
```

> 피드에 없는 외부 호스팅(JWP가 아닌) 미디어에서 재생 목록을 생성하는 등 특별한 사용 사례가 없는 한 재생 목록에 포함된 미디어 항목 목록을 포함하는 URL로 재생 목록 값을 설정하는 것이 좋습니다.

### Dashboard

다음 단계를 따라 JWP 대시보드에서 JWP 호스팅 미디어로 재생 목록을 생성하세요:

1. JWP 대시보드를 통해 여러 유형의 사용자 지정 가능한 [playlists](https://docs.jwplayer.com/platform/docs/vdh-playlist-overview) 중 하나를 만드세요.
2. 재생 목록의 **개발자 탭**에서 **JSON URL**을 복사합니다.
3. JSON URL을 사용하여 재생 목록 속성을 정의합니다.

### API

다음 단계를 따라 JWP API에서 JWP 호스팅 미디어로 재생 목록을 생성합니다:

1. API를 통해 여러 유형의 사용자 지정 가능한 재생 목록 중 하나를 만드세요:

- [Dynamic playlist](https://docs.jwplayer.com/platform/reference/post_v2-sites-site-id-playlists-dynamic-playlist)
- [Manual playlist](https://docs.jwplayer.com/platform/reference/post_v2-sites-site-id-playlists-manual-playlist)
- [Search playlist](https://docs.jwplayer.com/platform/reference/post_v2-sites-site-id-playlists-search-playlist)
- [Article Matching](https://docs.jwplayer.com/platform/reference/post_v2-sites-site-id-playlists-article-matching-playlist)
- [Recommendations playlist](https://docs.jwplayer.com/platform/reference/post_v2-sites-site-id-playlists-recommendations-playlist)

2. Copy the `id` from the API response.

3. In the following URL, replace the `{PLAYLIST_ID}` placeholder with the `id` value.

```
https://cdn.jwplayer.com/v2/playlists/{PLAYLIST_ID}
```

<br><br>

## playlist[]

`playlist`의 값은 각 미디어 항목의 기본 파일, 관련 자산 및 메타데이터를 정의할 수 있게 해주는 객체입니다.

```javascript
jwplayer("myElement").setup({
    playlist: [
        {
            "file": "/assets/sintel.mp4",
            "mediaid": "d0dra573",
            "title": "Basic Playlist",
            "description": "A basic playlist item with a media item not hosted by JWP.",
            "image": "/assets/sintel.jpg",
            "starttime": 10,
            "adschedule": [...],
            "tracks": [...]
        },
        {
            "sources": [...],
            "mediaid": "ddr1x3v2",
            "title": "Playlist Item with Multiple Sources or Qualities",
            "description": "A playlist item with multiple media sources and a custom header and DVR window for the HLS source.",
            "image": "/assets/bigbuckbunny.jpg"
        },
        {
            "file": "/assets/bigbuckbunny_live_feature.m3u8",
            "mediaid": "ddrx3v3",
            "title": "Live Streaming Item with FreeWheel Asset ID",
            "description": "A live streaming playlist item that has a FreeWheel asset ID and streamtype within the freewheel object.",
            "fwassetid": "big_buck_bunny_live",
            "freewheel": {...},
            "streamtype": "live"
        }
    ]
});
```

- **file\*** (string)

  - (필수) 설정 또는 소스에 지정된 미디어 파일

- **fwassetid\*** (string)

  - (FreeWheel - 필수) FreeWheel MRM에서 구성된 콘텐츠의 FreeWheel 식별자
  - 예제: DemoVideoGroup.01

- **adschedule** (string \| array)

  - 재생 목록의 특정 미디어 항목에 대한 광고 중단 예약
  - 참조: [playlist[].adschedule](https://docs.jwplayer.com/players/reference/playlists#playlistfreewheel)

- **description** (string)

  - 항목에 대한 간단한 설명
  - 이 설명은 제목 아래에 표시됩니다. `displaydescription` 옵션으로 숨길 수 있습니다.

- **freewheel** (object)

  - (FreeWheel) 프리휠 광고 클라이언트 설정
  - 참조: [playlist[].freewheel](https://docs.jwplayer.com/players/reference/playlists#playlistfreewheel)

- **image** (string)

  - 재생 전후에 표시되는 포스터 이미지 URL

- **link** (string)

  - 정의된 경우, 플레이어가 동영상을 공유하면 수신자가 이 URL로 이동합니다

- **mediaid** (string)

  - 미디어 항목의 고유 식별자
  - 이 값은 광고, 분석 및 검색 서비스에서 사용됩니다.

- **minDvrWindow** (number)

  - (HLS-only) DVR 모드를 트리거하는 데 필요한 M3U8 콘텐츠의 최소 용량(초)
  - DVR 모드를 항상 표시하려면 이 속성을 0으로 설정하세요. 기본값은 120입니다.

- **sources** (array)

  - 고품질 토글 및 대체 소스에 사용됩니다.
  - \[playlist.sources\]() 참조

- **starttime** (number)

  - 미디어 항목을 시작하는 데 걸리는 시간(초)
  - 참고: MP4 비디오 파일과 함께 사용하면 [seek](https://docs.jwplayer.com/players/reference/javascript-player-api-introduction#section-jwplayer-on-seek-) 이벤트와 [seeked](https://docs.jwplayer.com/players/reference/javascript-player-api-introduction#section-jwplayer-on-seeked-) 이벤트가 모두 트리거됩니다. DASH 또는 HLS 스트림과 함께 사용하면 두 이벤트 모두 트리거되지 않습니다.

- **streamtype** (string)

  - (FreeWheel) 미디어 항목이 라이브 스트리밍 자산인지 여부를 나타냅니다
  - 이 속성을 사용할 때, 값을 라이브로 설정하세요.
  - 참고: 이 속성과 동일한 미디어 항목 객체 내에서 [playlist[].freewheel.streamtype](https://docs.jwplayer.com/players/reference/playlists#freewheel-streamtype)를 사용하지 마십시오.

- **title** (string)

  - 항목 제목
  - 재생 전 플레이어 내부와 시각적 재생 목록에 표시됩니다. `displaytitle` 옵션으로 숨길 수 있습니다.

- **tracks** (array)

  - 미디어 항목의 캡션, 챕터 또는 썸네일

- **withCredentials** (boolean)

  - true 면, WithCredentials는 CORS 대신 미디어 파일을 요청하는 데 사용됩니다.

<br>

표준 미디어 정보(`title`, `description`, `mediaid`) 외에도 사용자 지정 속성을 사용하여 추가 메타데이터를 삽입할 수도 있습니다. 이 정보는 재생 목록 내부에 입력해야 하며 설정 블록 내부에 직접 설정할 수 없습니다.

### playlist[].adschedule

`playlist[.adschedule]` 속성은 특정 `playlist` 항목 전반에 걸쳐 광고 중단을 예약하는 데 사용됩니다.

재생 목록 광고 일정은 다음 데이터 유형 중 하나로 정의할 수 있습니다:

- 광고 태그의 URL을 참조하는 [문자열](https://docs.jwplayer.com/players/reference/playlists#playlistadschedule-1)
- 여러 개의 광고 중단 객체가 포함된 [배열](https://docs.jwplayer.com/players/reference/playlists#playlistadschedule-2)

#### playlist.adschedule

외부 VMAP XML(스트링)에서 광고 일정을 로드할 수 있습니다.

```
"adschedule": "https://playertest.longtailvideo.com/adtags/vmap2-nonlinear.xml"
```

#### playlist.adschedule[]

광고 중단을 사용할 때는 `adschedule` 속성 내에 구성된 최소한 하나의 광고 중단을 정의해야 합니다. 각 광고 중단에는 `offset`과 `tag` 또는 `vastxml`이 포함되어야 합니다.

```json
"adschedule": [
    {
        "offset": "pre",
        "tag": "myAdTag.xml"
    },
    {
        "offset": 10,
        "tag": "myMidroll.xml",
        "type": "nonlinear"
    }
]
```

- **offset\*** (string \| number)

  - (필수) 구성된 광고 태그를 재생할 시기
  - 가능한 가치:
    - `(number)`: (number) 지정된 초 후에 광고가 재생됩니다
    - `(xx%)` (string - VAST 전용) 광고는 콘텐츠의 xx% 다음에 재생됩니다
    - `pre`: (string) 광고가 프리롤로 재생됩니다
    - `post`: (string) 광고가 포스트 롤로 재생됩니다

- **tag\*** (string \| array)

  - (필수) 문자열, VAST 및 IMA 플러그인용 광고 태그의 URL 또는 FreeWheel용 문자열 자리 표시자인 경우
  - (VAST/IMA) 배열의 경우, 하나 이상의 광고 태그가 렌더링에 실패할 경우 Fallback으로 사용할 VAST 광고 태그의 URL
  - VAST 태그를 사용할 때, [매크로를 타겟팅하는 광고 태그](https://docs.jwplayer.com/platform/docs/ad-tag-targeting-macro-reference)를 추가하여 GDPR 동의와 같은 기능을 정의할 수 있습니다.
  - 참고: 이 속성과 동일한 광고 중단 시간 내에 [playlist[].adschedule[].vastxml](https://docs.jwplayer.com/players/reference/playlists#vastxml)을 사용하지 마십시오.

- **type** (string)

  - 광고 중단 기간 내에 제공될 광고 형식을 나타내는 속성
  - 가능한 값
    - `linear`: 비디오 콘텐츠 재생을 방해하는 비디오 광고
    - `nonlinear`: 플레이어의 일부를 겹치고 재생을 방해하지 않는 정적 디스플레이 광고. 이 광고 중단에 대한 광고 큐포인트는 표시되지 않습니다.
  - 광고 중단 시간 내에 선형 광고와 비선형 광고가 혼합된 경우 이 속성을 설정하지 마십시오. 플레이어는 선형 광고의 경우 동영상 재생을 중단하고 비선형 광고의 경우 동영상 재생을 중단하지 않습니다.

- **vastxml** (string)

  - 구성된 광고 중단 중에 요청되는 방대한 XML 광고 태그
  - 참고: 이 속성과 [advertising.schedule[].tag](https://docs.jwplayer.com/players/reference/playlists#tag)를 동일한 광고 시간 내에 사용하지 마십시오.

### playlist[].freewheel

```json
"freewheel": {
    "networkid": 12345,
    "adManagerURL": "https://mssl.fwmrm.net/libs/adm/6.24.0/AdManager.js",
    "serverid": "http://demo.v.fwmrm.net/ad/g/1",
    "profileid": "12345:html5_test",
    "sectionid": "test_site_section",
    "streamtype": "live"
}
```

- **adManagerURL** (string)

  - 프리휠 광고 관리자의 URL
  - 예: `https://mssl.fwmrm.net/libs/adm/6.24.0/AdManager.js`
  - 참고: 프리휠 솔루션 엔지니어 또는 계정 담당자가 AdManager 버전 URL을 제공할 수 있습니다.

- **networkid** (number)

  - 네트워크의 프리휠 식별자
  - 예: `96749`

- **profileid** (string)

  - 특정 애플리케이션 환경의 FreeWheel 식별자
  - 예제: `global-js`

- **sectionid** (string)

  - 비디오 콘텐츠가 재생되는 위치의 FreeWheel 식별자
  - 예제: `DemoSiteGroup.01`

- **serverid** (string)

  - 무료 휠 어댑터 서버 URL
  - 예를 들어 `http://demo.v.fwmrm.net/ad/g/1`

- **streamtype** (string)

  - (FreeWheel)는 미디어 항목이 라이브 스트리밍 자산인지 여부를 나타냅니다.
  - 이 속성을 사용할 때 값을 설정합니다.
  - 참고: 이 속성과 동일한 미디어 항목 객체 내에서 [playlist[].streamtype](https://docs.jwplayer.com/players/reference/playlists#streamtype)를 사용하지 마십시오.

### playlist[].sources[]

`sources[]`를 사용하여 미디어 항목의 여러 소스 또는 품질을 정의할 수 있습니다.

- **Multiple Sources**

  - `sources[]`에 다양한 파일 유형을 나열하면, 플레이어는 미디어 파일의 순서를 사용하여 제공자(HTML5, HLS 또는 DASH)가 파일을 로드하지 못할 경우 페일오버 순서를 결정합니다.
  - 참고: 파일이 성공적으로 로드되었지만 스트리밍 중에 오류가 발생하면 플레이어는 목록의 다음 공급자로 전환하지 않습니다.

- **Multiple Qualities**

  - `sources[]`에서 동일한 파일 유형의 다양한 파일 품질을 나열하면, 플레이어는 시청자가 품질 메뉴에서 선택할 수 있는 다양한 품질 옵션을 나열합니다.
  - 이 접근 방식은 HLS나 DASH와 같은 스트리밍 기술을 사용할 수 없을 때 유용합니다.
  - 참고: 여러 품질이 정의되면 플레이어는 대역폭이나 다운로드 속도에 맞게 자동으로 품질 간을 전환하지 않습니다.

<br>

Multiple Sources

```json
"sources": [
    {
        "file": "myVideo.m3u8",
        "onXhrOpen": function(xhr, url) {
            xhr.setRequestHeader('customHeader', 'customHeaderValue');
        }
    },{
        "file": "myVideo.mp4"
    },{
        "file": "myVideo.webm"
    }
]
```

Multiple Qualities

```json
"sources": [
    {
        "file": "myVideo-720p.mp4",
        "label": "HD"
    },{
        "file": "myVideo-480p.mp4",
        "label": "SD",
        "default": true
    }
]
```

- **file\*** (string)

  - (필수) 이 재생 목록 항목 소스의 동영상 파일, 오디오 파일, YouTube 동영상 또는 라이브 스트림 URL

- **default** (boolean)

  - (다중 품질) true로 설정되면 시작 시 재생할 특정 미디어 소스를 설정합니다.
  - 이 속성이 품질 소스에 대해 설정되지 않은 경우, 먼저 나열된 소스가 사용됩니다.

- **drm** (object)

  - 특정 출처에 대한 DRM 정보
  - 참조: [playlist[].sources[].drm](https://docs.jwplayer.com/players/reference/playlists#playlistsourcesdrm)

- **label** (string)

  - (다중 품질) 수동 품질 선택 메뉴에 표시되는 미디어 소스의 라벨
  - 동영상의 품질이 두 가지 이상인 경우 설정하세요.

- **onXhrOpen** (function)

  - (여러 출처) HLSjs 재생을 위해 미디어 XHR 요청에 사용자 지정 헤더를 추가할 수 있습니다
  - `onXhrOpen` 콜백을 사용하여 미디어 XHR 요청에 사용자 지정 헤더를 추가할 수 있습니다. 이 콜백은 `XMLHTTPRequest.open()` 이후와 플레이어가 수행한 HLS 매니페스트, 키 및 세그먼트 요청에 대해 `XMLHTTPRequest.send()` 이전에 실행됩니다.
  - 중요: 이것은 HLS가 네이티브로 재생되는 Safari 브라우저에서는 사용할 수 없습니다.

- **type** (string)

  - (다중 품질) 미디어 유형 강제
  - 참고: 이 속성은 .php 또는 특정 토큰을 사용하는 등 파일 확장자가 없거나 인식되지 않는 경우에만 정의해야 합니다.

#### playlist[].sources[].drm

DRM을 사용할 때는 적절한 미디어 소스 내부에 drm 블록을 배치하는 것이 좋습니다. 이렇게 하면 올바른 미디어와 DRM 쌍이 적절한 브라우저에 맞게 선택됩니다.

Clearkey

```json
"sources": [
    {
        "file": "myClearkeyStream.mpd",
        "drm": {
            "clearkey": {
                "key": "1234clear5678key",
                "keyId": "fefde00d-efde-adbf-eff1-baadf01dd11d"
            }
        }
    }
]
```

FairPlay

```json
"sources": [
    {
        "file": "myFairplayStream.m3u8",
        "drm": {
            "fairplay": {
                "certificateUrl": "http://myfairplay.com/fairplay/cert",
                "processSpcUrl": "http://myfairplay.com/fairplay/ckc"
            }
        }
    }
]
```

Playready

```json
"sources": [
    {
        "file": "myPlayreadyStream.mpd",
        "drm": {
            "playready": {
                "url": "http://myplayreadyurl.com/drm"
            }
        },
    }
]
```

Widevine

```json
"sources": [
    {
        "file": "myWidevineStream.mpd",
        "drm": {
            "widevine": {
                "url": "http://mywidevineurl.com/drm"
            }
        }
    }
]
```

- **certificateUrl** (string)

  - (Fairplay) `keySession.certificateUrl`을 초기화하는 데 사용되는 세션 데이터의 일부인 인증서 경로입니다

- **key** (string)

  - (Clearkey) DRM 콘텐츠를 해독하는 데 필요한 키

- **keyId** (string)

  - (Clearkey) mpd의 `default_KID` 값에 지정된 키 ID

- **processSpcUrl** (function \| string)

  - (Fairplay) 라이선스 서버 경로
  - URL을 동적으로 구성해야 하는 경우, URL을 반환하는 사용자 지정 기능을 이 구성 옵션으로 전달할 수 있습니다

- **url** (string)

  - 라이선스 서버의 (Playready, Widevine) URL

<br>

자세한 내용은 [drm](https://docs.jwplayer.com/players/reference/drm) 섹션을 참조하세요.

### playlist[].tracks[]

트랙은 캡션, 썸네일 또는 챕터를 위해 미디어에 첨부할 수 있습니다:

- 썸네일 및 챕터 파일은 [WEBVTT 형식](https://docs.jwplayer.com/platform/docs/vdh-vtt-file-creation)이어야 합니다.
- 캡션은 WEBVTT 및 SRT 형식을 사용할 수 있습니다. JWP는 [WEBVTT 형식](https://docs.jwplayer.com/platform/docs/vdh-vtt-file-creation)을 강력히 권장합니다.

```json
"tracks": [
    {
        "file": "https://cdn.jwplayer.com/tracks/abcd1234.vtt",
        "kind": "chapters",
        "label": "Big Buck Bunny"
    },
    {
        "file": "https://cdn.jwplayer.com/tracks/abce1235.vtt",
        "kind": "captions",
        "label": "English"
    },
    {
        "file": "https://cdn.jwplayer.com/strips/J8iBKS1l-120.vtt",
        "kind": "thumbnails"
    }
]
```

- **default** (boolean)

  - (Captions only) true로 설정하면 기본적으로 표시할 특정 캡션 트랙을 설정합니다

- **file** (string)

  - 캡션, 챕터 또는 썸네일 텍스트 트랙 파일의 URL

- **kind** (string)

  - 일종의 텍스트 트랙
  - 가능한 값:
    - `captions` (Default): 비디오 재생 중에 표시되는 캡션
    - `chapters`: 비디오에 마커를 배치하거나 다른 섹션을 표시합니다
    - `thumbnails`: 마우스 커서가 타임슬라이더를 흔들 때 나타나는 썸네일 목록

- **label** (string)

  - 텍스트 트랙의 라벨
  - 이 값은 CC 선택 메뉴에 레이블이 표시되는 여러 캡션이 있는 설정에서만 사용됩니다.
  - Default: `(텍스트 트랙의 인덱스)`
