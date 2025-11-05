# Sharing

이 옵션 블록은 소셜 공유 옵션이 있는 설정 하위 메뉴를 제어합니다: 임베드 코드 복사, 비디오 링크 복사 및 소셜 네트워크에 비디오 공유.

> 개발자가 아니거나 개발자 리소스가 없거나 더 간단한 구현을 선호하는 경우 JWP 대시보드에서 [소셜 공유 설정](https://docs.jwplayer.com/players/docs/player-sharing)을 할 수 있습니다.

빈 `"sharing":{}` 옵션 블록을 설정하면 제어 표시줄에서 소셜 공유 메뉴와 아이콘이 활성화됩니다. 중첩된 구성 옵션이 없으면 페이지 URL 링크가 기본 공유 사이트와 함께 표시되지만 임베딩 코드는 표시되지 않습니다.

> 시청자가 이 공유 기능을 사용하면 동영상이 내장된 전체 페이지가 공유됩니다. 이 기능은 재생 목록 내에서 특정 재생 목록 항목을 공유하지 않습니다.

```javascript
player.setup({
  file: 'http://example.com/myVideo.mp4',
  sharing: {
    sites: ['reddit', 'facebook', 'twitter'],
  },
});
```

- **code** (string)

  - 임베딩 코드 필드에 표시할 임베딩 코드
  - 코드가 설정되지 않으면 필드가 표시되지 않습니다.

- **heading** (string) <sup>< 8.6.0</sup>

  - 공유 화면 상단에 표시할 짧고 유익한 텍스트
  - Default: `Share Video`
  - 경고: JWP 8.6.0부터 [intl.{lang}sharing.heading](https://docs.jwplayer.com/players/reference/internationalization#intllangsharing-object)을 사용합니다.

- **link** (string)

  - 정의된 경우, 플레이어가 동영상을 공유하면 수신자가 이 URL로 이동합니다
  - 기본값: `현재 페이지의 URL`
  - 참고: 링크는 재생 목록 항목별로 정의할 수 있습니다

- **sites** (array)

  - 소셜 아이콘을 사용자 지정할 수 있습니다
  - 가능한 값
    - bluesky
    - email
    - facebook
    - linkedin
    - pinterest
    - reddit
    - tumblr
    - twitter (X)
    - whatsapp
  - Default: `["facebook", "twitter", "email"]`
