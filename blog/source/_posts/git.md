---
title: 깃 연습
date: 2018-11-28 09:09:18
tags:
categories:
- 개발공부
- 뉴딜과정
---

# Git 기본 사용법

## **버전 관리 시스템**(VCS; Version Control System)이란? 

- 개요
  - **파일 변화를 시간에 따라 기록했다가 나중에 특정 시점의 버전을 다시 꺼내올 수 있는 시스템**이다.
  - 파일 별로 이전 상태로 되돌리거나 프로젝트 통째로 이전 상태로 되돌릴 수 있다.
  - 시간에 따라 수정 내용을 비교해 볼 수 있다.
  - 누가 문제를 일으켰는지 **추적**할 수 있고, 누가 언제 만들어낸 이슈인지도 알 수 있다.
- 종류
  - 로컬 버전 관리 시스템
    - 간단한 데이터베이스를 이용하여 파일에서 변경되는 부분(patch set)을 관리한다.
    - 예) RCS(Revision Control System), 이클립스
  - 중앙집중식 버전 관리
    - 파일의 마지막 스냅샷만 받는다(checkout).
    - 스냅샷(snapshot)? 특정 시점의 파일 버전을 기록한 것.
    - 만약 서버에 문제가 생기면 모든 변경 내력(history)을 잃는다.
    - 예) CVS, Subversion, Perforce 등
      - CVS
        1. 개발자 A의 PC에 프로젝트를 하나 만든들어 체크인(서버에 올린다)한다.
        2. 다른 개발자 B가 이를 체크아웃(복사)하여 작업을 한다.
        3. 작업이 끝난 B는 이를 다시 서버에 올린다. **이 때 파일의 전체가 올라간다.** 때문에 네트워크의 오버헤드가 발생한다.
      - subversion
        - CVS의 단점을 해결하기 위해 등장한 것으로, 변경된 것만 서버에 올린다.
        - 변경 기록은 모두 서버에 있다.
          **로컬에 없다, 로컬은 단순한 작업 디렉토리**
          때문에 서버가 장애가 발생했을 경우 변경 기록이 모두 유실된다.
  - 분산 버전 관리 시스템
    - 저장소 전부를 복제한다. 
    - **변경 내력(history)까지 모두 복제**한다.
    - 예) **Git**, Mercurial, Bazaar, Darcs 등

## Git

리눅스 커널 관리를 위해 리누스 토발즈 및 리눅스 개발 커뮤니티에서 개발하였다. 2005년 4월에 개발을 시작하여 2005년 7월 11일에 첫 버전을 출시하였다.

- 목표
  - 빠른 속도
  - 단순한 구조
  - 수천 개의 동시다발적인 브랜치가 가능한 비선형적인 구조
  - 완벽한 분산
  - 대형 프로젝트에서도 유용할 것

### Git의 파일 상태

Git은 다음 세가지 상태로 파일을 관리한다.

- Committed 
  - 로컬 데이터베이스에 안전하게 저장되었다는 뜻이다.
- Modified
  - 파일이 변경되었지만 아직 로컬 데이터베이스에 저장되지 않았다는 뜻이다.
- Staged
  - 로컬 데이터베이스에 저장할 파일임을 표시했다는 뜻이다.
  - 다음에 커밋을 수행할 때 Staged로 표시된 파일의 변경 내용이 저장될 것이다.

### Git 프로젝트의 단계 

Git 프로젝트는 다음 세 가지 단계로 관리된다.

- .git Directory
  - **프로젝트의 메타데이터와 객체 데이터베이스를 저장**하는 곳.
  - 즉 변경 내력과 그 내용이 저장된다.
  - 원격 저장소를 clone 할 때 생성됨.
- Working Directory
  - 특정 버전을 체크아웃(checkout)하면, `.git` 디렉토리에 있는 압축된 데이터베이스에서 파일을 가져와서 작업 디렉토리를 만든다.
- Staging Area
  - `.git` 디렉토리에 존재하는 단순한 파일이다.
  - commit 할 파일의 정보(*스냅샷이라 부른다*)를 담고 있다. 
  - `git commit`을 실행하면 이 스냅샷에 기록된 파일을 저장소에 보관하는 것이다.
  - 커밋을 한 후에는 Staging Area는 새 스냅샷을 준비한다.
  - Staging Area는 *인덱스*라는 이름으로도 부르지만, *Staging Area* 가 표준 이름으로 사용되는 추세이다.

### Git 파일의 상태 변화 

다음은 Git 명령에 따른 파일의 상태 변화를 보여준다. 

```
         Working Directory         | Staging Area | .git Directory(Repository)
[Untracked] [Unmodified] [Modified]|[   Staged   ]|[  Committed  ]    
-----------------------------------|--------------|---------------------------
    +------------------------------------->>                      : git add
                              +----------->>                      : git add
                                           +------------->>       : git commit
                  <<-----------------------+                  
                  +---------->>                                   : 파일 편집
    <<------------+                                               : 파일 삭제
```

- **Untracked** - 작업 디렉토리에 새로 파일을 추가한 경우. 아직 스냅샷이나 Staging Area에 등록되지 않은 파일. 즉 git의 버전 관리 대상이 아닌 상태.
- **Unmodified** - 마지막 커밋(commit) 이후에 아무것도 수정하지 않은 상태.
- **Modified** - 파일의 내용을 변경한 상태.
- **Staged** - 다음 커밋에서 저장하도록 Staging Area에 등록되고 표시된 상태.

## Git 명령

다음은 콘솔에서 git 명령을 사용하는 방법이다.

### git config

- git의 사용 환경을 설정한다.
- /etc/gitconfig 설정 파일
  - 시스템의 모든 사용자와 모든 저장소에 적용되는 설정.
  - `git config --system` 옵션으로 이 파일을 읽고 쓴다.
  - Windows OS의 경로 - C:/ProgramData/Git/config
- ~/.gitconfig, ~/.config/git/config 설정 파일
  - 특정 사용자만 적용되는 설정.
  - `git config --global` 옵션으로 이 파일을 읽고 쓴다.
  - Windows OS의 경로 - C:/Users/사용자홈/.gitconfig
- .git/config
  - 특정 저장소에만 적용되는 설정.
  - `git config` 옵션을 지정하지 않으면 이 파일을 읽고 쓸 수 있다.

```
예1) 사용자 이름 설정하기
$ git config --global user.name "Jinyoung Eom"
$ git config --global user.email "jinyoung.eom@gmail.com"
```

```
예2) 기본 텍스트 편집기 설정하기
$ git config --global core.editor emacs
```

```
예3) 설정 확인하기
$ git config --list
```

```
예4) 특정 값 확인하기
$ git config user.name
```

### git help [명령], git [명령] --help

- 명령어에 대한 도움말을 볼 수 있다.

```
예1) config 명령에 대한 도움말 보기
$ git help config
$ git config --help
```

### git init

- 존재하는 폴더를 깃 저장소로 만든다.
- .git 폴더를 생성한다.
  - 깃 저장소 관련 파일을 두는 폴더이다.

```
예) ~/git/myProject 폴더를 깃 저장소로 설정하기
~/git/myProject$ git init
```

