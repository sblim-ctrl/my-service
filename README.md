# PM 데일리 보드

> PO/PM이 아침마다 여는 한 화면: 스프린트 일정, 글로벌 미팅 시간, 해외 툴 비용

🔗 **바로 열기: https://sblim-ctrl.github.io/my-service/**

## 서비스 소개
PO/PM은 매일 "이번 스프린트는 언제 끝나지?", "지금 해외 팀에 미팅 잡아도 되나?", "우리 팀 SaaS 구독료가 원화로 얼마지?"를 따로따로 찾아봅니다.
PM 데일리 보드는 이 세 가지를 한 화면에서 바로 확인하게 해 줍니다. 키 없는 공개 API만 쓰고, HTML 파일 하나로 동작합니다.

## 기능
1. **스프린트 종료일 계산**: 시작일·영업일 수·국가(한국·미국·일본·싱가포르·독일)를 넣으면 종료일과, 그 사이에 낀 공휴일 이름·날짜가 화면에 보인다.
2. **글로벌 미팅 보드**: 도시 카드마다 현지 시각, "업무 중/퇴근/주말" 배지, 현재 기온·날씨가 보인다. 도시 이름으로 검색해 추가하고, 표시 이름 수정·삭제할 수 있다.
3. **해외 SaaS 비용 관리**: 서비스 이름·좌석당 단가·통화(USD/EUR)·결제 주기(월/연)·좌석 수·메모로 구독을 등록·수정·삭제하고, 목록에서 서비스별 원화 월 비용과 전체 월·연 합계, 환율 기준 날짜를 본다.

도시 목록과 SaaS 구독 목록은 이 브라우저(`localStorage`)에만 저장됩니다. 다른 기기·브라우저와는 공유되지 않습니다.

## API 문서 6칸

| 칸 | 찾을 것 | Nager.Date (공휴일) | Open-Meteo (날씨) | Open-Meteo Geocoding (도시 검색) | Frankfurter (환율) |
| --- | --- | --- | --- | --- | --- |
| ① 기본 주소 | 모든 주소의 앞부분 | `https://date.nager.at/api/v3/` | `https://api.open-meteo.com/v1/` | `https://geocoding-api.open-meteo.com/v1/` | `https://api.frankfurter.dev/v1/` |
| ② 메뉴 | 무엇을 받을 수 있나 | **`PublicHolidays/{연도}/{나라}`** (사용), `NextPublicHolidays/{나라}`, `LongWeekend/{연도}/{나라}`, `AvailableCountries` | **`forecast`** (예보·현재 날씨, 사용) | **`search`** (도시 이름 → 좌표·시간대, 사용) | **`latest`** (최신 기준환율, 사용), `/{날짜}`, `/{시작}..{끝}`, `currencies` |
| ③ 옵션 | 주소 뒤 `?이름=값` | `?` 대신 주소 안에 연도·나라 코드 (`2026/KR`, `2027/US`) | `latitude`, `longitude`, `current=temperature_2m,weather_code`, `timezone=auto` | `name=도쿄`, `count=5`, `language=ko` | `base=USD`, `symbols=KRW,EUR` |
| ④ 응답 모양 | 어떤 JSON이 오나 | `[{ "date": "2026-01-01", "localName": "새해", "name": "New Year's Day", "global": true, "types": ["Public"] }, …]` | `{ "timezone": "Asia/Seoul", "current": { "time": "2026-10-21T16:15", "temperature_2m": 13.2, "weather_code": 0 } }` | `{ "results": [{ "name": "도쿄", "latitude": 35.6895, "longitude": 139.69171, "timezone": "Asia/Tokyo", "country": "일본", "admin1": "도쿄 도" }, …] }` (결과가 없으면 `results` 없음) | `{ "base": "USD", "date": "2026-10-20", "rates": { "EUR": 0.89166, "KRW": 1327.84 } }` |
| ⑤ 키 | 가입·키가 필요한가 | 필요 없음 | 필요 없음 | 필요 없음 | 필요 없음 |
| ⑥ 규칙 | 무료 범위·출처 표기 | 문서에 "No Rate Limits", 이용약관 동의 필요 · 화면에 "Holiday data by Nager.Date" 표기 | 비상업적 용도 무료, 하루 1만 회 이하 권장, 출처 표기 요청 → "Weather data by Open-Meteo.com" | 날씨와 같은 Open-Meteo 규칙 · 출처는 날씨 카드의 표기로 함께 | 할당량 없음(남용 방지용 속도 제한만), 하루 한 번 갱신되는 기준환율 · 화면에 "Frankfurter (ECB 기준환율)" 표기 |
