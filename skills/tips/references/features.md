# Claude Code 기능 레퍼런스

공식 문서(docs.anthropic.com) 기반 정리. 각 기능의 활용 시나리오와 감지 기준 포함.

---

## 컨텍스트 관리

### /compact
- **설명**: 대화 이력을 요약하여 컨텍스트 공간 확보
- **시나리오**: 긴 작업 세션에서 컨텍스트가 가득 찰 때, `/compact focus on <topic>`으로 특정 주제 보존 가능
- **감지 기준**: 대화가 길어졌는데 /compact를 사용하지 않은 경우
- **문서**: https://docs.anthropic.com/en/docs/claude-code/best-practices#manage-context-aggressively

### /clear
- **설명**: 컨텍스트를 완전히 초기화
- **시나리오**: 서로 관련 없는 작업 사이에서 깨끗하게 시작할 때
- **감지 기준**: 주제가 크게 전환되었는데 /clear 없이 계속 진행한 경우
- **문서**: https://docs.anthropic.com/en/docs/claude-code/best-practices#course-correct-early-and-often

### /btw (사이드 질문)
- **설명**: 컨텍스트에 추가되지 않는 일회성 질문
- **시나리오**: 작업 중 빠르게 확인할 것이 있지만 메인 컨텍스트를 오염시키고 싶지 않을 때
- **감지 기준**: 메인 작업과 관련 없는 짧은 질문을 본 대화에서 직접 한 경우
- **문서**: https://docs.anthropic.com/en/docs/claude-code/interactive-mode#side-questions-with-btw

### @ 파일/디렉토리 참조
- **설명**: 파일 경로에 @를 붙여 즉시 컨텍스트에 포함
- **시나리오**: 이미 어떤 파일이 필요한지 알고 있을 때 Claude가 읽기를 기다리지 않고 바로 포함
- **감지 기준**: 특정 파일을 반복적으로 읽어달라고 요청한 경우
- **문서**: https://docs.anthropic.com/en/docs/claude-code/common-workflows#reference-files-and-directories

### 서브에이전트로 조사 위임
- **설명**: 탐색/조사를 서브에이전트에 위임하여 메인 컨텍스트를 깨끗하게 유지
- **시나리오**: 코드베이스를 광범위하게 탐색해야 하지만 결과 요약만 필요할 때
- **감지 기준**: 많은 파일을 읽으며 탐색했는데 서브에이전트를 쓰지 않은 경우
- **문서**: https://docs.anthropic.com/en/docs/claude-code/best-practices#use-subagents-for-investigation

### Ctrl+G (에디터에서 편집)
- **설명**: 프롬프트나 Plan을 기본 텍스트 에디터(VS Code, vim 등)에서 편집
- **시나리오**: 긴 프롬프트를 작성하거나 Plan을 수정할 때 터미널 입력이 불편할 때
- **감지 기준**: 매우 긴 프롬프트를 터미널에서 직접 작성한 경우
- **문서**: https://docs.anthropic.com/en/docs/claude-code/interactive-mode#keyboard-shortcuts

---

## 세션 관리

### /resume, /rename, --continue
- **설명**: 이전 대화를 이어서 계속하거나 이름을 붙여 나중에 찾기
- **시나리오**: 작업을 중단했다가 나중에 이어서 할 때
- **감지 기준**: 이전 대화의 맥락을 처음부터 다시 설명한 경우
- **문서**: https://docs.anthropic.com/en/docs/claude-code/common-workflows#resume-previous-conversations

### --from-pr
- **설명**: 특정 PR에 연결된 세션을 재개
- **시나리오**: `gh pr create`로 PR을 만든 후, 나중에 해당 PR 관련 작업을 이어갈 때
- **감지 기준**: PR 관련 작업을 새 세션에서 처음부터 시작한 경우
- **문서**: https://docs.anthropic.com/en/docs/claude-code/common-workflows#create-pull-requests

