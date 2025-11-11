# Options

## Setup Options

플레이어의 레이아웃과 재생 동작(playback behavior) 을 설정하는 항목입니다.

- [Appearance](./setup.md#Appearance)
- [Behavior](./setup.md#Behavior)
- [Rendering and Loading](./setup.md#RenderingandLoading)

## Advertising

이 객체는 JW Player의 비디오 광고 기능을 구성합니다.

- [advertising.bids](/advertising.md#advertisingbids)
- [advertising.bids.bidders[]](/advertising.md#advertisingbidsbidders)
- [advertising.bids.bidders[].optionalParams](/advertising.md#advertisingbidsbiddersoptionalparams)
- [advertising.bids.ortbParams](/advertising.md#advertisingbidsortbparams)
- [advertising.bids.settings](/advertising.md#advertisingbidssettings)
- [advertising.bids.settings.buckets[]](/advertising.md#advertisingbidssettingsbuckets)
- [advertising.bids.settings.consentManagement](/advertising.md#advertisingbidssettingsconsentmanagement)
- [advertising.bids.settings.consentManagement.gdpr](/advertising.md#advertisingbidssettingsconsentmanagementgdpr)
- [advertising.bids.settings.consentManagement.gdpr.rules](/advertising.md#advertisingbidssettingsconsentmanagementgdprrules)
- [advertising.bids.settings.consentManagement.usp](/advertising.md#advertisingbidssettingsconsentmanagementusp)
- [advertising.companiondiv](/advertising.md#advertisingcompaniondiv)
- [advertising.enablePPS](/advertising.md#advertisingenablepps)
- [advertising.freewheel](/advertising.md#advertisingfreewheel)
- [advertising.rules](/advertising.md#advertisingrules)
- [advertising.schedule](/advertising.md#advertisingschedule)

## Auto Pause

플레이어가 특정 규칙에 따라 자동으로 일시정지되도록 설정합니다.

- [Auto Pause](./auto-pause.md#AutoPause)

## Captions

이 옵션 블록은 JWP 웹 및 iOS 플레이어용 자막(Closed Captions) 의 스타일(모양) 을 구성합니다.

- [Captions](./captions.md#captions)

## Casting

캐스팅(Casting) 기능을 사용하면 시청자가 Google Cast 또는 Apple AirPlay 기술을 이용해  
비디오 및 오디오 콘텐츠를 호환되는 TV 또는 사운드 시스템으로 스트리밍할 수 있습니다.

- [Casting](./casting.md)

## Common Media Client Data

공통 미디어 클라이언트 데이터, CMCD  
이 속성은 모든 HLS.js 또는 Shaka 요청에 CMCD(Common Media Client Data) 정보를 추가합니다.

- [Common Media Client Data](./common-media-client-data.md)

## Do Not Save Cookies

doNotSaveCookies 속성은 플레이어 내 데이터 수집을 제한합니다.

- [Do Not Save Cookies](./do-not-save-cookies.md)

## DRM

MPEG-DASH 스트림(PlayReady, Widevine, ClearKey) 및  
HLS 스트림(FairPlay)에 대한 DRM 관련 구성 옵션이 제공됩니다.

- [drm.playready](./drm.md#drm.playready)
- [drm.widevine](./drm.md#drm.widevine)
- [drm.widevine/playready.headers](./drm.md#drm.widevine/playready.headers)
- [drm.fairplay](./drm.md#drm.fairplay)
- [drm.clearkey](./drm.md#drm.clearkey)

## Float on scroll

이 기능은 페이지를 스크롤할 때 원래 위치의 플레이어가 화면 밖으로 벗어나면,  
플레이어를 화면의 한쪽 구석으로 축소해 계속 표시합니다.

- [Float on scroll](./float-on-scroll.md)

## Google Analytics (ga)

이 옵션은 Google Analytics(GA) 와의 내장 통합 기능(built-in integration) 을 구성합니다.

- [Google Analytics (ga)](./google-analytics.md)

## Internationalization

intl 객체는 새로운 언어 번역을 추가하거나,  
플레이어 텍스트 및 aria-label 값의 번역을 사용자 지정(customize) 하고,  
자동화된 플레이어 로컬라이제이션 기능(automated player localization) 을 활용할 수 있게 해줍니다.

- [intl.{lang}.advertising object](./intl.md#intllangadvertising-object)
- [intl.{lang}.captionsStyles object](./intl.md#intllangcaptionsStyles-object)
- [intl.{lang}.errors object](./intl.md#intllangerrors-object)
- [intl.{lang}.related object](./intl.md#intllangrelated-object)
- [intl.{lang}.sharing object](./intl.md#intllangsharing-object)
- [intl.{lang}.shortcuts object](./intl.md#intllangshortcuts-object)
- [Transition table](./intl.md#Transition-table)

## Logo

이 옵션 블록은 비디오 위에 오버레이되는 클릭 가능한 워터마크(logo) 를 설정합니다.

- [Logo](./logo.md)

## Playlists

플레이리스트(playlist)는 여러 개의 비디오 또는 오디오 파일을 재생하기 위해 사용되는 JWP 플레이어의 강력한 기능입니다.

- [playlist](./playlists.md#playlist)
- [playlist[]](./playlists.md#playlist-1)

## Related

이 객체는 추천(related) 플레이리스트의 표시 방식과 동작(behavior) 을 제어합니다.

- [Related](./related.md)

## Sharing

이 옵션 블록은 소셜 공유 설정 서브메뉴(submenu) 를 제어합니다.

- [Sharing](./sharing.md)

## Skin

W Player는 기본적으로 11가지 스킨 구성 옵션(skin configuration options) 을 제공합니다.

- [Color Customization](./skin.md#colorcustomization)

## Time slider

이 옵션 블록은 새로운 타임 슬라이더(time slider) 를 활성화하며, 슬라이더의 손잡이(thumb 또는 knob) 표시 여부를 제어할 수 있습니다.

- [Time slider](./time-slider.md)
