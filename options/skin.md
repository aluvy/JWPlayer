# Skin

JW Player는 11가지 스킨 구성 옵션을 즉시 제공합니다. 브랜드 아이덴티티를 세밀하게 제어할 수 있어 플레이어를 그 어느 때보다 쉽게 커스터마이징할 수 있습니다.

## Color Customization

색상은 [hex 값](https://www.w3schools.com/colors/colors_picker.asp), [RGBA 색상 값](https://www.w3schools.com/css/css3_colors.asp) 또는 [색상 이름](https://www.w3schools.com/colors/colors_names.asp)으로 지정할 수 있습니다.<sup>8.0+</sup>

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

- **controlbar** (object)

  - 컨트롤 바의 색상 사용자 지정
  - 참조: [controlbar](https://docs.jwplayer.com/players/reference/skin#controlbar)

- **menus** (object)

  - 메뉴의 색상 사용자 지정
  - 참조: [menus](https://docs.jwplayer.com/players/reference/skin#menus)

- **name** (string)

  - 레이어 스타일링에 사용할 사용자 지정 스킨의 이름
  - `skin.url`를 지정하는 경우 CSS 파일의 클래스 이름과 일치하는 `skin.name` 을 지정해야 합니다.
  - 맞춤 스킨에 대한 자세한 내용은 [브랜딩](https://docs.jwplayer.com/players/docs/jw8-styling-and-behavior) 문서를 참조하세요.

- **timeslider** (object)

  - 시간 슬라이더의 색상 사용자 지정
  - 참조: [timeslider](https://docs.jwplayer.com/players/reference/skin#timeslider)

- **tooltips** (object)

  - 도구 팁을 위한 색상 사용자 지정
  - 참조: [tooltips](https://docs.jwplayer.com/players/reference/skin#tooltips)

- **url** (string)

  - 플레이어를 스타일링할 외부 CSS 파일을 지정합니다
  - `skin.url`를 지정하는 경우 CSS 파일의 클래스 이름과 일치하는 `skin.name` 을 지정해야 합니다.
  - 맞춤 스킨에 대한 자세한 내용은 [브랜딩](https://docs.jwplayer.com/players/docs/jw8-styling-and-behavior) 문서를 참조하세요.

<br>

### controlbar

- **background** (string)

  - 컨트롤 바와 볼륨 슬라이더의 배경 색상
  - 기본 배경은 투명합니다.
  - Default: `"rgba(0,0,0,0)"`

- **icons** (string)

  - 컨트롤 바에 있는 모든 아이콘의 기본 비활성 색상
  - 이 옵션은 비활성 상태와 완료 상태에서 재생, 일시 중지 및 재생 아이콘의 색상도 제어합니다.
  - Default: `"rgba(255,255,255,0.8)"`

- **iconsActive** (string)

  - 컨트롤 바에서 호버링되거나 선택된 아이콘의 색상
  - Default: `#ffffff`

- **text** (string)

  - 컨트롤 바에 있는 일반 텍스트의 색상(예: 시간)
  - Default: `#ffffff`

<br>

### menus

- **background** (string)

  - 메뉴의 배경 색상과 다음 단계 오버레이
  - Default: `#333333`

- **text** (string)

  - 메뉴의 비활성 기본 텍스트 색상과 다음 위로 오버레이
  - Default: `"rgba(255,255,255,0.8)"`

- **textActive** (string)

  - 메뉴에서 검색되거나 선택된 텍스트의 색상
  - 이 옵션은 또한 Discover 오버레이의 텍스트 색상과 Next Up 오버레이의 호버 상태 텍스트 색상을 제어합니다.
  - Default: `"#FFFFFF"`

<br>

### timeslider

- **progress** (string)

  - 비디오 시작부터 현재 위치까지 채워진 시간 슬라이더의 막대 색상
  - 컨트롤 바의 버퍼 영역은 이 색상의 불투명도의 50%입니다. 볼륨 슬라이더의 색상도 이 옵션에 의해 제어됩니다.
  - Default: `"#F2F2F2"`

- **rail** (string)

  - 타임 슬라이더의 밑면 색상, 레일이라고 함
  - Default: `"rgba(255,255,255,0.3)"`

<br>

### tooltips

- **background** (string)

  - 도구 팁의 배경 색상
  - Default: `"#000000"`

- **text** (string)

  - 도구 팁의 텍스트 색상
  - default: `"#000000"`