### .gitignore

- Git으로 관리하지 않을 파일을 지정한다.
- 예를 들면 로그 파일(.log)이나 빌드 도구가 자동으로 생성한 파일 또는 디렉토리 등.
- 패턴을 사용하여 Git이 무시할 파일을 지정한다.
  - 빈 줄이나 `#`으로 시작하는 줄은 주석으로 간주한다.
  - 표준 Glob 패턴을 사용한다.
  - `/`로 시작하면 하위 디렉토리에 적용되지 않는다.
  - 디렉토리는 끝에 `/`을 붙인다.
  - `!`로 시작하는 파일은 무시하지 않는다.

```
예1) 주석을 표시하는 방법
#이것은 주석입니다. 또는 빈 줄.

예2) bin/ 디렉토리를 통째로 무시하기
bin/

예3) 현재 디렉토리의 *.log 파일만 무시하기. src/*.log처럼 기타 하위 디렉토리에 있는 *.log 파일은 포함하기
/*.log

예4) src/*.class 파일은 무시하고, src/main/*.class 파일은 포함하기
src/*.class

예5) src 디렉토리 및 그 하위 디렉토리에 있는 *.class 파일 무시하기
src/**/*.class

예6) 현재 디렉토리 및 그 하위 디렉토리에 있는 모든 *.log 파일 무시하기
*.log

예7) 확장자가 '.o' 또는 '.a'인 파일 무시하기
*.[oa]

예8) *~
파일명이 ~로 끝나는 파일

예9) 만약 *.log 파일을 무시한다면, cotext.log 파일은 무시하지 않고 포함하기
!context.log
```

### git clone [url][폴더]

- url로 지정한 서버의 저장소를 로컬로 복제한다.
- 폴더 이름을 지정하지 않으면 저장소 이름으로 폴더를 만들어 복제한다.
- 폴더 이름을 지정하면 그 이름으로 폴더를 만들어 복제한다.

```
예1) github.com의 저장소를 로컬에 복제하기
$ git clone https://github.com/eomjinyoung/myProject
```

```
예2) github.com의 저장소를 myProject2라는 이름으로 폴더를 만들어 로컬에 복제하기
$ git clone https://github.com/eomjinyoung/myProject myProject2
```

### git add [파일]

- 작업 디렉토리에 새로 추가한 파일인 경우 Staging Area에 새로 추가된 파일임을 기록한다.
- 변경한 파일인 경우 Staging Area에 변경된 파일임을 기록한다.
- 이렇게 Stating Area에 기록된 파일은 한 스냅샷으로 묶이며 커밋할 때 이 스냅샷의 파일들이 저장소에 보관된다.
- Staging Area에 기록된 파일은 *Staged* 상태가 된다.

```
예1) 현재 폴더에서 확장자가 c인 파일을 Staging Area에 기록하기
$ git add *.c
```

```
예2) 현재 폴더에서 LICENSE 이름을 가진 파일을 Staging Area에 기록하기
$ git add LICENSE
```

```
예3) 현재 폴더나 하위 폴더의 파일 중에 변경되거나 추가된 파일을 Staging Area에 기록하기
$ git add .
```

### git commit -m '이번 스냅샷을 저장하는 이유'

- Staging Area에 기록된 파일들(스냅샷)을 로컬 저장소에 보관한다. 
- 파일을 새로 추가하거나 변경하였다면 반드시 `git add`를 실행하여 Staging Area에 기록해야 한다.
- 기록되어 있지 않은 파일이나 변경 사항은 저장소에 보관되지 않는다.
- 커밋 할 때 마다 스냅샷에 대해 새 체크섬(checksum) 값이 부여되고 이 값이 스냅샷을 구분하는 식별자로 사용된다.

```
예1) Staging Area에 있는 파일을 저장소에 보관하기
$ git commit -m '첫 번째 버전'
```

```
예2) git add + git commit = git commit -a -m '설명'
    저장소에 넣기 전에 매번 Staging Area에 기록하는 것은 매우 귀찮은 일이다.
    이를 한 번에 할 수 있다.
    단 Tracked 파일(Staging Area에 있거나 저장소에 있는 파일)만 대상으로 한다.
    새로 추가한 파일 중에 아직 staged 상태가 아닌 파일은 제외한다.
$ git commit -a -m '바로 저장소로 보관하기'
```

### git status

- 작업 파일의 상태를 조회한다.

```
예1) 현재 작업 디렉토리의 파일 상태 보기
$ git status
On branch master
Your branch is up to date with 'origin/master'.

Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	modified:   src/main/webapp/test02.html
	modified:   src/main/webapp/test03.html
	new file:   src/main/webapp/test05.html

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   src/main/webapp/index.html
	modified:   src/main/webapp/test01.html

Untracked files:
  (use "git add <file>..." to include in what will be committed)

	src/main/webapp/test05.html
```

```
예2) 현재 작업 디렉토리의 파일 상태를 짤막하게 보기
$ git status --short
 M src/main/webapp/test01.html
M  src/main/webapp/test02.html
MM src/main/webapp/test03.html
?? src/main/webapp/test04.html
A  src/main/webapp/test05.html
AM src/main/webapp/test06.html
```

[출력 결과 보는 법]

- [Staged 상태][Unstaged 상태] [파일 경로]
- `_M src/main/webapp/index.html`
  - 작업 디렉토리에 있는 파일을 변경한 경우.
- `M_ src/main/webapp/test01.html`
  - 작업 디렉토리에 있는 파일을 변경한 후,
    'git add' 명령으로 Stated 상태로 만든 경우.
- `MM src/main/webapp/test02.html`
  - Staged 상태의 파일을 다시 변경한 경우.
- `?? src/main/webapp/test03.html`
  - 작업 디렉토리에 새로 파일을 추가한 경우.
- `A_ src/main/webapp/test04.html`
  - 새로 추가한 파일을 'git add' 명령으로 Stating Area에 등록한 경우.
- `AM src/main/webapp/test05.html`
  - 새로 추가한 파일을 Staged 상태로 만든 후, 다시 변경한 경우.

### git diff [옵션]

- 파일의 변경 내용을 비교한다.

```
예1) 작업 디렉토리에서 변경한 파일과 Staging Area에 등록된 파일과 비교하여
     변경경 전/후를 출력하기
    
$ git diff
diff --git a/src/main/webapp/test01.html b/src/main/webapp/test01.html
index 80ba906..3322e11 100644
--- a/src/main/webapp/test01.html
+++ b/src/main/webapp/test01.html
@@ -8,6 +8,6 @@
     <link rel="stylesheet" type="text/css" media="screen" href="main.css" />
 </head>
 <body>
-<h1>Hello!</h1>    
+<h1>Hello!x</h1>    
 </body>
-</html>
\ No newline at end of file
+</html>
diff --git a/src/main/webapp/test03.html b/src/main/webapp/test03.html
index 37f2fb7..57c25bf 100644
...
```

```
예2) 특정 파일에 대해 변경 전/후를 비교하기
$ git diff src/main/webapp/test01.html
...
```

