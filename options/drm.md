# DRM

> JW Player의 **Studio DRM**을 사용 중이라면,  
> JW Platform에서 [Studio DRM 적용하기(Apply Studio DRM with JW Platform)](https://docs.jwplayer.com/platform/docs/protection-studio-drm-jwp-web-player-integration) 문서를 참고하세요.

<br>

MPEG-DASH 스트림(PlayReady, Widevine, ClearKey) 및  
HLS 스트림(FairPlay)에 대한 **DRM 관련 구성 옵션**이 제공됩니다.

JW Player는 개별 **플레이리스트 소스(playlist source)** 에 DRM을 추가할 수 있는 기능을 포함하고 있습니다.  
이 방식을 사용하면 **여러 DRM 유형이 동시에 구성된 경우**,  
브라우저가 자동으로 **적합한 DRM 방식을 선택**하여 재생합니다.

따라서, 기존 구성은 이 새로운 방식으로 **업데이트할 것을 강력히 권장합니다.**

> 모든 DRM 보호 콘텐츠는 HTTPS 연결을 통해서만 제공되어야 합니다.

DRM에 대한 자세한 정보와 예시는 JW Player [지원 문서(Support Article)](<(https://docs.jwplayer.com/platform/docs/protection-overview)>) 를 참고하세요.

<br><br>

---

## drm.playready

**PlayReady DRM은 Windows 8.1 이상 운영체제의 Internet Explorer 11 및 Microsoft Edge** 브라우저에 특화된 DRM 방식입니다.

<br>

- **url\*** (string)

  - PlayReady 라이선스 서버의 URL을 지정합니다.

- **audioRobustness** (string) <sup>< 8.18.0+</sup>

  - 오디오 스트림의 **보안 강도(robustness level)** 를 설정합니다.
  - 가능한 값:
    - `HW_SECURE_ALL`
    - `HW_SECURE_CRYPTO`
    - `HW_SECURE_DECODE`
    - `SW_SECURE_CRYPTO`
    - `SW_SECURE_DECODE`

- **headers** (array)

  - PlayReady 라이선스 서버로 전송할 **사용자 정의 HTTP 헤더**를 지정합니다.
  - 자세한 내용은 [`headers`](https://docs.jwplayer.com/players/reference/drm#headers) 항목을 참고하세요.

- **licenseRequestFilter** (function)

  - 하나의 `request` 인자를 받는 함수를 지정합니다.
  - 이 함수는 **라이선스 요청이 전송되기 전에** 요청을 가로채어 수정할 수 있습니다.  
    (`licenseRequestHeaders`가 추가되기 전에 실행됩니다.)

- **licenseResponseFilter** (function)

  - 하나의 `response` 인자를 받는 **함수**를 지정합니다.
  - 이 함수는 **라이선스 응답이 세션에 적용되기 전에** 응답을 가로채어 수정할 수 있습니다.

- **videoRobustness** (string) <sup>< 8.18.0+</sup>

  - 비디오 스트림의 **보안 강도(robustness level)** 를 설정합니다.
  - 가능한 값:
    - `HW_SECURE_ALL`
    - `HW_SECURE_CRYPTO`
    - `HW_SECURE_DECODE`
    - `SW_SECURE_CRYPTO`
    - `SW_SECURE_DECODE`

<br><br>

---

## drm.widevine

**Widevine DRM**은 **iOS를 제외한 기기에서의 Google Chrome 브라우저**에 특화된 DRM 방식입니다.  
또한 **데스크톱 환경의 Firefox 브라우저**에서도 동작합니다.

<br>

- **url\*** (string)

  - Widevine **라이선스 서버의 URL**을 지정합니다.

- **audioRobustness** (string) <sup>< 8.18.0+</sup>

  - 오디오 스트림의 **보안 강도(robustness level)** 를 설정합니다.
  - 가능한 값:
    - `HW_SECE_ALL`
    - `HW_SECUR_CRIPTO`
    - `HW_SECE_DECODE`
    - `SW_SECUR_CRIPTO`
    - `SW_SECE_DECODE`

- **headers** (array)

  - Widevine 라이선스 서버 요청에 포함될 **사용자 정의 HTTP 헤더**를 지정합니다.
  - 자세한 내용은 [`headers`](https://docs.jwplayer.com/players/reference/drm#headers) 항목을 참고하세요.

- **licenseRequestFilter** (function)

  - 단일 인자 `request`를 받는 함수를 지정합니다.
  - 이 함수는 **라이선스 요청이 전송되기 전에 요청을 가로채어 수정할 수 있는 필터**입니다.  
    (`licenseRequestHeaders`가 추가되기 전에 실행됩니다.)

- **licenseResponseFilter** (function)

  - 단일 인자 `response`를 받는 함수를 지정합니다.
  - 이 함수는 **라이선스 응답이 세션에 적용되기 전에 응답을 가로채어 수정할 수 있는 필터**입니다.

- **serverCertificateUrl** (string)

  - Widevine **서비스 인증서(service certificate)** 의 URL을 지정합니다.

- **videoRobustness** (string) <sup>< 8.18.0+</sup>

  - 비디오 스트림의 **보안 강도(robustness level)** 를 설정합니다.
  - 가능한 값:
    - `HW_SECE_ALL`
    - `HW_SECUR_CRIPTO`
    - `HW_SECE_DECODE`
    - `SW_SECUR_CRIPTO`
    - `SW_SECE_DECODE`

<br><br>

---

## drm.\[widevine\/playready\].headers

`headers` 구성을 사용하면 **라이선스 요청(license request)에 사용자 정의 HTTP 헤더 데이터(customized HTTP header data)** 를 추가할 수 있습니다.  
이 설정은 과거 `widevine` 및 `playready` 환경에서 사용되던 **정적 `customData`** 구성 옵션을 대체합니다.

또한, `"headers"` 배열에 **여러 개의 객체를 포함하여 복수의 사용자 정의 헤더**를 추가하는 것도 가능합니다.

DRM 구성 예시

```json
"drm": {
  "playready": {
    "url": "mydrmserver.com",
    "headers": [{
      "name": "customData",
      "value": "hereismycustomdatastring"
    }]
  }
}
```

이전 버전의 구성 방식

이전 버전에서는 `"customData"` 속성을 직접 지정하는 형태로 사용되었습니다.

```json
"drm": {
  "playready": {
    "url": "mydrmserver.com",
    "customData": "hereismycustomdatastring"
  }
}
```

<br>

- **name** (string)

  - 요청에 포함될 **HTTP 헤더의 이름**을 지정합니다.

- **value** (string)

  - 요청에 포함될 **HTTP 헤더의 값**을 지정합니다.

<br><br>

---

## drm.fairplay

JW Player는 **맞춤형 FairPlay 통합(custom FairPlay integrations)** 을 위한 구성 옵션을 제공합니다.
맞춤 FairPlay DRM 통합에 대한 자세한 정보와 예시는 JW Player [지원 문서](https://docs.jwplayer.com/players/reference/drm#drmfairplay)를 참고하세요.

<br>

- **certificateUrl\*** (string)

  - keySession을 초기화할 때 사용되는 **세션 데이터의 일부인 인증서(certificate)의 경로**를 지정합니다.
  - certificateUrl 속성은 FairPlay 인증서 파일의 URL을 의미합니다.

- **processSpcUrl\*** (function \| string)

  - **CKC(Content Key Context)** 를 제공하는 **라이선스 서버(Server Playback Context)** 의 경로를 지정합니다.
  - 서버의 직접 URL을 기대하며,  
    만약 URL을 동적으로 생성해야 하는 경우에는 URL을 반환하는 **사용자 정의 함수**를 이 옵션에 전달할 수 있습니다.

- **extractContentId** (function)

  - 이 옵션은 다음 조건을 만족하는 함수를 기대합니다.
    - `needkey` 이벤트로부터 전달된 **initData URI(문자열로 변환된 값)** 를 인자로 받아,
    - `keySession` 초기화에 사용되는 **세션 데이터의 일부인 contentId** 를 반환하는 함수입니다.

- **extractKey** (function)

  - 이 옵션은 다음 조건을 만족하는 함수를 기대합니다.
    - 라이선스 서버에서 반환된 **CKC** 를 인자로 받아,  
      활성 키 세션을 갱신하는 데 사용되는 **키(key)** 를 반환하는 함수입니다.
    - 키를 비동기적으로 추출해야 하는 경우(예: blob 응답에서 바이트 데이터를 읽는 경우),  
      이 함수는 **Promise** 를 반환할 수 있습니다.

- **licenseRequestFilter** (function)

  - 단일 인자 `request`를 받는 함수를 기대합니다.
  - 라이선스 요청 필터는 **`licenseRequestHeaders`가 추가되기 전에**  
    라이선스 요청을 가로채어 수정하거나 확인할 수 있습니다.

- **licenseRequestHeaders** (array)

  - 라이선스 서버 요청에 포함될 헤더 정보를 지정합니다.
  - **헤더 이름(name)** 과 **값(value)** 속성을 가진 객체의 배열을 기대합니다.
    ```javascript
    licenseRequestHeaders: [
      { name: 'Authorization', value: 'Bearer token' },
      { name: 'X-Custom-Header', value: 'example' },
    ];
    ```

- **licenseRequestMessage** (function)

  - 이 옵션은 다음 조건을 만족하는 함수를 기대합니다.
    - 라이선스 키 메시지를 인자로 받아,  
      라이선스 서버로 전송할 메시지를 반환하는 함수입니다.
    - 기본값인 `licenseResponseType: "arraybuffer"` 설정에서는  
      이 함수가 `keymessage` 이벤트의 `message` 속성을 그대로 전달합니다.

- **licenseResponseFilter** (function)

  - 단일 인자 `response`를 받는 함수를 기대합니다.
  - 라이선스 응답 필터는 **세션에 라이선스 키를 적용하기 전**  
    응답을 가로채어 수정하거나 검사할 수 있습니다.

- **licenseResponseType** (string)

  - 라이선스 서버로부터의 **XHR 요청 응답 데이터 형식**을 지정합니다.
  - 기본값은 `arraybuffer`이며, 다음과 같은 형식을 사용할 수 있습니다.
    - `blob`
    - `json`
    - `text`
  - 이 옵션은 `licenseRequestMessage`의 처리 방식에도 영향을 미칩니다.

<br><br>

---

## drm.clearkey

**ClearKey**는 플레이어 설정 내에 복호화 키(decryption key)를 직접 명시하는 **가장 기본적인 형태의 DRM**입니다.

이 방식은 **보안 수준이 가장 낮지만, 브라우저 간 구현이 가장 간단**합니다.

이 방법을 사용할 경우 콘텐츠 복호화를 위한 **추가적인 서버 리소스가 필요하지 않습니다.**

ClearKey는 **Chrome**과 **Firefox** 브라우저에서 지원됩니다.

<br>

- **key\*** (string)

  - DRM 콘텐츠를 복호화하기 위해 필요한 **키 값**을 지정합니다.

- **keyId\*** (string)

  - `mpd` 파일의 `default_KID` 값으로 지정된 **키 식별자(Key ID)** 를 설정합니다.
