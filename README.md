# HealthCare 관리 — 사용 설명서

한 페이지 HTML 건강관리 앱. 보조제 복용과 피트니스 운동을 주간 캘린더로 기록하며 Supabase Storage에 자동 저장합니다.

---

## 주요 기능

### 보조제 복용 관리 (좌측 패널)
- **+ 추가**: 이름 입력 → 아이콘 선택 → 추가
- **− 제거**: 목록에서 클릭으로 선택 후 제거
- **복용 정보 보기 (우클릭 / 길게 누르기)**: 보조제 카드를 **우클릭**(PC)하거나 **길게 누르면**(모바일), 또는 카드 우측 **ⓘ**를 누르면 팝업으로 **권장 빈도(주당 횟수)**, **권장 시간대**, **±2시간 조절 가능 범위**, 복용 메모를 확인할 수 있습니다.
- **권장 시간 자동 배정**: 기본 제공 보조제는 캘린더 날짜에 기록할 때 **권장 시간대가 자동으로 채워집니다.** 사용자가 시간을 직접 타이핑하지 않으며, **− / + 버튼으로 15분 단위**로만, **권장 시간 기준 ±3시간 범위 안에서만** 조절할 수 있습니다. (범위 끝에 닿으면 해당 버튼이 비활성화됩니다)
- **기본 제공 보조제(최초 1회 자동 추가)**: Lion's Mane (500mg) · L-Theanine (200mg) · Collagen Complex (powder) · Saffron (88.5mg Extract) · Full Spectrum Mineral Caps · L-Threonate (2000mg) · Omega 3 (fish oil 1250mg)

### 피트니스 관리 (우측 패널)
- **+ 추가**: 이름 + 아이콘 + **세트×반복** 입력 (예: 3×15) → 추가
- **− 제거**: 체크 후 제거
- **기본 제공 운동(최초 1회 자동 추가)**: 바벨 스쿼트 · 덤벨 숄더 프레스 · 체스트 프레스 · 펙 플라이 머신 · 시티드 로우 · 랫풀다운 · 종아리 운동 (덜 일반적인 운동 7종 — 버피·줄넘기·수영·사이클링·로잉·딥스·플랭크 — 은 자동 제거)

### 주간 캘린더 (중앙)
- 기본 보기: 지난주 1주 + 이번주 + 다음 2주 = 총 4주 표시
- **← →**: 1주씩 이동, **○**: 오늘로 돌아오기
- **날짜 우클릭 / 길게 누르기** (PC: 우클릭 / 모바일: 터치 600ms 유지) → 편집 모달
  - 해당 날짜에 보조제/운동 추가·제거
  - **항목별 시간**: 체크하면 − / + 시간 조절칸이 나타납니다. 보조제는 **권장 시간으로 자동 설정**되며 15분 단위·±3시간 범위로만 조절 가능합니다.
- **날짜 클릭/탭** → 해당 날짜 상세 보기 팝업
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

### 보조제 (파워 타입 ★)
비타민C/D/B12/E/A · 오메가3 · 칼슘 · 마그네슘 · 아연 · 철분 · 엽산 · 비오틴 · 유산균 · 강황 · 콜라겐  
사자갈기버섯 · L-테아닌 · 사프란 · 종합미네랄 · L-트레오네이트  
★ 크레아틴 · ★ 프로틴 · ★ 프리워크아웃 · ★ BCAA · ★ 베타알라닌

### 피트니스
스쿼트 · 데드리프트 · 벤치프레스 · 풀업 · 푸시업 · 숄더프레스 · 바이셉컬 · 트라이셉 · 레터럴레이즈 · 레그프레스  
달리기 · 런지 · 버피 · 스트레칭 · 체스트프레스 · 펙플라이 · 시티드로우 · 랫풀다운 · 종아리
