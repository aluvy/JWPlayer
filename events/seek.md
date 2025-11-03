# Seek Events

These API calls are used to retrieve and update the current media playback position.

<br>

## .on('absolutePositionReady')

Fired when [.getAbsolutePosition()](https://docs.jwplayer.com/players/reference/getabsoluteposition) is ready to return data.

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

## .on('seek')

Fired when a seek has been requested either by scrubbing the control bar or through the API.

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

## .on('seeked')

Fired when video position changes after seeking, as opposed to on('seek') which triggers as a seek occurs.

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

## .on('time')

Fired when the playback position updates as the player is playing. This may occur as frequently as 10 times per second.

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
