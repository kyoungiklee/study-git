## 2.1 Git의 기초 - Git 저장소 만들기
### Git 저장소 만들기
#### 기존 디렉토리를 Git 저장소로 만들기
$ git init
Initialized empty Git repository in D:/project/study/studyGit/.git/

이 명령은 .git 이라는 하위 디렉토리를 만든다. .git 디렉토리에는 저장소에 필요한 뼈대 파일(Skeleton)이 들어 있다. 이 명령만으로는 
아직 프로젝트의 어떤 파일도 관리하지 않는다. (.git 디렉토리가 막 만들어진 직후에 정확히 어떤 파일이 있는지에 대한 내용은 Git의 내부에서 다룬다)

#### 기존 저장소를 Clone 하기
$ git clone https://github.com/roadseeker/redmine_manual.git
```
Cloning into 'redmine_manual'...
remote: Enumerating objects: 27, done.
remote: Counting objects: 100% (27/27), done.
remote: Compressing objects: 100% (10/10), done.
remote: Total 27 (delta 6), reused 27 (delta 6), pack-reused 0
Receiving objects: 100% (27/27), 6.66 KiB | 1.66 MiB/s, done.
Resolving deltas: 100% (6/6), done.
```
다른 프로젝트에 참여하려거나(Contribute) Git 저장소를 복사하고 싶을 때 git clone 명령을 사용한다. 
이미 Subversion 같은 VCS에 익숙한 사용자에게는 "checkout" 이 아니라 "clone" 이라는 점이 도드라져 보일 것이다. 
Git이 Subversion과 다른 가장 큰 차이점은 서버에 있는 거의 모든 데이터를 복사한다는 것이다. 
git clone 을 실행하면 프로젝트 히스토리를 전부 받아온다.

## 2.2 Git의 기초 - 수정하고 저장소에 저장히기
### 수정하고 저장소에 저장하기
워킹 디렉토리의 모든 파일은 크게 Tracked(관리대상임)와 Untracked(관리대상이 아님)로 나눈다.
Tracked 파일은 이미 스냅샷에 포함돼 있던 파일이다. Tracked 파일은 또 Unmodified(수정하지 않음)와 Modified(수정함) 그리고 Staged(커밋으로 저장소에 기록할) 상태 중 하나이다. 

![그림 8 파일의 라이프 사이클](images/chapter2/image8.png)

#### 파일 상태 확인하기
파일의 상태를 확인하려면 보통 git status 명령을 사용한다. Clone 한 후에 바로 이 명령을 실행하면 아래과 같은 메시지를 볼 수 있다.

```
$ echo "Git User Guide" > README.md
$ git status                                                   
On branch master                                                
Untracked files:                                                
  (use "git add <file>..." to include in what will be committed)
        README.md                                                           
                                                                            
nothing added to commit but untracked files present (use "git add" to track)

```
README 파일은 “Untracked files” 부분에 속해 있는데 이것은 README 파일이 Untracked 상태라는 것을 말한다. 
Git은 Untracked 파일을 아직 스냅샷(커밋)에 넣어지지 않은 파일이라고 본다. 
파일이 Tracked 상태가 되기 전까지는 Git은 절대 그 파일을 커밋하지 않는다. 
그래서 일하면서 생성하는 바이너리 파일 같은 것을 커밋하는 실수는 하지 않게 된다. 
README 파일을 추가해서 직접 Tracked 상태로 만들어 보자.

#### 파일을 새로 추적하기
git add 명령으로 파일을 새로 추적할 수 있다. 아래 명령을 실행하면 Git은 README 파일을 추적한다.

```
$ git add README.md
$ git status
On branch master
Changes to be committed:
(use "git restore --staged <file>..." to unstage)
new file:   README.md
```
“Changes to be committed” 에 들어 있는 파일은 Staged 상태라는 것을 의미한다. 
커밋하면 git add 를 실행한 시점의 파일이 커밋되어 저장소 히스토리에 남는다. 
앞에서 git init 명령을 실행한 후, git add (files) 명령을 실행했던 걸 기억할 것이다. 
이 명령을 통해 디렉토리에 있는 파일을 추적하고 관리하도록 한다. 
git add 명령은 파일 또는 디렉토리의 경로를 아규먼트로 받는다. 
디렉토리면 아래에 있는 모든 파일들까지 재귀적으로 추가한다.

