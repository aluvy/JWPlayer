# Internationalization

`intl` 객체는 **새로운 언어 번역을 추가하거나**,  
**플레이어 텍스트 및 aria-label 값의 번역을 사용자 지정(customize)** 하고,  
**자동화된 플레이어 로컬라이제이션 기능(automated player localization)** 을 활용할 수 있게 해줍니다.

<br>

> 주의:  
> `intl` 객체 외부에서 정의된 **기존 언어 커스터마이징 또는 번역 설정**은  
> **자동 로컬라이제이션 및 `intl` 객체 내 설정을 모두 덮어씁니다.**
>
> 만약 `intl` 외부에서 언어 설정을 이미 구성한 경우,  
> 아래 표를 참고하여 해당 값을 `intl.{lang}.{property}` 구조로 복사해 사용하세요.

<br>

각 언어별로, **두 글자 언어 코드** 또는 **로케일(locale) 기반 코드**를 사용하여  
언어별 객체를 정의합니다.  
아래 코드 예시와 표를 참고하여 `intl` 객체를 구성하세요.

<br>

#### JavaScript 예시

```javascript
player.setup({
    ...
    "intl": {

        // Quebec French
        "fr-ca": {
            "play": "reproduire"
        },

        // French
        "fr": {
            "replay": "Répéter",
            "play": "jouer"
        },

        // Spanish
        "es": {
            "replay": "Repetir"
        },

        // Frisian
        "fy": {
            "advertising": {
                "loadingAd": "Advertinsje lade"
            }
        }
    }
});
```

---

<br>

## 주요 속성

### advertising (object)

광고 객체의 텍스트 및 ARIA 레이블을 현지화합니다.  
참고: _advertising object_

<br>

### airplay (string)

컨트롤 바에서 **Apple AirPlay 캐스팅 아이콘**의 툴팁 및 `aria-label` 텍스트입니다.

- 기본값: `Airplay`

<br>

### audioTracks (string)

**오디오 트랙 메뉴 아이콘**의 툴팁 및 `aria-label` HTML 속성 텍스트입니다.

- 기본값: `Audio Tracks`

<br>

### auto (string)

플레이어가 자동으로 최적 품질을 선택할 수 있게 하는 **품질 선택 옵션**의 라벨 및 `aria-label` 텍스트입니다.

- 기본값: `Auto`

<br>

### buffer (string)

플레이어가 **버퍼링 상태**일 때의 `aria-label` 텍스트입니다.

- 기본값: `Loading`

<br>

### captionsStyles (object) <sup>8.12.3+</sup>

자막 스타일 메뉴 현지화 설정입니다.  
참고: _captionsStyles object_

<br>

### cast (string)

**Google Chromecast 아이콘**의 툴팁 및 `aria-label` HTML 속성 텍스트입니다.

- 기본값: `Chromecast`

<br>

### cc (string)

**자막(Closed Captions) 메뉴 아이콘**의 툴팁 및 `aria-label` HTML 속성 텍스트입니다.

- 기본값: `Closed Captions`

<br>

### close (string)

메뉴 또는 오버레이를 닫는 아이콘의 툴팁 및 `aria-label` HTML 속성 텍스트입니다.

- 기본값: `Close`

<br>

### disabled (string)

기능을 비활성화할 때 표시되는 레이블 텍스트입니다.

- 기본값: `Disabled`

<br>

### enabled (string)

기능을 활성화할 때 표시되는 레이블 텍스트입니다.

- 기본값: `Enabled`

<br>

### errors (object)

오류 메시지 현지화 설정입니다.  
참고: _errors object_

<br>

### exitFullscreen (string) <sup>8.7.0+</sup>

전체화면 모드일 때 컨트롤 바의 **전체화면 아이콘**에 표시되는 툴팁 및 `aria-label` 텍스트입니다.

- 기본값: `Exit Fullscreen`

<br>

### fullscreen (string)

**전체화면 아이콘**의 툴팁 및 `aria-label` HTML 속성 텍스트입니다.

- 기본값: `Fullscreen`

<br>

### hd (string)

**화질(HD) 설정 메뉴 아이콘**의 툴팁 및 `aria-label` HTML 속성 텍스트입니다.

- 기본값: `Quality`

<br>

