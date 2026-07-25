# 외갓집 가는 길 — 양평 여행 날짜 맞추기

6명이 8/22~8/31 중 **1박 2일** 갈 수 있는 날을 체크하면 겹치는 주말을 찾아주고, 계획 게시판까지 있는 한 페이지짜리 앱.

- 📍 양평 외갓집 체험마을 · 🌙 1박 2일 · 🚐 투루카 카니발 · 👥 6명
- 파일은 `index.html` 하나면 끝. (Firemase는 실시간 공유용 옵션)

---

## 1. GitHub Pages에 올리기

1. GitHub에서 새 저장소(repository) 만들기 — 예: `trip`
2. `index.html`을 저장소에 업로드 (드래그해서 Commit)
3. 저장소 → **Settings → Pages**
4. **Source**를 `Deploy from a branch`, 브랜치 `main` / 폴더 `/ (root)` 선택 후 저장
5. 1~2분 뒤 `https://<아이디>.github.io/trip/` 주소가 생김 → 친구들에게 공유

이 상태로도 바로 열려요. 단, **Firebase를 연결하기 전엔 "미리보기 모드"** 라 입력이 자기 기기에만 저장됩니다.

---

## 2. Firebase 연결 (6명 실시간 공유 — 무료)

각자 폰에서 입력한 걸 서로 보려면 공용 저장소가 필요해요. Firebase Realtime Database가 무료이고 5분이면 됩니다.

1. https://console.firebase.google.com → **프로젝트 만들기** (Google Analytics는 꺼도 됨)
2. 왼쪽 **빌드 → Realtime Database → 데이터베이스 만들기**
   - 위치: `asia-southeast1` 등 아무거나
   - 규칙: **테스트 모드로 시작** 선택 (아래 3번에서 다시 손봄)
3. **규칙(Rules)** 탭에서 아래로 교체하고 게시:
   ```json
   {
     "rules": {
       "rooms": {
         "$room": { ".read": true, ".write": true }
       }
     }
   }
   ```
   > 친구끼리 쓰는 용도라 열어둔 규칙이에요. 링크를 아는 사람만 접근하는 수준의 보안입니다. 더 잠그고 싶으면 로그인(Auth)을 붙이면 돼요.
4. 프로젝트 개요 옆 **⚙️ → 프로젝트 설정 → 내 앱 → 웹(</>) 앱 추가**
5. 표시되는 `firebaseConfig` 값을 복사해서, `index.html` 위쪽 **`FIREBASE_CONFIG`** 자리에 붙여넣기:
   ```js
   const FIREBASE_CONFIG = {
     apiKey: "AIza...",
     authDomain: "xxx.firebaseapp.com",
     databaseURL: "https://xxx-default-rtdb.firebasedatabase.app",
     projectId: "xxx",
     appId: "1:xxx:web:xxx"
   };
   ```
   `databaseURL`이 꼭 들어가야 해요. (Realtime Database 주소)
6. 다시 커밋하면 끝. 페이지 아래에 🟢 **실시간 공유 모드**라고 뜨면 성공.

---

## 바꿔 쓰기

- **날짜/장소/차량**: `index.html`의 `<header>`와 `TRIP.dates` 값만 고치면 됩니다.
- **여행방 여러 개**: `const ROOM = "yangpyeong2026";` 값만 다르게 하면 데이터가 분리돼요.
- **게시판 분류**: `const CATS = [...]` 배열에서 추가/삭제.

문제 생기면 어디가 막히는지 알려줘요.