#### Modified 상태의 파일을 Stage 하기
이미 Tracked 상태인 파일을 수정하는 법을 알아보자.
"chapter2. Git Basics.md" 라는 파일을 수정하고 나서 git status 명령을 다시 실행하면 결과는 아래와 같다.
```
$ git status
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   README.md

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   chapter2. Git Basics.md

```

 이 "chapter2. Git Basics.md" 파일은 "Changes not staged for commit" 에 있다. 
 이것은 수정한 파일이 Tracked 상태이지만 아직 Staged 상태는 아니라는 것이다.
 Staged 상태로 만들려면 git add 명령을 실행해야 한다. git add 명령은 파일을 새로 추적할 때도 사용하고 수정한 파일을 Staged 상태로 만들 때도 사용한다. 
 Merge 할 때 충돌난 상태의 파일을 Resolve 상태로 만들때도 사용한다. 
 add의 의미는 프로젝트에 파일을 추가한다기 보다는 다음 커밋에 추가한다고 받아들이는게 좋다. 
 git add 명령을 실행하여 CONTRIBUTING.md 파일을 Staged 상태로 만들고 git status 명령으로 결과를 확인해보자.
 
```
$ git status
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   README.md
        modified:   chapter2. Git Basics.md
```
두 파일 모두 Staged 상태이므로 다음 커밋에 포함된다. 하지만 아직 더 수정해야 한다는 것을 알게 되어 바로 커밋하지 못하는 상황이 되었다고 생각해보자. 이 상황에서 CONTRIBUTING.md 파일을 열고 수정한다. 
이제 커밋할 준비가 다 됐다고 생각할 테지만, Git은 그렇지 않다. git status 명령으로 파일의 상태를 다시 확인해보자.

```
$ git status
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   README.md
        modified:   chapter2. Git Basics.md

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   chapter2. Git Basics.md

```

"chapter2. Git Basics.md" 가 Staged 상태이면서 동시에 Unstaged 상태로 나온다. 어떻게 이런 일이 가능할까? git add 명령을 실행하면 Git은 파일을 바로 Staged 상태로 만든다. 
지금 이 시점에서 커밋을 하면 git commit 명령을 실행하는 시점의 버전이 커밋되는 것이 아니라 마지막으로 git add 명령을 실행했을 때의 버전이 커밋된다. 
그러니까 git add 명령을 실행한 후에 또 파일을 수정하면 git add 명령을 다시 실행해서 최신 버전을 Staged 상태로 만들어야 한다.

```
$ git add "chapter2. Git Basics.md" 

$ git status
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   README.md
        modified:   chapter2. Git Basics.md

```

#### 파일 상태를 짤막하게 확인하기
```
$ git status -s
A  README.md
MM "chapter2. Git Basics.md"

```

#### 파일 무시하기

어떤 파일은 Git이 관리할 필요가 없다. 보통 로그 파일이나 빌드 시스템이 자동으로 생성한 파일이 그렇다. 
그런 파일을 무시하려면 .gitignore 파일을 만들고 그 안에 무시할 파일 패턴을 적는다. 아래는 .gitignore 파일의 예이다.

```
# 확장자가 .a인 파일 무시
*.a

# 윗 라인에서 확장자가 .a인 파일은 무시하게 했지만 lib.a는 무시하지 않음
!lib.a

# 현재 디렉토리에 있는 TODO파일은 무시하고 subdir/TODO처럼 하위디렉토리에 있는 파일은 무시하지 않음
/TODO

# build/ 디렉토리에 있는 모든 파일은 무시
build/

# doc/notes.txt 파일은 무시하고 doc/server/arch.txt 파일은 무시하지 않음
doc/*.txt

# doc 디렉토리 아래의 모든 .pdf 파일을 무시
doc/**/*.pdf
```