### liveBroadcast (string)

컨트롤 바에서 **라이브 스트림**의 라벨 텍스트 및 `aria-label` HTML 속성입니다.

- 기본값: `Live`

<br>

### logo (string)

플레이어 내 **로고**의 `aria-label` HTML 속성입니다.

- 기본값: `Logo`

<br>

### mute (string) <sup>8.7.0+</sup>

플레이어가 **음소거되지 않았을 때**, 컨트롤 바의 볼륨 아이콘 툴팁 및 `aria-label` 텍스트입니다.

- 기본값: `Mute`

<br>

### next (string)

여러 페이지로 구성된 오버레이에서 **오른쪽 화살표 버튼**의 `aria-label` 텍스트입니다.

- 기본값: `Next`

<br>

### nextUp (string)

**다음 자동 재생 항목(Next Up)** 오버레이의 제목 텍스트 및 `aria-label` 속성입니다.

- 기본값: `Next Up`

<br>

### notLive (string)

컨트롤 바에서 **현재 재생 위치가 실시간보다 지연됨을 표시하는 라벨 및 aria-label** 텍스트입니다.

- 기본값: `Not Live`

<br>

### off (string)

기능을 끌 때 표시되는 메뉴 옵션 텍스트입니다.

- 기본값: `Off`

<br>

### pause (string)

컨트롤 바의 **일시정지(Pause)** 아이콘 `aria-label` HTML 속성입니다.

- 기본값: `Pause`

<br>

### pipIcon (string)

**화면 속 화면(PiP)** 아이콘의 툴팁 및 `aria-label` HTML 속성 텍스트입니다.

- 기본값: `Picture in Picture (PiP)`

<br>

### play (string)

**재생(Play)** 아이콘의 `aria-label` HTML 속성입니다.

- 기본값: `Play`

<br>

### playback (string)

플레이어 대기 화면의 **재생 버튼 아래에 표시되는 안내문(Call-to-action)** 텍스트입니다.

- 기본값: `Play`

<br>

### playbackRates (string)

**재생 속도 조절 메뉴**의 툴팁 및 `aria-label` HTML 속성 텍스트입니다.

- 기본값: `Playback Rates`

<br>

### player (string)

비디오 플레이어 애플리케이션의 `aria-label` HTML 속성입니다.

- 기본값: `Video Player`

<br>

### ⚠️ playlist (string) <sup>< 8.8.0</sup>

_(지원 중단됨)_  
플레이리스트 오버레이의 툴팁, 제목, 및 `aria-label` 텍스트입니다.  
JWP 8.8.0 이상에서는 `related.heading` 속성을 대신 사용해야 합니다.

- 기본값: `Playlist`

<br>

### poweredBy (string)

우클릭 메뉴에서 **JW Player 이름 및 로고 앞에 표시되는 텍스트**입니다.

- 기본값: `Powered by`

<br>

### prev (string)

여러 페이지로 구성된 오버레이에서 **왼쪽 화살표 버튼**의 `aria-label` HTML 속성입니다.

- 기본값: `Previous`

<br>

### related (object)

관련(추천) 콘텐츠 텍스트 및 ARIA 레이블 현지화 설정입니다.  
참고: _related object_

<br>

### replay (string)

영상 재생이 완료된 후 **재시작 버튼(Replay)** 의 툴팁 및 `aria-label` HTML 속성 텍스트입니다.

- 기본값: `Replay`

<br>

### reset (string)

옵션을 **기본값으로 재설정**하는 버튼의 레이블 텍스트입니다.

- 기본값: `Reset`

<Br>

### rewind (string)

컨트롤 바의 **10초 되감기 버튼**의 툴팁 및 `aria-label` HTML 속성 텍스트입니다.

- 기본값: `Rewind 10 Seconds`

<br>

### settings (string)

**설정(Settings)** 메뉴 아이콘의 툴팁 및 `aria-label` HTML 속성 텍스트입니다.

- 기본값: `Settings`

<br>

### sharing (object)

공유(Sharing) 메뉴 관련 텍스트 및 ARIA 레이블 현지화 설정입니다.  
참고: _sharing object_

<br>

### shortcuts (object) <sup>8.11.0+</sup>

