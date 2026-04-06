# CLAUDE.md
This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Rules
* 반드시 한국어로 대답할 것 (Korean)
* 새로운 기능 구현시 함부로 추론하지말고 프로젝트 내에서 필요한 정보 또는 기획 내용이 필요할시 요청할 것

## Project Overview
- [프로젝트에 대한 설명과 사용중인 기술 스택 명시]


## Coding Rules

- Unity + C# 선호
- `private` / `protected` 멤버 변수: `_[변수명]` 형식 (예: `_instance`, `_managers`)
- 클래스 / 구조체: Pascal 표기법 (예: `Managers`, `IManager`)
- 지역 변수 / 함수 파라미터: Camel 표기법 (예: `manager`, `existing`)
- 상수 / 읽기 전용 변수: 대문자 + `_` (예: `MAX_COUNT`, `DEFAULT_SIZE`)
- 열거형 / 인터페이스는 E, I로 시작 (예: EPlayerType, IDamageable)
- getter, setter가 아닌 프로퍼티를 적극적으로 사용해서 값에 접근하고 수정하도록 한다 프로퍼티 이름도 Pascal 표기법을 사용
- Nullable의 사용은 되도록 삼항 연산를 대체하기 위한 ?.만 사용하도록 한다 (예: Action?.Invoke())
- Bool을 반환하는 메서드나 프로퍼티의 경우 변수 이름은 Is,Can,Has로 시작한다 (긍정 의문형), 단 이 조건은 위에 Pascal 또는 Camel 표기법을 먼저 지켜야 한다
- 클래스 설계는 다음과 같은 순서로 작성한다(1. 변수는 public -> protected -> private 순서로 변수를 선언, 2. 프로퍼티는 1.이랑 동일하지만 변수와 바로 대응이 되는 프로퍼티에 경우 변수 바로 밑에 선언
  3. 함수는 public -> protected -> private 순서로 작성하고 virtual이나 abstract 함수는 가장 상단에 적는다)
- 비슷한 기능끼리 뭉쳐있는 경우 #region #endregion을 사용하여 그룹을 짓는다
- 비동기 작업이 처리될 경우 함수 이름 끝 부분에 Async를 붙여야한다 (예: LoadAsync() )
- 대부분에 클래스에서 사용할 가능성이 있는 기능들은 제네릭을 사용하여 Util.cs, Extension.cs 파일에 함수를 정의한다.

## Architecture
이 프로젝트에서는 아래와 같은 패키지가 포함되어 있어야 한다
- Addressable (Unity Package Manager를 통해 설치)
- Newtonsoft Json (Package Manager에서 Add Package by name을 선택후 com.unity.nuget.newtonsoft-json 링크를 통해 다운)
  또는 LitJson 라이브러리 (dll 파일을 다운로드 후 Plugins 폴더내에 넣어야 함)
- DOTween (프로젝트내에 없을시 반드시 전달을 해야 함)
 
### 스크립트 구조

모든 게임 스크립트는 `Assets/@Scripts/` 하위에 역할별로 분류된다:
- `Managers/` — 서비스 로케이터 패턴의 매니저 시스템
- `Core/` — 핵심 공통 로직
- `UI/` — UI 관련 스크립트
- `Scenes/` — Scene 관련 스크립트
- `Editor/` — Unity 에디터 전용 스크립트
- `Utils/` — 개발중 사용하는 각종 유틸리티성 스크립트들

### Manager 시스템
`Managers` 클래스는 싱글턴 서비스 로케이터로 동작한다 (`Assets/@Scripts/Managers/Managers.cs`):

- `[RuntimeInitializeOnLoadMethod(RuntimeInitializeLoadType.BeforeSceneLoad)]`로 씬 로드 전에 자동 부트스트랩된다.
- 모든 매니저는 `IManager` 인터페이스(`Initialize()` / `Release()`)를 구현해야 한다.
- `IManager` 인터페이스를 상속받은 매니저급 클래스들은 Managers에 static 인스턴스를 정의하고 Get<T>을 호출한다
- `Bootstrap()` 함수내에서 `Managers.Add<T>()` 함수를 호출해 새로운 매니저급 클래스들을 생성하고 등록한다
- `Managers.Register<T>(instance)` — 외부에서 생성된 매니저를 등록
- `Managers.Get<T>()` / `Managers.TryGet<T>()` — 등록된 매니저 조회
- `Managers.Remove<T>()` — 특정 매니저 해제
- `DontDestroyOnLoad`로 씬 전환 시에도 유지된다.

### 주요 패키지

| 패키지 | 버전 | 용도 |
|---|---|---|
| `com.unity.nuget.newtonsoft-json` | 3.2.1 | 엑셀 데이터를 Json으로 파싱하기 위한 라이브러리 |
| `com.unity.Spine` | 4.1 | 스파인 |

## Development

Unity Editor에서 직접 플레이 버튼으로 실행하며, CLI 빌드가 필요한 경우 아래 명령을 사용한다:

```bash
# Windows Editor 빌드 (Unity 설치 경로에 따라 조정)
"C:/Program Files/Unity/Hub/Editor/6000.1.17f1/Editor/Unity.exe" \
  -batchmode -quit \
  -projectPath "C:/Users/equat/Desktop/Unity/UnityMCP" \
  -buildTarget StandaloneWindows64 \
  -executeMethod BuildScript.Build \
  -logFile build.log
```

테스트는 Unity Test Runner(Window > General > Test Runner)에서 실행하거나 CLI로 실행한다:

```bash
"C:/Program Files/Unity/Hub/Editor/6000.1.17f1/Editor/Unity.exe" \
  -batchmode -quit \
  -projectPath "C:/Users/equat/Desktop/Unity/UnityMCP" \
  -runTests -testPlatform EditMode \
  -testResults results.xml \
  -logFile test.log
```
