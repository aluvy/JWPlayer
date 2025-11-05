# Do Not Save Cookies

`DoNotSaveCookies` 속성은 플레이어의 데이터 수집을 제한합니다.

```javascript
jwplayer("myElement").setup({
    "playlist": "https://cdn.jwplayer.com/v2/playlists/{playlist_id}",
    ...
    "doNotSaveCookies": false
});
```

- **doNotSaveCookies** (boolean)

  - 플레이어가 사용자 ID를 저장하는 것을 허용하거나 금지합니다
  - 가능한 값:
    - `false` (Default): 사용자 쿠키가 저장됩니다.
    - `true`: 사용자 쿠키는 저장되지 않습니다.

<br>

> 콘텐츠 관리 플랫폼(CMP)을 사용 중인 경우,  
> 사용자가 쿠키 추적을 선택하지 않고 플레이어가 사용자의 선호도를 준수하는지 확인할 때  
> `"doNotSaveCookies": true`를 설정할 수 있습니다.