GitHub은 다양한 프로젝트에서 자주 사용하는 .gitignore 예제를 관리하고 있다. 
어떤 내용을 넣을지 막막하다면 https://github.com/github/gitignore 사이트에서 적당한 예제를 찾을 수 있다.

#### Staged와 Unstaged 상태의 변경 내용을 보기

단순히 파일이 변경됐다는 사실이 아니라 어떤 내용이 변경됐는지 살펴보려면 git status 명령이 아니라 git diff 명령을 사용해야 한다. 
``` 
$ git status
On branch master
Changes to be committed:
(use "git restore --staged <file>..." to unstage)
new file:   README.md
modified:   chapter2. Git Basics.md

Changes not staged for commit:
(use "git add <file>..." to update what will be committed)
(use "git restore <file>..." to discard changes in working directory)
modified:   chapter2. Git Basics.md
```
git diff 명령을 실행하면 수정했지만 아직 staged 상태가 아닌 파일을 비교해 볼 수 있다.

``` 
$ git diff
diff --git a/chapter2. Git Basics.md b/chapter2. Git Basics.md
index 568e689..5ac03b5 100644
--- a/chapter2. Git Basics.md
+++ b/chapter2. Git Basics.md
@@ -186,4 +186,5 @@ Changes not staged for commit:
 (use "git restore <file>..." to discard changes in working directory)
 modified:   chapter2. Git Basics.md

+git diff 명령을 실행하면 수정했지만 아직 staged 상태가 아닌 파일을 비교해 볼 수 있다.

```

커밋하려고 Staging Area에 넣은 파일의 변경 부분을 보고 싶으면 git diff --staged 옵션을 사용한다. 이 명령은 저장소에 커밋한 것과 Staging Area에 있는 것을 비교한다.
``` 
$ git diff --staged
diff --git a/chapter2. Git Basics.md b/chapter2. Git Basics.md
index 494f125..828648e 100644
--- a/chapter2. Git Basics.md
+++ b/chapter2. Git Basics.md
@@ -200,4 +200,5 @@ index 568e689..5ac03b5 100644

 +git diff 명령을 실행하면 수정했지만 아직 staged 상태가 아닌 파일을 비교해 볼 수 있다.

-```
\ No newline at end of file
+```
+
```

git diff 는 Unstaged 상태인 것들만 보여준다. 수정한 파일을 모두 Staging Area에 넣었다면 git diff 명령은 아무것도 출력하지 않는다.

Staged 상태인 파일은 git diff --cached 옵션으로 확인한다. --staged 와 --cached 는 같은 옵션이다.

#### 변경사항 커밋하기

Git은 생성하거나 수정하고 나서 git add 명령으로 추가하지 않은 파일은 커밋하지 않는다. 그 파일은 여전히 Modified 상태로 남아 있다. 
커밋하기 전에 git status 명령으로 모든 것이 Staged 상태인지 확인할 수 있다. 그 후에 git commit 을 실행하여 커밋한다.

Git 설정에 지정된 편집기가 실행되고, 아래와 같은 텍스트가 자동으로 포함된다 (아래 예제는 Vim 편집기의 화면이다. 이 편집기는 쉘의 EDITOR 환경 변수에 등록된 편집기이고 보통은 Vim이나 Emacs을 사용한다. 
또 시작하기 에서 설명했듯이 git config --global core.editor 명령으로 어떤 편집기를 사용할지 설정할 수 있다).

