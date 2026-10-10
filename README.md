# ValueInsights 홈페이지

https://valueinsights.co.kr — Firebase Hosting + GitHub 자동 배포

## 폴더 구조

```
public/              ← 실제로 배포되는 파일 (이 폴더만 올라갑니다)
  index.html         홈 · Services · About · Contact (한 파일, #/ 주소로 화면 전환)
  privacy.html       개인정보처리방침 (/privacy)
  404.html           없는 주소로 들어왔을 때
  favicon.svg, apple-touch-icon.png
  og-image.png       카카오톡·SNS 링크 미리보기 이미지 (1200×630)
  robots.txt, sitemap.xml
firebase.json        호스팅 설정
.gitignore
```

상위 폴더의 `ValueInsights — 직접 만들고, 직접 운영합니다.html`은 수정 전 원본입니다. `public/` 밖에 있어서 배포되지 않습니다.

## 처음 한 번만: 배포 설정

1. **Firebase 프로젝트 만들기:** [Firebase 콘솔](https://console.firebase.google.com)에서 새 프로젝트를 만듭니다.
2. **CLI 설치·로그인:** 이 폴더에서 실행합니다.
   ```
   npm install -g firebase-tools
   firebase login
   firebase use --add        # 방금 만든 프로젝트 선택 → .firebaserc 생성
   ```
3. **GitHub 저장소에 올리기:**
   ```
   git init
   git add .
   git commit -m "홈페이지 오픈"
   git branch -M main
   git remote add origin https://github.com/<계정>/<저장소>.git
   git push -u origin main
   ```
4. **자동 배포 연결:**
   ```
   firebase init hosting:github
   ```
   질문에는 이렇게 답합니다.
   - 저장소 이름: `<계정>/<저장소>`
   - build script: **No**
   - merge 시 자동 배포: **Yes**
   - 브랜치: **main**

   `.github/workflows/`에 파일 2개가 생기고, GitHub Secret도 자동으로 등록됩니다. 생긴 파일을 commit·push하세요.
5. **첫 배포:** `firebase deploy --only hosting`
   - 이후로는 main에 push하면 자동으로 배포됩니다.
   - PR을 열면 미리보기 주소가 따로 생깁니다.

## 도메인 연결 (valueinsights.co.kr)

1. Firebase 콘솔 → Hosting → **커스텀 도메인 추가**에서 `valueinsights.co.kr`을 입력합니다.
2. 안내되는 **TXT 레코드**(소유권 확인)와 **A 레코드**를 도메인을 산 곳(가비아 등)의 DNS 설정에 입력합니다.
3. `www.valueinsights.co.kr`도 추가하고, `valueinsights.co.kr`로 리디렉션되게 설정합니다.
4. SSL 인증서는 자동으로 발급됩니다. 몇 분에서 최대 24시간까지 걸립니다.

## 로컬에서 미리 보기

```
firebase serve
```

`http://localhost:5000`에서 확인합니다.
`public/index.html`을 더블클릭해서 열어도 되지만, 개인정보처리방침·404 페이지의 링크는 서버로 띄워야 제대로 동작합니다.

## 오픈 직후 할 일

- [ ] `help@valueinsights.co.kr` 메일이 실제로 받아지는지 확인
  - 도메인 DNS에 MX 레코드를 설정해야 합니다. 하이웍스·네이버웍스·Google Workspace 등을 쓸 수 있습니다.
- [ ] 구글폼 수정
  - '문의 목적'의 "구독 서비스 오픈 알림"을 **"구독 서비스 이용 문의"**로 바꾸기
  - 개인정보 수집·이용 동의 항목 추가
- [ ] [네이버 서치어드바이저](https://searchadvisor.naver.com), [Google Search Console](https://search.google.com/search-console)에 사이트를 등록하고 sitemap 제출
- [ ] 카카오톡 미리보기가 예전 것으로 보이면 [카카오 공유 디버거](https://developers.kakao.com/tool/debugger/sharing)에서 캐시 초기화

## 나중에 고칠 것

- **사업자등록번호:** `index.html`, `privacy.html` 푸터 두 곳에 있습니다. 개인정보처리방침의 책임자 정보도 함께 확인하세요.
- **이용약관:** 구독 결제를 시작하면 이용약관 페이지를 추가하고, 푸터에 링크를 겁니다.
  - 통신판매업 신고번호도 그때 푸터에 표시해야 합니다.
- **문의 신청서 주소:** `index.html` 안에서 `forms.gle/b7zeSE2xyVuEjWZ77`를 찾아 한꺼번에 바꾸면 됩니다.
