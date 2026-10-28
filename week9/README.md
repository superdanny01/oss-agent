# week9 - 박스오피스 관객 수 조회

## 무엇을 하는 코드인가
영화진흥위원회(KOBIS) 일별 박스오피스 API로 특정 날짜에 특정 영화의 관객 수를 알려 줍니다.
예: `get_movie_audience("20260824", "오디세이")` → `"186,587명"`

## 설치
```bash
pip install -r requirements.txt
```
그리고 week9 폴더에 `secret.py`를 만들어 KOBIS API 키를 넣습니다.
```python
API_KEY = "발급받은_키"
```

## 실행
```bash
cd week9
python main.py
```