```
예3) Staging Area에 있는 파일과 저장소에 있는 파일을 비교하기
    이 경우 새로 추가한 파일도 보여준다.
    --staged 와 --cached 는 같은 옵션이다.
$ git diff --staged 또는 git diff --cached
diff --git a/src/main/webapp/test02.html b/src/main/webapp/test02.html
index 53e43f2..2f411f8 100644
...
diff --git a/src/main/webapp/test03.html b/src/main/webapp/test03.html
index 53e43f2..2f411f8 100644
...
diff --git a/src/main/webapp/test05.html b/src/main/webapp/test05.html
new file mode 100644      <=== 새 파일이 Staging Area에 있는 경우에 이 문구가 붙는다. 
index 0000000..3081b8d
...
diff --git a/src/main/webapp/test06.html b/src/main/webapp/test06.html
new file mode 100644      <=== 새 파일이 Staging Area에 있는 경우에 이 문구가 붙는다.
index 0000000..3081b8d
...

```

### git checkout [파일]

- 작업 디렉토리의 파일을 변경한 후 변경 전으로 되돌릴 때 사용한다.
- Staging Area에 마지막으로 기록된 버전으로 되돌린다.
- `git add`를 **수행한 적이 없다면 Staging Area에는 마지막으로 커밋한 파일을 가리킨다. 따라서 마지막으로 커밋된 파일로 되돌릴 것**이다.
- 최후에는 저장소에 있는 것을 가져온다. 

```
예) src/main/webapp/index.html 파일을 편집 전으로 되돌리기
$ git checkout src/main/webapp/index.html
```

### git rm [파일]

- Staging Area의 기록에서 지정된 파일을 뺀다.
- **작업 디렉토리에 해당 파일이 있다면 그 파일도 자동 삭제**된다. 
- 이전 스냅샷에는 해당 파일이 계속 남아 있다. 
- 잘 사용하지 않는다...

```
예1) 작업 디렉토리에 있는 파일을 삭제한 후 Git에서도 제거하기
$ rm test01.html        <=== 작업 디렉토리에서 파일을 삭제한다.
$ git rm test01.html    <=== Staging Area에 삭제 파일 정보를 등록한다.
                             'rm' 대신 'add'를 사용하여 삭제된 파일을 표시해도 된다.
                             예) git add test01.html
$ git commit            <=== 저장소에서 Staging Area의 정보에 따라 
```

```
예2) 변경한 파일이나 Staging Area에 이미 기록된 파일을 강제로 제거하기
$ git rm -f test01.html <=== 변경된 내용을 버리고 현재 스냅샷에서 빼버린다. 
                        <=== 변경한 파일이 Staging Area에 기록되었더라도 
                             변경된 내용을 버리고 스냅샷에서 빼버린다.
```

### git mv [기존파일명][새파일명]

- 파일 이름을 변경한다.

```
예1) test01.html 파일의 이름을 test02.html로 변경하기
$ mv test01.html test02.html
$ git rm test01.html
$ git add test02.html
```

```
예2) test01.html 파일의 이름을 test02.html로 변경하기 II
    위 작업을 좀 더 쉽게 다음과 같이 단축으로 처리할 수 있다. 
$ git mv test01.html test02.html
```

### git log

- Git 저장소의 변경 내력을 조회한다.

```
예1) 저장소의 변경 내력을 최신 커밋 순으로 출력하기
$ git log
...
commit 80e779cf97f2b8857b1fd4e3796a612470255852   <=== 커밋 체크섬
Author: eomjinyoung <jinyoung.eom@gmail.com>      <=== 커밋한 사람의 이름과 이메일
Date:   Tue Aug 21 10:04:49 2018 +0900            <=== 커밋한 날짜

    ok                                            <=== 커밋할 때 입력한 내용

commit d89524011f2c873fedca8643fa2cb95e02cb6656
Author: Jinyoung Eom <jinyoung.eom@gmail.com>
Date:   Tue Aug 21 09:35:48 2018 +0900

    Initial commit
```

```
예2) 저장소의 변경 내력을 최신 커밋 순서로 출력하는데 최근 두 개의 결과만 출력하기(-2 옵션)
    각 커밋의 변경 내용을 보여주기(-p 옵션)
$ git log -p -2
commit cc898de0b9ea138f554aeb59910b348cb34850f4 (HEAD -> master)
Author: Jinyoung Eom <jinyoung.eom@gmail.com>
Date:   Sun Aug 26 19:55:21 2018 +0900

    p 태그 추가

diff --git a/src/main/webapp/ex03.html b/src/main/webapp/ex03.html
index 0afb588..c0e04ec 100644
--- a/src/main/webapp/ex03.html
+++ b/src/main/webapp/ex03.html
@@ -5,5 +5,6 @@
 </head>
 <body>
 <h1>테스트</h1>
+<p>내용 추가</p>
 </body>
 </html>
...
```

```
예3) 저장소에 대한 각 커밋의 통계 정보를 조회한다.
$ git log ---stat
...
commit c555b1b128453d18ac2a5d3493b79021dce3f470
Author: Jinyoung Eom <jinyoung.eom@gmail.com>
Date:   Sun Aug 26 19:54:50 2018 +0900

    HTML 내용 변경

 src/main/webapp/ex03.html | 6 ++++--              <=== 어떤 파일의 추가되거나 삭제된 줄 수 
 1 file changed, 4 insertions(+), 2 deletions(-)   <=== 요약 정보
...
```

```
예4) 추가되거나 삭제된 내용 중에 '<p>' 문구를 포함한 정보 조회
$ git log -p -S '<p>'
commit cc898de0b9ea138f554aeb59910b348cb34850f4 (HEAD -> master)
Author: Jinyoung Eom <jinyoung.eom@gmail.com>
Date:   Sun Aug 26 19:55:21 2018 +0900

    p 태그 추가

diff --git a/src/main/webapp/ex03.html b/src/main/webapp/ex03.html
index 0afb588..c0e04ec 100644
--- a/src/main/webapp/ex03.html
+++ b/src/main/webapp/ex03.html
@@ -5,5 +5,6 @@
 </head>
 <body>
 <h1>테스트</h1>
+<p>내용 추가</p>
 </body>
 </html>
```

### git commit --amend

- 마지막 커밋을 다시 현재의 Staging Area의 내용으로 덮어쓴다. 
- 그래서 커밋 완료 후 빠뜨린 파일이 있거나 제거하지 못한 파일이 있을 경우 사용한다.
- 마지막 커밋 후에 변경 사항이 없다면 단지 커밋 메시지만 변경한다.
- 이전 커밋을 덮어쓰는 것이지만 커밋의 체크섬은 새로 발급된다.

```
예1) ex03.html을 변경한 후 커밋한다. 그 후에 다시 ex04.html을 변경한 후 이전 커밋과 합친다. 
$ git add ex03.html
$ git commit -m 'ex03 변경'
$ git add ex04.html
$ git commit --amend -m 'ex03 및 ex04 변경'
```

### git reset HEAD [파일]

