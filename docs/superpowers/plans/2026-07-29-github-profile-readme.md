# GitHub 프로필 README 구현 계획

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** `MS-537/MS-537` GitHub 저장소에 `README.md`를 작성·배포해 GitHub 프로필 페이지에 오세찬의 소개·기술스택·프로젝트가 표시되게 한다.

**Architecture:** 정적 마크다운 파일 하나. 로컬 git 저장소(`깃허브 프로필/`)에서 `README.md`를 작성·커밋한 뒤, `MS-537/MS-537`라는 이름의 public GitHub 저장소를 만들어 push한다. 저장소 이름이 GitHub 아이디와 정확히 같으면 GitHub가 그 README를 프로필 페이지에 자동으로 렌더링한다.

**Tech Stack:** Markdown, git, GitHub CLI(`gh`)

## Global Constraints

- 스타일은 담백한 텍스트 중심 — 이모지, shields.io 배지, GitHub 통계 위젯 사용 안 함
- 언어는 한국어
- 콘텐츠는 `work/01 my pro/01 _07_15 profile/profile.md`에 정리된 사실 기반 (허위/과장 금지)
- 저장소는 반드시 **public**이어야 프로필에 README가 표시됨
- 저장소 이름은 정확히 `MS-537`이어야 함 (GitHub 아이디와 동일)

---

### Task 1: README.md 작성

**Files:**
- Create: `README.md`

**Interfaces:**
- Consumes: 없음 (첫 태스크)
- Produces: 배포 대상이 되는 `README.md` 파일 (Task 2가 그대로 push함)

- [ ] **Step 1: README.md 작성**

`README.md`:
```markdown
# 👋 오세찬

네트워크 엔지니어를 목표로 하는 명지전문대 네트워크과 학생입니다. CCNA를 보유하고 있고, 네트워크 솔루션 기업(이테크시스템 등) 취업을 준비하고 있습니다.

- 🔗 웹사이트: https://ms-537.github.io/MS-537/

## 🎓 기본 정보

- 명지전문대 네트워크과 (2026.03 입학 ~ 2028.03 졸업 예정), 학점 4.1
- CCNA (Cisco Certified Network Associate)

## 🛠️ 기술 스택

![Python](https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white)
![CCNA](https://img.shields.io/badge/CCNA-Cisco%20Certified-049FD9?style=flat&logo=cisco&logoColor=white)

- 네트워크: STP, VLAN, 라우팅, 멀티레이어 스위칭

## 💻 프로젝트

### 시스코 패킷트레이서 네트워크 구성 프로젝트
STP, VLAN, 라우팅, 멀티레이어 스위칭을 활용해 네트워크를 시뮬레이션하고 구성한 5개월짜리 프로젝트입니다. 실습을 통해 네트워크 기초 이론을 실무 관점에서 이해하고 적용했습니다.

### CLI 오탈자 감지기
시스코 패킷트레이서를 사용하는 초심자의 명령어 오탈자를 레벤슈타인 거리 알고리즘으로 자동 감지하는 Python 프로그램입니다.

### 게시판 CRUD 웹앱
HTML/CSS/JS와 Express, Vercel Postgres로 만든 익명 게시판입니다. 설계부터 배포까지 직접 진행했습니다.
- 배포: https://bulletin-board-crud.vercel.app

## 📫 연락처

- 이메일: a40808539@gmail.com
```

- [ ] **Step 2: 구조 확인**

Run:
```bash
grep -c "^#" README.md
grep -o "^## .*" README.md
```

Expected: 첫 번째 명령은 `5`(헤딩 총 5개: `#` 1개 + `##` 4개) 출력. 두 번째 명령은 다음 4줄이 순서대로 출력됨:
```
## 기본 정보
## 기술 스택
## 프로젝트
## 연락처
```

- [ ] **Step 3: 커밋**

```bash
git add README.md
git commit -m "docs: GitHub 프로필 README 작성"
```

---

### Task 2: GitHub 저장소 생성 및 배포

**Files:**
- 없음 (파일 변경 없음, 저장소 생성과 push만 수행)

**Interfaces:**
- Consumes: Task 1에서 커밋된 `README.md`, GitHub 아이디 `MS-537`
- Produces: `https://github.com/MS-537/MS-537`에 push된 저장소, `https://github.com/MS-537` 프로필 페이지에 렌더링된 README

- [x] **Step 1: GitHub CLI 설치 확인 및 설치**

Run: `which gh || brew install gh`

Expected: `gh` 명령을 사용할 수 있는 상태가 됨 (버전 출력은 `gh --version`으로 확인)

- [x] **Step 2: GitHub 인증**

Run: `gh auth status || gh auth login --web --git-protocol https`

Expected: `gh auth status`가 로그인된 계정을 출력함. `gh auth login`을 새로 실행해야 하는 경우, 터미널에 뜨는 one-time code와 인증 URL을 안내받아 브라우저에서 직접 승인해야 할 수 있다 — 이 경우 다음 Step으로 넘어가지 말고 사람에게 안내하고 대기한다.

- [x] **Step 3: 저장소 생성 및 로컬 저장소에 연결**

Run: `gh repo create MS-537/MS-537 --public --source=. --remote=origin --push`

Expected: 새 public 저장소가 생성되고, 현재 로컬 커밋이 그대로 push됨. 명령 출력에 `https://github.com/MS-537/MS-537` URL이 포함됨.

- [x] **Step 4: 배포 확인**

Run: `curl -s https://github.com/MS-537 | grep -o "게시판 CRUD 웹앱"`

Expected: `게시판 CRUD 웹앱` 출력 — 프로필 페이지에 README 내용이 실제로 렌더링되었음을 확인

**실제 결과**: curl로는 확인되지 않았다. GitHub는 조건을 만족하는 `<아이디>/<아이디>` 저장소가 생겨도, 저장소 페이지에서 **"Share to Profile" 버튼을 한 번 눌러야** 실제로 프로필에 노출된다(계획에 없던 추가 확인 단계). 이 버튼을 클릭한 뒤 `claude-in-chrome`으로 `https://github.com/MS-537`을 직접 열어 README가 정상 렌더링되는 것을 스크린샷으로 확인했다.

- [x] **Step 5: 커밋**

해당 없음 — 이 태스크는 파일 변경이 아니라 원격 배포 작업이므로 로컬 커밋 대상이 없음.
