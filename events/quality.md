# Quality Events

These API calls are used to listen to or update the video quality if multiple quality levels of a video are provided. Quality levels are sorted and given index numbers.

> An index of 0 will always be "Auto".

<br>
<br>

# .on('levels')

Fired when the list of available quality levels is updated. Happens e.g. shortly after a playlist item starts playing.

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
<br>

# .on('levelsChanged')

Fired when the active quality level is changed. Happens in response to e.g. a user clicking an option in the quality menu or a script calling setCurrentQuality.

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
<br>

# .on('visualQuality')

Fires when the active quality level is changed for HLS

This is different than [levelsChanged](https://docs.jwplayer.com/players/reference/quality-events#onlevelschanged) since this will trigger when adaptive streaming automatically shifts quality.

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
