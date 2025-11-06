# Sharing

이 옵션 블록은 **소셜 공유 설정 서브메뉴(submenu)** 를 제어합니다.  
이 메뉴에는 **임베드 코드 복사, 영상 링크 복사, 소셜 네트워크 공유** 등의 기능이 포함됩니다.

<br>

> 참고:
> 개발자가 아니거나, 개발 리소스가 없거나, 간단한 방식으로 구현하고 싶다면  
> **JWP 대시보드(JWP Dashboard)** 에서 직접 **소셜 공유 기능을 설정**할 수 있습니다.

<br>

빈 `"sharing": {}` 옵션 블록을 설정하면  
**컨트롤 바에 소셜 공유 메뉴와 아이콘이 활성화**됩니다.

중첩 구성(nested config) 옵션이 없을 경우,  
**기본 공유 사이트와 함께 현재 페이지 URL** 이 표시되지만, **임베드 코드는 제공되지 않습니다.**

<br>

> 참고:
> 시청자가 이 공유 기능을 사용할 때,  
> 공유되는 대상은 비디오가 삽입된 전체 페이지입니다.  
> 플레이리스트 내 특정 항목(playlist item) 만 별도로 공유되지는 않습니다.

<br>

```javascript
player.setup({
  file: 'http://example.com/myVideo.mp4',
  sharing: {
    sites: ['reddit', 'facebook', 'twitter'],
  },
});
```

---

<br>

### code (string)

**임베드 코드(embed code)** 를 표시할 필드의 내용을 지정합니다.  
이 속성이 설정되지 않으면 해당 필드는 표시되지 않습니다.

<br>

### heading (string) <sup>< 8.6.0</sup>

공유 화면 상단에 표시되는 **간단한 안내 텍스트**를 지정합니다.

- 기본값: `Share Video`

> 주의: JWP 8.6.0 이상에서는 `intl.{lang}.sharing.heading` 을 사용해야 합니다.

<br>

### link (string)

영상이 플레이어에서 공유될 때, **공유 대상자가 이동할 URL** 을 지정합니다.

- 기본값: `현재 페이지의 URL`

> 참고: `link` 는 **playlist item 단위**로도 설정할 수 있습니다.

<br>

### sites (array)

**공유 아이콘(social icons)** 을 사용자 정의할 수 있습니다.

**가능한 값:**

- `bluesky`
- `email`
- `facebook`
- `linkedin`
- `pinterest`
- `reddit`
- `tumblr`
- `twitter` (X)
- `whatsapp`

  <br>

- 기본값: `["facebook", "twitter", "email"]`