### 체크포인트 & 리와인드 (Esc+Esc, /rewind)
- **설명**: Claude의 모든 작업이 체크포인트로 저장되어, 이전 상태로 되돌리기 가능
- **시나리오**: 잘못된 방향으로 진행했을 때 대화/코드 상태를 복구
- **감지 기준**: 수동으로 변경 사항을 되돌리거나, 잘못된 접근을 처음부터 다시 시작한 경우
- **문서**: https://docs.anthropic.com/en/docs/claude-code/best-practices#rewind-with-checkpoints

### --fork-session (세션 분기)
- **설명**: 기존 세션을 분기하여 다른 접근법을 시도
- **시나리오**: 현재 세션을 유지하면서 다른 방향으로 실험해보고 싶을 때
- **감지 기준**: 다른 접근법을 시도하기 위해 세션을 새로 시작한 경우
- **문서**: https://docs.anthropic.com/en/docs/claude-code/common-workflows#resume-previous-conversations

---

## 계획 & 분석

### Plan Mode (Shift+Tab 또는 --permission-mode plan)
- **설명**: 읽기 전용 모드에서 코드를 분석하고 계획을 세운 후 실행
- **시나리오**: 여러 파일에 걸친 복잡한 변경, 코드베이스 탐색, 아키텍처 결정
- **감지 기준**: 복잡한 작업을 바로 구현에 들어간 경우
- **문서**: https://docs.anthropic.com/en/docs/claude-code/common-workflows#use-plan-mode-for-safe-code-analysis

### AskUserQuestion 인터뷰
- **설명**: Claude가 먼저 질문하여 요구사항을 명확히 한 후 작업 시작
- **시나리오**: 큰 기능 구현 전 기술적 결정, UI/UX, 엣지 케이스 등을 정리
- **감지 기준**: 모호한 요구사항으로 바로 구현에 들어간 경우
- **문서**: https://docs.anthropic.com/en/docs/claude-code/best-practices#let-claude-interview-you

### Extended Thinking & Effort Level
- **설명**: Claude의 사고 깊이를 조절. `ultrathink`로 한 턴만 높은 사고 수준 적용 가능
- **시나리오**: 복잡한 아키텍처 결정, 어려운 버그 디버깅
- **감지 기준**: 복잡한 문제에서 기본 effort로 진행한 경우
- **문서**: https://docs.anthropic.com/en/docs/claude-code/common-workflows#use-extended-thinking-thinking-mode

---

## 자동화

### Hooks
- **설명**: Claude의 특정 동작(파일 편집, 명령 실행 등) 전후에 자동으로 쉘 명령 실행
- **시나리오**: 파일 편집 후 자동 포맷팅, 보호된 파일 수정 차단, 린팅 자동화
- **감지 기준**: 매번 수동으로 포맷터/린터를 실행한 경우
- **문서**: https://docs.anthropic.com/en/docs/claude-code/hooks

### Headless 모드 (-p 플래그)
- **설명**: 비대화형으로 Claude를 실행. 파이프 입력, CI/CD 통합 가능
- **시나리오**: 로그 분석, CI에서 자동 코드 리뷰, 벌크 작업
- **감지 기준**: 반복적인 작업을 매번 대화형으로 수행한 경우
- **문서**: https://docs.anthropic.com/en/docs/claude-code/cli-reference

### --dangerously-skip-permissions (샌드박스 내에서)
- **설명**: 모든 권한 프롬프트 생략 (샌드박스 환경에서만 사용 권장)
- **시나리오**: 린트 수정, 보일러플레이트 생성 등 안전한 반복 작업
- **감지 기준**: 권한 프롬프트를 반복적으로 승인한 경우
- **문서**: https://docs.anthropic.com/en/docs/claude-code/permissions

### ! (Bash 모드)
- **설명**: `!` 접두사로 Claude를 거치지 않고 직접 셸 명령 실행, 결과는 대화에 추가
- **시나리오**: git status, ls 등 간단한 명령을 빠르게 실행하고 싶을 때
- **감지 기준**: 단순한 셸 명령을 Claude에게 요청한 경우
- **문서**: https://docs.anthropic.com/en/docs/claude-code/interactive-mode#bash-mode-with-prefix

