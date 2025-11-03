# Control Events

This API call allows developers to interact with the built-in player controls (control bar and display icons).

> Controls will still fade out during playback if the video has no keyboard/mouse focus. When controls are disabled, JW Player is completely chrome-less.

<br>
<br>

# .on('controls')

Fired when controls are enabled or disabled by a script.

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

# .on('displayClick')

Fires when a user clicks the video display

This is especially useful for wiring your own controls when the built-in ones are disabled.

> The default click action (toggling play/pause) will still occur if controls are enabled.

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