- 커밋 버전을 취소하는 것
- `git add`를 실행하면 Staging Area에 해당 파일이 기록되어 커밋할 스냅샷으로 묶인다.
- 스냅샷으로 묶인 파일들은 커밋할 때 저장소에 그 변경 내용이 보관된다.
- Staging Area의 현재 스냅샷에서 빼고 싶은 파일이 있다면 이 명령을 사용한다. 
- 이 명령을 수행하면 변경된 상태이지만 아직 Staging Area의 현재 스냅샷에 포함되지 않은 파일이 된다.

```
예1) 
'ex03.html'과 'ex04.html' 파일을 변경한 후 Staging Area에 넣기
$ git add ex03.html ex04.html

Staging Area에 넣은 두 개 파일 중에서 ex03.html은 제외하기 
$ git reset HEAD ex03.html

```

### git revert

- 직전 버전의 것만 제거할 때

- ```
  git revert head
  ```


> ### 그런데 직전 버전이 아닌 과거의 버전으로 돌리고자 할 때
>
> ```
> git revert 커밋아이디
> ```
>
> 를 cmd에 치면
>
> ```
> error: could not revert 커밋아이디... '커밋메시지'
> hint: after resolving the conflicts, mark the corrected paths
> hint: with 'git add <paths>' or 'git rm <paths>'
> hint: and commit the result with 'git commit'
> ```
>
> 에러 발생하고, 파일을 열게 되면
>
> ```
> <<<<<<< HEAD
> 11111
> 33333
> 22222
> =======
> 11111
> >>>>>>> parent of c088056... '22222'
> ```
>
> head부터 내가 쓴 것,
>
> ======는 돌리려고 했던 것의 부모

### git remote 

- 현재 프로젝트에 등록된 원격 저장소를 확인하거나 추가한다.
- `git clone [url]`으로 원격 저장소를 복제하면 원격 저장소가 **origin** 이라는 이름으로 자동 등록된다.
- 서버를 가리키는 포인터

```
예1) 원격 저장소의 이름을 알아내기
$ git remote
origin 

```

```
예2) 원격 저장소의 이름 뿐만아니라 URL도 알아내기
$ git remote -v
origin	https://github.com/eomjinyoung/test.git (fetch)
origin	https://github.com/eomjinyoung/test.git (push)

```

```
예3) 원격 저장소 추가하기
$ git remote add [단축이름] [URL]
$ git remote add cs https://github.com/eomcs/test.git
origin	https://github.com/eomjinyoung/test.git (fetch)
origin	https://github.com/eomjinyoung/test.git (push)
cs	https://github.com/eomcs/test.git (fetch)
cs	https://github.com/eomcs/test.git (push)

```

```
예4) 원격 저장소의 정보를 조회하기
$ git remote show [원격 저장소 이름]
$ git remote show origin
* remote origin
  Fetch URL: https://github.com/eomjinyoung/test.git
  Push  URL: https://github.com/eomjinyoung/test.git
  HEAD branch: master
  Remote branch:                           <=== 로컬 저장소와 연결된 원격 저장소의 브랜치
    master tracked                         
  Local branch configured for 'git pull':  <=== git pull 했을 때 merge 할 
    master merges with remote master            원격 저장소의 브랜치와 로컬 저장소의 브랜치
  Local ref configured for 'git push':     <=== git push 했을 때 push 할 
    master pushes to master (up to date)        로컬 저장소의 브랜치와 원격 저장소의 브랜치

```

```
예5) 원격 저장소의 단축 이름을 변경하기
$ git remote rename [현재 단축이름] [새 단축이름]
$ git remote rename cs eomcs
$ git remote
origin
eomcs

```

```
예6) 원격 저장소를 삭제하기
     - 원격 저장소의 서버 정보가 변경되었을 때
     - 별도의 복제가 필요하지 않을 때 
     - 기여자가 활동하지 않을 때 
$ git remote rm [원격 저장소의 단축이름]
$ git remote eomcs
$ git remote
origin

```

### git fetch [원격저장소이름]

- 로컬에는 없고 원격 저장소에만 있는 데이터를 모두 가져온다.
- 단 가져온 데이터를 로컬 파일에 자동으로 합치지는(merge) 않는다.
- 개발자가 직접 merge 해야 한다.

```
예1) 원격 저장소에 마지막으로 push 한 다음에 변경된 모든 것을 가져오기
$ git fetch origin
```

### git pull

- `git pull` = `git fetch origin` + Merge 
- 즉 원격 저장소의 데이터를 가져온 후에 로컬 파일과 합친다. 
- 원격 저장소를 clone 하게 되면 로컬 저장소에 master 브랜치가 생긴다.
- 로컬 저장소의 master 브랜치는 자동으로 원력 저장소의 master 브랜치를 추적한다.
- 따라서 원격 저장소에서 가져온 데이터를 로컬 저장소의 master 브랜치와 합쳐진다.

```
예1) 원격 저장소의 파일을 가져와 로컬 저장소의 파일과 병합하기 
$ git pull
```

### git push [원격 저장소 이름][로컬 브랜치 이름]

- 로컬 저장소 브랜치를 원격 저장소로 업로드(push) 한다.
- 전제 조건
  - 원격 저장소에 쓰기 권한이 있어야 한다.
  - 아직 다른 사람이 push 한 적이 없다. 
  - 다른 사람이 push 한 적이 있다면, 먼저 원격 저장소의 데이터를 가져와서 merge 한 다음에 push 해야 한다.

```
예1) 로컬 저장소의 master 브랜치를 원격 저장소에 업로드 하기
$ git push origin master
```

### git tag

- 존재하는 태그를 조회한다.

```
예1) 저장소에 존재하는 태그를 조회한다.
$ git tag
v0.1
v0.2
...
```

```
예2) 저장소에 있는 태그 중에서 v10.0 버전의 태그들만 검색하기
$ git tag -l 'v10.0*'
v10.0.0.1
v10.0.0.2
```

```
예3) 현재 저장소에 저장된 파일에 대해 태그 붙이기
     - 마지막 커밋에 대해 태그를 붙인다.
     - 태그를 만든 사람의 이름과 이메일, 날짜, 메시지도 저장한다.
     - 이렇게 붙인 태그를 'Annotated 태그'라 부른다.
$ git tag -a [태그명] -m '태그 메시지'
$ git tag -a v0.1 -m 'my version 0.1'
```

```
예4) 현재 저장소에 저장된 파일, 즉 마지막 커밋에 대해 태그 붙이기
     - 태그에 대한 추가 정보를 입력하지 않는다.
     - 이렇게 붙인 태그를 Lightweight 태그'라 부른다.
$ git tag [태그명]
$ git tag v0.2
```

```
예5) 이전 커밋에 대해 태그 붙이기
$ git tag -a [태그명] [커밋 체크섬]
$ git log --pretty=oneline
75ff5353c41f3a33de4a7da91887d0ecbc2cbca6 (HEAD -> master, tag: v0.2) test..ok2
e08e7f0c4a5d068dcc20148e7e7b007958bd3b05 (tag: v0.1, origin/master, origin/HEAD) test..ok
7c239ac5c89bcca468cfd0f412bef104e25b071a okok
bb4aad9f4c000851f950feefdca4874cfa830734 ex03 편집, ex04 편집
5607c88f763a12557adb69442b054990a1487d4f ex03 편집
d166310b5c4502fc820bbab960094911466745e4 ex03 편집, ex04 편집
bfa6df7c89c245e750c7c59f3c6fb06dfa801a74 다시 커밋
c555b1b128453d18ac2a5d3493b79021dce3f470 HTML 내용 변경
...
$ git tag -a v0.0.1 c555b1b1 -m 'my version 0.0.1'     <=== 중복되지 않는다면, 체크섬의 앞쪽 일부 값만 지정해도 된다.
$ git tag
v0.0.1    <=== 추가된 태그
v0.1
v0.2
```