``` 
# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
#
# On branch master
# Changes to be committed:
#       modified:   chapter2. Git Basics.md
#
# Untracked files:
#       -
#       -mutilnst
#       noPlugin
#
.git/COMMIT_EDITMSG [unix] (15:04 24/11/2023)                                                                                                                                                                                                                                                              1,0-1 All
"/d/project/study/studyGit/.git/COMMIT_EDITMSG" [unix] 13L, 272B
```
메시지를 인라인으로 첨부할 수도 있다. commit 명령을 실행할 때 아래와 같이 -m 옵션을 사용한다.
``` 
$ git commit -m "git commit 내용 수정"
[master c253fc9] ígit commit 내용 수정
 1 file changed, 3 insertions(+)

```
Git은 Staging Area에 속한 스냅샷을 커밋한다는 것을 기억해야 한다. 수정은 했지만, 아직 Staging Area에 넣지 않은 것은 다음에 커밋할 수 있다. 
커밋할 때마다 프로젝트의 스냅샷을 기록하기 때문에 나중에 스냅샷끼리 비교하거나 예전 스냅샷으로 되돌릴 수 있다.


#### Staging Area 생략하기

Staging Area는 커밋할 파일을 정리한다는 점에서 매우 유용하지만 복잡하기만 하고 필요하지 않은 때도 있다. 
아주 쉽게 Staging Area를 생략할 수 있다. git commit 명령을 실행할 때 -a 옵션을 추가하면 Git은 Tracked 상태의 파일을 자동으로 Staging Area에 넣는다. 
그래서 git add 명령을 실행하는 수고를 덜 수 있다.
``` 
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   chapter2. Git Basics.md

no changes added to commit (use "git add" and/or "git commit -a

$ git commit -a -m "staging area 생략하기 추가"
[master 805f7a3] staging area 생ê°략하기 추가
 1 file changed, 37 insertions(+), 1 deletion(-)

$git status
On branch master
nothing to commit, working tree clean

```

이 예제에서는 커밋하기 전에 git add 명령으로 CONTRIBUTING.md 파일을 추가하지 않았다는 점을 눈여겨보자. -a 옵션을 사용하면 모든 파일이 자동으로 추가된다. 
편리한 옵션이긴 하지만 주의 깊게 사용해야 한다. 생각 없이 이 옵션을 사용하다 보면 추가하지 말아야 할 변경사항도 추가될 수 있기 때문이다

#### 파일 삭제하기
Git에서 파일을 제거하려면 git rm 명령으로 Tracked 상태의 파일을 삭제한 후에(정확하게는 Staging Area에서 삭제하는 것) 커밋해야 한다. 
이 명령은 워킹 디렉토리에 있는 파일도 삭제하기 때문에 실제로 파일도 지워진다.

Git 명령을 사용하지 않고 단순히 워킹 디렉터리에서 파일을 삭제하고 git status 명령으로 상태를 확인하면 
Git은 현재 “Changes not staged for commit” (즉, Unstaged 상태)라고 표시해준다.
``` 
$ rm PROJECT.md 

$ git status
On branch master
Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        deleted:    PROJECT.md
        
no changes added to commit (use "git add" and/or "git commit -a")
```
그리고 git rm 명령을 실행하면 삭제한 파일은 Staged 상태가 된다.

```
$ git rm PROJECT.md 
rm 'PROJECT.md'

$ git status
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        deleted:    PROJECT.md

$ git commit -m "delete file PROJECT.md"
[master dbc62bf] delete file PROJECT.md
 1 file changed, 1 deletion(-)
 delete mode 100644 PROJECT.md


```
커밋하면 파일은 삭제되고 Git은 이 파일을 더는 추적하지 않는다. 이미 파일을 수정했거나 Staging Area에(역주 - Git Index라고도 부른다) 추가했다면 -f 옵션을 주어 강제로 삭제해야 한다. 
이 점은 실수로 데이터를 삭제하지 못하도록 하는 안전장치다. 커밋 하지 않고 수정한 데이터는 Git으로 복구할 수 없기 때문이다.

또 Staging Area에서만 제거하고 워킹 디렉토리에 있는 파일은 지우지 않고 남겨둘 수 있다. 다시 말해서 하드디스크에 있는 파일은 그대로 두고 Git만 추적하지 않게 한다. 
이것은 .gitignore 파일에 추가하는 것을 빼먹었거나 대용량 로그 파일이나 컴파일된 파일인 .a 파일 같은 것을 실수로 추가했을 때 쓴다. --cached 옵션을 사용하여 명령을 실행한다.

