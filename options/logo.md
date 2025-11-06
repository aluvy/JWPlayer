# Logo

이 옵션 블록은 **비디오 위에 오버레이되는 클릭 가능한 워터마크(logo)** 를 설정합니다.

```javascript
player.setup({
  file: 'http://example.com/myVideo.mp4',
  logo: {
    file: '/assets/jw-logo-red-46px.png',
    link: 'https://www.jwplayer.com',
    hide: 'true',
    position: 'top-left',
  },
});
```

---

<br>

### file (string)

워터마크로 사용할 **외부 이미지(JPG, PNG, GIF)** 의 URL을 지정합니다.  
예시: `"/assets/logo.png"`

**투명도가 있는 24비트 PNG 이미지 사용을 권장합니다.**

<br>

### hide (boolean)

`true`로 설정하면, 로고가 **플레이어 컨트롤과 함께 자동으로 표시 및 숨김** 처리됩니다.

- 기본값: `false`

<br>

### link (string)

워터마크 이미지를 클릭했을 때 이동할 **URL 주소**를 지정합니다.  
이 속성이 설정되지 않으면 로고 클릭 시 아무 동작도 하지 않습니다.

<br>

### margin (number)

로고가 **플레이어 화면 가장자리로부터 떨어진 거리(px 단위)** 를 지정합니다.

- 기본값: `8`

<br>

### position (string) <sup>8.0+</sup>

워터마크를 표시할 **위치(코너)** 를 지정합니다.

`"control-bar"`로 설정하면, **로고가 컨트롤 바 오른쪽 버튼 그룹의 가장 왼쪽 아이콘**으로 추가됩니다.

가능한 값:

- `"top-right"` (기본값)
- `"bottom-left"`
- `"bottom-right"`
- `"control-bar"`
- `"top-left"`

<br>

> 참고:  
> 플레이어 내 로고 이미지는 **저해상도(low-resolution)** 를 사용하는 것이 좋습니다.  
> 특히 **Internet Explorer**에서는 고해상도 이미지를 제대로 리사이징하지 못할 수 있습니다.
