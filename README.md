# smooth-fonts-Windows

How to get Mac-like smooth fonts on Windows 11/10

- https://gigglehd.com/gg/soft/12183718

# How to?

1. 시작 메뉴에서 실행을 눌러 regedit를 실행합니다.
2. `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\FontSubstitutes`에 들어갑니다.
3. 빈 곳에 마우스 우클릭을 하고 다음의 문자열 값을 추가하고 값을 전부 `원하는 폰트명`으로 만들어줍니다:
   - Arial, Gulim, GulimChe, Segoe UI, Malgun Gothic
4. 오타가 발생하지 않았는지 제대로 확인 하신 후 재부팅을 합니다.
5. 이제 시스템 언어 대부분이 `원하는 폰트명`으로 바뀌어 있을 겁니다.

# Known bug

간혹 시작메뉴 부분이 폰트가 바뀌질 않고 기존 윈도우 폰트들로 돌아오는 경우가 있습니다. 아마도 MacType보다 `explorer.exe`가 먼저 실행되면서 생기는 문제가 아닐까 추정되는데 이런 경우 작업관리자에서 용량 큰 `explorer.exe`를 강제 종료합니다.
그런 다음 파일 -> 새 작업 실행에 들어가서 `explorer.exe`를 실행하면 제대로 바꾼 폰트가 적용되어서 나타나더군요. 참 아쉬운 부분이긴 하나 윈도우의 기본 파일인 `explorer`가 우선순위가 높은 만큼 이는 어쩔 수가 없는 것 같습니다.

# Webbrowser

크로미움 계열의 브라우저에서 보안 문제로 서드파티 DLL를 막는 경우가 있습니다. 레지스트리 모드를 사용하던 시절의 문제점이긴 하나 혹여 이런 경우로 MacType가 브라우저에서만 작동하지 않는다면 RendererCodeIntegrity 기능을 꺼야 하는데 방법은 공식적으로 두 가지가 존재합니다.

1. 인터넷 브라우저의 바로가기 속성에서 바로가기 탭에 들어가 대상 끝 부분에 `--disable-features=RendererCodeIntegrity`라고 매개변수를 추가해줍니다. 하지만, 이렇게 할 경우 수정한 바로가기 파일에만 해당 사항이 적용됩니다.
2. 아예 브라우저 자체의 `RendererCodeIntegrity`를 끄고 싶다면 아래와 같이 레지스트리를 수정합니다.

## RendererCodeIntegrity를 끄는 방법

1. 시작 메뉴에서 실행을 눌러 regedit를 실행합니다.
2. `HKEY_LOCAL_MACHINE\SOFTWARE\Policies`에 들어가 해당하는 브라우저에 맞게 아래와 같이 키를 만들어 줍니다.

```
크로미움은 \Chromium
구글 크롬은 \Google\Chrome
브레이브는 \BraveSoftware\Brave
마이크로소프트 엣지는 \Microsoft\Edge
네이버 웨일은 \Naver\Naver Whale
```

3. 빈 곳에 우클릭 하여 DWORD 값 만들기를 선택해 RendererCodeIntegrityEnabled라고 이름을 써주고 값을 0으로 설정해줍니다.
4. 이제 브라우저를 실행해 policy 화면을 확인해 제대로 false라고 뜨는지 확인하면 끝입니다. policy 화면은 주소 창에 아래와 같이 입력해서 접속하면 됩니다.

```
크로미움, 구글 크롬은 chrome://policy
마이크로소프트 엣지는 edge://policy
네이버 웨일은 whale://policy
```

## Firefox의 경우

파이어폭스에서는 일반적으로 DirectWrite 모드 덕분에 문제 없이 작동하지만, 간혹 DirectWrite의 렌더링이 MacType의 GDI와 맞지 않는 경우가 발생할 때가 있습니다. 이런 경우 버전에 따라 아래와 같은 방법을 써볼 수 있습니다.

1. 파이어폭스 주소창에 about:config를 입력해 들어가세요.
2. cleartype을 검색하세요.
3. gfx.font_rendering.cleartype_params.rendering_mode의 값을 5로 바꾸고 저장해주세요.

# JAVA 기반의 앱

Java 기반 앱들에서 제대로 작동하지 않는 경우 ini 파일에 clipboxfix=1을 추가해 이를 보완할 수 있습니다. 예를 들어 개발용 프로그램인 IntelliJ IDEA의 경우 ini 파일에 다음과 같이 문구를 추가하시면 됩니다.

[Experimental@idea64.exe]
clipboxfix=1

레지스트리 모드를 권장하던 시절의 문제점이라 서비스 모드를 기본으로 사용하는 지금은 해당 사항이 없어야 합니다만, Secure Boot가 켜져 있을 때 문제가 발생하는 경우가 있습니다. 혹시 문제가 생겼고 Secure Boot가 필요 없으신 분은 해제해보세요. 다만, Secure Boot를 해제하는 일은 일반적으로 권장하지 않습니다.
