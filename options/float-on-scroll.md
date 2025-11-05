# Float on scroll

- 원래 플레이어 위치를 화면의 한 구석으로 최소화하여 스크롤할 때 플레이어가 보이지 않게 유지합니다.
- 세로 방향의 장치에서 플레이어는 원래 크기를 사용하여 페이지 상단에 고정됩니다.
- 플로팅 시 시청자는 플레이어를 드래그하여 위치를 변경할 수 있습니다. 이 기능은 광고 재생 중에 비활성화됩니다.
- 기본적으로 빈 플로팅 객체를 추가하면 플로팅 플레이어 기능이 활성화되며 `dismissible: true`도 설정됩니다.
- 다음 CSS 클래스를 사용하여 플로팅 플레이어를 사용자 지정합니다:
  - [.jw-flag-floating .jw-wrapper](https://docs.jwplayer.com/players/reference/css-skin-reference#floating-flag)
  - [.jw-float-icon](https://docs.jwplayer.com/players/reference/css-skin-reference#floating)
- [플로팅 API](https://docs.jwplayer.com/players/reference/floating-player) 방법을 사용하여 맞춤형 플로팅 타이밍을 만들 수도 있습니다.

> 스크롤에 플로팅하는 것은 iframe에 내장된 플레이어와 함께 사용할 수 없습니다.

```javascript
jwplayer("myElement").setup({
    "playlist": "https://cdn.jwplayer.com/v2/playlists/{playlist_id}",
    ...
    "floating": {
        "dismissible": true
    }
});
```

- **dismissible** (boolean)

  - 플로팅 플레이어를 닫는 것을 허용하거나 금지합니다
  - 가능한 값
    - `true` (Default): 뷰어는 플로팅 플레이어를 닫을 수 있습니다
    - `false`: 뷰어가 플로팅 플레이어를 닫을 수 없습니다

- **mode** (string) <sup>< 8.17.0+</sup>

  - 플레이어에게 플로팅 동작이 어떻게 작동해야 하는지 알려줍니다
  - 가능한 값:
    - `notVisible` (Default): 플레이어가 보이는 화면 영역에서 스크롤할 때 플로팅이 시작되고, 플레이어가 다시 시야로 스크롤할 때 플로팅이 멈춥니다
    - `always`: 항상 플레이어를 띄웁니다
    - `never`: 플레이어를 띄우지 마세요

- **showTitle** (boolean)

  - 동영상 제목이 플로팅 플레이어 닫기(X) 버튼으로 표시되는지 확인합니다
  - 가능한 값:
    - `false` (Default): 동영상 제목이 외부 닫기 표시줄에 표시되지 않습니다.
    - `true`: 비디오 제목이 외부 닫기 표시줄에 표시됩니다.
