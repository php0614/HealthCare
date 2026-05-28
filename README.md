# HealthCare 관리 — 사용 설명서

한 페이지 HTML 건강관리 앱. 보조제 복용과 피트니스 운동을 주간 캘린더로 기록하며 Supabase Storage에 자동 저장합니다.

---

## 주요 기능

### 보조제 복용 관리 (좌측 패널)
- **+ 추가**: 이름 입력 → 아이콘 선택(20종, 크레아틴/프로틴/프리워크아웃/BCAA/베타알라닌 포함 파워 타입 5종) → 추가
- **− 제거**: 목록에서 클릭으로 선택 후 제거

### 피트니스 관리 (우측 패널)
- **+ 추가**: 이름 + 아이콘(20종) + **세트×반복** 입력 (예: 3×15) → 추가
- **− 제거**: 체크 후 제거

### 주간 캘린더 (중앙)
- 기본 보기: 지난주 1주 + 이번주 + 다음 2주 = 총 4주 표시
- **← →**: 1주씩 이동, **○**: 오늘로 돌아오기
- **날짜 길게 누르기** (PC: 마우스 600ms 누름 / 모바일: 터치 600ms 유지)
  - 해당 날짜에 보조제/운동 추가·제거
  - 시간 지정 후 저장
- 각 날짜 칸에 `아이템명 | 02:23 PM` 형식으로 표시

---

## Supabase 설정

### 1. 버킷 생성
대시보드 → Storage → New Bucket → 이름: `csv-data`

### 2. 스토리지 정책 추가
버킷 → Policies → New Policy → Full customization:

| 항목 | 값 |
|---|---|
| Policy name | `allow anon read write` |
| Allowed operations | SELECT, INSERT, UPDATE |
| Target roles | anon |
| USING expression | `true` |
| WITH CHECK expression | `true` |

### 3. 데이터 파일
파일명: `pl_healthcare-data.csv` (JSON 형식으로 저장)

```json
{
  "supplements": [{"id":"...", "name":"비타민D", "iconKey":"vitamin_d"}],
  "fitness":     [{"id":"...", "name":"스쿼트",   "iconKey":"squat", "sets":3, "reps":15}],
  "records":     [{"id":"...", "date":"2026-05-29", "time":"08:30",
                   "type":"supplement", "itemId":"...",
                   "itemName":"비타민D", "iconKey":"vitamin_d"}]
}
```

---

## 배포 방법

### GitHub Pages
```bash
git init && git add . && git commit -m "init"
git branch -M main
git remote add origin https://github.com/<user>/healthcare.git
git push -u origin main
# GitHub 저장소 Settings → Pages → Source: main /root
```

### Netlify
Netlify 대시보드 → "Add new site" → "Deploy manually" → `index.html` 파일 드래그

---

## 아이콘 목록

### 보조제 (20종 / 파워 타입 ★)
비타민C/D/B12/E/A · 오메가3 · 칼슘 · 마그네슘 · 아연 · 철분 · 엽산 · 비오틴 · 유산균 · 강황 · 콜라겐  
★ 크레아틴 · ★ 프로틴 · ★ 프리워크아웃 · ★ BCAA · ★ 베타알라닌

### 피트니스 (20종)
스쿼트 · 데드리프트 · 벤치프레스 · 풀업 · 푸시업 · 숄더프레스 · 바이셉컬 · 트라이셉 · 레터럴레이즈 · 레그프레스  
플랭크 · 달리기 · 사이클링 · 수영 · 줄넘기 · 로잉 · 딥스 · 런지 · 버피 · 스트레칭
