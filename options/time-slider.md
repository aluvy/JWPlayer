# Time slider

이 옵션 블록은 새로운 타임 슬라이더를 가능하게 하며 엄지나 손잡이를 토글할 수 있게 해줍니다.

```javascript
player.setup({
  playlist: 'https://cdn.jwplayer.com/v2/media/hWF9vG66',
  timeSlider: {
    legacy: true,
    preferChapterImages: false,
    showKnob: true,
    showAdMarkers: false,
  },
});
```

- **showAdMarkers** (boolean)

  - 광고 마커 또는 광고 카운트다운 표시 여부를 제어합니다.
  - 가능한 값:
    - `false`(Default) : 광고 마커가 시간 슬라이더에서 제거되고 중간 롤이 시작되기 5초 전에 광고 카운트다운 요소가 나타납니다.
    - `true`: 광고 마커가 시간 슬라이더에 표시됩니다.

- **showKnob** (boolean)

  - 타임 슬라이더 노브의 표시 여부를 제어합니다
  - 가능한 값:
    - `true` (Default): 시간 슬라이더 노브가 표시됩니다.
    - `false`: 타임 슬라이드 노브가 표시되지 않습니다.
