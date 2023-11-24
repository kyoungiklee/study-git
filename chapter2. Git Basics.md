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