### Ctrl+B (백그라운드 실행)
- **설명**: 실행 중인 Bash 명령이나 에이전트를 백그라운드로 전환
- **시나리오**: 빌드, 테스트 등 오래 걸리는 명령을 기다리지 않고 다른 작업 계속
- **감지 기준**: 오래 걸리는 명령이 끝날 때까지 기다린 경우
- **문서**: https://docs.anthropic.com/en/docs/claude-code/interactive-mode#background-bash-commands

---

## 병렬 작업

### 서브에이전트 (Subagents)
- **설명**: 독립적인 컨텍스트에서 병렬로 작업하는 Claude 인스턴스
- **시나리오**: 보안 리뷰 + 성능 리뷰 + 스타일 리뷰를 동시에 수행
- **감지 기준**: 순차적으로 여러 분석을 수행한 경우
- **문서**: https://docs.anthropic.com/en/docs/claude-code/sub-agents

### Git Worktree (--worktree, -w)
- **설명**: 격리된 작업 디렉토리에서 별도 브랜치로 병렬 작업
- **시나리오**: 기능 개발과 버그 수정을 동시에 진행
- **감지 기준**: 브랜치 전환을 반복한 경우
- **문서**: https://docs.anthropic.com/en/docs/claude-code/common-workflows#run-parallel-claude-code-sessions-with-git-worktrees

### Agent Teams
- **설명**: 여러 Claude 인스턴스가 협력하여 복잡한 문제 해결
- **시나리오**: 경쟁 가설 검증, 전문화된 역할 분담
- **감지 기준**: 매우 복잡한 다단계 작업을 단일 세션으로 처리한 경우
- **문서**: https://docs.anthropic.com/en/docs/claude-code/agent-teams

---

## 확장

### MCP 서버
- **설명**: 외부 서비스(GitHub, Jira, Slack, DB 등)를 Claude에 연결
- **시나리오**: 이슈 트래커에서 티켓 읽기 → 구현 → PR 생성 → Slack 알림 등 종단간 자동화
- **감지 기준**: 외부 서비스 정보를 수동으로 복사/붙여넣기한 경우
- **문서**: https://docs.anthropic.com/en/docs/claude-code/mcp

### Skills (.claude/skills/)
- **설명**: 재사용 가능한 워크플로우를 스킬로 정의하여 필요 시 자동 적용
- **시나리오**: 팀 공통 코드 리뷰 체크리스트, 배포 워크플로우
- **감지 기준**: 같은 패턴의 작업을 매번 처음부터 설명한 경우
- **문서**: https://docs.anthropic.com/en/docs/claude-code/skills

### Plugins
- **설명**: Skills, Hooks, 서브에이전트, MCP를 패키지로 묶어 설치/공유
- **시나리오**: 팀 표준을 플러그인으로 배포, 커뮤니티 도구 활용
- **감지 기준**: 수동으로 여러 설정을 반복 구성한 경우
- **문서**: https://docs.anthropic.com/en/docs/claude-code/plugins

### CLAUDE.md
- **설명**: 프로젝트 루트에 두면 매 세션 시작 시 자동 로드되는 설정 파일
- **시나리오**: 코딩 표준, 빌드 명령어, 프로젝트 규칙 등 지속적 컨텍스트
- **감지 기준**: 매 세션마다 같은 지시를 반복한 경우
- **문서**: https://docs.anthropic.com/en/docs/claude-code/memory

### /init (CLAUDE.md 자동 생성)
- **설명**: 현재 프로젝트 구조를 분석하여 CLAUDE.md 초안을 자동 생성
- **시나리오**: 새 프로젝트에서 CLAUDE.md를 처음 만들 때
- **감지 기준**: CLAUDE.md가 없는 프로젝트에서 작업한 경우
- **문서**: https://docs.anthropic.com/en/docs/claude-code/best-practices#write-an-effective-claudemd