```
예6) 로컬 저장소에 있는 태그를 서버에 공유하기
$ git push [원격저장소 단축이름] [태그 이름]
$ git push origin v0.1
Counting objects: 1, done.
Writing objects: 100% (1/1), 166 bytes | 166.00 KiB/s, done.
Total 1 (delta 0), reused 0 (delta 0)
To https://github.com/eomjinyoung/test.git
 * [new tag]         v0.1 -> v0.1
```

```
예7) 원격 저장소에 없는 모든 로컬 저장소의 태그를 서버에 공유하기
$ git push origin --tags
Counting objects: 7, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (7/7), done.
Writing objects: 100% (7/7), 658 bytes | 658.00 KiB/s, done.
Total 7 (delta 3), reused 0 (delta 0)
remote: Resolving deltas: 100% (3/3), completed with 3 local objects.
To https://github.com/eomjinyoung/test.git
 * [new tag]         v0.0.1 -> v0.0.1
 * [new tag]         v0.2 -> v0.2
```

### git show [태그명]

- 태그 정보와 커밋 정보를 모두 확인한다.

```
예1) 태그 v0.1의 정보와 커밋 정보를 확인하기
$ git show v0.1
tag v0.1
Tagger: Jinyoung Eom <jinyoung.eom@gmail.com>   <=== 태그 붙인 사람 정보
Date:   Mon Aug 27 00:03:56 2018 +0900          <=== 태그 붙인 날짜 

my version 0.1                                  <=== 태그 붙일 때 작성한 메시지

commit e08e7f0c4a5d068dcc20148e7e7b007958bd3b05 (HEAD -> master, tag: v0.1, origin/master, origin/HEAD)
Author: Jinyoung Eom <jinyoung.eom@gmail.com>
Date:   Sun Aug 26 23:29:24 2018 +0900

    test..ok
...
```

```
예2) Lightweight 태그의 정보를 확인하기
    - 태그를 저장할 때 메시지를 지정하지 않았으면 태그 정보가 출력되지 않는다.
$ git show v0.2-lw
      <=== 태그를 붙인 사람의 정보가 없다.
commit 75ff5353c41f3a33de4a7da91887d0ecbc2cbca6 (HEAD -> master, tag: v0.2)
Author: Jinyoung Eom <jinyoung.eom@gmail.com>
Date:   Mon Aug 27 00:13:30 2018 +0900

    test..ok2
...
```

### git checkout -b [새브랜치명][태그명]

- 작업 디렉토리의 내용물을 특정 태그의 커밋 버전으로 바꾼다.
- 태그가 가리키는 커밋의 파일들을 가져오려면 브랜치를 생성해야 한다.
- 특정 태그가 붙은 커밋 버전으로 브랜치를 만들고 작업 디렉토리는 그 브랜치의 파일들로 바꾼다.

```
예1) v0.1 태그가 붙은 커밋 파일들로 version2 라는 이름의 브랜치를 만들고 작업 디렉토리에 가져오기
$ git checkout -b version2 v0.1
Switched to a new branch 'version2'   <=== 작업 디렉토리의 파일들이 변경된다.
```

### git config --global alias.별명 [원래명령어]

- 자주 사용하는 명령어에 대해 별명을 부여한다.

```
예1) status 명령과 status --short 대해 별명을 지정하기
$ git config --global alias.st status
$ git config --global alias.st2 'status --short'
$ git st                <=== 'git status' 와 같다.
$ git st2               <=== 'git status --short' 와 같다.
```

```
예2) Git 별명을 이용하여 'nano' 편집기를 실행하기
$ git config --global alias.별명 '!실행파일명'
$ git config --global alias.nn '!nano'
$ git nn              
[nano 편집기가 실행될 것이다.]
```

------

$ git branch -vv
  b1     c2be10d [origin/b1] v1.3
* b2     664dbb5 v3.1
  master 09fb339 [origin/master: ahead 2] v3.0   <=== 로컬 브랜치가 커밋을 2개 앞서 있다는 의미
  other  ed485e2 [origin/other] v1.2
  other2 ed485e2 [origin/other] v1.2







------







# Git 브랜치 사용법

## 커밋 정보

Git에서 commit을 수행하면 다음의 절차에 따라 커밋 정보를 저장한다.

- `git add` 실행
  - **Blob** 생성
    - Git 저장소에 저장되는 파일이다.
    - 각 파일은 SHA-1 해시 알고르즘으로 계산된 40바이트 크기의 고유의 체크섬(checksum) 값을 가진다.
  - Staging Area에 Blob의 체크섬을 기록한다.
- `git commit` 실행
  - **트리 객체** 생성
    - 디렉토리와 파일의 구조 정보가 들어 있다.
    - 파일 정보는 Blob의 체크섬이다. 
    - 각 트리를 구분하기 위한 SHA-1 해시로 생성한 체크섬을 가진다.
  - **커밋 객체** 생성
    - 작성자, 커미터, 커밋 메시지 등 메타 정보가 들어 있다.
    - 트리 객체를 가리키는 정보가 들어 있다.
    - 각 커밋을 식별하기 위한 SHA-1 해시로 생성한 체크섬을 가진다.
    - 이전 커밋을 가리키기 위해 이전 커밋의 체크섬이 들어 있다.

>  이 모든 것들은 .git에 들어있다!

- 객체들 간의 관계
  - [커밋 객체]----> [트리 객체]----> [Blob 객체들]     

## 브랜치

- Git의 브랜치는 커밋 사이를 이동할 때 사용하는 포인터 같은 것이다.
- 커밋의 체크섬을 이용하여 여러 커밋들 중에서 한 커밋을 가리킨다.
- 즉 새 브랜치를 만드는 것은 단순히 41바이트(40바이트 체크섬 + 1바이트 줄 바꿈 문자)의 파일을 하나 만드는 것에 불과하다. 따라서 브랜치를 여러 개 만들어도 전혀 상관없다. 
- Git은 브랜치를 만들어 작업하고 나중에 merge 하는 것을 권장한다.
- 하루에 수십 번씩 해도 괜찮다고 제안하고 있다. 

### master 브랜치

- `git init`를 통해 Git 저장소를 만들 때 'master'라는 이름으로 기본 브랜치를 생성한다.
- master 브랜치로 작업하는 동안에는 항상 가장 마지막 커밋을 가리킨다.

### HEAD

- 현재 작업 중인 로컬 브랜치(워킹 디렉토리)를 가리키는 특수한 포인터이다.
- 와일드 표시(*)가 붙는다.

### 토픽 브랜치 

- 어떤 한가지 주제나 작업을 위해 만든 짧은 호흡의 브랜치이다. 

### 트래킹 브랜치 = upstream 브랜치

