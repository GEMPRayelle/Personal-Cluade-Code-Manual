# Personal-Cluade-Code-Manual
개인용 클로드 코드 설치부터 세팅 가이드 정리

>클로드 설치
1. VSCode나 Cursor등 터미널을 제공하는 환경에서 기본 cmd에서 Powershell로 변경
2. irm https://claude.ai/install.ps1 | iex 명령어 실행
3. claude --version 명령어로 설치가 완료됐는지 확인
4. 설치가 성공적으로 안됐을시 환경 변수 -> 사용자 변수 -> Path 편집 -> ("C:\Users\[Local PC 이름]\.local\bin") 추가
5. 클로드 로그인 후 연동 완료

>유용한 명령어들

`/init` : CLAUDE.md 파일을 생성하며 프로젝트 구조를 정리한다

`/btw` : 메인 작업이 멈추지 않고, 백그라운드에서 돌아간다. 이 질문이 히스토리에 들어가지 않음. Space/Enter로 원래 대화로 복귀 가능

-> 메인 대화와 관련없이 토큰을 절약하며 현재 세션 내에서 할 수 있는 질문을 할 수 있다.

`/clear` : 이전 작업과 관련없는 새로운 작업을 시작할때 현재 대화 내역을 초기화해 성능 최적화 + Claude 집중력 향상

`#` : 이전에 수행했던 작업을 CLAUDE.md 파일에 추가
