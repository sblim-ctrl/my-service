# PM 데일리 보드

> PO/PM이 아침마다 여는 한 화면: 스프린트 일정, 글로벌 미팅 시간, 해외 툴 비용

## 서비스 소개
PO/PM은 매일 "이번 스프린트는 언제 끝나지?", "지금 해외 팀에 미팅 잡아도 되나?", "이 툴 좌석 늘리면 원화로 얼마지?"를 따로따로 찾아봅니다.
PM 데일리 보드는 이 세 가지를 한 화면에서 바로 확인하게 해 줍니다. 키 없는 공개 API만 쓰고, HTML 파일 하나로 동작합니다.

## 기능
1. **스프린트 종료일 계산**: 시작일과 영업일 수를 넣으면 종료일과, 그 사이에 낀 한국 공휴일 이름·날짜가 화면에 보인다.
2. **글로벌 미팅 보드**: 서울·샌프란시스코·런던·싱가포르 카드 4개에 현지 시각, "업무 중/퇴근" 표시, 현재 기온·날씨가 보인다.
3. **해외 SaaS 비용 환산**: 달러 월 단가와 좌석 수를 넣으면 원화 월 비용과 환율 기준 날짜가 화면에 보인다.

## API 문서 6칸

| 칸 | 찾을 것 | Nager.Date (공휴일) | Open-Meteo (날씨) | Frankfurter (환율) |
| --- | --- | --- | --- | --- |
| ① 기본 주소 | 모든 주소의 앞부분 | `https://date.nager.at/api/v3/` | `https://api.open-meteo.com/v1/` | `https://api.frankfurter.dev/v1/` |
| ② 메뉴 | 무엇을 받을 수 있나 | **`PublicHolidays/{연도}/{나라}`** (사용), `NextPublicHolidays/{나라}`, `LongWeekend/{연도}/{나라}`, `AvailableCountries` | **`forecast`** (예보·현재 날씨, 사용) | **`latest`** (최신 기준환율, 사용), `/{날짜}`, `/{시작}..{끝}`, `currencies` |
| ③ 옵션 | 주소 뒤 `?이름=값` | `?` 대신 주소 안에 연도·나라 코드 (`2026/KR`, `2027/US`) | `latitude`, `longitude`, `current=temperature_2m,weather_code`, `timezone=auto` | `base=USD`, `symbols=KRW` |
| ④ 응답 모양 | 어떤 JSON이 오나 | `[{ "date": "2026-01-01", "localName": "새해", "name": "New Year's Day", "global": true, "types": ["Public"] }, …]` | `{ "timezone": "Asia/Seoul", "current": { "time": "2026-10-21T16:15", "temperature_2m": 13.2, "weather_code": 0 } }` | `{ "base": "USD", "date": "2026-10-20", "rates": { "KRW": 1327.84 } }` |
| ⑤ 키 | 가입·키가 필요한가 | 필요 없음 | 필요 없음 | 필요 없음 |
| ⑥ 규칙 | 무료 범위·출처 표기 | 문서에 "No Rate Limits", 이용약관 동의 필요 · 화면에 "Holiday data by Nager.Date" 표기 | 비상업적 용도 무료, 하루 1만 회 이하 권장, 출처 표기 요청 → "Weather data by Open-Meteo.com" | 할당량 없음(남용 방지용 속도 제한만), 하루 한 번 갱신되는 기준환율 · 화면에 "Frankfurter (ECB 기준환율)" 표기 |