- 원격 브랜치를 체크아웃 하여 만든 로컬 브랜치이다.
- 트래킹 브랜치에서 `git pull` 을 실행하면 이 로컬 브랜치와 연결된 원격 브랜치에서 데이터를 받아 로컬 브랜치로 자동 merge 한다. 

## 브랜치 명령

### git branch

- 브랜치를 관리한다.

```
예1) b1 이라는 이름으로 브랜치를 새로 만들기
        git branch [새 브랜치 이름]
    - 새로 만든 브랜치도 지금 작업하고 있는 커밋을 가리킨다.
    - HEAD 포인터는 브랜치 생성과 상관없이 기존의 브랜치를 계속 가리킨다.
$ git branch b1
$ git log --oneline                   <=== 커밋 정보를 한 줄 씩 출력한다.
f559e21 (HEAD -> master, b1) v0.3
5896279 v0.2
8dd76bf v0.1
5d8d97b (origin/master, origin/HEAD) Initial commit 
```

```
예2) 브랜치 목록을 조회하기 
        git branch
    - 아무런 옵션 없이 실행하면 브랜치의 목록을 출력한다.
$ git branch
  b1
* master      <=== 현재 작업하는 브랜치에 * 가 붙는다.
```

```
예3) 브랜치들 중에서 merge 한 브랜치를 조회하기
$ git branch --merged
```

```
예4) 브랜치들 중에서 merge 하지 않은 브랜치를 조회하기
$ git branch --no-merged
```

```
예5) 브랜치 삭제하기
$ git branch -d b1    <=== merge 되지 않은 브랜치는 삭제되지 않는다.
error: The branch 'b1' is not fully merged.    
If you are sure you want to delete it, run 'git branch -D b1'.

$ git branch -D b1    <=== -D 옵션으로 merge 되지 않은 브랜치를 강제 삭제하라.
Deleted branch b1 (was 519ee27).
```



### git checkout [브랜치 이름]

- HEAD 포인터가 다른 브랜치를 가리키게 한다.
- HEAD 포인터가 가리키는 브랜치가 바뀌면, 작업 디렉토리도 그 브랜치의 커밋 정보에 따라 바뀐다.

```
예1) HEAD 포인터를 b1 브랜치로 옮긴다.
$ git checkout b1
$ git log --oneline                   <=== 로그 정보를 확인해 보라.
f559e21 (HEAD -> b1, master) v0.3     <=== HEAD는 b1을 가리키고 있다. 
5896279 v0.2
8dd76bf v0.1
5d8d97b (origin/master, origin/HEAD) Initial commit
```

```
예2) test04.txt를 만들어 b1 브랜치에 추가하기
    test04.txt 파일을 만들었다고 가정하자!
$ git add test04.txt
$ git commit -m 'v0.4'
$ git log --oneline
9cf510e (HEAD -> b1) v0.4    <=== b1은 새로 커밋한 스냅샷을 가리킨다. HEAD는 현재 작업 브랜치인 b1을 가리킨다.
f559e21 (master) v0.3        <=== master가 가리키는 스냅샷은 변경되지 않는다. 
5896279 v0.2
8dd76bf v0.1
5d8d97b (origin/master, origin/HEAD) Initial commit
```

```
예3) test05.txt를 만들어 b1 브랜치에 추가하기
    test05.txt 파일을 만들었다고 가정하자!
$ git add test05.txt
$ git commit -m 'v0.5'
$ git log --oneline
34fda9c (HEAD -> b1) v0.5    <=== b1은 새로 커밋한 스냅샷을 가리킨다. HEAD는 현재 작업 브랜치인 b1을 가리킨다.
9cf510e v0.4    
f559e21 (master) v0.3        <=== master가 가리키는 스냅샷은 변경되지 않는다. 
5896279 v0.2
8dd76bf v0.1
5d8d97b (origin/master, origin/HEAD) Initial commit
```

```
예4) 작업할 브랜치를 b1에서 다시 master로 교체하기
    브랜치를 교체한 후 작업 디렉토리를 확인해 보면, 다시 master가 가리키는 스냅샷으로 돌아 온 것을 확인할 수 있다.
$ git checkout master
$ git log --oneline
f559e21 (HEAD -> master) v0.3    <=== 전체 스냅샷 중에서 master 브랜치와 연결된 스냅샷만 화면에 출력된다.
5896279 v0.2
8dd76bf v0.1
5d8d97b (origin/master, origin/HEAD) Initial commit
```

```
예5) test06.txt를 만들어 master 브랜치에 추가하고 커밋 내력 확인하기
    test06.txt 파일을 만들었다고 가정하자!
$ git add test06.txt
$ git commit -m 'v0.6'
$ git log --oneline
6f4725e (HEAD -> master) v0.6
f559e21 v0.3
5896279 v0.2
8dd76bf v0.1
5d8d97b (origin/master, origin/HEAD) Initial commit
```

```
예6) 현재 HEAD가 가리키는 브랜치의 역사 뿐만 아니라 다른 브랜치의 역사까지 출력하기
$ git log --oneline --graph --all
* 6f4725e (HEAD -> master) v0.6
| * 34fda9c (b1) v0.5
| * 9cf510e v0.4
|/  
* f559e21 v0.3
* 5896279 v0.2
* 8dd76bf v0.1
* 5d8d97b (origin/master, origin/HEAD) Initial commit
```

```
예7) 체크아웃 할 때 자동으로 새 브랜치를 만들기
    'git branch' + 'git checkout' = git checkout -b [새 브랜치 이름] 
$ git checkout -b b2
$ git log --oneline --all --graph
* 6f4725e (HEAD -> b2, master) v0.6
| * 34fda9c (b1) v0.5
| * 9cf510e v0.4
|/  
* f559e21 v0.3
* 5896279 v0.2
* 8dd76bf v0.1
* 5d8d97b (origin/master, origin/HEAD) Initial commit
```

```
예8) 새로 만든 b2 브랜치에 test07.txt 파일을 추가하고 커밋하기
$ git add test07.txt
$ git commit -m 'v0.7'
$ git log --oneline --all --graph
* 33c8c8d (HEAD -> b2) v0.7
* 6f4725e (master) v0.6
| * 34fda9c (b1) v0.5
| * 9cf510e v0.4
|/  
* f559e21 v0.3
* 5896279 v0.2
* 8dd76bf v0.1
* 5d8d97b (origin/master, origin/HEAD) Initial commit
```

### git merge [브랜치 이름]

- 현재 브랜치의 커밋에 다른 브랜치의 커밋 내용을 합친다.

```
예1) 합치려는 브랜치가 현 브랜치 보다 Upstream(이후 버전)일 경우,
    별도의 merge 과정이 필요없고, 해당 브랜치의 최신 버전의 커밋으로 이동한다.
    이런 merge 방식을 'fast forward'라 부른다.
$ git checkout master
$ git merge b2
$ git log --oneline --all --graph
* 33c8c8d (HEAD -> master, b2) v0.7   <=== master가 b2가 가리키는 커밋으로 이동한다.
* 6f4725e v0.6
| * 34fda9c (b1) v0.5
| * 9cf510e v0.4
|/  
* f559e21 v0.3
* 5896279 v0.2
* 8dd76bf v0.1
* 5d8d97b (origin/master, origin/HEAD) Initial commit
```

