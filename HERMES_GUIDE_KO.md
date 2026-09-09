# Hermes Agent 한국어 종합 가이드 ☤

> 프로젝트 분석 · 설치 · 사용법 · 수익화 아이디어 · React/PHP 연동까지 한 번에 정리한 문서입니다.

## 📌 저장소 주소

| 구분 | 주소 |
|---|---|
| **내 저장소 (포크)** | https://github.com/bmshin94/hermes-agent |
| **원본 저장소 (upstream)** | https://github.com/NousResearch/hermes-agent |
| **공식 문서** | https://hermes-agent.nousresearch.com/docs/ |
| **전체 문서 색인 (LLM용)** | https://hermes-agent.nousresearch.com/docs/llms.txt |
| **스킬 허브** | https://agentskills.io |
| **커뮤니티 Discord** | https://discord.gg/NousResearch |
| **만든 곳** | https://nousresearch.com |

---

## 목차

1. [Hermes Agent가 뭔가요?](#1-hermes-agent가-뭔가요)
2. [폴더 분석 결과](#2-폴더-분석-결과)
3. [핵심 기능](#3-핵심-기능)
4. [설치하기](#4-설치하기)
5. [첫 설정](#5-첫-설정)
6. [기본 사용법](#6-기본-사용법)
7. [메신저 연결 (텔레그램)](#7-메신저-연결-텔레그램)
8. [예약 자동화 (Cron)](#8-예약-자동화-cron)
9. [스킬 · 메모리 · MCP](#9-스킬--메모리--mcp)
10. [설정 파일과 보안](#10-설정-파일과-보안)
11. [문제 해결](#11-문제-해결)
12. [수익화 아이디어](#12-수익화-아이디어)
13. [React / PHP 연동 아키텍처](#13-react--php-연동-아키텍처)
14. [학습 로드맵](#14-학습-로드맵)

---

## 1. Hermes Agent가 뭔가요?

**Nous Research**가 만든 **오픈소스 AI 에이전트 프레임워크**입니다. (MIT 라이선스)

> **한 줄 요약**
> "내 컴퓨터나 서버에 살면서, 메신저로 시키면 알아서 일하고, 쓸수록 똑똑해지는 나만의 AI 비서"

### Claude Code / Cursor 와의 차이

| | Claude Code · Cursor | **Hermes Agent** |
|---|---|---|
| 비유 | 사무실에 출근한 동료 | **집에 사는 집사** |
| 실행 위치 | 내 노트북 | 내 서버 / VPS / 클라우드 |
| 접근 방법 | 터미널 앞에 앉아야 함 | **폰(텔레그램)으로도 가능** |
| 작동 시점 | 내가 말 걸 때만 | **예약해두면 혼자서도** |
| 학습 | 세션 끝나면 초기화 | **경험을 스킬로 저장** |
| 모델 | 고정 | **아무거나 갈아끼움** |

### 언제 쓰면 좋은가

- 노트북을 닫아도 계속 일이 진행돼야 할 때
- 매일 반복하는 잡일을 자동화하고 싶을 때
- 팀 슬랙/디스코드에 AI 봇을 두고 싶을 때
- 회사 데이터를 외부로 못 보내서 셀프호스팅이 필요할 때
- API 비용을 아끼려고 모델을 자유롭게 바꾸고 싶을 때

> **반대로** 단순히 "코딩만 도와줘"가 목적이면 Claude Code나 Cursor가 더 가볍고 편합니다.
> Hermes는 **"24시간 대기하는 비서"**가 필요할 때 진가가 나옵니다.

---

## 2. 폴더 분석 결과

실제로 이 저장소를 뜯어본 수치입니다.

| 항목 | 수치 |
|---|---|
| Python 코드 | **약 77만 줄** |
| TypeScript / React 코드 | 약 10만 줄 |
| 기본 탑재 스킬 | **58개** |
| 추가 설치 가능 MCP 연동 | **65개** |
| 툴셋 카테고리 | **34종** |
| 지원 메신저 플랫폼 | **22개** |
| 테스트 파일 | 4,000개 이상 |

### 주요 폴더 지도

| 폴더 | 역할 | 쉽게 말하면 |
|---|---|---|
| `agent/` | LLM 어댑터, 대화 루프, 메모리, 학습 | 🧠 **두뇌** |
| `tools/` | 파일·터미널·브라우저·검색 등 도구 | 🔧 **손발** |
| `skills/` | 절차적 지식 (SKILL.md) | 📓 **비법 노트** |
| `gateway/` | 메신저 플랫폼 연결 + API 서버 | 📮 **우체국** |
| `cron/` | 스케줄러 | ⏰ **알람시계** |
| `plugins/` | 플러그인 (플랫폼·모델·기능 확장) | 🔌 **확장 슬롯** |
| `optional-mcps/` | 65개 외부 서비스 연동 | 🌐 **연결선** |
| `web/` | React 19 웹 대시보드 | 🖥️ **웹 화면** |
| `apps/desktop/` | Electron 데스크톱 앱 | 💻 **앱** |
| `ui-tui/` | Ink 기반 터미널 UI | ⌨️ **터미널 화면** |
| `hermes_cli/` | CLI 명령어 구현 | 🎛️ **조작판** |
| `hermes_state_*.py` | SQLite 상태 저장소 (20개 파일로 분할) | 💾 **기억 창고** |

### 눈에 띈 점

- `cli.py` 한 파일이 **21만 줄** (단일 파일로는 매우 큰 편)
- `.env.example`이 **27KB** — 설정 옵션이 수백 개
- `hermes_state_*.py`를 검색/압축/복구/WAL까지 20개로 쪼개 관리 → 설계 참고용으로 좋음
- `agent/pet/` — 펫 마스코트 기능까지 있음

---

## 3. 핵심 기능

### 3-1. 자기 학습 루프 🧠
- 복잡한 작업을 끝내면 **스스로 스킬(SKILL.md)을 만들어 저장**
- 다음에 비슷한 일이 오면 그 스킬을 자동으로 불러옴
- **FTS5 전문검색**으로 과거 대화를 뒤져 "저번에 어떻게 했지?"에 답함
- 사용자 취향·환경·말버릇을 세션 넘어 계속 축적
- 관련 코드: `agent/learning_graph.py`, `agent/curator.py`, `agent/memory_manager.py`
- 페르소나 파일: `SOUL.md` (Claude Code의 `CLAUDE.md`와 같은 개념)

### 3-2. 22개 메신저 플랫폼 💬
```
telegram · discord · slack · whatsapp · signal · email · sms
matrix · mattermost · teams · line · simplex · ntfy
google_chat · homeassistant · dingtalk · feishu · wecom
weixin · irc · a2a · photon(iMessage)
```
- 음성 메시지 자동 전사(STT), 파일/사진 분석 지원
- 플랫폼 추가는 **플러그인 방식** → 코어 코드 수정 불필요 (`gateway/platforms/ADDING_A_PLATFORM.md`)

> ⚠️ **카카오톡은 지원 목록에 없습니다.** (검색 결과 관련 코드 0건) — 12장 수익화 아이디어 참고

### 3-3. 예약 자동화 ⏰
- 자연어로 등록: `"매일 아침 8시에 뉴스 요약해서 텔레그램으로 보내줘"`
- 스케줄 표현: `"30m"`, `"every 2h"`, `"every monday 9am"`, `"0 9 * * *"`, ISO 타임스탬프
- 작업별로 모델/스킬/작업폴더 지정 가능, 작업 A의 출력을 작업 B로 연결 가능

### 3-4. 어디서든 실행 🌍
터미널 백엔드 7종: `로컬` · `Docker` · `SSH` · `Singularity` · `Modal` · `Daytona` · `Vercel Sandbox`
- Modal / Daytona는 **서버리스 하이버네이션** → 유휴 시 비용 거의 0원
- 월 5천원짜리 VPS에서도 구동 가능

### 3-5. 모델 자유 선택 🔀
Anthropic · OpenAI · Gemini · Bedrock · Vertex · Azure · OpenRouter · DeepSeek · xAI · Ollama 등 **35개 이상 프로바이더 프로필** 지원. `hermes model` 한 줄로 전환.

---

## 4. 설치하기

### 준비물

| 항목 | 설명 |
|---|---|
| OS | macOS / Linux / Windows / WSL2 / Android(Termux) |
| Python 3.11 | 설치 스크립트가 자동 설치 (uv 사용) |
| Node.js | `.nvmrc` 기준 26 (설치 스크립트가 처리) |
| **AI API 키** | **필수** — Hermes는 두뇌가 없어서 꼭 필요 |

### macOS / Linux / WSL2

```bash
curl -fsSL https://hermes-agent.nousresearch.com/install.sh | bash
source ~/.bashrc      # zsh: source ~/.zshrc
```

### Windows (네이티브, PowerShell)

```powershell
iex (irm https://hermes-agent.nousresearch.com/install.ps1)
```

설치 스크립트가 uv, Python 3.11, Node.js, ripgrep, ffmpeg, 포터블 Git Bash(MinGit)까지 전부 처리합니다.

> ⚠️ 백신이 `uv.exe`를 악성코드로 오탐할 수 있습니다. Astral의 정식 Rust 바이너리이며, README에 검증 방법과 예외 등록법이 있습니다.

### Android (Termux)
공식 [Termux 가이드](https://hermes-agent.nousresearch.com/docs/getting-started/termux) 참고. `.[termux]` 전용 extra를 사용합니다.

### 설치 확인

```bash
hermes --version
hermes doctor          # 의존성 · 설정 진단
```

---

## 5. 첫 설정

```bash
hermes setup           # 대화형 마법사 (모델/키/도구/게이트웨이)
```

### AI 제공사 선택 가이드

| 추천 | 이유 | 발급처 | 환경변수 |
|---|---|---|---|
| 🥇 **OpenRouter** | 키 하나로 수백 개 모델 — **초보자 최적** | openrouter.ai | `OPENROUTER_API_KEY` |
| 🥈 **Nous Portal** | 모델+웹검색+이미지생성+TTS를 구독 하나로 | portal.nousresearch.com | `hermes auth add nous` |
| 🥉 **Anthropic / OpenAI** | 이미 키가 있다면 | console.anthropic.com | `ANTHROPIC_API_KEY` |
| 💰 **Ollama (로컬)** | 완전 무료, 대신 성능 낮음 | ollama.com | - |

Nous Portal을 쓸 경우 원커맨드 설정:

```bash
hermes setup --portal          # OAuth 로그인 + Tool Gateway 자동 활성화
```

나중에 모델 변경:

```bash
hermes model                   # 대화형 모델/프로바이더 선택기
hermes fallback add            # 1순위 실패 시 대체 체인 등록
hermes auth                    # 여러 키를 풀로 등록 → 자동 로테이션
```

---

## 6. 기본 사용법

### CLI 실행

```bash
hermes                                   # 대화 시작
hermes chat -q "파이썬 리스트 정렬법 알려줘"   # 단발 질문
hermes -c                                # 마지막 대화 이어하기
hermes -m claude-sonnet-4.6              # 이번만 다른 모델
hermes -s github                          # 특정 스킬 미리 로드
hermes -w                                 # 격리된 git worktree에서 실행 (안전)
hermes --yolo                             # 승인 없이 실행 (⚠️ 위험)
hermes --safe-mode                        # 모든 커스텀 끄고 실행 (문제 해결용)
```

### 주요 CLI 명령어

```bash
hermes setup / model / config / doctor / status / update
hermes tools [list|enable|disable NAME]
hermes skills [list|browse|search|install|publish]
hermes mcp [add|catalog|install|list|serve]
hermes gateway [setup|start|stop|restart|status]
hermes cron [list|create|edit|pause|resume|run|remove]
hermes sessions [list|browse|rename|delete|export|prune|stats]
hermes profile [list|create|use|delete]     # 완전 독립된 인스턴스 여러 개
hermes auth / memory / kanban / dashboard / desktop / proxy
hermes logs [-f] [errors]
```

### 대화 중 슬래시 명령어 (자주 쓰는 것)

**세션 관리**
```
/new (/reset)     새 대화 시작
/undo [N]         N턴 되돌리기
/retry            다시 답변 받기
/compress         컨텍스트 압축 (토큰 절약)
/sessions         이전 대화 목록 / 이어하기
/branch (/fork)   대화 분기
/status           모델·토큰·컨텍스트 정보
/bg <프롬프트>     백그라운드 세션에서 실행
/queue (/q)       다음 턴에 실행할 프롬프트 예약
```

**설정**
```
/model [이름] [--global]   모델 전환 (기본은 세션 한정)
/personality [이름]        성격 지정
/reasoning [레벨]          추론 강도 (none~ultra)
/voice [on|off|tts]        음성 모드
/skin [이름]               테마 변경
/yolo                      승인 우회 토글
```

**기능**
```
/skills          스킬 검색·설치·관리
/learn <소스>     지금 대화에서 재사용 스킬 생성 ⭐
/memory          기억한 내용 / 대기 중인 저장 검토
/cron            예약 작업 관리
/curator         스킬 정리 (사용 안 하는 스킬 아카이브)
/tools           도구 켜고 끄기
/journey         학습한 스킬·메모리 타임라인
/usage           토큰 사용량과 요금
/help            전체 명령어
```

> 💡 도구·스킬 변경은 **`/new`(새 세션)부터 적용**됩니다. 프롬프트 캐시 보존을 위한 의도된 동작입니다.

---

## 7. 메신저 연결 (텔레그램)

### 1) 봇 만들기
1. 텔레그램에서 **`@BotFather`** 검색
2. `/newbot` → 이름 입력 → 아이디 입력 (반드시 `bot`으로 끝나야 함)
3. **토큰 복사** (`123456:ABC-DEF...` 형태)

### 2) 내 텔레그램 ID 확인
**`@userinfobot`** 에게 아무 메시지나 보내면 숫자 ID를 알려줍니다.
→ 이걸 등록하지 않으면 **아무나 봇을 사용할 수 있습니다.**

### 3) 등록 & 실행

```bash
hermes gateway setup       # 텔레그램 선택 → 토큰 + 내 ID 입력
hermes gateway start
hermes gateway status
```

### 관련 환경변수 (`~/.hermes/.env`)

```bash
TELEGRAM_BOT_TOKEN=              # BotFather 토큰
TELEGRAM_ALLOWED_USERS=          # 허용 사용자 ID (쉼표 구분) ← 보안 필수
TELEGRAM_HOME_CHANNEL=           # cron 결과를 보낼 기본 채팅방
TELEGRAM_WEBHOOK_URL=            # 웹훅 방식 사용 시
TELEGRAM_WEBHOOK_SECRET=         # 프로덕션 권장
```

### 텔레그램에서 되는 것
- 일반 대화 / 파일·사진 분석
- **음성 메시지 자동 전사** (STT)
- `/new`, `/model` 등 슬래시 명령어 그대로 사용
- 위험 명령 승인: `/approve`, `/deny`

> 다른 플랫폼(디스코드, 슬랙 등)도 절차는 동일하게 `hermes gateway setup` 입니다.
> - **디스코드 봇이 조용하다면** → Bot 설정에서 **Message Content Intent** 활성화
> - **슬랙이 DM에서만 된다면** → `message.channels` 이벤트 구독 추가

---

## 8. 예약 자동화 (Cron)

### 자연어로 등록 (가장 쉬움)
대화창에서 그냥 말하면 됩니다.
```
> 매일 아침 8시에 오늘 IT 뉴스 요약해서 텔레그램으로 보내줘
```

### 명령어로 관리

```bash
hermes cron list
hermes cron create "0 9 * * *"
hermes cron run <ID>          # 지금 즉시 테스트 실행
hermes cron pause <ID> / resume <ID> / remove <ID>
hermes cron status
```

### 스케줄 표현
```
"30m"                 30분마다
"every 2h"            2시간마다
"every monday 9am"    매주 월요일 오전 9시
"0 9 * * *"           매일 오전 9시 (5필드 크론)
2026-01-01T09:00:00Z  ISO 타임스탬프 (1회)
```

### 알아둘 점
- 실행당 **3분 하드 인터럽트**
- `.tick.lock` 으로 중복 실행 방지
- cron 세션은 기본적으로 `skip_memory=True`
- 작업별 `skills`, `model`, `workdir`, `context_from`(작업 체이닝) 지정 가능

> ⚠️ **게이트웨이가 켜져 있어야 예약이 돌아갑니다.** 24시간 운영하려면 VPS 등에 올리세요.
> - SSH 로그아웃 시 죽는다면: `sudo loginctl enable-linger $USER`
> - WSL2에서 죽는다면: `/etc/wsl.conf`에 `systemd=true`

---

## 9. 스킬 · 메모리 · MCP

### 스킬 (절차적 기억)

```bash
hermes skills list / browse / search <키워드>
hermes skills install <ID>          # 허브 ID 또는 SKILL.md 직링크
hermes skills publish <경로>         # 내 스킬 배포
hermes skills tap add <REPO>        # 깃허브 저장소를 스킬 소스로 추가
hermes bundles                       # 여러 스킬을 한 번에 로드하는 묶음
```

기본 58개 탑재 (엑셀/PPT/워드/PDF, 깃허브, arXiv, 노션, 에어테이블, 애플 메모 등)

**대화 중 `/learn`** 을 치면 방금 한 작업을 재사용 스킬로 저장합니다.

> 💡 **`SKILL.md`는 그냥 마크다운 파일입니다.** 프론트매터(name, description)와 절차만 적으면 되므로 **코딩 없이도 스킬을 만들 수 있습니다.**

```markdown
---
name: 세금계산서-정리
description: 영수증 이미지에서 세금계산서 정보를 뽑아 엑셀로 정리
---

# 세금계산서 정리
1. 사용자가 준 이미지를 vision 도구로 읽는다
2. 공급자 / 공급가액 / 세액 / 일자를 추출한다
3. xlsx 스킬로 표를 만들어 저장한다
```

### 큐레이터 (스킬 자동 정리)
- 에이전트가 만든 스킬만 대상 (`created_by: agent`)
- 안 쓰는 스킬을 stale → archive 처리, **삭제는 절대 안 함**
- 기본 정리 sweep은 **토큰 비용 0**

```bash
hermes curator status / run / pin / archive / restore
```

### 메모리

```bash
hermes memory setup / status / off / reset
```
대화 중 `/memory` 로 저장 대기 항목 승인/거부 가능.

### MCP (외부 서비스 연동)

```bash
hermes mcp catalog              # 큐레이션된 카탈로그
hermes mcp install notion
hermes mcp add NAME --url ... / --command ...
hermes mcp list / test NAME
hermes mcp serve                # Hermes 자체를 MCP 서버로 노출
```

`optional-mcps/` 에 **65개** 준비됨: Notion, Linear, Figma, Stripe, Sentry, Supabase, Vercel, Cloudflare, Datadog, Asana, Todoist, Atlassian 등

---

## 10. 설정 파일과 보안

### 주요 경로

```
~/.hermes/
├── config.yaml            설정 (비밀 아님)
├── .env                   API 키 · 시크릿 전용
├── state.db               세션 저장소 (SQLite + FTS5)
├── auth.json              OAuth 토큰 / 크리덴셜 풀
├── skills/                설치된 스킬
├── skins/                 테마
├── logs/                  로그 (gateway.log 등)
└── sessions/              대화 기록 (*.jsonl)
```

프로필 사용 시 `~/.hermes/profiles/<이름>/` 아래 같은 구조. **`$HERMES_HOME`을 기준으로 해석**하세요.

### 설정 명령

```bash
hermes config show / edit / get KEY / set KEY VALUE / check / path
```

### 자주 쓰는 설정

```bash
hermes config set display.interface tui      # 예쁜 TUI 화면
hermes config set display.language ko        # 한국어
hermes config set display.show_cost true     # 비용 표시
hermes config set compression.enabled true   # 컨텍스트 자동 압축
hermes config set terminal.backend docker    # 실행 환경 격리
hermes config set approvals.mode smart       # 승인 모드
```

### 보안 설정 (중요)

| 항목 | 명령 | 기본값 |
|---|---|---|
| 명령 승인 모드 | `hermes config set approvals.mode smart` | `smart` |
| 시크릿 자동 마스킹 | `hermes config set security.redact_secrets true` | **켜짐** |
| PII 마스킹 (게이트웨이) | `hermes config set privacy.redact_pii true` | 꺼짐 |
| 허용 명령 초기화 | `hermes config set command_allowlist '[]'` | - |

**승인 모드 3가지**
- `smart` — 위험 명령만 보조 LLM이 판단해 확인 (**권장**)
- `manual` — 항상 확인 (가장 안전)
- `off` — 전부 통과 (`--yolo`와 동일, **비권장**)

> ⚠️ `security.redact_secrets`는 **프로세스 시작 시점에 고정**됩니다. 세션 중간에 바꿔도 적용되지 않으며, 이는 LLM이 스스로 보호장치를 끄지 못하게 하는 **의도된 설계**입니다.
> ⚠️ YOLO 모드를 켜도 시크릿 마스킹은 꺼지지 않습니다. 둘은 독립적입니다.

### 안전 수칙
- `--yolo`는 정말 필요할 때만 (파일 삭제 사고 위험)
- 중요한 폴더에서는 `hermes -w` (격리 worktree) 사용
- 메신저 연결 시 **반드시 허용 사용자 ID 지정**
- 처음엔 `manual`로 시작 → 익숙해지면 `smart`

---

## 11. 문제 해결

```bash
hermes doctor              # 진단 (가장 먼저)
hermes doctor --fix        # 자동 수리 시도
hermes status --all
hermes logs / hermes logs errors
hermes update
hermes --safe-mode         # 모든 커스터마이징 비활성화
```

| 증상 | 해결 |
|---|---|
| 도구가 안 보임 | `hermes tools` 확인 → `/new` 로 새 세션 |
| 설정이 반영 안 됨 | CLI는 재시작, 게이트웨이는 `/restart` |
| 모델/프로바이더 오류 | `hermes doctor` → `hermes auth` 재인증 → `.env` 키 확인 |
| 게이트웨이가 SSH 로그아웃 시 죽음 | `sudo loginctl enable-linger $USER` |
| 게이트웨이 크래시 루프 | `systemctl --user reset-failed hermes-gateway` |
| 디스코드 봇 무응답 | **Message Content Intent** 활성화 |
| 슬랙이 DM에서만 동작 | `message.channels` 이벤트 구독 |
| 웹 페이지가 옛날 내용 | web 캐시 20분 TTL → `web.cache_exempt_hosts` 등록 |
| 보조 모델(비전/압축) 실패 | `OPENROUTER_API_KEY` 또는 `GOOGLE_API_KEY` 설정 |

게이트웨이 로그 확인:
```bash
grep -i "failed to send\|error" ~/.hermes/logs/gateway.log | tail -20
```

---

## 12. 수익화 아이디어

### 라이선스 확인

**MIT 라이선스** (Copyright (c) 2025 Nous Research)

| 가능 ✅ | 조건 ⚠️ |
|---|---|
| 상업적 판매 | 저작권 고지문 유지 |
| 수정 후 자체 제품화 | "Hermes" 상표는 별도 주의 |
| SaaS 구독 서비스 | 무보증 (원저작자 면책) |

> **현실 인식:** MIT는 나에게만 자유로운 게 아니라 **경쟁자에게도 자유롭습니다.**
> 코드 소유는 해자(moat)가 되지 않습니다. **수익은 "코드"가 아니라 "코드로 해결한 남의 문제"에서 나옵니다.**

### 발견한 시장 빈틈: 카카오톡 미지원 🇰🇷

지원 플랫폼 22개를 전수 확인한 결과 — 중국(WeChat/DingTalk/Feishu), 일본(LINE), 미국(iMessage)은 있는데 **한국 카카오톡만 없습니다.** (`grep -ril "kakao"` 결과 0건)

그리고 `gateway/platforms/ADDING_A_PLATFORM.md`에 명시:
> *"플러그인으로 만들면 코어 코드 수정이 전혀 필요 없다"*

### 아이디어 6가지

#### 1️⃣ 자동화 구축 대행 (외주)
> 난이도 ⭐☆☆☆☆ · 초기비용 0원 · 첫 수익 2~4주 · **가장 추천**

소상공인/1인 기업 대상 "AI 비서 세팅" 서비스.
- 예: 경쟁사 가격 일일 모니터링 → 알림 / 고객 문의 자동 분류·초안
- 가격: 기본 세팅 30~50만원 + 월 관리 5~15만원
- 핵심: 사장님들은 터미널을 못 씁니다. **그 간극이 시장입니다.**

#### 2️⃣ 업종별 스킬팩 판매
> 난이도 ⭐⭐☆☆☆ · `hermes skills publish` + agentskills.io 로 유통 경로 확보됨

- 부동산팩 / 세무팩 / 쇼핑몰팩 / 마케터팩
- 가격: 개당 3~9만원 또는 월 1~3만원 구독
- **기본 58개 스킬은 전부 범용** — 한국 실무(세금계산서, 사업자번호, 국내 서식)용은 아직 없음
- **SKILL.md는 마크다운이라 코딩 없이 제작 가능**

#### 3️⃣ 카카오톡 플러그인 → 한국형 배포판
> 난이도 ⭐⭐⭐⭐☆ · 임팩트 최대

```
1단계  카카오톡 플러그인 오픈소스 공개 → 인지도 확보
2단계  "한글 완전 지원 배포판" → 사용자 확보
3단계  설치 대행 / 관리형 호스팅으로 과금
```

**⚠️ 법적 제약 (반드시 확인)**

| 방식 | 가능 여부 |
|---|---|
| 카카오 i 오픈빌더 챗봇 | ✅ 합법 (채널 개설 + 심사 필요, 기능 제약) |
| 알림톡 / 친구톡 API | ✅ 합법 (발신 전용, 대화형 불가) |
| **개인 계정 자동화** | ❌ **약관 위반 — 계정 정지 위험** |

→ 현실적으로 **"카카오 채널 챗봇"** 방향, 즉 개인 비서보다 **소상공인 고객응대 봇** 제품이 됩니다.

#### 4️⃣ 관리형 호스팅 SaaS
> 난이도 ⭐⭐⭐⭐☆ · 수익 규모 최대

"설치 없이 가입하면 바로 24시간 AI 비서". `terminal.backend`의 Docker/Modal/Daytona로 사용자별 격리, 서버리스라 유휴 비용 거의 0.
- 가격: Free(월 100회) / Basic 9,900원 / Pro 29,900원
- **리스크:** API 비용 폭탄(사용량 제한 필수), 개인정보 보관 책임, 24시간 장애 대응

#### 5️⃣ 기업 온프레미스 구축
> 난이도 ⭐⭐⭐☆☆ · **단가 최고**

"ChatGPT에 회사 자료를 못 올리는" 기업(병원, 법무법인, 제조, 금융, 공공) 대상.
- 데이터 외부 유출 0 / 로컬 모델 시 인터넷 없이도 작동 / MIT라 라이선스 비용 없음 / Slack·Teams 기존 흐름 유지
- 가격: 구축 500~3,000만원 + 연 유지보수 15~20%
- 영업 사이클 3~6개월 → 업계 인맥이 있으면 강력

#### 6️⃣ 콘텐츠 · 교육
유튜브 / 뉴스레터 / 강의. 수익 자체보다 **1~5번 고객을 데려오는 깔때기**로서의 가치가 큼.

### 추천 진행 순서

```
[1개월] 내 업무 자동화 → 사례 3개 확보
   ↓
[2개월] 콘텐츠로 공유 → 문의 수집 (6번)
   ↓
[3개월] 대행 시작 → 첫 매출 (1번)
   ↓
[4~6월] 반복 요청을 스킬팩으로 제품화 (2번)
   ↓
[6월~] SaaS / 기업 영업으로 확장 (3·4·5번)
```

### 법적·운영 체크리스트

| 항목 | 주의 |
|---|---|
| MIT 고지 | 재배포 시 저작권 문구 유지 |
| 상표 | "Hermes" 이름 그대로 쓰지 말고 자체 브랜드 |
| API 비용 | 대행 시 **사용량 상한 필수** |
| 개인정보 | 고객 데이터 취급 시 처리방침 필수 |
| 카카오 약관 | **개인 계정 자동화 절대 금지** |
| 계약 | 대행 시 범위·책임 명시 |

---

## 13. React / PHP 연동 아키텍처

### 결론

> **Hermes를 React/PHP로 다시 만들 필요는 없습니다.**
> Hermes는 **REST API 서버를 내장**하고 있어서, 엔진으로 두고 그 위에 제품을 얹으면 됩니다.

```
┌─────────────────────────────────┐
│  🎨 React      화면 (내가 만듦)     │
│  로그인 · 채팅UI · 대시보드          │
└──────────────┬──────────────────┘
               │ HTTP
┌──────────────▼──────────────────┐
│  🐘 PHP        서버 (내가 만듦)     │
│  회원 · 결제 · 사용량 제한 · 키 보관   │
└──────────────┬──────────────────┘
               │ HTTP (localhost:8642)
┌──────────────▼──────────────────┐
│  ☤ Hermes    엔진 (그냥 켜둠)      │
│  Python — 수정 불필요               │
└─────────────────────────────────┘
```

### API 서버 실체

`gateway/platforms/api_server.py` 헤더:
> *"OpenAI-compatible frontend connects at `http://localhost:8642/v1` with `API_SERVER_KEY`"*

**제공되는 엔드포인트 (실측)**

```
# OpenAI 호환
GET    /v1/models
POST   /v1/chat/completions          ← OpenAI 규격 그대로
POST   /v1/responses
GET    /api/model/options

# 실행(run) 제어
POST   /v1/runs
GET    /v1/runs/{run_id}
GET    /v1/runs/{run_id}/events      ← 실시간 이벤트 스트림
POST   /v1/runs/{run_id}/approval    ← 위험 명령 승인
POST   /v1/runs/{run_id}/steer       ← 진행 중 방향 전환
POST   /v1/runs/{run_id}/stop

# 세션 CRUD
GET    /api/sessions
POST   /api/sessions
GET    /api/sessions/{id}
PATCH  /api/sessions/{id}
DELETE /api/sessions/{id}
GET    /api/sessions/{id}/messages
POST   /api/sessions/{id}/fork
POST   /api/sessions/{id}/chat
POST   /api/sessions/{id}/chat/stream   ← 스트리밍
POST   /api/sessions/{id}/model

# 기타
GET    /v1/health, /v1/capabilities, /v1/skills, /v1/toolsets
POST   /v1/artifacts/upload
GET    /v1/artifacts/download/{artifact_id}
POST   /v1/browser-control/register
GET    /v1/browser-control/ws
```

**`/v1/chat/completions`가 OpenAI 호환**이므로 기존 OpenAI SDK의 base URL만 바꾸면 그대로 동작합니다.

### 이미 있는 React 대시보드

`web/` 폴더는 이미 완성된 React 앱입니다. 처음부터 짤 필요가 없습니다.

```json
"react": "19.2.7",  "react-router": "8.3.0",
"tailwindcss": "4.3.3",  "@xterm/xterm": "6.0.0",
"motion": "12.42.2",  "lucide-react", "@observablehq/plot"
```
```
web/src/  ├ components/  ├ pages/  ├ hooks/
          ├ contexts/    ├ i18n/   └ themes/
```

```bash
hermes dashboard      # 웹 관리 패널 + 내장 채팅 실행
```

### 구현 스케치

**① Hermes 기동**
```bash
hermes gateway setup      # API Server 선택
hermes gateway start      # → http://localhost:8642
```

**② PHP 중계 (`api/chat.php`)**
```php
<?php
session_start();

// 1) 인증
if (!isset($_SESSION['user_id'])) {
    http_response_code(401);
    exit(json_encode(['error' => 'login required']));
}

// 2) 사용량 제한 (핵심)
if (getMonthlyUsage($_SESSION['user_id']) >= getPlanLimit($_SESSION['user_id'])) {
    http_response_code(429);
    exit(json_encode(['error' => 'quota exceeded']));
}

// 3) Hermes로 중계
$ch = curl_init('http://localhost:8642/v1/chat/completions');
curl_setopt_array($ch, [
    CURLOPT_POST           => true,
    CURLOPT_POSTFIELDS     => file_get_contents('php://input'),
    CURLOPT_RETURNTRANSFER => true,
    CURLOPT_HTTPHEADER     => [
        'Content-Type: application/json',
        // 키는 서버에만 존재 — 브라우저로 절대 내보내지 않음
        'Authorization: Bearer ' . getenv('API_SERVER_KEY'),
    ],
]);
$response = curl_exec($ch);

// 4) 사용량 기록
incrementUsage($_SESSION['user_id']);

header('Content-Type: application/json');
echo $response;
```

**③ React 프론트 (`src/hooks/useChat.js`)**
```jsx
export function useChat() {
  const [messages, setMessages] = useState([]);
  const [loading, setLoading] = useState(false);

  async function send(text) {
    setLoading(true);
    const next = [...messages, { role: 'user', content: text }];
    setMessages(next);

    // PHP만 호출 — Hermes 주소는 브라우저가 알 필요 없음
    const res = await fetch('/api/chat.php', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ messages: next }),
    });

    const data = await res.json();
    setMessages(m => [...m, data.choices[0].message]);
    setLoading(false);
  }

  return { messages, send, loading };
}
```

### 🚨 보안 필수 수칙

| 절대 금지 | 올바른 방법 |
|---|---|
| React에서 Hermes 직접 호출 | 반드시 **PHP를 경유** |
| `API_SERVER_KEY`를 프론트에 포함 | **서버 환경변수에만** |
| 8642 포트를 외부에 개방 | **localhost 바인딩 유지** |
| 사용량 제한 없이 공개 | **플랜별 상한 필수** |

> 키가 프론트에 노출되면 제3자가 내 계정으로 AI를 사용합니다. **PHP 중계는 선택이 아니라 필수입니다.**

### 언어별 담당 범위

| 만들 것 | 언어 | 난이도 |
|---|---|---|
| 웹 화면 | **React** | ⭐⭐ |
| 회원 / 결제 / 사용량 | **PHP** | ⭐⭐ |
| 스킬팩 | **마크다운** | ⭐ |
| 예약 자동화 | **자연어** | ⭐ |
| 카카오톡 플러그인 | Python | ⭐⭐⭐⭐ |
| 엔진 수정 | Python | ⭐⭐⭐⭐⭐ (권장 안 함) |

### 개발 일정 (PHP + React 경험자 기준)

```
1주차  hermes gateway start → curl/Postman으로 API 검증
2주차  PHP 중계 파일 1개 작성 → 동작 확인
3~4주  React 채팅 UI 연결 → 개인용 버전 완성
5~6주  로그인 + 사용량 제한
7~8주  결제(토스페이먼츠/아임포트) 연동 → 서비스 오픈
```

---

## 14. 학습 로드맵

| 시기 | 할 일 | 목표 |
|---|---|---|
| **1일차** | 설치 → `hermes setup` → 대화해보기 | 사용감 익히기 |
| **2~3일차** | `/help`, `/model`, `/new`, `/status` 실습 · 스킬 구경 | 명령어 익숙해지기 |
| **4~5일차** | `hermes gateway setup` → 텔레그램 연결 | 폰으로 AI 사용 |
| **1주차** | cron으로 매일 자동 작업 등록 | 무인 자동화 경험 |
| **2주차~** | MCP 연동 · 스킬 제작 · VPS 배포 | 확장 |
| **1개월~** | 12장 수익화 시나리오 착수 | 제품화 |

### 최소한 이 5개만 기억하면 됩니다

```bash
hermes            # 대화 시작
hermes setup      # 설정
hermes model      # 모델 변경
hermes doctor     # 문제 진단
/help             # 대화 중 도움말
```

---

## 부록: 한 줄 요약

> **Hermes Agent는 "엔진"입니다.**
> Python 코드를 건드리지 않고, 내장 REST API 위에 React/PHP로 제품을 얹는 것이 가장 빠른 길입니다.
> 그리고 **파는 것은 코드가 아니라, 남의 시간을 되돌려주는 결과**입니다.

---

<div align="center">

**저장소** · https://github.com/bmshin94/hermes-agent
**원본** · https://github.com/NousResearch/hermes-agent
**문서** · https://hermes-agent.nousresearch.com/docs/

작성일: 2026-09-09 · 라이선스: MIT

</div>
