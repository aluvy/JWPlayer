# Playlists

플레이리스트(playlist)는 **여러 개의 비디오 또는 오디오 파일을 재생하기 위해 사용되는 JWP 플레이어의 강력한 기능**입니다.

플레이리스트는 다음 중 하나의 데이터 유형으로 정의할 수 있습니다:

- RSS 피드 또는 JSON 파일의 URL을 참조하는 **문자열(string)**
- 여러 개의 미디어 객체를 포함하는 **배열(array)**

<br>

# playlist

`playlist`의 값은 각 미디어 항목의 **기본 파일(primary file), 관련 리소스(related assets)**, 및 **메타데이터(metadata)** 를 포함하는 **RSS 피드 또는 JSON 파일의 URL**입니다.

<br>

### playlist를 단일 URL로 지정하는 예시

```javascript
player.setup({
  playlist: 'http://example.com/myPlaylist.json',
});
```

> 참고:  
> 플레이리스트에 포함된 미디어 항목 목록을 담고 있는 URL을 `playlist` 값으로 지정하는 것을 권장합니다.  
> 단, 외부에 호스팅된(비-JWP) 피드가 없는 미디어로 플레이리스트를 구성해야 하는 **특수한 경우**에는 예외로 할 수 있습니다.

<br>

---

### Dashboard

다음 단계에 따라 **JWP 대시보드(JWP Dashboard)** 에서 JWP에 호스팅된 미디어를 포함한 **플레이리스트(playlist)** 를 생성할 수 있습니다.

