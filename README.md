# Film Green Book

미술팀의 소품·재료 구매 방식과 이동수단에 따른 탄소 배출량을 계산하는 웹 앱입니다.

- 미술견적서를 입력하면 기준 탄소량이 계산됩니다.
- 소품과 구매 방식, 출발지·도착지, 이동수단을 입력하면 총 배출량이 기준치와 비교됩니다.
- 기준치를 넘으면 줄이는 방법이 단계별로 제안됩니다.

서버나 빌드 과정이 필요 없는 정적 웹 앱이며, 휴대폰과 컴퓨터에 **앱처럼 설치**할 수 있는 PWA입니다.

## 파일 구성

| 파일 | 역할 |
| --- | --- |
| `index.html` | 앱 본체 |
| `manifest.webmanifest` | 앱 이름, 아이콘, 색상 등 설치 정보 |
| `sw.js` | 오프라인에서도 열리게 하는 서비스 워커 |
| `icon-192.png`, `icon-512.png`, `apple-touch-icon.png` | 앱 아이콘 |
| `.nojekyll` | GitHub Pages가 파일을 그대로 서비스하게 하는 빈 파일 |

## 앱으로 설치하기

배포(아래 순서)를 마친 뒤, `https://<내-아이디>.github.io/film-green-book/` 주소에서 설치합니다. 반드시 이 https 주소로 열어야 설치 메뉴가 나타납니다.

- **안드로이드 (Chrome)**: 메뉴(⋮) → **앱 설치** 또는 **홈 화면에 추가**
- **아이폰 (Safari)**: 공유 버튼 → **홈 화면에 추가**
- **컴퓨터 (Chrome / Edge)**: 주소창 오른쪽의 **설치** 아이콘 클릭

설치하면 홈 화면 아이콘으로 실행되고 주소창 없이 전체 화면으로 열립니다. 한 번 열어 둔 뒤에는 인터넷이 없어도 계산 화면이 열리지만, 도로 거리와 주소 검색은 인터넷이 필요합니다. 앱 파일을 수정해 다시 올렸는데 예전 화면이 보이면 `sw.js`의 `CACHE='fgb-v1'`을 `fgb-v2`처럼 바꿔서 올리세요.

## GitHub Pages로 배포하기

1. GitHub에서 **New repository**를 눌러 저장소를 만듭니다. 이름 예시: `film-green-book`, 공개(Public)로 설정합니다.
2. 이 폴더의 파일 전체(`index.html`, `manifest.webmanifest`, `sw.js`, 아이콘 3개, `README.md`, `.nojekyll`)를 저장소에 올립니다.
   - 웹에서: 저장소 화면의 **Add file → Upload files**로 끌어다 놓고 **Commit changes**를 누릅니다.
   - 명령어로:
     ```bash
     git init
     git add .
     git commit -m "Initial commit"
     git branch -M main
     git remote add origin https://github.com/<내-아이디>/film-green-book.git
     git push -u origin main
     ```
3. 저장소의 **Settings → Pages**로 이동합니다.
4. **Build and deployment**의 Source를 **Deploy from a branch**로 두고, Branch를 `main`, 폴더를 `/ (root)`로 선택한 뒤 **Save**를 누릅니다.
5. 1~2분 뒤 `https://<내-아이디>.github.io/film-green-book/` 주소로 접속할 수 있습니다.

## 주소 검색 정확도 높이기 (선택)

기본 상태에서도 서울 동 단위, 전국 시/군/구 단위로 거리가 계산됩니다. 도로명 단위로 더 정확하게 쓰려면 카카오 지도 키를 설정합니다.

1. [Kakao Developers](https://developers.kakao.com)에서 앱을 만들고 **플랫폼 → Web**에 배포 주소(`https://<내-아이디>.github.io`)를 등록합니다.
2. 제품 설정에서 카카오맵을 켭니다.
3. JavaScript 키를 복사해 앱 상단 "Address search settings"에 붙여넣고 Save를 누릅니다. 키는 해당 브라우저에만 저장됩니다.

## 계수 수정

모든 계산 계수는 `index.html` 안의 `CFG` 블록에 있습니다. 필요하면 아래 값을 수정하세요. 모두 대략적인 추정값입니다.

- `CFG.cat`: 재료별 생산 배출량(kg CO₂e/kg)과 물 발자국
- `CFG.method`: 구매 방식별 배출 비율과 택배 포장·배송 배출량
- `CFG.mode`: 이동수단별 배출 계수(kg CO₂e/km)와 평균 속도
- `CFG.spend`: 견적서 기준치 계산용 계수(₩10,000당 kg CO₂e)
