# Auto Pause

특정 규칙에 따라 플레이어를 자동으로 일시 정지합니다.

기본적으로 빈 `autoPause` 객체를 추가하면 자동 일시 정지 플레이어 기능이 활성화되며 `viewability: true` 도 설정됩니다.

```javascript
player.setup({
  "playlist": "https://example.com/myVideo.mp4",
  ...
  "autoPause": {
    "viewability": true,
    "pauseAds": true
  }
});
```

- **pauseAds** (boolean) <sup>8.10.0+</sup>

  - 플레이어가 더 이상 보이지 않을 때 광고 재생이 중지되는지 여부를 제어합니다
  - 가능한 값:
    - `false` (Default): 플레이어가 더 이상 보이지 않을 때만 비디오 재생이 일시 중지됩니다.
    - `true`: 플레이어가 더 이상 볼 수 없게 되면 광고 재생이 일시 중지됩니다. 플레이어가 다시 볼 수 있게 되면 광고 재생이 재개됩니다.
  - > 참고: `viewability: false`인 경우, `pauseAds: true`를 설정해도 아무런 효과가 없습니다.

- **viewability** (boolean)

  - 플레이어가 더 이상 보이지 않을 때 비디오 재생이 중지되는지 여부를 제어합니다
  - 가능한 값:
    `true` (Default): 플레이어가 더 이상 볼 수 없게 되면 비디오 재생이 일시 중지됩니다.  
    플레이어가 다시 볼 수 있게 되면 재생이 재개됩니다.  
    광고 중단이 시작된 후에도 플레이어가 더 이상 볼 수 없게 되면 광고 중단이 완료될 때까지 계속 재생된 후 일시 중지됩니다.
    `false`: 자동 일시 정지 기능이 비활성화되었습니다.