1. JWP 대시보드에서 제공되는 **여러 유형의 맞춤형(Customizable) [플레이리스트](https://docs.jwplayer.com/platform/docs/vdh-playlist-overview) 중 하나를 생성**합니다.

2. 생성된 플레이리스트의 **Developers 탭**으로 이동하여 **JSON URL을 복사**합니다.

3. 플레이어 설정에서 `playlist` 속성에 해당 **JSON URL을 정의**합니다.

<br>

---

### API

다음 단계에 따라 **JWP API를 통해 JWP에 호스팅된 미디어를 포함한 플레이리스트**를 생성할 수 있습니다.

1. API를 사용하여 다음 중 하나의 **맞춤형 플레이리스트 유형**을 생성합니다:

- [Dynamic playlist](https://docs.jwplayer.com/platform/reference/post_v2-sites-site-id-playlists-dynamic-playlist)
- [Manual playlist](https://docs.jwplayer.com/platform/reference/post_v2-sites-site-id-playlists-manual-playlist)
- [Search playlist](https://docs.jwplayer.com/platform/reference/post_v2-sites-site-id-playlists-search-playlist)
- [Article Matching](https://docs.jwplayer.com/platform/reference/post_v2-sites-site-id-playlists-article-matching-playlist)
- [Recommendations playlist](https://docs.jwplayer.com/platform/reference/post_v2-sites-site-id-playlists-recommendations-playlist)

2. API 응답(Response)에서 `id` 값을 복사합니다.

3. 아래 URL에서 `{PLAYLIST_ID}` 자리 표시자를 복사한 id 값으로 교체합니다.

<br>

### Playlist URL

```
https://cdn.jwplayer.com/v2/playlists/{PLAYLIST_ID}
```

<br>

---

<br>

# playlist[]

`playlist`의 값은 **각 미디어 항목(media item)** 의 **기본 파일(primary file), 관련 리소스(related assets), 및 메타데이터(metadata)** 를 정의할 수 있는 **객체(object)** 입니다.

### 예시 (JavaScript)

```javascript
player.setup({
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

<br>

### file\* (string)

_(필수)_ 설정 또는 `sources`에서 지정된 **미디어 파일 경로**입니다.

<br>

### fwassetid\* (string)

_(FreeWheel - 필수)_ FreeWheel MRM에 구성된 콘텐츠의 **FreeWheel 식별자(Identifier)** 입니다.  
예: `DemoVideoGroup.01`

<br>

### adschedule (string \| array)

플레이리스트 내 특정 미디어 항목에 대한 **광고 구간(ad break) 스케줄링** 설정입니다.  
참고: [playlist[].adschedule](https://docs.jwplayer.com/players/reference/playlists#playlistfreewheel)

<br>

### description (string)

미디어 항목의 **간단한 설명 텍스트**입니다.  
이 설명은 제목 아래에 표시되며, `displaydescription` 옵션으로 숨길 수 있습니다.

<br>

### freewheel (object)

_(FreeWheel)_ FreeWheel 광고 클라이언트 설정입니다.  
참고: [playlist[].freewheel](https://docs.jwplayer.com/players/reference/playlists#playlistfreewheel)

<br>

### image (string)

**재생 전 및 재생 후 표시되는 포스터 이미지 URL** 입니다.

<br>

### link (string)

정의된 경우, 플레이어에서 영상을 공유할 때 **수신자가 이동할 링크 URL** 입니다.

<br>

### mediaid (string)

**미디어 항목의 고유 식별자(Unique ID)** 입니다.  
이 값은 광고, 분석, 추천(Discovery) 서비스에서 사용됩니다.

<br>

### minDvrWindow (number)

_(HLS 전용)_ DVR 모드가 트리거되기 위해 M3U8 내에 필요한 최소 콘텐츠 길이(초)입니다.  
항상 DVR 모드를 표시하려면 이 값을 `0`으로 설정합니다.

- 기본값: `120`

<br>

### sources (array)

**화질 전환(quality toggling) 및 대체 소스 설정**에 사용됩니다.  
참고: playlist.sources

<br>

### starttime (number)

미디어 항목의 **시작 시간(초 단위)** 입니다.

> 참고: MP4 파일 사용 시 `seek`, `seeked` 이벤트가 모두 트리거되며, DASH 또는 HLS 스트림에서는 발생하지 않습니다.

<br>

### streamtype (string)

_(FreeWheel)_ 해당 미디어 항목이 **라이브 스트리밍(live streaming)** 콘텐츠인지 여부를 나타냅니다.  
이 속성을 사용할 때 값은 `"live"`로 설정합니다.

> 주의: 동일한 미디어 객체 내에서 `playlist[].freewheel.streamtype`과 함께 사용하지 마십시오.

<br>

### title (string)

미디어 항목의 **제목(title)** 입니다.  
이 값은 재생 전 플레이어 내부 및 시각적 플레이리스트에서 표시되며, `displaytitle` 옵션으로 숨길 수 있습니다.

<br>

### tracks (array)

해당 미디어 항목의 **자막(captions), 챕터(chapters)** 또는 **썸네일(thumbnails)** 정보를 정의합니다.

<br>

### withCredentials (boolean)

`true`로 설정 시, CORS 대신 `withCredentials`를 사용하여 미디어 파일을 요청합니다.

<br>

> 추가 정보:  
> `title`, `description`, `mediaid`와 같은 기본 미디어 정보 외에도,  
> **사용자 정의 속성(custom properties)** 을 통해 추가 메타데이터를 삽입할 수 있습니다.  
> 이 정보는 반드시 **플레이리스트 내부에 정의되어야 하며**,  
> `setup` 블록 안에서 직접 설정할 수는 없습니다.

<br><br>

---

## playlist[].adschedule

`playlist[].adschedule` 속성은 **특정 플레이리스트 항목 내에서 광고 구간(ad break)을 예약(scheduling)** 하는 데 사용됩니다.

<br>

**플레이리스트 광고 스케줄(playlist ad schedule)은 다음 중 하나의 데이터 유형으로 정의할 수 있습니다:**

- 광고 태그(ad tag)의 URL을 참조하는 [문자열(string)](https://docs.jwplayer.com/players/reference/playlists#playlistadschedule-1)
- 여러 개의 광고 구간 객체(ad break objects)를 포함하는 [배열(array)](<(https://docs.jwplayer.com/players/reference/playlists#playlistadschedule-2)>)

<br>

### playlist.adschedule

외부 VMAP XML 파일(string) 에서 광고 스케줄을 불러올 수도 있습니다.

VMAP 예시

```json
"adschedule": "https://playertest.longtailvideo.com/adtags/vmap2-nonlinear.xml"
```

<br>

---

### playlist.adschedule[]

광고 구간(ad break)을 사용할 경우,  
`adschedule` 속성 내부에 최소 한 개 이상의 **광고 구간 객체(ad break object)** 를 정의해야 합니다.  
각 광고 구간에는 반드시 **offset** 과 **tag** (또는 **vastxml**)이 포함되어야 합니다.

JSON 예시

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

<br>

### offset\* (string \| number)

_(필수)_ 광고 태그를 재생할 시점을 지정합니다.

**가능한 값:**

- **(number)**: 지정된 초 단위 시점 이후 광고가 재생됩니다.
- **(xx%) (string - VAST 전용)**: 콘텐츠의 xx% 진행 시 광고가 재생됩니다.
- **pre (string)**: 프리롤(pre-roll) 광고로 재생됩니다.
- **post (string)**: 포스트롤(post-roll) 광고로 재생됩니다.

<br>

### tag\* (string \| array)

_(필수)_ 광고 태그의 URL 또는 배열을 정의합니다.

- **문자열(string)**: VAST 및 IMA 플러그인용 광고 태그 URL, 또는 FreeWheel용 문자열 플레이스홀더입니다.

- **배열(array)**: 하나 이상의 광고 태그가 실패할 경우를 대비한 대체(fallback) VAST 광고 태그 URL 목록입니다.

VAST 태그를 사용하는 경우, **GDPR 동의 정보 등**을 정의하기 위한 **광고 타기팅 매크로(ad tag targeting macros)** 를 추가할 수 있습니다.

> 주의: 동일한 광고 구간(ad break) 내에서 `playlist[].adschedule[].vastxml` 과 이 속성을 동시에 사용하지 마십시오.

<br>

### type (string)

광고 구간 내에서 제공되는 **광고 형식(format)** 을 지정합니다.

**가능한 값:**

- **linear**: 영상 재생을 중단하고 표시되는 비디오 광고
- **nonlinear**: 플레이어 일부 위에 오버레이되어 재생을 방해하지 않는 정적 광고 (이 경우 광고 큐포인트가 표시되지 않음)

선형(linear)과 비선형(nonlinear) 광고가 혼합되어 제공될 경우, **이 속성을 설정하지 마십시오.**  
플레이어는 선형 광고는 재생을 중단하고, 비선형 광고는 중단하지 않습니다.

<br>

### vastxml (string)

구성된 광고 구간에서 요청되는 **VAST XML 광고 태그** 입니다.

> 주의: 동일한 광고 구간(ad break) 내에서 `advertising.schedule[].tag` 와 함께 사용하지 마십시오.

<br><br>

---

## playlist[].freewheel

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

<br>

### adManagerURL (string)

FreeWheel **Ad Manager의 URL** 입니다.  
예: `https://mssl.fwmrm.net/libs/adm/6.24.0/AdManager.js`

> 참고: FreeWheel 솔루션 엔지니어(Solution Engineer) 또는 계정 담당자(Account Representative)를 통해 버전이 지정된 AdManager URL을 받을 수 있습니다.

<br>

### networkid (number)

FreeWheel 네트워크의 **식별자(Identifier)** 입니다.  
예: `96749`

<br>

### profileid (string)

FreeWheel 내 **특정 애플리케이션 환경(application environment)** 의 식별자입니다.  
예: `global-js`

<br>

### sectionid (string)

FreeWheel에서 **비디오 콘텐츠가 재생되는 위치(location)** 의 식별자입니다.  
예: `DemoSiteGroup.01`

<br>

### serverid (string)

FreeWheel **광고 서버(ad server)** 의 URL입니다.  
예: `http://demo.v.fwmrm.net/ad/g/1`

<br>

### streamtype (string)

_(FreeWheel 전용)_ 해당 미디어 항목이 **라이브 스트리밍(live streaming)** 콘텐츠인지 여부를 나타냅니다.  
이 속성을 사용할 때 값은 `"live"`로 설정합니다.

> 주의: 동일한 미디어 항목 객체 내에서 `playlist[].streamtype` 과 함께 사용하지 마십시오.

<br><br>

---

## playlist[].sources[]

`sources[]`를 사용하면 **하나의 미디어 항목(media item)** 에 대해 **여러 개의 소스(source)** 또는 **품질(quality)** 을 정의할 수 있습니다.

- **Multiple Sources**

  - `sources[]`에 서로 다른 파일 형식(file type)을 나열하면, 플레이어는 **미디어 파일의 순서**를 기반으로 **재생 실패 시(failover)** 사용할 **백업 순서(failover sequence)** 를 결정합니다.

  - > 참고: 파일이 로드된 후 스트리밍 중 오류가 발생하더라도, 플레이어는 다음 제공자(provider: HTML5, HLS, DASH 등)로 자동 전환하지 않습니다.

- **Multiple Qualities**

  - 동일한 파일 형식의 여러 화질(quality)을 `sources[]`에 나열하면, 플레이어는 시청자가 **품질 메뉴(quality menu)** 에서 선택할 수 있도록 다양한 품질 옵션을 표시합니다.  
    이 방식은 HLS 또는 DASH 와 같은 스트리밍 기술을 사용할 수 없을 때 유용합니다.
  - > 주의: 여러 품질이 정의된 경우, 플레이어는 네트워크 속도나 대역폭에 따라 **자동으로 품질을 전환하지 않습니다.**

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

<br>

### file\* (string)

_(필수)_ 이 소스에 해당하는 **비디오 파일, 오디오 파일, YouTube 영상, 또는 라이브 스트림의 URL** 입니다.

<br>

### default (boolean)

_(Multiple Qualities)_ `true`로 설정하면 **시작 시 기본으로 재생할 미디어 소스**로 지정됩니다.  
이 속성이 설정되지 않은 경우, 목록에서 **첫 번째 소스**가 기본값으로 사용됩니다.

<br>

### drm (object)

특정 소스의 DRM 정보를 정의합 니다.
참고: [playlist[].sources[].drm](https://docs.jwplayer.com/players/reference/playlists#playlistsourcesdrm)

<br>

### label (string)

_(Multiple Qualities)_ 수동 품질 선택 메뉴에 표시되는 **소스 라벨(화질명)** 입니다.  
두 가지 이상의 화질 옵션이 있을 때 설정하는 것을 권장합니다.

<br>

### onXhrOpen (function)

_(Multiple Sources)_ **HLS.js 재생 시 미디어 XHR 요청에 커스텀 헤더를 추가**할 수 있게 합니다.

`onXhrOpen` 콜백은 플레이어가 HLS 매니페스트(manifest), 키(key), 또는 세그먼트(segment) 요청을 보낼 때, `XMLHttpRequest.open()` 호출 후 `XMLHttpRequest.send()` 전에 실행됩니다.

> 중요: 이 기능은 **Safari 브라우저(네이티브 HLS 재생 환경)** 에서는 사용할 수 없습니다.

<br>

### type (string)

_(Multiple Qualities)_ **미디어 유형(media type)** 을 강제로 지정합니다.

> 참고: 이 속성은 `.php` 파일처럼 확장자가 없거나 인식되지 않는 파일에만 정의해야 합니다.

<br><br>

### playlist[].sources[].drm

DRM(디지털 권한 관리)을 사용할 때는,  
**해당 미디어 소스 내부에 `drm` 블록을 배치할 것을 권장합니다.**
이렇게 하면 **브라우저별로 올바른 미디어와 DRM 조합(media-DRM pair)** 이 정확히 선택됩니다.

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

<br>

<br>

### certificateUrl (string)

_(FairPlay)_ `keySession.certificateUrl` 초기화에 사용되는 세션 데이터의 일부로서, **인증서의 경로**를 지정합니다.

<br>

### key (string)

_(Clearkey)_ DRM 콘텐츠를 **복호화(decrypt)** 하는 데 필요한 **키 값(key)** 입니다.

<br>

### keyId (string)

_(Clearkey)_ MPD 파일의 `default_KID` 값으로 지정된 **키 ID** 입니다.

<br>

### processSpcUrl (function \| string)

_(FairPlay)_ **라이선스 서버(license server)** 의 경로를 지정합니다.  
URL을 동적으로 구성해야 할 경우, **해당 URL을 반환하는 커스텀 함수**를 이 속성에 전달할 수 있습니다.

<br>

### url (string)

_(Playready, Widevine)_ **라이선스 서버의 URL** 입니다.

<br>

> 자세한 내용은 [drm](https://docs.jwplayer.com/players/reference/drm) 섹션을 참조하십시오.

<br><br>

---

## playlist[].tracks[]

`track은` 미디어 항목에 **자막(captions), 썸네일(thumbnails)**, 또는 **챕터(chapters)** 정보를 추가할 때 사용됩니다.

- 썸네일(thumbnail)과 챕터(chapter) 파일은 **WEBVTT 형식**이어야 합니다.

- 자막(captions)은 **WEBVTT** 및 **SRT 형식**을 지원하지만,  
  **JWP에서는 WEBVTT 형식 사용을 강력히 권장합니다.**

- [WEBVTT 형식](https://docs.jwplayer.com/platform/docs/vdh-vtt-file-creation)

<br>

예시 (JSON)

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

<br><br>

### default (boolean)

_(자막 전용)_ `true`로 설정 시, **기본적으로 표시할 자막 트랙(captions track)** 을 지정합니다.

<br>

### file (string)

**자막(captions), 챕터(chapters)**, 또는 **썸네일(thumbnails)** 텍스트 트랙 파일의 URL 입니다.

<br>

### kind (string)

텍스트 트랙의 종류(kind) 를 지정합니다.

**가능한 값:**

- `captions`: (기본값) 비디오 재생 중 표시되는 자막
- `chapters`: 비디오의 특정 구간에 마커를 추가하거나 챕터를 구분
- `thumbnails`: 타임슬라이더 위에 마우스를 올렸을 때 표시되는 썸네일 목록

<br>

### label (string)

**텍스트 트랙의 라벨(이름)** 입니다.  
이 값은 여러 자막 트랙이 있는 설정에서, **자막 선택 메뉴(CC Selection Menu)** 에 표시됩니다.

- 기본값: `트랙의 인덱스 번호`
