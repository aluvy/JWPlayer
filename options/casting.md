# Casting

캐스팅을 통해 시청자는 Google Cast 또는 Apple AirPlay 기술을 사용하여 비디오 및 오디오 콘텐츠를 호환되는 TV 또는 사운드 시스템으로 스트리밍할 수 있습니다. 플레이어의 캐스팅 기능을 활성화하면 시청자는 컨트롤 바의 아이콘을 탭하여 캐스트 호환 장치에서 콘텐츠를 스트리밍할 수 있습니다. 플레이어에서 호환되는 장치가 감지되지 않으면 캐스트 아이콘이 표시되지 않습니다.

참고: [FAQ](https://docs.jwplayer.com/players/docs/players-enable-casting-and-airplay#faqs)

캐스팅을 활성화하려면 설정에 빈 캐스트 객체를 추가합니다.

```javascript
player.setup({
  playlist: 'https://cdn.jwplayer.com/v2/playlists/1a2Bc3d4',
  height: 360,
  width: 640,
  cast: {},
});
```

사용자 지정 수신기를 사용하는 경우, 사용자 지정 수신기의 식별자를 `cast.appid` 속성에 할당합니다.

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

- **appid** (string)

  - 사용자 지정 수신기를 사용할 때 수신기 앱의 식별자

- **interceptCast** (boolean)

  - 기본 크롬캐스트 동작을 비활성화합니다
  - 캐스팅 아이콘을 클릭하면 더 이상 작동하지 않습니다. 플레이어는 캐스트 인터셉티드 이벤트를 트리거합니다.
  - > 이 구성이 설정되면 사용자는 콘텐츠 캐스팅을 시작하려면 `requestCast`를 호출하거나 콘텐츠 캐스팅을 중지하려면 `stopCasting`를 호출해야 합니다.