### Auto Memory (자동 메모리)
- **설명**: Claude가 작업 중 학습한 내용(빌드 명령, 디버깅 인사이트 등)을 자동 저장
- **시나리오**: 세션 간 맥락 유지, 프로젝트별 학습 축적
- **감지 기준**: 이전 세션에서 이미 해결한 문제를 다시 설명한 경우
- **문서**: https://docs.anthropic.com/en/docs/claude-code/memory#auto-memory

### 커스텀 서브에이전트 (.claude/agents/)
- **설명**: 전문화된 도구/모델/지시를 가진 서브에이전트 정의
- **시나리오**: 보안 리뷰어, 테스트 작성기 등 반복 사용할 전문 에이전트
- **감지 기준**: 같은 유형의 전문 분석을 매번 프롬프트로 설명한 경우
- **문서**: https://docs.anthropic.com/en/docs/claude-code/sub-agents

---

## 검증

### 테스트 실행 후 검증
- **설명**: Claude에게 테스트를 실행하고 결과를 확인하도록 요청
- **시나리오**: 코드 변경 후 테스트로 자동 검증
- **감지 기준**: 코드를 작성만 하고 테스트를 실행하지 않은 경우
- **문서**: https://docs.anthropic.com/en/docs/claude-code/best-practices#give-claude-a-way-to-verify-its-work

### Chrome 확장 (UI 검증)
- **설명**: 브라우저에서 UI를 확인하고 스크린샷으로 비교
- **시나리오**: 프론트엔드 변경 후 시각적 검증
- **감지 기준**: UI 관련 작업에서 시각적 확인 없이 완료한 경우
- **문서**: https://docs.anthropic.com/en/docs/claude-code/chrome

---

## 권한 & 설정

### 권한 모드 순환 (Shift+Tab)
- **설명**: Normal → Auto-Accept → Plan 모드를 키보드로 빠르게 전환
- **시나리오**: 안전한 편집 작업에서 Auto-Accept로 전환하여 승인 피로 감소
- **감지 기준**: 안전한 작업에서 매번 권한을 승인한 경우
- **문서**: https://docs.anthropic.com/en/docs/claude-code/permissions#permission-modes

### /permissions
- **설명**: 안전한 명령을 허용 목록에 추가
- **시나리오**: `npm run lint`, `git commit` 등 반복 승인하는 명령 허용
- **감지 기준**: 같은 명령에 대해 반복적으로 권한 승인한 경우
- **문서**: https://docs.anthropic.com/en/docs/claude-code/permissions

### 샌드박싱 (/sandbox)
- **설명**: OS 레벨 격리로 파일시스템/네트워크 접근 제한, Claude가 더 자유롭게 작업 가능
- **시나리오**: 신뢰할 수 없는 코드를 안전하게 실행하거나, 권한 프롬프트 없이 작업하고 싶을 때
- **감지 기준**: 보안이 중요한 작업에서 샌드박스 없이 진행한 경우
- **문서**: https://docs.anthropic.com/en/docs/claude-code/sandboxing

### Status Line (커스텀 상태 줄)
- **설명**: 터미널 하단에 커스텀 정보(컨텍스트 사용량, git 브랜치 등) 표시
- **시나리오**: 컨텍스트 사용량을 실시간 모니터링
- **감지 기준**: 컨텍스트 관리가 필요한 긴 세션에서 상태 확인 수단이 없었던 경우
- **문서**: https://docs.anthropic.com/en/docs/claude-code/statusline

---

## 입출력

### 이미지 분석
- **설명**: 이미지를 드래그/붙여넣기하여 Claude가 분석
- **시나리오**: UI 목업 → 코드 생성, 에러 스크린샷 분석
- **감지 기준**: 시각적 정보를 텍스트로 장황하게 설명한 경우
- **문서**: https://docs.anthropic.com/en/docs/claude-code/common-workflows#work-with-images

