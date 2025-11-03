# Sharing Events

Sharing API calls work in conjunction with our `getPlugin() method`. For instance, all of our sharing instances are using the `getPlugin(‘sharing’)` API call to refer to this particular plugin. The following will target our sharing plugin:

```javascript
player.on('ready', function (event) {
  sharingPlugin = jwplayer().getPlugin('sharing');
});
```

All `sharingPlugin` references below will assume that the above code is implemented on your page.

### 호출시점

### 이벤트 객체 구조 (콜백 파라미터)

```json

```

| Value | Description |
| :---- | :---------- |
|       |             |

### 활용

### 주의사항

### 특징

<br>

## ("sharing").on('open')

Listens for the opening of the plugin

### 호출시점

### 이벤트 객체 구조 (콜백 파라미터)

```json

```

| Value | Description |
| :---- | :---------- |
|       |             |

### 활용

### 주의사항

### 특징

<br>

## ("sharing").on('close')

Listens for the closing of the plugin

### 호출시점

### 이벤트 객체 구조 (콜백 파라미터)

```json

```

| Value | Description |
| :---- | :---------- |
|       |             |

### 활용

### 주의사항

### 특징

<br>

## ("sharing").on('click')

Triggers whenever somebody shares content from within the sharing plugin

### 호출시점

### 이벤트 객체 구조 (콜백 파라미터)

```json

```

| Value | Description |
| :---- | :---------- |
|       |             |

### 활용

### 주의사항

### 특징
