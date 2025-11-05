# Logo

이 옵션 블록은 동영상에 오버레이되는 클릭 가능한 워터마크를 구성합니다.

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

- **file** (string)

  - 워터마크로 사용할 외부 JPG, PNG 또는 GIF 이미지의 URL
  - Example: `"/assets/logo.png"`
  - 24비트 PNG 이미지를 투명하게 사용하는 것을 권장합니다.

- **hide** (boolean)

  - 이 옵션이 true로 설정되면 로고가 자동으로 표시되고 다른 플레이어 컨트롤과 함께 숨깁니다
  - Default: `false`

- **link** (string)

  - 워터마크 이미지를 클릭할 때 방문할 URL
  - 로고를 클릭하는 것은 이것이 구성되지 않는 한 아무런 영향을 미치지 않습니다.

- **margin** (number)

  - 디스플레이 가장자리에서 로고까지의 거리(픽셀 단위)
  - Default: `8`

- **position** (string) <sup>8.0+</sup>

  - 워터마크를 표시할 모서리를 설정합니다
  - `"control-bar"`를 설정하면 제어 막대의 오른쪽 버튼 그룹에서 로고가 가장 왼쪽에 있는 아이콘으로 추가됩니다.
  - 가능한 값
    - `"top-right"` (Default)
    - `"bottom-left"`
    - `"bottom-right"`
    - `"control-bar"`
    - `"top-left"`

<br>
> 특히 고해상도인 경우 Internet Explorer가 이미지 크기를 조정하지 않을 수 있으므로 플레이어의 로고에 저해상도 이미지를 사용하는 것이 좋습니다.
