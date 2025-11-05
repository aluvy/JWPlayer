# Captions

이 옵션 블록은 JWP 웹 및 iOS 플레이어를 위한 닫힌 캡션의 스타일링을 구성합니다.

```
- 고려해야 할 두 가지 캡션 팁은 다음과 같습니다:
  - 캡션이 브라우저에 의해 렌더링되는지 플레이어에 의해 렌더링되는지 제어하려면, `setup()`에서 [renderCaptionsNative](https://docs.jwplayer.com/players/reference/setup-options#render-captions-natively) 속성을 글로벌 수준으로 설정합니다.
  - iOS와 Android에서는 FCC 규정에 따라 사용자가 시스템 설정을 통해 이를 제어할 수도 있습니다.
```

- **backgroundColor** (string)

  - 캡션 문자의 배경 색
  - Default: `#000000`

- **backgroundOpacity** (number)

  - 캡션 문자 배경의 투명도
  - Default: `75`

- **color** (string)

  - 캡션 문자 색
  - Default: `#ffffff`

- **edgeStyle** (string)

  - 캡션 문자의 테두리/외곽 효과
  - Default: `none`

- **fontFamily** (string)

  - 캡션의 폰트 패밀리
  - Default: `sans`

- **fontOpacity** (number)

  - 캡션 폰트의 투명도
  - Default: `100`

- **fontSize** (number)

  - 캡션 텍스트의 크기(브라우저를 통해 캡션을 렌더링할 때 텍스트 크기에는 영향을 미치지 않음)
  - Default: `15`

- **windowColor** (string)

  - 전체 캡션 영역의 배경 색
  - Default: `#000000`

- **windowOpacity** (number)

  - 전체 캡션 영역의 배경 투명도
  - Default: `0`