``` 
$ git rm --cached README.md 
rm 'README.md

$ git status
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        deleted:    README.md

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        README.md

```

여러 개의 파일이나 디렉토리를 한꺼번에 삭제할 수도 있다. 아래와 같이 git rm 명령에 file-glob 패턴을 사용한다.

* 앞에 \ 을 사용한 것을 기억하자. 파일명 확장 기능은 쉘에만 있는 것이 아니라 Git 자체에도 있기 때문에 필요하다. 
* 이 명령은 log/ 디렉토리에 있는 .log 파일을 모두 삭제한다.
``` 
$ git rm log/\*.log
$ git rm \*~
```

#### 파일이름 변경하기
git mv 명령은 일종의 단축 명령어이다. 이 명령으로 파일 이름을 바꿔도 되고 mv 명령으로 파일 이름을 직접 바꿔도 된다. 단지 git mv 명령은 편리하게 명령을 세 번 실행해주는 것 뿐이다. 
어떤 도구로 이름을 바꿔도 상관없다. 중요한 것은 이름을 변경하고 나서 꼭 rm/add 명령을 실행해야 한다는 것 뿐이다
``` 
$ git mv README.md README

$ git status
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        renamed:    README.md -> README

```

## 2.3 Git의 기초 - 커밋 히스토리 조회하기
### 커밋 히스토리 조회하기
새로 저장소를 만들어서 몇 번 커밋을 했을 수도 있고, 커밋 히스토리가 있는 저장소를 Clone 했을 수도 있다. 어쨌든 가끔 저장소의 히스토리를 보고 싶을 때가 있다. 
Git에는 히스토리를 조회하는 명령어인 git log 가 있다.

``` 
$git log
commit dbc62bfb92733c511a736dee5202e22ed162c493 (HEAD -> master)
Author: kyoungiklee <kyoungik.lee+1@gmail.com>
Date:   Fri Nov 24 15:23:10 2023 +0900

    delete file PROJECT.md

commit 972accb8ce60ae06ce5c3092938634fc8594fb24
Author: kyoungiklee <kyoungik.lee+1@gmail.com>
Date:   Fri Nov 24 15:20:46 2023 +0900

    add file PROJECT.md

commit 442b959aaeee86278414c857530ba8445e342ce4
Author: kyoungiklee <kyoungik.lee+1@gmail.com>
Date:   Fri Nov 24 15:18:22 2023 +0900
.....
```
특별한 아규먼트 없이 git log 명령을 실행하면 저장소의 커밋 히스토리를 시간순으로 보여준다. 즉, 가장 최근의 커밋이 가장 먼저 나온다. 
그리고 이어서 각 커밋의 SHA-1 체크섬, 저자 이름, 저자 이메일, 커밋한 날짜, 커밋 메시지를 보여준다.


여러 옵션 중 -p, --patch 는 굉장히 유용한 옵션이다. -p 는 각 커밋의 diff 결과를 보여준다. 
다른 유용한 옵션으로 -2 가 있는데 최근 두 개의 결과만 보여주는 옵션이다:
``` 
$ git log -p -2
commit dbc62bfb92733c511a736dee5202e22ed162c493 (HEAD -> master)
Author: kyoungiklee <kyoungik.lee+1@gmail.com>
Date:   Fri Nov 24 15:23:10 2023 +0900

    delete file PROJECT.md

diff --git a/PROJECT.md b/PROJECT.md
deleted file mode 100644
index 31abe30..0000000
--- a/PROJECT.md
+++ /dev/null
@@ -1 +0,0 @@
-delete file

commit 972accb8ce60ae06ce5c3092938634fc8594fb24
Author: kyoungiklee <kyoungik.lee+1@gmail.com>
Date:   Fri Nov 24 15:20:46 2023 +0900

```
it log 명령에는 히스토리의 통계를 보여주는 옵션도 있다. --stat 옵션으로 각 커밋의 통계 정보를 조회할 수 있다.