키보드 단축키(Shortcuts) 메뉴 현지화 설정입니다.  
참고: _shortcuts object_

<br>

### slider (string)

**비디오 탐색 바(scrub bar)** 의 `aria-label` HTML 속성입니다.

- 기본값: `Seek`

<br>

### stop (string)

**라이브 스트림용 정지(Stop) 버튼**의 `aria-label` HTML 속성입니다.

- 기본값: `Stop`

<br>

### unmute (string) <sup>8.7.0+</sup>

플레이어가 음소거 상태일 때, 컨트롤 바의 볼륨 아이콘 툴팁 및 `aria-label` HTML 속성 텍스트입니다.

- 기본값: `Unmute`

<br>

### videoInfo (string)

**우클릭 메뉴 버튼**의 레이블 텍스트 및 `aria-label` HTML 속성입니다.

- 기본값: `About This Video`

<br>

### volume (string)

컨트롤 바의 **볼륨 아이콘** 툴팁 및 `aria-label` HTML 속성 텍스트입니다.

- 기본값: `Volume`

<br>

### volumeSlider (string)

**볼륨 슬라이더**의 `aria-label` HTML 속성입니다.

- 기본값: `Volume`

<br><br>

---

## intl.{lang}.advertising object

이 객체는 **플레이어의 광고(advertising) 관련 텍스트 및 ARIA 레이블(접근성 속성)** 을 현지화합니다.

### admessage (string)

광고의 **남은 재생 시간**을 표시하는 **카운트다운 메시지 텍스트**입니다.

- 기본값: `This ad will end in xx`

<br>

### cuetext (string)

콘텐츠가 **광고임을 표시하는 툴팁 텍스트** 및 `aria-label` HTML 속성입니다.  
이 텍스트는 사용자가 **타임 슬라이더의 예약된 광고 마커(cue marker)** 위에 마우스를 올렸을 때 표시됩니다.

- 기본값: `Advertisement`

<br>

### displayHeading (string)

**광고 제목(Heading)** 으로 표시되는 텍스트입니다.

- 기본값: `Advertisement`

<br>

### loadingAd (string)

**광고가 로딩 중일 때 표시되는 텍스트**입니다.

- 기본값: `Loading ad`

<br>

### podmessage (string)

**광고 묶음(ad pod)** 재생 중에 표시되는 텍스트입니다.  
`__AD_POD_CURRENT__`는 현재 재생 중인 광고 번호를, `__AD_POD_LENGTH__`는 총 광고 개수를 나타냅니다.

- 기본값: `Ad __AD_POD_CURRENT__ of __AD_POD_LENGTH__.`

<br>

### skipmessage (string)

광고를 **건너뛸 수 있기까지 남은 시간**을 표시하는 **스킵 카운트다운 메시지 텍스트**입니다.

- 기본값: `Skip ad in xx`

<br>

### skiptext (string)

광고를 건너뛸 수 있을 때 표시되는 **버튼 텍스트** 및 `aria-label` HTML 속성입니다.

- 기본값: `Skip`

<br><br>

---

## intl.{lang}.captionsStyles object

이 객체는 플레이어 내 자막 스타일링 메뉴(captions styling menu) 의 텍스트를 현지화합니다.

### backgroundColor (string)

자막의 **배경색(background color)** 을 제어하는 서브메뉴 이름입니다.

- 기본값: `Background Color`

<br>

### backgroundOpacity (string)

자막의 배경 **투명도(background transparency)** 를 제어하는 서브메뉴 이름입니다.

- 기본값: `Background Opacity`

<br>

### black (string)

색상 옵션 이름입니다.

- 기본값: `Black`

<br>

### blue (string)

색상 옵션 이름입니다.

- 기본값: `Blue`

<br>

### edgeStyle (string)

자막의 `문자 장식(text decoration)` 을 제어하는 서브메뉴 이름입니다.

- 기본값: `Character Edge`

<br>

### cyan (string)

색상 옵션 이름입니다.

- 기본값: `Cyan`

<br>

### depressed (string)

자막 스타일 중, **텍스트가 눌린 듯한(음각)** 형태를 나타내는 옵션의 라벨입니다.

- 기본값: `Depressed`

<br>

### dropShadow (string)

자막 텍스트에 **그림자(shadow)** 를 추가하는 스타일 옵션의 라벨입니다.

