# Captions

**Closed Captions Styling**

이 옵션 블록은 JWP 웹 및 iOS 플레이어용 자막(Closed Captions) 의 스타일(모양) 을 구성합니다.

<br>

---

**참고할 만한 두 가지 자막 설정 팁**

1. 자막이 **브라우저에서 렌더링될지, JW Player에서 렌더링될지**를 제어하려면,  
   `setup()`의 전역 설정에서 `[renderCaptionsNatively]`(https://docs.jwplayer.com/players/reference/setup-options#render-captions-natively) 속성을 지정하세요.

2. **iOS 및 Android**에서는 **FCC(미 연방통신위원회)** 규정에 따라  
   사용자가 **시스템 설정을 통해 자막 스타일을 직접 제어**할 수도 있습니다.

---

<br>

- **backgroundColor** (string)

  - 자막 문자 **배경의 색상**을 16진수(hex)로 지정합니다.
  - 기본값: `#000000`

- **backgroundOpacity** (number)

  - 자막 문자 **배경의 불투명도(투명도%)** 를 설정합니다.
  - 기본값: `75`

- **color** (string)

  - 자막 **텍스트 색상**을 16진수(hex)로 지정합니다.
  - 기본값: #ffffff

- **edgeStyle** (string)

  - 자막 문자가 **배경과 구분되는 방식(테두리 스타일)** 을 설정합니다.
  - 기본값: `none`

- **fontFamily** (string)

  - 자막 **텍스트의 폰트 패밀리(Font Family)** 를 지정합니다.
  - 기본값: `sans`

- **fontOpacity** (number)

  - 자막 **텍스트의 불투명도(투명도%)** 를 설정합니다.
  - 기본값: `100`

- **fontSize** (number)

  - 자막 **텍스트 크기(px)** 를 지정합니다.
  - 단, 브라우저가 자막을 렌더링하는 경우에는 크기에 영향을 주지 않습니다.
  - 기본값: `15`

- **windowColor** (string)

  - 자막 전체 영역의 **배경색**을 16진수(hex)로 지정합니다.
  - 기본값: `#000000`

- **windowOpacity** (number)

  - 자막 전체 영역의 **배경 불투명도(투명도%)** 를 설정합니다.
  - 기본값: `0`

<br>

> 참고: 자막 스타일을 설정할 때, color 값은 반드시 **16진수(hex 형식)** 로 지정해야 합니다.