``` 
$ git log --stat
commit dbc62bfb92733c511a736dee5202e22ed162c493 (HEAD -> master)
Author: kyoungiklee <kyoungik.lee+1@gmail.com>
Date:   Fri Nov 24 15:23:10 2023 +0900

    delete file PROJECT.md

 PROJECT.md | 1 -
 1 file changed, 1 deletion(-)

commit 972accb8ce60ae06ce5c3092938634fc8594fb24
Author: kyoungiklee <kyoungik.lee+1@gmail.com>
Date:   Fri Nov 24 15:20:46 2023 +0900

    add file PROJECT.md

 PROJECT.md | 1 +
 1 file changed, 1 insertion(+)

commit 442b959aaeee86278414c857530ba8445e342ce4
Author: kyoungiklee <kyoungik.lee+1@gmail.com>
Date:   Fri Nov 24 15:18:22 2023 +0900

    PROdelete JECT.md 랴file

 PROJECT.md | 1 -
 1 file changed, 1 deletion(-)

commit da7acf2deb6ded2ff780b8926f40dc3fad971349
Author: kyoungiklee <kyoungik.lee+1@gmail.com>
Date:   Fri Nov 24 15:16:07 2023 +0900

    add PROJECT.md file

 PROJECT.md | 1 +
 1 file changed, 1 insertion(+)

commit 805f7a3fef58931c62b8627bc92b472b7969125c
Author: kyoungiklee <kyoungik.lee+1@gmail.com>
Date:   Fri Nov 24 15:10:43 2023 +0900

 chapter1. Getting Started.md | 66 ++++++++++++++++++++++++++++++++++++++++++++
 chapter2. Git Basics.md      | 58 ++++++++++++++++++++++++++++++++++++++
 2 files changed, 124 insertions(+)


```
이 결과에서 --stat 옵션은 어떤 파일이 수정됐는지, 얼마나 많은 파일이 변경됐는지, 또 얼마나 많은 라인을 추가하거나 삭제했는지 보여준다. 
요약정보는 가장 뒤쪽에 보여준다.

다른 또 유용한 옵션은 --pretty 옵션이다. 이 옵션을 통해 히스토리 내용을 보여줄 때 기본 형식 이외에 여러 가지 중에 하나를 선택할 수 있다. 
몇개 선택할 수 있는 옵션의 값이 있다. oneline 옵션은 각 커밋을 한 라인으로 보여준다. 
이 옵션은 많은 커밋을 한 번에 조회할 때 유용하다. 추가로 short, full, fuller 옵션도 있는데 이것은 정보를 조금씩 가감해서 보여준다.

``` 
$ git log --pretty=oneline
dbc62bfb92733c511a736dee5202e22ed162c493 (HEAD -> master) delete file PROJECT.md
972accb8ce60ae06ce5c3092938634fc8594fb24 add file PROJECT.md
442b959aaeee86278414c857530ba8445e342ce4 PROdelete JECT.md 랴file
da7acf2deb6ded2ff780b8926f40dc3fad971349 add PROJECT.md file
805f7a3fef58931c62b8627bc92b472b7969125c staging area 생ê°략하기 추가
c253fc92e79529e37cf4b54d975183f92901716e ígit commit 내용 수정
191e9c2848ad9e7f3057fdec5f9e43e37dc8e8b4 변경사항 커밋하기 추가
cd164a1a2330bc6e3ca91176aa479d34b8c8e17c git commit 내용 추가
6d870cba0147436609d49b4f55e289fb2308b42c git 기초 사용법 작성
89764f271b516969e4523d3dbb062ecbe61a45fa git ignore 파일 추가
b2a0a7c0bdadd613e1fef05defc5cb632a33c8c9 타이틀 작성
c4c41d63067663c5ca3cc91fe988ae021d2227ee 타이틀 작성
64340b74a75015e7413de0c558056cff88a087b9 Git Study Init

```

