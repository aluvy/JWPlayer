# Playlist Events

이 API 호출은 **플레이리스트(하나 이상의 항목)** 를 불러오거나 조회하고, **플레이리스트 항목 간을 탐색**할 때 사용됩니다.  
API를 통해 접근할 때, **플레이리스트는 하나 이상의 객체를 포함하는 배열(Array)** 형태로 제공됩니다.  
각 객체에는 다음과 같은 속성이 포함됩니다.

- **description** (string)

  - 플레이리스트 내에서 지정된 설명

- **mediaid** (string)

  - 콘텐츠 형식과 관계없이 특정 콘텐츠를 식별하는 고유 미디어 식별자

- **file** (string)

  - `sources[0].file`로의 바로가기 역할을 합니다.
  - 대체 파일들은 `allSources` 배열에 나열됩니다.

- **image** (string)

  - 플레이어 내에 로드되는 포스터 이미지 파일

- **preload** (string)

  - 현재 항목의 프리로드(preload) 상태
  - 가능한 값: `metadata` \｜ `auto` \｜ `none`

- **title** (string)

  - 플레이리스트 항목의 제목

- **tracks** (array)

  - `getCaptionsList()`와 유사하게, 플레이리스트 항목에 포함된 전체 트랙 배열
  - tracks 배열은 설정된 트랙 정보를 기반으로 하는 객체 목록을 포함합니다.

    - **tracks.file** (string)

      - 사용된 트랙을 포함하는 파일

    - **tracks.label** (string)

      - 자막을 사용하는 경우, 특정 화질에 지정된 라벨

    - **tracks.kind** (string)

      - 비디오에 할당된 트랙 유형
      - 가능한 값: `captions` \｜ `chapters` \｜ `thumbnails`

- **sources** (array)

  - 현재 사용 중인 소스에 대한 정보를 포함하는 단일 객체를 담은 배열
  - `sources` 배열은 현재 사용 중인 소스에 대한 정보를 담은 단일 객체를 포함합니다.

    - **sources.file** (string)

      - `source` 배열 항목에서 사용된 파일

    - **sources.label** (string)

      - 특정 화질에 할당된 라벨

    - **sources.type** (string)

      - 미디어 유형

    - **sources.default** (boolean)

      - 이 미디어 소스가 기본 재생용으로 정의되어 있는지 여부

- **allSources** (array)

  - 현재 플레이리스트 항목에 대해 구성된 모든 소스를 포함하는 배열

> 각 플레이리스트 항목은 최상위 수준에서 file 속성을 가지며,  
> 이는 `sources` 객체의 첫 번째 항목에 있는 파일에 대한 바로가기 역할을 합니다.

<br>
<br>

## .on('nextClick')

컨트롤 바의 **다음(next) 버튼** 또는 **Next Up 오버레이**가 클릭된 후 트리거됩니다.

<br>
<br>

## .on('playlist')

완전히 새로운 **플레이리스트가 플레이어에 로드될 때** 트리거됩니다.

> 이 이벤트는 **초기 플레이리스트 로드 시에는 트리거되지 않습니다.**  
> 이러한 경우에는 대신 `.on('ready')` 이벤트를 사용해야 합니다.

- **playlist** (array)

  - 새로 로드된 플레이리스트 배열.
  - `getPlaylist()`와 동일한 출력 형식을 가집니다.

<br>
<br>

## .on('playlistItem')

플레이리스트의 **인덱스가 새 항목으로 변경될 때** 트리거됩니다.  
이 이벤트는 **플레이어가 새로운 플레이리스트 항목을 재생하기 전에** 발생합니다.

- **index** (Number)

  - 현재 재생 중인 플레이리스트 항목의 인덱스

- **item** (Object)

  - 현재 재생 중인 플레이리스트 항목 객체.
  - `getPlaylistItem()`과 동일한 출력 형식을 가집니다.

<br>
<br>

## .on('playlistComplete')

다음 조건이 모두 충족될 때 트리거됩니다:

- 플레이어가 **플레이리스트의 모든 항목 재생을 완료했을 때**
- **repeat 옵션이 false**로 설정되어 있을 때

```json
{
  "type": "playlistComplete"
}
```

- **type** (string)

  - 이벤트의 유형.
  - 이 값은 항상 `"playlistComplete"`
