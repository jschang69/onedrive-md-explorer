# OneDrive MD Explorer (무료 PWA)

Windows와 iPhone에서 개인 OneDrive의 Markdown(.md) 파일을 검색하고 미리보며 여러 파일을 선택하여 공유 또는 다운로드하는 정적 웹앱입니다.

## 1. Microsoft 앱 등록 (최초 1회)

1. https://entra.microsoft.com/ 의 **App registrations > New registration**으로 이동합니다. 개인 계정에서 등록 메뉴 접근이 제한될 경우 Microsoft Azure 계정/테넌트 생성이 필요할 수 있습니다.
2. 이름: `OneDrive MD Explorer`.
3. **Supported account types**에서 `Personal Microsoft accounts only`를 선택합니다.
4. 앱을 생성하고 **Application (client) ID**를 복사합니다. **Client secret은 생성하지 않습니다.**
5. **Authentication > Add a platform > Single-page application (SPA)**에서 배포 URL을 등록합니다. 예: `https://사용자명.github.io/onedrive-md-explorer/`. 정확히 앱 화면에 표시되는 Redirect URL을 사용합니다.
6. 앱이 요청하는 Microsoft Graph delegated `Files.Read`, `User.Read` 권한을 로그인 과정에서 승인합니다. (Microsoft 계정의 환경에 따라 앱 등록 UI가 다를 수 있습니다.)

## 2. Windows에서 무료 배포 (GitHub Pages)

1. 무료 GitHub 계정을 만듭니다: https://github.com/.
2. 새 공개 저장소 `onedrive-md-explorer`를 생성합니다.
3. 이 ZIP을 풀고 **폴더 안의 파일 5개** (`index.html`, `app.js`, `manifest.webmanifest`, `sw.js`, `icon.svg`)를 저장소 최상위에 업로드합니다. `README.md`는 선택입니다.
4. 저장소 **Settings > Pages > Build and deployment > Deploy from a branch > main / (root)**를 선택합니다.
5. 생성된 `https://사용자명.github.io/onedrive-md-explorer/` 주소로 접속합니다. HTTPS가 필수입니다.
6. 해당 URL을 Microsoft 앱 등록의 SPA Redirect URI로 등록합니다. 앱 화면의 Redirect URL과 정확히 일치해야 합니다.
7. 웹앱에서 Client ID를 입력하고 **설정 저장 > OneDrive 로그인**을 누릅니다.

**주의:** GitHub Pages의 무료 저장소는 공개됩니다. 이 앱 소스코드에는 비밀번호나 개인 파일이 포함되지 않습니다. Client ID는 공개 식별자이며 비밀 키가 아닙니다. 개인 Markdown 파일은 사용자의 브라우저에서 인증 후 OneDrive에서 직접 읽습니다. 그래도 회사/기밀 데이터를 다룬다면 공개 웹앱 운영 정책과 Microsoft 계정 보안 설정을 확인하세요.

## 3. 아이폰 설치

Safari에서 배포 URL을 연 뒤 **공유 > 홈 화면에 추가**를 선택합니다. 최초 로그인 후에는 가능한 경우 MSAL 캐시를 이용해 재연결합니다. iOS Safari와 홈 화면 PWA는 저장 공간이 분리될 수 있어 각각 최초 로그인해야 할 수 있습니다.

## 4. Windows 설치

Edge/Chrome에서 배포 URL을 열고 주소 표시줄의 **앱 설치** 또는 메뉴의 **이 사이트를 앱으로 설치**를 선택합니다.

## 5. 사용법

- **보기**: 파일 1개의 Markdown 내용을 앱 안에서 확인. ChatGPT를 실행하지 않습니다.
- **체크박스**: 여러 `.md` 파일 선택. 폴더를 바꿔도 선택 상태가 유지됩니다.
- **검색**: 현재 폴더 및 하위 폴더의 파일명을 검색합니다. '본문까지 검색'을 켜면 검색어가 파일명에 없는 `.md` 파일의 내용을 내려받아 검사하므로 파일이 많으면 오래 걸릴 수 있습니다.
- **공유 메뉴 열기**: 브라우저가 여러 파일 Web Share API를 지원하면 iOS 공유 메뉴를 표시합니다. ChatGPT가 다중 파일 공유를 받아들이는지는 별도 확인이 필요합니다.
- **선택 파일 다운로드**: 선택한 `.md` 파일을 개별 다운로드합니다. 모바일 브라우저가 다중 다운로드를 차단할 수 있습니다. ChatGPT에서 첨부 메뉴를 열고 저장된 파일을 선택하세요.

## 보안 및 제한

- 기본적으로 `Files.Read` 읽기 권한만 요청합니다. 원본 수정/삭제 기능은 없습니다.
- Microsoft 인증은 MSAL Browser로 처리하며, 앱이 Microsoft 비밀번호를 직접 받지 않습니다.
- 로그인 캐시는 브라우저 로컬 저장소를 사용합니다. 자동 재로그인은 보안 정책과 브라우저 캐시 상태에 따라 다시 인증이 필요할 수 있습니다.
- 외부 라이브러리(MSAL, marked, DOMPurify)는 CDN에서 불러옵니다. 초기 접속에는 인터넷이 필요합니다.
- 파일 내용 검색은 서버 인덱스가 아니라 파일을 하나씩 읽는 방식이므로 대규모 OneDrive에서는 느릴 수 있습니다.
- 이 앱은 ChatGPT 내부에서 실행되는 앱이 아니며, ChatGPT 특정 대화에 자동으로 파일을 주입할 수 없습니다.