- 기본값: `Drop Shadow`

<br>

### color (string)

자막의 **글자색(font color)** 을 제어하는 서브메뉴 이름입니다.

- 기본값: `Font Color`

<br>

### fontFamily (string)

자막의 **글꼴(font)** 을 제어하는 서브메뉴 이름입니다.

- 기본값: `Font Family`

<br>

### fontOpacity (string)

자막의 **텍스트 투명도(transparency)** 를 제어하는 서브메뉴 이름입니다.

- 기본값: `Font Opacity`

<br>

### userFontScale (string)

자막의 **글자 크기(size)** 를 제어하는 서브메뉴 이름입니다.

- 기본값: `Font Size`

<br>

### green (string)

색상 옵션 이름입니다.

- 기본값: `Green`

<br>

### magenta (string)

색상 옵션 이름입니다.

- 기본값: `Magenta`

<Br>

### none (string)

`옵션을 선택하지 않음(Off)` 상태를 나타내는 메뉴 항목의 라벨입니다.

- 기본값: `None`

<br>

### raised (string)

자막 텍스트가 **도드라져 보이는(양각)** 스타일의 라벨입니다.

- 기본값: `Raised`

<br>

### red (string)

색상 옵션 이름입니다.

- 기본값: `Red`

<Br>

### subtitleSettings (string)

자막 스타일을 변경할 수 있는 **설정 메뉴의 제목(heading)** 입니다.

- 기본값: `Subtitle Settings`

<br>

### uniform (string)

자막 텍스트가 **윤곽선(outlined)** 형태로 표시되는 스타일 옵션의 라벨입니다.

- 기본값: `Uniform`

<br>

### white (string)

색상 옵션 이름입니다.

- 기본값: `White`

<br>

### windowColor (string)

자막의 **창 배경색(window color)** 을 제어하는 서브메뉴 이름입니다.

- 기본값: `Window Color`

<br>

### windowOpacity (string)

자막 **창의 투명도(window transparency)** 를 제어하는 서브메뉴 이름입니다.

- 기본값: `Window Opacity`

<br>

### yellow (string)

색상 옵션 이름입니다.

- 기본값: `Yellow`

<br><br>

---

## intl.{lang}.errors object

이 객체는 **플레이어에 표시되는 오류 메시지(error messages)** 를 현지화합니다.

<br>

### badConnection (string)

**인터넷 연결 문제로 인해 재생이 불가능할 때** 표시되는 오류 메시지 텍스트입니다.

- 기본값: `This video cannot be played because of a problem with your internet connection.`

<br>

### cantLoadPlayer (string)

네트워크 이외의 이유로 **플레이어 로딩(instantiate)에 실패했을 때** 표시되는 오류 메시지 텍스트입니다.  
예: 잘못된 JSON 형식 또는 라이선스 키 오류

- 기본값: `Sorry, the video player failed to load.`

<br>

### cantPlayInBrowser (string)

브라우저 지원 문제로 인해 **영상 재생을 시작할 수 없을 때** 표시되는 오류 메시지 텍스트입니다.  
예: Flash 오류, DASH 재생 오류, 브라우저 미지원 등

- 기본값: `The video cannot be played in this browser.`

<br>

### cantPlayVideo (string)

**미디어 항목 로드 실패 시** 표시되는 오류 메시지 텍스트입니다.

- 기본값: `This video file cannot be played.`

<br>

### errorCode (string)

**숫자형 오류 코드(Error Code)** 에 대한 라벨 텍스트입니다.  
예: `Error code: 50244402`

- 기본값: `Error Code`

<br>

### liveStreamDown (string)

**라이브 스트림에 기술적 문제가 발생했거나 종료된 경우** 표시되는 오류 메시지 텍스트입니다.

- 기본값: `The live stream is either down or has ended.`

<br>

### protectedContent (string)

**DRM 또는 보호된 콘텐츠 접근 실패 시** 표시되는 오류 메시지 텍스트입니다.

- 기본값: `There was a problem providing access to protected content.`

<br>

### technicalError (string)

다른 오류 메시지 조건이 해당되지 않을 때 표시되는 **일반(기본) 기술적 오류 메시지 텍스트**입니다.

