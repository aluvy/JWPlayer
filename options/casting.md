# Casting

**캐스팅(Casting)** 기능을 사용하면 시청자가 **Google Cast** 또는 **Apple AirPlay** 기술을 이용해  
비디오 및 오디오 콘텐츠를 **호환되는 TV 또는 사운드 시스템으로 스트리밍**할 수 있습니다.

플레이어에서 캐스팅 기능을 활성화하면, 시청자는 **컨트롤 바의 캐스트 아이콘**을 눌러  
콘텐츠를 캐스트 호환 기기에서 재생할 수 있습니다.

플레이어가 **호환 기기를 감지하지 못한 경우, 캐스트 아이콘은 표시되지 않습니다.**

> 참고: [FAQs](https://docs.jwplayer.com/players/docs/players-enable-casting-and-airplay#faqs) 도 함께 확인하세요.

<br>

---

## 캐스팅 활성화 방법

플레이어 설정(`setup()`)에 빈 **cast 객체**를 추가합니다.

```javascript
player.setup({
  playlist: 'https://cdn.jwplayer.com/v2/playlists/1a2Bc3d4',
  height: 360,
  width: 640,
  cast: {},
});
```

<br>

## 커스텀 리시버(Custom Receiver) 사용 시

커스텀 리시버를 사용하는 경우, **리시버 앱의 식별자(identifier)** 를  
`cast.appid` 속성에 지정합니다.

```javascript
player.setup({
  playlist: 'https://cdn.jwplayer.com/v2/playlists/1a2Bc3d4',
  height: 360,
  width: 640,
  cast: {
    appid: 'XXXXXX',
    interceptCast: true,
  },
});
```

<br>

- **appid** (string)

  - 커스텀 리시버를 사용할 때, **리시버 앱의 식별자(identifier)** 를 지정합니다.

- **interceptCast** (boolean)

  - **기본 Chromecast 동작을 비활성화**합니다.
  - 이 설정이 활성화되면, **캐스트 아이콘 클릭 시 기본 전송 동작은 실행되지 않으며**,  
    대신 플레이어에서 `castIntercepted` 이벤트가 트리거됩니다.

<br>

> 참고: 이 구성이 설정된 경우, **사용자는 직접 API를 호출해야 합니다.**
>
> - 캐스팅 시작: `requestCast()`
> - 캐스팅 중지: `stopCasting()`
