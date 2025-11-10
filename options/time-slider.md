# Time slider

이 옵션 블록은 **새로운 타임 슬라이더(time slider)** 를 활성화하며,
**슬라이더의 손잡이(thumb 또는 knob)** 표시 여부를 제어할 수 있습니다.

<br>

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

<br>

- **showAdMarkers** (boolean)

  - **광고 마커(ad markers)** 또는 **광고 카운트다운(ad countdown)** 표시 여부를 제어합니다.
  - 가능한 값:
    - `false`: (기본값) 타임 슬라이더에서 광고 마커가 제거되며, **미드롤(mid-roll)** 광고가 시작되기 5초 전에 **광고 카운트다운 요소**가 표시됩니다.
    - `true`: 타임 슬라이더에 **광고 마커**가 표시됩니다.

- **showKnob** (boolean)

  - **타임 슬라이더의 손잡이(knob)** 표시 여부를 제어합니다.
  - 가능한 값:
    - `true`: (기본값) **타임 슬라이더 손잡이**가 표시됩니다.
    - `false`: 타임 슬라이더 손잡이가 표시되지 않습니다.