- 기본값: `This video cannot be played because of a technical error.`

<br><br>

---

## intl.{lang}.related object

이 객체는 **플레이어 내 추천 영상(related object)** 의 텍스트와 **ARIA 레이블(접근성 속성)** 을 현지화합니다.

### autoplaymessage (string)

**다음 영상이 재생되기까지 남은 시간**을 표시하는 카운트다운 메시지 텍스트입니다.

- 기본값: `Next up in xx`

<br>

### heading (string)

추천 영상 인터페이스의 **버튼 텍스트, 오버레이 제목(heading), 및 ARIA 레이블 HTML 속성**을 정의합니다.

- 기본값: `More Videos`

<br><br>

---

## intl.{lang}.sharing object

이 객체는 **공유(Sharing) 기능**의 플레이어 텍스트와 **ARIA 레이블(접근성 속성)** 을 현지화합니다.

### copied (string)

공유 메뉴(Sharing menu)에서 **링크가 클립보드에 복사되었을 때** 표시되는 툴팁 텍스트 및 `aria-label` HTML 속성입니다.

- 기본값: `Copied`

<br>

### email (string)

공유 메뉴에서 **이메일로 영상 링크를 전송하는 옵션**의 라벨 텍스트 및 `aria-label` HTML 속성입니다.

- 기본값: `Email`

<br>

### embed (string)

공유 메뉴에서 **임베드 코드(Embed code)를 클립보드에 복사하는 옵션**의 라벨 텍스트 및 `aria-label` HTML 속성입니다.

- 기본값: `Embed`

<br>

### heading (string)

컨트롤 바(control bar)에 있는 **공유 버튼의 툴팁 텍스트** 및 `aria-label` HTML 속성입니다.

- 기본값: `Share`

<br>

### link (string)

공유 메뉴에서 **영상 링크를 복사하는 옵션**의 라벨 텍스트 및 `aria-label` HTML 속성입니다.

- 기본값: `Link`

<br><br>

---

## intl.{lang}.shortcuts object <sup>8.11.0 +</sup>

이 객체는 **비디오 플레이어의 키보드 단축키 메뉴 항목**을 현지화합니다.

### captionsToggle (string)

**자막 표시를 켜거나 끄는 단축키 동작**에 대한 설명입니다.

- 기본값: `Captions On/Off`

<br>

### decreaseVolume (string)

**비디오 볼륨을 낮추는 단축키 동작**에 대한 설명입니다.

- 기본값: `Decrease Volume`

<br>

### fullscreenToggle (string)

**전체화면 전환(켜기/끄기) 단축키 동작**에 대한 설명입니다.

- 기본값: `Fullscreen/Exit Fullscreen`

<br>

### increaseVolume (string)

**비디오 볼륨을 높이는 단축키 동작**에 대한 설명입니다.

- 기본값: `Increase Volume`

<br>

### keyboardShortcuts (string)

비디오 플레이어에서 **사용 가능한 모든 키보드 단축키 목록의 제목(heading)** 입니다.

- 기본값: `Keyboard Shortcuts`

<br>

### playPause (string)

**재생/일시정지(toggle)** 단축키 동작에 대한 설명입니다.

- 기본값: `Play/Pause`

<br>

### seekBackward (string)

비디오를 **5초 뒤로 이동**하는 단축키 동작에 대한 설명입니다.

- 기본값: `Seek Backward`

<br>

### seekForward (string)

비디오를 **5초 앞으로 이동**하는 단축키 동작에 대한 설명입니다.

- 기본값: `Seek Forward`

<br>

### seekPercent (string)

비디오의 **10% 단위 위치로 빠르게 이동**하는 단축키 동작에 대한 설명입니다.
예: 숫자 5 키를 누르면 영상의 50% 지점으로 이동하며, 2 키를 누르면 20% 지점으로 이동합니다.

- 기본값: `Seek %`

<br>

### spacebar (string)

키보드의 **스페이스바(spacebar)** 키 이름입니다.

- 기본값: `SPACE`

<br>

### volumeToggle (string)

**음소거/음소거 해제(Mute/Unmute)** 단축키 동작에 대한 설명입니다.

- 기본값: `Mute/Unmute`

<br><br>

---

## Transition table

