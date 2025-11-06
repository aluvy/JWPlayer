# Skin

JW Player는 **기본적으로 11가지 스킨 구성 옵션(skin configuration options)** 을 제공합니다.
브랜드 아이덴티티를 세밀하게 제어할 수 있어, 플레이어를 보다 **쉽게 맞춤화(customize)** 할 수 있습니다.

## 색상 사용자 정의 (Color Customization)

색상은 **16진수(hex), RGBA 색상 값**, 또는 **색상 이름(color name)** 으로 지정할 수 있습니다. (8.0+ 지원)

- [hex 값](https://www.w3schools.com/colors/colors_picker.asp)
- [RGBA 색상 값](https://www.w3schools.com/css/css3_colors.asp)
- [색상 이름](https://www.w3schools.com/colors/colors_names.asp)

## 예시: Skin 구성

```json
player.setup({
    "playlist": "https://cdn.jwplayer.com/v2/media/hWF9vG66",
    "skin": {
        "controlbar": {
            "background": "rgba(0,0,0,0)",
            "icons": "rgba(255,255,255,0.8)",
            "iconsActive": "#FFFFFF",
            "text": "#FFFFFF"
        },
        "menus": {
            "background": "#333333",
            "text": "rgba(255,255,255,0.8)",
            "textActive": "#FFFFFF"
        },
        "timeslider": {
            "progress": "#F2F2F2",
            "rail": "rgba(255,255,255,0.3)"
        },
        "tooltips": {
            "background": "#FFFFFF",
            "text": "#000000"
        }
    }
});
```

---

<br>

### controlbar (object)

컨트롤 바(Control Bar)의 색상 구성 옵션

참고: [controlbar](https://docs.jwplayer.com/players/reference/skin#controlbar)

<br>

### menus (object)

메뉴의 색상 구성 옵션

참고: [menus](https://docs.jwplayer.com/players/reference/skin#menus)

<br>

### name (string)

플레이어 스타일링에 사용할 **커스텀 스킨 이름**을 지정합니다.  
만약 `skin.url` 을 지정한다면, **반드시 `skin.name` 도 함께 지정**해야 하며,  
해당 값은 CSS 파일 내 클래스 이름과 일치해야 합니다.

자세한 내용은 [Branding](https://docs.jwplayer.com/players/docs/jw8-styling-and-behavior) 문서를 참고하세요.

<br>

### timeslider (object)

타임 슬라이더(Time Slider)의 색상 구성 옵션

참고: [timeslider](https://docs.jwplayer.com/players/reference/skin#timeslider)

<br>

### tooltips (object)

툴팁(Tooltip)의 색상 구성 옵션

참고: [tooltips](https://docs.jwplayer.com/players/reference/skin#tooltips)

<br>

### url (string)

플레이어를 스타일링하기 위한 **외부 CSS 파일의 경로**를 지정합니다.  
`skin.url`을 지정하는 경우, 반드시 `skin.name`도 지정해야 하며,  
CSS 파일의 클래스 이름과 일치해야 합니다.

자세한 내용은 [Branding](https://docs.jwplayer.com/players/docs/jw8-styling-and-behavior) 문서를 참고하세요.

<br>

## controlbar

### background (string)

컨트롤 바 및 볼륨 슬라이더의 **배경색**을 설정합니다.  
기본 배경은 투명입니다.

- 기본값: `"rgba(0,0,0,0)"`

<br>

### icons (string)

컨트롤 바 내 **모든 아이콘의 기본(비활성) 색상**을 설정합니다.
이 옵션은 **재생(play), 일시정지(pause), 리플레이(replay)** 아이콘의 비활성 및 완료 상태 색상에도 적용됩니다.

- 기본값: `"rgba(255,255,255,0.8)"`

<br>

### iconsActive (string)

컨트롤 바에서 **호버(hover)** 또는 **선택된 아이콘**의 색상을 설정합니다.

- 기본값: `"#FFFFFF"`

<br>

### text (string)

컨트롤 바의 **일반 텍스트(예: 시간 표시)** 색상을 설정합니다.

- 기본값: `"#FFFFFF"`

<br>

## menus

### background (string)

**메뉴와 Next Up 오버레이의 배경색**을 설정합니다.

- 기본값: `"#333333"`

<br>

### text (string)

메뉴 및 Next Up 오버레이의 **비활성 텍스트(기본 텍스트) 색상**을 설정합니다.

- 기본값: `"rgba(255,255,255,0.8)"`

<br>

### textActive (string)

메뉴에서 **호버되거나 선택된 텍스트의 색상**을 설정합니다.
이 옵션은 **Discover 오버레이의 텍스트 색상 및 Next Up 오버레이의 호버 상태 텍스트 색상**에도 적용됩니다.

- 기본값: `"#FFFFFF"`

<br>

## timeslider

### progress (string)

비디오 시작 지점부터 현재 위치까지 채워진 **타임 슬라이더 바 색상**을 설정합니다.
컨트롤 바의 버퍼 영역은 이 색상의 **불투명도 50%** 로 표시됩니다.
또한 **볼륨 슬라이더 색상**도 이 옵션으로 제어됩니다.

- 기본값: `"#F2F2F2"`

<br>

### rail (string)

타임 슬라이더의 **기본 베이스 색상(rail)** 을 설정합니다.

- 기본값: `"rgba(255,255,255,0.3)"`

<br>

## tooltips

### background (string)

툴팁의 **배경색**을 설정합니다.

- 기본값: `"#000000"`

<br>

### text (string)

툴팁의 **텍스트 색상**을 설정합니다.

- 기본값: `"#000000"`
