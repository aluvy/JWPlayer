# Captions

**Closed Captions Styling**

이 옵션 블록은 JWP 웹 및 iOS 플레이어용 자막(Closed Captions) 의 스타일(모양) 을 구성합니다.

---

**참고할 만한 두 가지 자막 설정 팁**

1. 자막이 **브라우저에서 렌더링될지, JW Player에서 렌더링될지**를 제어하려면,  
   `setup()`의 전역 설정에서 `[renderCaptionsNatively]`(https://docs.jwplayer.com/players/reference/setup-options#render-captions-natively) 속성을 지정하세요.

2. **iOS 및 Android**에서는 **FCC(미 연방통신위원회)** 규정에 따라  
   사용자가 **시스템 설정을 통해 자막 스타일을 직접 제어**할 수도 있습니다.

---

<br>

### backgroundColor (string)

- 캡션 문자의 배경 색
- Default: `#000000`

<br>

### **backgroundOpacity** (number)

- 캡션 문자 배경의 투명도
- Default: `75`

<br>

### **color** (string)

- 캡션 문자 색
- Default: `#ffffff`

<br>

### **edgeStyle** (string)

- 캡션 문자의 테두리/외곽 효과
- Default: `none`

<br>

### **fontFamily** (string)

- 캡션의 폰트 패밀리
- Default: `sans`

<br>

### **fontOpacity** (number)

- 캡션 폰트의 투명도
- Default: `100`

<br>

### **fontSize** (number)

- 캡션 텍스트의 크기(브라우저를 통해 캡션을 렌더링할 때 텍스트 크기에는 영향을 미치지 않음)
- Default: `15`

<br>

### **windowColor** (string)

- 전체 캡션 영역의 배경 색
- Default: `#000000`

<br>

### **windowOpacity** (number)

- 전체 캡션 영역의 배경 투명도
- Default: `0`
