# Float on scroll

스크롤 시 플레이어 고정

이 기능은 **페이지를 스크롤할 때 원래 위치의 플레이어가 화면 밖으로 벗어나면**,  
플레이어를 **화면의 한쪽 구석으로 축소해 계속 표시**합니다.

세로 방향(포트레이트) 모드의 기기에서는  
플레이어가 **원래 크기로 페이지 상단에 고정(fixed)** 됩니다.

플로팅 상태에서는 시청자가 **플레이어를 드래그하여 위치를 변경**할 수 있습니다.  
단, **광고 재생 중에는 이 기능이 비활성화**됩니다.

기본적으로, **빈 `floating` 객체를 추가하면 플로팅 플레이어 기능이 활성화되며**,  
`dismissible: true`가 자동으로 설정됩니다.

### 커스터마이징용 CSS 클래스

플로팅 플레이어의 스타일을 사용자 정의하려면 아래 CSS 클래스를 사용할 수 있습니다.

```
.jw-flag-floating .jw-wrapper
.jw-float-icon
```

또한 **Floating API** 메서드를 사용하면  
**사용자 정의 플로팅 타이밍(floating timing)** 을 구현할 수 있습니다.

<br>

> 주의: **Float on scroll** 기능은 **iframe에 삽입된 플레이어**에서는 사용할 수 없습니다.

<br>

```javascript
player.setup({
    "playlist": "https://cdn.jwplayer.com/v2/playlists/{playlist_id}",
    ...
    "floating": {
        "dismissible": true
    }
});
```

---

<br>

### dismissible (boolean)

시청자가 **플로팅 플레이어를 닫을 수 있는지 여부**를 제어합니다.

가능한 값:

- `true`: (기본값) 시청자가 플로팅 플레이어를 닫을 수 있음
- `false`: 시청자가 플로팅 플레이어를 닫을 수 없음

<br>

### mode (string) <sup>< 8.17.0+</sup>

**플로팅 동작 방식**을 설정합니다.

가능한 값:

- `notVisible`: (기본값) 플레이어가 화면에서 벗어나면 플로팅 시작, 다시 보이면 플로팅 종료
- `always`: 항상 플로팅 상태 유지
- `never`: 플로팅 기능 사용 안 함

<br>

### showTitle (boolean)

**플로팅 플레이어 상단의 닫기(X) 버튼과 함께 동영상 제목을 표시할지 여부**를 설정합니다.

가능한 값:

- `false`: (기본값) 닫기 바에 동영상 제목 표시 안 함
- `true`: 닫기 바에 동영상 제목 표시