### 파이프 입력
- **설명**: `cat file | claude -p "..."` 형태로 데이터를 직접 전달
- **시나리오**: 로그 분석, 에러 메시지 해석
- **감지 기준**: 파일 내용을 수동으로 복사/붙여넣기한 경우
- **문서**: https://docs.anthropic.com/en/docs/claude-code/cli-reference

### --output-format json
- **설명**: 구조화된 JSON 출력으로 후처리 파이프라인 연결
- **시나리오**: Claude 출력을 다른 스크립트에서 파싱할 때
- **감지 기준**: 텍스트 출력을 수동으로 파싱한 경우
- **문서**: https://docs.anthropic.com/en/docs/claude-code/cli-reference

### --json-schema (구조화된 출력)
- **설명**: JSON Schema를 지정하여 Claude가 해당 스키마에 맞는 검증된 JSON 출력 생성
- **시나리오**: 데이터 추출, API 응답 생성 등 정형화된 출력이 필요할 때
- **감지 기준**: Claude 출력을 특정 구조로 변환하는 후처리를 한 경우
- **문서**: https://docs.anthropic.com/en/docs/claude-code/cli-reference

### 멀티라인 입력
- **설명**: `\+Enter`, `Option+Enter`, `Ctrl+J` 등으로 여러 줄 입력 가능
- **시나리오**: 복잡한 프롬프트나 코드 스니펫을 입력할 때
- **감지 기준**: 긴 프롬프트를 한 줄에 억지로 작성한 경우
- **문서**: https://docs.anthropic.com/en/docs/claude-code/interactive-mode#multiline-input

### Vim 모드 (/vim)
- **설명**: vim 스타일 키 바인딩으로 프롬프트 편집
- **시나리오**: vim에 익숙한 사용자가 효율적으로 프롬프트를 편집하고 싶을 때
- **감지 기준**: (vim 사용자인 경우에만 해당)
- **문서**: https://docs.anthropic.com/en/docs/claude-code/interactive-mode#vim-editor-mode

### Task List (Ctrl+T)
- **설명**: 복잡한 작업의 진행 상황을 터미널 상태 영역에 표시
- **시나리오**: 다단계 작업에서 어디까지 진행되었는지 추적
- **감지 기준**: 복잡한 작업의 진행 상황을 파악하기 어려웠던 경우
- **문서**: https://docs.anthropic.com/en/docs/claude-code/interactive-mode#task-list

### Ctrl+R (히스토리 검색)
- **설명**: 이전 입력을 검색하여 재사용
- **시나리오**: 이전에 사용한 프롬프트를 다시 사용하고 싶을 때
- **감지 기준**: 비슷한 프롬프트를 처음부터 다시 작성한 경우
- **문서**: https://docs.anthropic.com/en/docs/claude-code/interactive-mode#command-history

### 프롬프트 제안 (Tab)
- **설명**: Claude가 다음에 할 수 있는 작업을 자동 제안, Tab으로 수락
- **시나리오**: 다음 단계가 자연스럽게 이어지는 작업 흐름
- **감지 기준**: (자동 제안이 도움이 될 수 있었던 경우)
- **문서**: https://docs.anthropic.com/en/docs/claude-code/interactive-mode#prompt-suggestions

---

## 크로스 플랫폼

### Remote Control
- **설명**: 터미널의 Claude Code를 claude.ai나 모바일에서 원격 제어
- **시나리오**: 자리를 떠나서도 작업 이어서 하기
- **문서**: https://docs.anthropic.com/en/docs/claude-code/remote-control

### /teleport
- **설명**: 웹에서 시작한 세션을 로컬 터미널로 가져오기
- **시나리오**: 모바일에서 시작한 작업을 데스크톱에서 이어서
- **문서**: https://docs.anthropic.com/en/docs/claude-code/claude-code-on-the-web

### /desktop
- **설명**: 터미널 세션을 데스크톱 앱으로 전환하여 비주얼 diff 리뷰
- **시나리오**: 코드 변경사항을 시각적으로 리뷰할 때
- **문서**: https://docs.anthropic.com/en/docs/claude-code/desktop