가장 재밌는 옵션은 format 옵션이다. 나만의 포맷으로 결과를 출력하고 싶을 때 사용한다. 특히 결과를 다른 프로그램으로 파싱하고자 할 때 유용하다. 
이 옵션을 사용하면 포맷을 정확하게 일치시킬 수 있기 때문에 Git을 새 버전으로 바꿔도 결과 포맷이 바뀌지 않는다.

```
$ git log --pretty=format:"%h - %an, %ar : %s"
dbc62bf - kyoungiklee, 53 minutes ago : delete file PROJECT.md
972accb - kyoungiklee, 55 minutes ago : add file PROJECT.md
442b959 - kyoungiklee, 57 minutes ago : PROdelete JECT.md 랴file
da7acf2 - kyoungiklee, 60 minutes ago : add PROJECT.md file
805f7a3 - kyoungiklee, 65 minutes ago : staging area 생ê°략하기 추가
```

git log --pretty=format 에 쓸 몇가지 유용한 옵션` 포맷에서 사용하는 유용한 옵션.
``` 
옵션	설명
%H 커밋 해시
%h 짧은 길이 커밋 해시
%T 트리 해시
%t 짧은 길이 트리 해시
%P 부모 해시
%p 짧은 길이 부모 해시
%an 저자 이름
%ae 저자 메일
%ad 저자 시각 (형식은 –-date=옵션 참고)
%ar 저자 상대적 시각
%cn 커미터 이름
%ce 커미터 메일
%cd 커미터 시각
%cr 커미터 상대적 시각
%s 요약
```
oneline 옵션과 format 옵션은 --graph 옵션과 함께 사용할 때 더 빛난다. 이 명령은 브랜치와 머지 히스토리를 보여주는 아스키 그래프를 출력한다.
``` 
$ git log --pretty=format:"%h %s" --graph                                                                                                                                                                                                                                                                           
* dbc62bf delete file PROJECT.md
* 972accb add file PROJECT.md
* 442b959 PROdelete JECT.md 랴file
* da7acf2 add PROJECT.md file
* 805f7a3 staging area 생ê°략하기 추가
* c253fc9 ígit commit 내용 수정
* 191e9c2 변경사항 커밋하기 추가
* cd164a1 git commit 내용 추가
* 6d870cb git 기초 사용법 작성
* 89764f2 git ignore 파일 추가
* b2a0a7c 타이틀 작성
* c4c41d6 타이틀 작성
* 64340b7 Git Study Init

```
git log 명령은 앞서 살펴본 것보다 더 많은 옵션을 지원한다. git log 주요 옵션 는 지금 설명한 것과 함께 유용하게 사용할 수 있는 옵션이다. 
각 옵션으로 어떻게 log 명령을 제어할 수 있는지 보여준다.

``` 
표 2. git log 주요 옵션
옵션	설명
-p 각 커밋에 적용된 패치를 보여준다.
--stat 각 커밋에서 수정된 파일의 통계정보를 보여준다.
--shortstat --stat 명령의 결과 중에서 수정한 파일, 추가된 라인, 삭제된 라인만 보여준다.
--name-only 커밋 정보중에서 수정된 파일의 목록만 보여준다.
--name-status 수정된 파일의 목록을 보여줄 뿐만 아니라 파일을 추가한 것인지, 수정한 것인지, 삭제한 것인지도 보여준다.
--abbrev-commit 40자 짜리 SHA-1 체크섬을 전부 보여주는 것이 아니라 처음 몇 자만 보여준다.
--relative-date 정확한 시간을 보여주는 것이 아니라 “2 weeks ago” 처럼 상대적인 형식으로 보여준다.
--graph 브랜치와 머지 히스토리 정보까지 아스키 그래프로 보여준다.
--pretty 지정한 형식으로 보여준다. 이 옵션에는 oneline, short, full, fuller, format이 있다. format은 원하는 형식으로 출력하고자 할 때 사용한다.
--oneline --pretty=oneline --abbrev-commit 두 옵션을 함께 사용한 것과 같다.
```