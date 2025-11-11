# Sharing Events

공유(Sharing) API 호출은 `getPlugin()` 메서드와 함께 동작합니다.  
예를 들어, 모든 공유 인스턴스는 이 특정 플러그인을 참조하기 위해  
`getPlugin('sharing')` API 호출을 사용합니다.

다음 예시는 공유 플러그인을 대상으로 합니다:

```javascript
.on('ready', function(event){
  sharingPlugin = jwplayer().getPlugin('sharing');
});
```

이후 나오는 **모든 `sharingPlugin` 참조**는
위 코드가 **페이지 내에 구현되어 있다고 가정합니다.**

<br>
<br>

## ("sharing").on('open')

공유(Sharing) 플러그인이 **열릴 때** 감지합니다.

```json
{
  "visible": true,
  "method": "interaction"
}
```

- **method** (string)

  - 공유 메뉴가 열리게 된 방식
  - 가능한 값:<
    - `interaction`: 클릭, 터치, 키보드 입력

- **visible** (string)

  - 공유 메뉴의 표시 여부
  - 이 값은 항상 `true`

<br>
<br>

## ("sharing").on('close')

공유(Sharing) 플러그인이 **닫힐 때** 감지합니다.

```json
{
  "visible": false,
  "method": "interaction"
}
```

- **method** (string)

  - 공유 메뉴가 열리게 된 방식
  - 가능한 값:
    - `interaction`: 클릭, 터치, 키보드 입력

- **visible** (string)

  - 공유 메뉴의 표시 여부
  - 이 값은 항상 `false`

<br>
<br>

## ("sharing").on('click')

공유(Sharing) 플러그인 내에서 **사용자가 콘텐츠를 공유할 때마다** 트리거됩니다.

```json
{
  "method": "facebook"
}
```

- **method** (string)

  - 사용된 공유 방식의 라벨
  - 가능한 값:
    - `email`
    - `facebook`
    - `link`
    - `twitter`