아래 표는 **이전 버전의 커스터마이징 또는 번역 값(old customization/translation values)** 을  
해당하는 **새로운 `intl.{lang}.{property}` 구조로 이전할 때** 참고할 수 있는 대응 관계를 보여줍니다.

| Old property                                   | New Property                           |
| :--------------------------------------------- | :------------------------------------- |
| `advertising.admessage`                        | `intl.{lang}.advertising.admessage`    |
| `advertising.cuetext`                          | `intl.{lang}.advertising.cuetext`      |
| `advertising.podmessage`                       | `intl.{lang}.advertising.podmessage`   |
| `advertising.skipmessage`                      | `intl.{lang}.advertising.skipmessage`  |
| `advertising.skiptext`                         | `intl.{lang}.advertising.skiptext`     |
| `localization.airplay`                         | `intl.{lang}.airplay`                  |
| `localization.audioTracks`                     | `intl.{lang}.audioTracks`              |
| `localization.buffer`                          | `intl.{lang}.buffer`                   |
| `localization.cast`                            | `intl.{lang}.cast`                     |
| `localization.cc`                              | `intl.{lang}.cc`                       |
| `localization.close`                           | `intl.{lang}.close`                    |
| `localization.copied 8.1.8+`                   | `intl.{lang}.sharing.copied`           |
| `localization.errors.badConnection8.4.0+`      | `intl.{lang}.errors.badConnection`     |
| `localization.errors.cantLoadPlayer 8.4.0+`    | `intl.{lang}.errors.cantLoadPlayer`    |
| `localization.errors.cantPlayInBrowser 8.4.0+` | `intl.{lang}.errors.cantPlayInBrowser` |
| `localization.errors.cantPlayVideo 8.4.0+`     | `intl.{lang}.errors.cantPlayVideo`     |
| `localization.errors.errorCode 8.4.0+`         | `intl.{lang}.errors.errorCode`         |
| `localization.errors.liveStreamDown 8.4.0+`    | `intl.{lang}.errors.liveStreamDown`    |
| `localization.errors.protectedContent 8.4.0+`  | `intl.{lang}.errors.protectedContent`  |
| `localization.errors.technicalError 8.4.0+`    | `intl.{lang}.errors.technicalError`    |
| `localization.fullscreen`                      | `intl.{lang}.fullscreen`               |
| `localization.hd`                              | `intl.{lang}.hd`                       |
| `localization.liveBroadcast`                   | `intl.{lang}.liveBroadcast`            |
| `localization.loadingAd`                       | `intl.{lang}.advertising.loadingAd`    |
| `localization.more`                            | `intl.{lang}.next`                     |
| `localization.next`                            | `intl.{lang}.next`                     |
| `localization.nextUp`                          | `intl.{lang}.nextUp`                   |
| `localization.nextUpClose`                     | `intl.{lang}.close`                    |
| `localization.pause`                           | `intl.{lang}.pause`                    |
| `localization.play`                            | `intl.{lang}.play`                     |
| `localization.playback`                        | `intl.{lang}.playback`                 |
| `localization.playbackRates`                   | `intl.{lang}.playbackRates`            |
| `localization.player`                          | `intl.{lang}.player`                   |
| `localization.playlist`                        | `intl.{lang}.playlist`                 |
| `localization.prev`                            | `intl.{lang}.prev`                     |
| `localization.related`                         | `intl.{lang}.related.heading`          |
| `localization.replay`                          | `intl.{lang}.replay`                   |
| `localization.rewind`                          | `intl.{lang}.rewind`                   |
| `localization.stop`                            | `intl.{lang}.stop`                     |
| `localization.videoInfo`                       | `intl.{lang}.videoInfo`                |
| `localization.volume`                          | `intl.{lang}.volume`                   |
| `related.autoplaymessage`                      | `intl.{lang}.related.autoplaymessage`  |
| `sharing.heading`                              | `intl.{lang}.sharing.heading`          |
| `sharing.link`                                 | `intl.{lang}.sharing.link`             |

<br>

> 참고:  
> 이전 `localization` 또는 `advertising`, `sharing` 객체를 사용 중이라면,  
> 위 표에 따라 모든 문자열 키를 새로운 `intl` **네임스페이스 구조**로 마이그레이션해야 합니다.
