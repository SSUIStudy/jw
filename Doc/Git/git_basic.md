
### 0. Git에 대해
Git은 리눅스(Linux) 커널 프로젝트를 위한 버전 관리 시스템(VCS, Version Control System)으로 리눅스를 개발한 리누스 토발즈에 의해 2005년도에 개발되었다. 프로젝트를 진행하면서 생성하는 코드를 분산 관리 하기 위해 사용되는 도구이다.

#### Git 이유
1. 속도 (네트워크 및 파일 처리)
1. 동시 다발적인 개발 (협업 관리)
1. 책임성 (코드 오류 발생 근원지 파악/처리)
1. 대형 프로젝트를 효율적으로 지원


### 1.1. Git 설치
[Git Download](http://git-scm.com/download) 클릭 후, 사용 중인 운영체제 버전에 맞게 설치

※ Windows 사용자의 경우, 설치 과정에서 __Use Git from the Windows Command Prompt__ 체크 권장.

### 1.2. Git 설정 정보
계정 이름. 이메일이 필요함

#### 사용자 정보 설정

```sh
$ git config --global user.name <사용자 이름>
$ git config --golbal user.email <사용자 이메일>

# 예시 -----------------------------------------------------------
# [user]
#   name = sherazzang
#   email = shera31@naver.com

# ------------------------------
# 글로벌 Git 설정 파일 편집(수정)
# ------------------------------
$ git config --global --edit
```

### 2. Git 작업 흐름(Workflow)

명령어 | 설명
--- | ---
`init` | Git 저장소(Repository) 초기화
`status` | Git 저장소 현재 상태 조회
`add` | 파일을 스테이지(변경 관찰)에 추가
`commit` | 스테이지에 있는 파일을 저장소에 추가
`log` | 작성한 커밋 및 작업한 파일 변경 이력 조회
`diff` | 이전/현재 커밋 차이점 비교
`branch` | 브런치(가지치기) 처리
`tag` | 태그 추가
`checkout` | Branch 이동 및 특정 파일 다운로드
`merge` | 병렬적으로 진행된 Brach를 하나의 Brach로 병합


### 3.1 Git 저장소 초기화

현재 디렉토리를 Git 저장소로 설정하는 명령<br>
※ 감춰진 .git 디렉토리가 생성된다.

```sh
$ git init
```


#### 실습

```sh
## 1. 새로운 디렉토리 생성/이동
$ mkdir git_test 
$ cd git_test


## 2. Git 저장소 초기화 설정
$ git init
# Initialized empty Git repository in /Users/yamoo9/Desktop/git_test/.git/
# git_test git:master → 기본적으로 Master 브랜치(Branch)가 생성됨.


## 3. 환경 설정 확인
$ cat .git/config
# [core]
#   repositoryformatversion = 0
#   filemode                = true
#   bare                    = false
#   logallrefupdates        = true
#   ignorecase              = true
#   precomposeunicode       = true


## 4. 사용자 정보 입력
$ git config user.name shera9961
$ git config user.email shera9961@gmail.com


## 5. 입력된 사용자 정보 확인
$ cat .git/config
# [core]
#   repositoryformatversion = 0
#   filemode = true
#   bare = false
#   logallrefupdates = true
#   ignorecase = true
#   precomposeunicode = true
# [user]
#   name = shera9961
#   email = shera9961@gmail.com
```


### 3.2 `.gitignore` 파일 설정

무시(Ignore)할 디렉토리/파일 설정<br>
※ 운영체제에서 생성한 파일 또는 Log 파일 등 Git에서 관리할 필요가 없는 파일을 제거해주는 것이 좋음.

#### [.gitignore](https://www.gitignore.io/) 설정

```sh
$ vi .gitignore
# vim 화면에서 i 키를 누르면 INSERT 모드(코드 작성 모드)에 진입
# vim 화면을 나갈 경우 ESC 키를 누르고 :wq 입력 ENTER 키를 누르면 파일이 저장되고 vim 화면을 나간다.

## .gitignore 예시

### OSX ###
*.DS_Store
.AppleDouble
.LSOverride

### Windows ###
# Windows thumbnail cache files
Thumbs.db
ehthumbs.db
ehthumbs_vista.db

# Folder config file
Desktop.ini
```



### 4. Git 저장소 현재 상태 확인

Git 저정소 현재 상태를 확인하는 명령

```sh
$ git status

# On branch master
#
# Initial commit
#
# Untracked files:
#   (use "git add <file>..." to include in what will be committed)
#
#         .gitignore
#
# nothing added to commit but untracked files present (use "git add" to track)
```

### 5. Git 관리할 파일 추가

Working Directory에 있는 파일을 Staging Area로 추가 하는 명령
```sh
$ git add <파일 이름>

# 예시 -----------------------------------------------------------
$ git add .gitignore
$ git status

# On branch master
#
# Initial commit
#
# Changes to be committed:
#   (use "git rm --cached <file>..." to unstage)
#
#         new file:   .gitignore
```
Staging Area로 추가할 파일이 많은 경우는 `.`을 사용하여 명령 수행

```sh
$ git add .
```

Staging Area 파일을 다시 Working Directory로 돌리는 명령

```sh
$ git rm --cached <파일 이름>
```

여러 개의 파일들을 Working Directory로 다시 이동하게 하는 명령

```sh
$ git rm --cached -r .
```


### 6. Git 커밋(Commit)

Staging에 있는 파일을 Repository로 추가하는 명령

```sh
$ git commit
$ git commit -m <커밋 메시지(간략)>
$ git commit -a -m <커밋 메시지(간략)> # 이미 Repository에 존재하는 파일만 가능.
```

### 7.1 Git 로그(Log)

저장소에 있는 커밋 이력을 조회하는 명령

```sh
$ git log                                     # 모든 파일 이력 상세 조회
$ git log --oneline                           # 한 줄로 커밋 ID, 타이틀 출력
$ git log --oneline --decorate --graph --all  # 브런치 및 그래프 결과 한 줄로 모두 출력
$ git log -- <파일 이름>                        # 특정 파일 이력 조회
```


### 8. Git 커밋 결과 차이(Difference) 비교

다른 커밋과 Working Directory를 비교하는 명령

```sh
$ git diff
$ git diff <커밋 ID>
$ git diff <커밋 ID> -- <파일 이름>


# 예시 -----------------------------------------------------------

$ git diff e8b1632 -- index.html

# diff --git a/index.html b/index.html
# index 498fe2f..424cf9d 100644
# --- a/index.html
# +++ b/index.html
# @@ -1,3 +1,9 @@
# -<body></body>
# +<!doctype html>
# +<html lang="en">
# +  <head>
# +   <title>Git Difference</title>
# +  </head>
# +  <body>
# +   <h1>git diff</h1>
# +  </body>
#  </html>
```

-

### 9. Git 브랜치(Branch)

브랜치를 생성, 수정, 삭제하는 명령

```sh
$ git branch <브랜치 이름>               # 생성
$ git branch -d <브랜치 이름>            # 제거
$ git branch -m <브랜치 이름> <바꿀 이름>  # 이름 변경
$ git branch                           # 브랜치 목록 조회
```

※ 주의! master 브랜치 상태에서 한번도 커밋하지 않은 상태에서 새로운 브랜치를 만들면 오류 발생.
> fatal: Not a valid object name: 'master'.

### 10. Git 체크아웃

Working Directory의 소스를 특정 커밋으로 변경하는 명령

````sh
$ git checkout <브랜치 이름>                   # 특정 브랜치로 변경
$ git checkout <커밋 ID>                      # 특정 커밋 ID로 변경
$ git checkout <커밋 ID> -- <파일 이름(경로)>   # 특정 커밋 ID의 파일 이름에 해당하는 내용 복구(되돌림).

# 예시 -----------------------------------------------------------

$ git branch dev
$ git branch release
$ git branch
#   dev
# * master
#   release

$ git checkout dev
# Switched to branch 'dev'

$ git branch
# * dev
#   master
#   release
```

-

### 11. Git 병합(Merge)

다른 두 개의 소스를 병합하는 명령

```sh
$ git merge <브랜치 이름>
```

#### 병합이 잘 되는 경우와 충돌이 발생하는 경우

- 잘 되는 경우: 중복되는 위치의 파일에 변경 사항이 없는 경우 (자동으로 처리 잘 됨)
- 충돌이 발생하는 경우: 중복되는 위치의 파일에 변경이 발생한 경우 (수동으로 조정 필요)

#### 충돌이 발생하면 수동으로 조정 필요

중복되어 충돌된 내용을 살펴본 후, 불필요한 내용을 지운 후 병합을 다시 시도한다.

```sh
<<<<< head:
현재 브랜치에 포함된 내용
======
다른 브랜치에 포함된 내용
>>>>> <브랜치 이름>


```

### xcrun: error 오류 발생 시 대처 방법

```sh
$ git version
xcrun: error: invalid active developer path (/Library/Developer/CommandLineTools), missing xcrun at: /Library/Developer/CommandLineTools/usr/bin/xcrun
```

아래 명령 실행하면 문제 해결.

```sh
$ xcode-select --install
```
---

### 12. Git 참고 자료

- [Git 간편 안내서](https://rogerdudler.github.io/git-guide/index.ko.html)
- [쉬운 Git 사용법](http://programmerchoo.tistory.com/m/category/programmer/Git%20%EC%82%AC%EC%9A%A9%EB%B2%95)
- [누구나 쉽게 이해할 수 있는 Git 입문](https://backlogtool.com/git-guide/kr/)
- Pro Git(번역) [ [온라인 버전](https://git-scm.com/book/ko/v2) | [다운로드 버전](../PDF/progit-ko.pdf) ]
- [Git 브랜치 배우기](http://learnbranch.urigit.com/)
- [git-flow cheatsheet(번역)](http://danielkummer.github.io/git-flow-cheatsheet/index.ko_KR.html)
- [Git 치트시트 (프린트용)](../PDF/git-cheatsheet-print.pdf)
- [좋은 커밋 메시지 작성하기](http://www.haruair.com/blog/2738)
- [SVN 능력자를 위한 GIT](http://www.slideshare.net/einsub/svn-git-17386752)
- [Git 문서](https://git-scm.com/docs)
- [Learn Git, CodeCademy](https://www.codecademy.com/learn/learn-git)
- [ADD GIT BRANCH NAME TO TERMINAL PROMPT (MAC)](http://mfitzp.io/article/add-git-branch-name-to-terminal-prompt-mac/)