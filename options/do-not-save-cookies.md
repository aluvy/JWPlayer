# Do Not Save Cookies

쿠키 저장 안 함

`doNotSaveCookies` 속성은 **플레이어 내 데이터 수집을 제한**합니다.

<br>

```javascript
player.setup({
    "playlist": "https://cdn.jwplayer.com/v2/playlists/{playlist_id}",
    ...
    "doNotSaveCookies": false
});
```

---

<br>

### doNotSaveCookies (boolean)

플레이어가 **사용자 ID를 저장할 수 있는지 여부**를 제어합니다.

가능한 값:

- `false`: (기본값) 사용자 쿠키가 저장됩니다.
- `true`: 사용자 쿠키가 저장되지 않습니다.

<br>

---

<br>

> 참고: 콘텐츠 관리 플랫폼(CMP, Content Management Platform)을 사용하는 경우,
> 사용자가 **쿠키 추적을 거부(opt-out)** 했을 때 `"doNotSaveCookies": true` 로 설정하면
> 플레이어가 사용자의 개인정보 선택 사항을 **준수하도록 보장**할 수 있습니다.