```
예2) 더 이상 필요없는 b2 브랜치를 삭제하기
$ git branch -d b2
$ git log --oneline --all --graph
* 33c8c8d (HEAD -> master) v0.7
* 6f4725e v0.6
| * 34fda9c (b1) v0.5
| * 9cf510e v0.4
|/  
* f559e21 v0.3
* 5896279 v0.2
* 8dd76bf v0.1
* 5d8d97b (origin/master, origin/HEAD) Initial commit
[~/git/git-test]$ 
```

```
예3) master 브랜치에 b1 브랜치 커밋 내용을 합치기
$ git checkout master     <=== 현재 브랜치가 master가 아니라면 이 명령을 수행한다.
$ git merge b1
$ git log --oneline --all --graph
*   58489d3 (HEAD -> master) v0.8    <=== master 브랜치에 b1 브랜치를 합친 새 커밋이 생성된다.
|\  
| * 34fda9c (b1) v0.5
| * 9cf510e v0.4
* | 33c8c8d v0.7
* | 6f4725e v0.6
|/  
* f559e21 v0.3
* 5896279 v0.2
* 8dd76bf v0.1
* 5d8d97b (origin/master, origin/HEAD) Initial commit

더 이상 필요없는 b1 브랜치를 삭제하기
$ git branch -d b1
[~/git/git-test]$ git log --oneline --all --graph
*   58489d3 (HEAD -> master) v0.8
|\  
| * 34fda9c v0.5
| * 9cf510e v0.4
* | 33c8c8d v0.7
* | 6f4725e v0.6
|/  
* f559e21 v0.3
* 5896279 v0.2
* 8dd76bf v0.1
* 5d8d97b (origin/master, origin/HEAD) Initial commit
```

### git rebase [브랜치명]

- 지정한 브랜치에 현재 브랜치의 변경 내력을 순서대로 합친다. 
- 작업 원리
  - 두 브랜치가 갈라지기 전인 공통 커밋으로 이동한다.
  - 공통 커밋 부터 현재 브랜치까지의 diff(변경 사항)를 차례로 만들어 임시 보관해 둔다.
  - 현재 브랜치가 지정한 브랜치를 가리키게 한다.
  - 임시 보관된 diff(변경 사항)을 차례로 적용한다.
- 특징
  - merge 보다 좀 더 깔끔한 history를 만든다.
  - history가 선형이다.
  - 모든 작업이 순서대로 진행된 것 처럼 보인다.
  - 보통 원격 브랜치에 커밋을 깔금하게 적용하고 싶을 때 사용한다.
  - rebase 브랜치의 변경 사항을 다른 브랜치에 순서대로 적용하면서 합친다.
  - merge는 두 브랜치의 최종 결과만을 가지고 합친다. 
- merge vs rebase
  - 로컬 저장소에서 브랜치를 정리할 때 rebase를 사용한다.
  - push로 공개한 커밋에 대해서는 rebase를 하지 말라!
  - 되도록 merge를 사용하여 역사를 기록하고 후세에 남겨 교훈이 되게 하라.

```
예1) b1 브랜치를 master 브랜치에 합치기 

현재 브랜치 내력을 조회한다.
$ git log --oneline --graph --all
* f9e2727 (HEAD -> master) C1

b1 브랜치를 만든다.
$ git branch b1
$ git log --oneline --graph --all
* f9e2727 (HEAD -> b1, master) C1

파일을 변경한 후 커밋한다.
$ git add .
$ git commit -m 'C2'
$ git log --oneline --graph --all
* 0ebbfb2 (HEAD -> b1) C2
* f9e2727 (master) C1

master 브랜치로 옮긴 후 파일을 변경한 후 커밋한다.
$ git checkout master
$ git add .
$ git commit -m 'C3'
$ git log --oneline --graph --all
* 1df28eb (HEAD -> master) C3
| * 0ebbfb2 (b1) C2
|/  
* f9e2727 C1

b1 브랜치로 옮긴 후 master 브랜치를 b1 브랜치쪽으로 rebase 한다.
$ git checkout b1
$ git rebase master
$ git log --oneline --graph --all
* 211448b (HEAD -> b1) C2
* 1df28eb (master) C3
* f9e2727 C1

master를 'fast-forward'로 merge 한다.
$ git checkout master
$ git merge b1
$ git log --oneline --graph --all
* 211448b (HEAD -> master, b1) C2
* 1df28eb C3
* f9e2727 C1
```

```
브랜치 history가 다음과 같다면,

C1 --- C2 --- C3   master
        \
         C4 --- C5 --- C6    b1
          \
           C7 --- C8 --- C9    b2

예2) b2 브랜치를 master 브랜치와 연결하기
        git rebase -onto [기준브랜치] [토픽 브랜치1] [토픽 브랜치2]
    - '-onto' 옵션을 사용하면 b2 브랜치로 체크아웃 할 필요없다.
    - '토픽 브랜치1'과 '토픽 브랜치2'의 공통 커밋 이후부터 '토픽 브랜치2'까지의 모든 변경 사항을 patch로 만들어서 '기준 브랜치'에 적용한다.
$ git rebase -onto master b1 b2

위 명령을 수행한 후 브랜치의 history
C1 --- C2 --- C3   master
        \      \
         \      C7' --- C8' --- C9'    b2
          \
           C4 --- C5 --- C6    b1

b2를 master에 합쳤으면 master에 대해 'fast-forward'를 수행한다.
$ git checkout master
$ git merge b2

위 명령을 수행한 후 브랜치의 history
C1 --- C2 --- C3 --- C7' --- C8' --- C9' master, b2
        \      
         C4 --- C5 --- C6    b1
```

```
예3) 위의 상황에서 b1 브랜치를 master에 합치기
        git rebase [기준 브랜치] [토픽 브랜치]
$ git rebase master b1

위 명령을 수행한 후 브랜치의 history
C1 --- C2 --- C3 --- C7' --- C8' --- C9' --- C4' --- C5' --- C6'
                                      |                       |
                                    master, b2               b1

b1을 master에 합쳤으면 master에 대해 'fast-forward'를 수행한다.
$ git checkout master
$ git merge b1
$ git branch -d b1    <=== 필요없는 b1 브랜치 삭제
$ git branch -d b2    <=== 필요없는 b2 브랜치 삭제

C1 --- C2 --- C3 --- C7' --- C8' --- C9' --- C4' --- C5' --- C6' 
                                                              |
                                                            master
```

## 원격 브랜치 

- 원격 브랜치는 원격 저장소에 있는 브랜치를 가리키는 레퍼런스(포인터) 이다.
- 원격 브랜치를 가리키는 형식
  - (remote)/(branch)
  - 예) origin/master
- 'git clone' 명령을 수행하면 원격 저장소를 가리키는 이름으로 'origin'이 자동 부여된다.

### git clone -o [원격저장소이름]

- '-o' 옵션을 이용하여 원격 저장소 이름을 지정하면 'origin' 대신 지정한 이름이 부여된다.

