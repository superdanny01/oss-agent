# week09/tools.py 함수 정리

`week09/tools.py`는 영화진흥위원회(KOBIS) 오픈 API의 **일별 박스오피스** 데이터를 조회하는 함수들을 담고 있다.

## 공통 설정

- `requests`: API 호출에 사용하는 라이브러리
- `API_KEY`: `secret.py`에서 불러오는 KOBIS API 키
- `BASE_URL`: 일별 박스오피스 조회 API 주소 (`searchDailyBoxOfficeList.json`)

## 함수 목록

### 1. `get_boxoffice_rank_1(date)`

해당 날짜의 박스오피스 **1위 영화 제목**을 돌려준다.

- **입력**: `date` (문자열) — 8자리 날짜, 예: `"20260101"`
- **출력**: 1위 영화 제목 (문자열), 예: `"오디세이"`
- **동작**
  1. `key`와 `targetDt`를 파라미터로 API에 GET 요청 (타임아웃 10초)
  2. 응답 JSON에서 `boxOfficeResult` → `dailyBoxOfficeList` 목록을 꺼냄
  3. 목록의 첫 번째 영화의 `movieNm`(영화 제목)을 반환
- **docstring**: 있음

### 2. `get_movie_audience(date, title)`

해당 날짜에 특정 영화의 **일일 관객 수**를 돌려준다.

- **입력**
  - `date` (문자열) — 8자리 날짜
  - `title` (문자열) — 찾을 영화 제목
- **출력** (문자열)
  - 찾았을 때: 천 단위 쉼표가 붙은 관객 수, 예: `"123,456명"`
  - 못 찾았을 때: `"'<제목>' 은(는) <날짜> 순위에 없습니다."`
- **동작**
  1. `get_boxoffice_rank_1`과 같은 방식으로 일별 박스오피스 목록을 가져옴
  2. 목록을 돌면서 `movieNm`이 `title`과 정확히 같은 영화를 찾음
  3. 찾으면 `audiCnt`(관객 수)를 정수로 바꿔 쉼표 형식으로 반환
- **docstring**: 없음

## 참고

- 두 함수 모두 API 호출 코드(파라미터 구성 → 요청 → JSON 파싱)가 똑같이 반복된다.
- 오류 처리(네트워크 오류, 잘못된 날짜, 빈 목록 등)는 없다. 예를 들어 목록이 비어 있으면 `get_boxoffice_rank_1`은 `IndexError`가 난다.
- 영화 제목은 완전히 같아야 찾을 수 있다 (띄어쓰기·부분 일치 불가).
