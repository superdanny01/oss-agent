작성: Claude Code / 확인: 김대현

# week09/tools.py 함수 정리

## 개요

- 영화진흥위원회(KOBIS) 오픈 API의 일별 박스오피스 데이터를 조회하는 파일임
- 함수는 총 2개임

## 공통 설정

- `requests`: API 호출용 라이브러리임
- `API_KEY`: `secret.py`에서 불러오는 KOBIS API 키임
- `BASE_URL`: 일별 박스오피스 조회 API 주소임 (`searchDailyBoxOfficeList.json`)

## 함수 목록

### 1. `get_boxoffice_rank_1(date)`

- 해당 날짜의 박스오피스 1위 영화 제목을 반환함
- 입력: `date` (문자열, 8자리 날짜, 예: `"20260101"`)
- 출력: 1위 영화 제목 (문자열, 예: `"오디세이"`)
- 동작
  - `key`, `targetDt`를 파라미터로 API에 GET 요청함 (타임아웃 10초)
  - 응답 JSON에서 `boxOfficeResult` → `dailyBoxOfficeList` 목록을 꺼냄
  - 목록 첫 번째 영화의 `movieNm`을 반환함
- docstring 있음

### 2. `get_movie_audience(date, title)`

- 해당 날짜에 특정 영화의 일일 관객 수를 반환함
- 입력
  - `date` (문자열, 8자리 날짜)
  - `title` (문자열, 찾을 영화 제목)
- 출력 (문자열)
  - 찾은 경우: 천 단위 쉼표가 붙은 관객 수 (예: `"123,456명"`)
  - 못 찾은 경우: `"'<제목>' 은(는) <날짜> 순위에 없습니다."`
- 동작
  - 1번 함수와 같은 방식으로 일별 박스오피스 목록을 가져옴
  - 목록을 돌며 `movieNm`이 `title`과 정확히 같은 영화를 찾음
  - 찾으면 `audiCnt`를 정수로 바꿔 쉼표 형식으로 반환함
- docstring 없음

## 참고

- 두 함수에 API 호출 코드(파라미터 구성 → 요청 → JSON 파싱)가 똑같이 반복됨
- 오류 처리(네트워크 오류, 잘못된 날짜, 빈 목록 등)가 없음
- 목록이 비어 있으면 `get_boxoffice_rank_1`에서 `IndexError`가 발생함
- 영화 제목은 완전히 일치해야 찾을 수 있음 (띄어쓰기 차이, 부분 일치 불가)

확인
