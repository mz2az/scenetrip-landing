# SceneTrip 랜딩 페이지

한국관광공사 TourAPI **운영계정** 재신청용 정적 소개 페이지다. data.go.kr 운영계정 신청이
"입력한 서비스 URL에 랜딩 페이지가 없다"는 이유로 반려됐다. 심사자가 실제 서비스가 있고
TourAPI 데이터를 어떻게 쓰는지 한 페이지에서 확인할 수 있게 만든다.

빌드 단계가 없다. 폴더를 그대로 아무 정적 호스팅에 올리면 된다.

## 들어 있는 것

| 파일 | 내용 |
| --- | --- |
| `index.html` | 한 페이지 소개. 한국어 기본, 오른쪽 위 버튼으로 영어 전환(`?lang=en` 도 됨). 서비스 소개·주요 기능 6개·앱 화면 5장·TourAPI 활용(사용 중 / 운영계정으로 확대)·팀·데이터 출처 |
| `privacy.html` | 개인정보처리방침 **초안**(DRAFT 표시). 푸터에서 링크 |
| `assets/haetae-*.png` | 마스코트 해태 (iOS 앱 `Images.xcassets` 원본 복사) |
| `assets/screen-*.jpg` | iOS 앱 화면 5장 (`Dropbox/SceneTrip_screens`, 1100px·JPEG 로 줄임) |
| `assets/favicon.png`, `apple-touch-icon.png` | 해태 얼굴 |
| `assets/og-image.jpg` | 링크 미리보기용. 스플래시 화면을 잘라 만듦 |

인라인 CSS, 외부 JS 없음(언어 전환용 짧은 인라인 스크립트만), 글꼴은 Google Fonts(Noto Sans KR).
라이트·다크 모드와 휴대폰 폭에 맞는다. 작품 포스터·스틸·배우 사진은 저작권 때문에 쓰지 않았다
(작품 검색 화면·찜한 작품 화면은 포스터가 보여서 뺐다).

로컬에서 보기: `index.html` 을 브라우저로 열면 된다.

## TODO

- [x] **연락처** — `geminiwombat@gmail.com` (2026-09-28). `index.html` 「문의하기」와 `privacy.html`
      개인정보 보호책임자 문의처 두 곳.
- [x] **도메인·주소** — `https://scenetrip.io` (가비아 구입, GitHub Pages `mz2az/scenetrip-landing`).
      `og:url`·`og:image` 는 이 주소로 적었고 `CNAME` 파일이 도메인을 Pages 에 묶는다.
- [ ] **가비아 DNS** — `@` A 레코드 넷(185.199.108.153 · 109 · 110 · 111), `www` CNAME →
      `mz2az.github.io.`. 기존 가비아 기본 A(121.254.178.253)는 지운다. 반영되면 Pages 에서 Enforce HTTPS.
      그 뒤 data.go.kr 신청서의 서비스 URL 에 `https://scenetrip.io` 를 적는다.
- [ ] **앱의 출처 표기** — 페이지는 "TourAPI 데이터가 쓰이는 화면에 「출처: 한국관광공사 TourAPI」를
      표기한다"고 적었다. 현재 앱 코드에는 이 문구가 아직 없다. 재신청 전에 앱에 넣거나 페이지 문구를 고친다.
- [ ] **다국어** — 영문·일문·중문 장소명은 현재 적재하지 않는다(ADR 0014, 보류). 페이지에는
      "운영계정으로 확대" 계획으로 구분해 적었다.
- [ ] **개인정보처리방침** — 시행일, 위탁·국외이전 대상(클라우드·지도·AI 모델 제공자), 보유 기간을
      실제 운영에 맞게 확정하고 DRAFT 표시를 뗀다.
- 팀 소개에는 **개인 이름을 싣지 않는다**(2026-09-28 결정 — 공개 페이지라 검색에 남는다).
  「팀 mz2az · AI·SW마에스트로 제17기」만 둔다. 개인정보 보호책임자도 팀명 + 팀 메일로 적는다.

## 호스팅 선택지

**어디에 올릴지는 사용자(팀)가 정한다.** 이 폴더를 만든 작업은 배포·push 를 하지 않았다.

1. **GitHub Pages (가장 빠름)** — 새 공개 저장소(예: `mz2az/scenetrip-landing`)에 이 폴더 내용을
   올리고 Settings → Pages 에서 `main` / root 를 켜면 `https://<계정>.github.io/<저장소>/` 주소가
   바로 생긴다. 사용자 정의 도메인도 붙일 수 있다. 심사용 URL 을 빨리 얻기에 가장 쉽다.
2. **팀 AWS DEV/PRD (PR #98 에서 들어온 배포)** — `~/workspace/SceneTrip/docs/ops/aws-deployment.md`
   참고. 다만 지금 형상은 ALB 에 **API 호스트의 `/v1` 규칙만** 있고, 진입을 **허용 CIDR(회사/VPN/검증
   단말)로 제한**한다 — 전체 인터넷이 아니다. 심사자가 열어 보려면 정적 페이지를 서빙할 대상(예: S3 +
   CloudFront, 또는 Ingress 에 `/` 규칙과 정적 서버)과 공개 접근 경로를 따로 더해야 하고, 이는 Terraform·
   Helm 변경과 공유 인프라 변경이라 팀 합의가 먼저다.
3. 그 밖에 Netlify·Cloudflare Pages 같은 정적 호스팅에 폴더를 끌어다 놓아도 된다.

## 출처

- 마스코트: `~/workspace/SceneTrip/apps/scenetrip-ios/resources/Images.xcassets/haetae-{sit,pinhold,face}.imageset`
- 앱 화면: `~/Library/CloudStorage/Dropbox/SceneTrip_screens/` 의 03·04·05·06·08 번
- TourAPI 활용 내용·수치: `docs/architecture/adr/0014-poi-source-is-public-data.md`, `docs/project/plans/poi.md`
