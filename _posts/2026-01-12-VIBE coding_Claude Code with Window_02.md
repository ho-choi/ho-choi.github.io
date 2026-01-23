---
layout: single
title: "[VIVE Conding] Claude Coding - 02" 
categories: VIVE Coding
typora-root-url: ../
toc: true
---

### 목표

지난 포스팅에서 **PyCharm**과 **Claude Code**를 연동하는 데 성공했다. 이제 본격적으로 **STPA 분석 서버**를 개발하기 위해 파이썬 환경을 잡아야 한다.

Window 11 + WSL(Ubuntu) 환경에서 최신 파이썬을 다룰 때 필연적으로 마주치는 에러와, PyCharm 인터프리터를 올바르게 연결하는 과정을 정리한다.

### 1. 문제 발생: `externally-managed-environment`

WSL 터미널에서 패키지를 설치하려고 무심코 `pip install fastapi`를 입력했더니, 아래와 같은 무시무시한 에러가 떴다.

```
error: externally-managed-environment

× This environment is externally managed
╰─> To install Python packages system-wide, try apt install ...
```

**원인:** 최신 리눅스 배포판(Ubuntu 23.04+ 등)에서는 시스템 파이썬(OS 구동용)을 보호하기 위해, `pip`로 전역(Global) 설치하는 것을 막아놨다. (PEP 668 정책)

**해결책:** **가상환경(Virtual Environment)**을 만들어서 그 안에서 설치해야 한다. Docker를 쓸 수도 있지만, 개발 초기 단계에서는 `venv`가 훨씬 빠르고 가볍다.



### 2. 가상환경 생성 중 겪은 시행착오 (`venv` 모듈 없음)

가상환경을 만들려고 했더니 이번엔 또 다른 에러가 났다.

```
python3 -m venv stpa_server
# The virtual environment was not created successfully because ensurepip is not available.
```

Ubuntu는 경량화를 위해 `venv` 모듈을 기본 설치에서 빼놓았기 때문이다. 수동으로 설치해줘야 한다.



#### 해결 순서

1. ##### **venv 모듈 설치**:

   ```
   sudo apt install -y python3.12-venv
   ```

2. ##### **가상환경 생성** (이름은 `stpa_server`로 정함):

   ```
   python3 -m venv stpa_server
   ```

3. ##### **가상환경 활성화**:

   ```
   source stpa_server/bin/activate
   ```

   👉 터미널 프롬프트 앞에 `(stpa_server)`가 뜨면 성공이다.

4. ##### **라이브러리 설치**: 이제 에러 없이 설치된다.

   ```
   pip install fastapi uvicorn
   ```



### 3. PyCharm에 가상환경 연결하기 (핵심)

터미널 세팅은 끝났지만, PyCharm은 아직 이 가상환경의 존재를 모른다. 그냥 두면 코드에 빨간 줄이 뜨고 자동완성도 안 된다. **PyCharm 인터프리터**를 방금 만든 `stpa_server`로 교체해야 한다.

#### 설정 방법

1. PyCharm 우측 하단 상태 표시줄의 `<No interpreter>` 또는 `Python 3.x` 클릭.
2. **Add New Interpreter** > **On WSL...** 선택.
3. 왼쪽 메뉴에서 **Existing environment (기존 환경)** 선택.
4. **Interpreter** 경로 옆 `...`을 눌러 아래 경로를 직접 지정:
   - `\\wsl$\Ubuntu\home\(사용자명)\project\stpa\stpa_server\bin\python3`
   - *(주의: 반드시 가상환경 폴더 안의 `bin/python3`를 선택해야 함)*



### 4. 결과 확인

세팅이 끝나자 PyCharm 우측 하단에 **`Python 3.12 (stpa)`** 라고 표시되었다. 이제 터미널(작업 공간)과 PyCharm(도구)이 완벽하게 동기화되었다.

이제 모든 준비는 끝났다. 다음 단계는 **Claude Code에게 명령하여 FastAPI 서버 코드를 생성하는 것**이다.
