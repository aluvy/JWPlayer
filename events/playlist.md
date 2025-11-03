# Playlist Events

These API calls are used for loading and retrieving the current playlist (of one or more items), as well as for navigating between playlist items. When accessed via the API, a playlist is an Array, containing one or more objects. Each of these objects contains the following:

<br>
<br>

# .on('nextClick')

Fires after the next button (in the control bar) or the next up overlay is clicked.

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

# .on('playlist')

Fired when an entirely new playlist has been loaded into the player.

> This event is not fired as part of the initial playlist load. Please use on('ready') in these cases.

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

# .on('playlistItem')

Fired when the playlist index changes to a new playlist item. This event occurs before the player begins playing the new playlist item.

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

# .on('playlistComplete')

Fires when the following occurs:

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
