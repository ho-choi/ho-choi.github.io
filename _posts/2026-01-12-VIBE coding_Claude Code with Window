---
layout: single
title: "[VIVE Conding] Claude Coding - 01" 
categories: VIVE Coding
typora-root-url: ../
toc: true
---


### 목표
---

최근 **STPA(System-Theoretic Process Analysis) 분석 서버 개발**을 목표로, 차세대 AI 코딩 파트너인 **Claude Code**를 워크플로우에 도입하기로 했다.

현재 내 개발 환경은 **Windows 11 + WSL2(Ubuntu)**이다. Claude Code는 터미널 기반 툴이라 WSL에 설치했는데, 이를 **PyCharm Professional**과 매끄럽게 연동하는 과정에서 몇 가지 시행착오가 있었다.

특히 Windows에서 WSL 내부 파일에 접근할 때 **"경로를 어떻게 잡아야 하는지"** 헤맸던 부분을 중점적으로 정리해 둔다.

#### 

### 1. 전제 조건 및 환경

- **OS**: Windows 11
- **IDE**: PyCharm Professional 2025.3 (WSL 연동은 Pro 버전이 확실히 편하다)
- **Subsystem**: WSL2 (Ubuntu)
- **Goal**: PyCharm 터미널에서 `claude` 명령어로 AI와 대화하며 바로 코딩하기



### 2. 라이선스 할당 및 설치

가장 먼저 JetBrains 계정에서 라이선스를 활성화했다. "Available" 상태인 라이선스를 내 계정 이메일로 **Assign(할당)** 해줘야 PyCharm 로그인 시 정품 인증이 된다. (이거 안 하고 켰다가 당황하지 말자.)



### 3. WSL에 Claude Code 설치

WSL 터미널(Ubuntu)을 열고 npm을 통해 Claude Code를 설치한다.

```
npm install -g @anthropic-ai/claude-code
```

설치 후 `claude` 명령어로 로그인 인증까지 마쳐둔다.



### 4. (핵심) PyCharm에서 WSL 프로젝트 열기 - "경로의 비밀"

가장 헤맸던 부분이다. PyCharm은 Windows에 설치되어 있고, 소스 코드는 WSL(리눅스) 안에 있다. `File > Open`을 눌렀는데, 탐색기에서 내 리눅스 파일들이 보이지 않아 순간 멍해졌다.

**해결 방법: `\\wsl$` 입력하기** PyCharm 파일 열기 창의 상단 주소 표시줄에 아래 경로를 직접 타이핑하고 엔터를 쳐야 한다.

```
\\wsl$
```

처음엔 아무것도 안 떠서 당황스러울 수 있다. 하지만 **`\\wsl$`을 입력하고 엔터**를 치면 마법처럼 `Ubuntu` 같은 배포판 폴더가 나타난다.

- 경로: `\\wsl$` -> `Ubuntu` -> `home` -> `(사용자명)` -> `(프로젝트 폴더)`

> **Tip:** 소스 코드를 Windows(`C:\`)에 두고 WSL로 돌리면 속도가 매우 느리다. 반드시 **소스 코드도 WSL 파일 시스템 안에** 두어야 한다.



### 5. PyCharm 터미널을 WSL로 교체하기

프로젝트를 열었다면, PyCharm 하단 터미널(`Alt+F12`)을 열었을 때 PowerShell이 아닌 **WSL(Bash)**이 뜨도록 설정해야 한다. 그래야 `claude` 명령어를 바로 쓸 수 있다.

1. `Settings (Ctrl+Alt+S)` -> `Tools` -> `Terminal`
2. **Shell path**를 `powershell.exe`에서 **`wsl.exe`** 로 변경
3. 저장 후 터미널을 다시 열면 리눅스 프롬프트(`$`)가 뜬다.



### 6. 연동 확인 및 Claude 초기화

터미널에서 `claude`를 입력하니 PyCharm 안에서 Claude Code 인터페이스가 완벽하게 실행되었다. 마지막으로 프로젝트 설정을 위해 초기화 명령어를 실행했다.

```
/init
```

이제 PyCharm의 강력한 IDE 기능과 Claude Code의 AI 기능을 한 화면에서 쓸 수 있게 되었다. 이번 주 목표는 **FastAPI를 이용한 STPA 지원 서버 구축**이다. 세팅은 끝났으니 이제 개발만 남았다!
