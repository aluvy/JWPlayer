# Audio Tracks Events

These API calls are used to listen to or update the audio track if multiple audio tracks of a video are provided.

<br>

## .on('audioTracks')

Fires when the list of available audio tracks is updated

This occurs shortly after a playlist item starts playing.

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

## .on('audioTrackChanged')

Fires when the active audio track is changed.

This occurs in response to a track change caused by a user selecting a track or a script calling [setCurrentAudioTrack()](https://docs.jwplayer.com/players/reference/getcurrentaudiotrack).

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