```
예1) 원격 저장소의 이름을 'orgin' 대신 'ohora'라 짓기
$ git clone -o ohora https://github.com/eomjinyoung/git-test
$ git remote
ohora
```

### git ls-remote

- 원격 레퍼런스(Refs)를 조회한다.

```
예1) 원격 저장소의 레퍼런스를 모두 출력하기
        git ls-remote [원격 저장소 이름]
$ git ls-remote    <=== 원격 저장소 이름을 생략하면 전체 출력
From https://github.com/eomjinyoung/git-test.git
9babde9de3ff3f9c979a8da0c9d65e008a13af31	HEAD
9babde9de3ff3f9c979a8da0c9d65e008a13af31	refs/heads/master

$ git ls-remote origin    <=== origin에 대한 것만 출력
9babde9de3ff3f9c979a8da0c9d65e008a13af31	HEAD
9babde9de3ff3f9c979a8da0c9d65e008a13af31	refs/heads/master
```

### git remote show [원격 저장소 이름]

- 원격 저장소에 대한 모든 브랜치와 정보를 조회한다.

```
예1) 원격 저장소의 브랜치 정보를 출력하기
        git remote show [원격 저장소 이름]
$ git remote show origin
* remote origin
  Fetch URL: https://github.com/eomjinyoung/git-test.git
  Push  URL: https://github.com/eomjinyoung/git-test.git
  HEAD branch: master
  Remote branch:
    master tracked
  Local branch configured for 'git pull':
    master merges with remote master
  Local ref configured for 'git push':
    master pushes to master (local out of date)
```

### git fetch [원격 저장소 이름]

- 원격 저장소가 로컬 저장소에 없는 정보를 가지고 있다면 모두 가져온다.
- 그리고 origin/master 포인터를 최신 커밋으로 이동시킨다.

```
현재 로컬 저장소의 브랜치 및 커밋 역사가 다음과 같다고 가정하자.
C1 --- C2 --- C3 --- C4 --- C5 --- C6 --- X1 --- X2
                                    |             |
                              origin/master     master

현재 원격 저장소의 브랜치 및 커밋 역사가 다음과 같다고 가정하자.
C1 --- C2 --- C3 --- C4 --- C5 --- C6 --- Y1 --- Y2
                                                  |
                                                master

예1) 원격 저장소의 내용을 로컬 저장소로 가져온다.
$ git fetch origin
$ git log --oneline --graph --all
* 9babde9 (origin/master, origin/HEAD) Y2
* 440c0c1 Y1
| * 82efd10 (HEAD -> master) X2
| * 4039833 X1
|/  
* b4dec77 C6
* 66d6384 C5
* 401d39d C4
* 4859dd1 C3
* 241b657 C2
* 046ea07 C1

즉 다음 그래프와 같이 커밋이 구성된다.
C1 --- C2 --- C3 --- C4 --- C5 --- C6 --- X1 --- X2 
                                     \            |
                                      \     HEAD -> master
                                       Y1 --- Y2
                                               |
                                         origin/master

예2) 원격에서 가져온 정보를 로컬 저장소에 merge하기 
$ git merge 9babde9
$ git log --oneline --graph --all
*   f5d2046 (HEAD -> master) X3
|\  
| * 9babde9 (origin/master, origin/HEAD) Y2
| * 440c0c1 Y1
* | 82efd10 X2
* | 4039833 X1
|/  
* b4dec77 C6
* 66d6384 C5
* 401d39d C4
* 4859dd1 C3
* 241b657 C2
* 046ea07 C1

즉 다음 그래프와 같이 커밋이 구성된다.
C1 --- C2 --- C3 --- C4 --- C5 --- C6 --- X1 --- X2 --- X3    HEAD -> master
                                    \                 /
                                     --- Y1 --- Y2 ---
                                                 |
                                           origin/master
```

```
예3) 원격 저장소의 정보를 가져와서 로컬 저장소와 합치기
        git pull = git fetch + get merge
    - 'git pull' 명령을 사용하면 더 간단히 처리할 수 있다.
$ git pull
```

```
예4) 모든 원격 저장소에서 데이터를 받아오기
$ git fetch --all
```

### git push

- 로컬 저장소의 정보를 원격 저장소에 올린다.
- 로컬에서 생성한 브랜치를 원격 저장소에 올릴 수 있다.
- 원격 저장소의 브랜치를 삭제할 수 있다.

```
예1) push를 사용하여 로컬 저장소의 정보를 원격 저장소에 올리기
$ git push    <=== master를 origin/master로 올린다.
$ git log --oneline --graph --all
*   f5d2046 (HEAD -> master, origin/master, origin/HEAD) X3
|\  
| * 9babde9 Y2
| * 440c0c1 Y1
* | 82efd10 X2
* | 4039833 X1
|/  
* b4dec77 C6
```

```
예2) 로컬 저장소에 있는 'b1' 브랜치를 원격 저장소에 올리기 
        git push [원격 저장소 이름] [로컬 브랜치 이름]
    - 로컬 브랜치 이름과 같은 원격 브랜치가 없으면 새로 만든다.
    - 로컬 브랜치 정보를 원격 브랜치에 올린다.
$ git push origin b1
```

```
예3) 로컬 저장소에 있는 'b1' 브랜치를 원격 저장소의 'other' 브랜치로 올리기
        git push [원격 저장소 이름] [로컬 브랜치 이름]:[원격 브랜치 이름]
    - 로컬 브랜치 이름과 원격 브랜치 이름을 다를 때 유용하다.
$ git push origin b1:other
```

```
예3) 원격 저장소의 'b1' 브랜치를 삭제하기
        git push [원격 저장소 이름] --delete [브랜치 이름]
$ git push origin --delete b1
```

### git checkout -b [로컬 브랜치][원격 저장소]/[원격 브랜치]

- 원격 저장소의 브랜치를 받아서 로컬 브랜치를 만든다.

```
예1) 원격 저장소의 origin/other 브랜치를 체그아웃 하여 other2 로컬 브랜치 만들기
$ git checkout -b other2 origin/other
Branch 'other2' set up to track remote branch 'other' from 'origin'.
Switched to a new branch 'other2'
```

```
예2) 원격 저장소의 origin/other 브랜치를 체크아웃 하여 같은 이름으로 로컬 브랜치 만들기
    - 같은 이름으로 만들 때는 --track 옵션을 사용한다.
$ git checkout --track origin/other
```

### git branch -vv

- 트래킹 브랜치의 설정 정보를 조회한다.
- 출력 결과
  - ahead n : 로컬 브랜치가 커밋을 n 개 앞서 있다. 즉 로컬 브랜치에 커밋이 2개 더 있다는 의미.
  - behind n : 원격 브랜치에서 로컬 브랜치로 merge 하지 않은 커밋이 n 개 있다는 의미.

```
$ git branch -vv
  b1     c2be10d [origin/b1] v1.3
* b2     664dbb5 v3.1
  master 09fb339 [origin/master: ahead 2] v3.0   <=== 로컬 브랜치가 커밋을 2개 앞서 있다는 의미
  other  ed485e2 [origin/other] v1.2
  other2 ed485e2 [origin/other] v1.2
```