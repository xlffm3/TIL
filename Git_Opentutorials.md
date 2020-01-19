GIT
---

---

GIT
---

-	목적
	-	Version
	-	Back-Up
	-	Collaboration<br><br>

버전 관리
---------

```git
git init : initialize repository
.git : git repository
git status : working tree status
git add : add to staging area
git commit : create version
git commit -am : add + commit
git log : show version
git log --stat
git diff : show changes
git log -p : 세부 변경 사항
git checkout
git reset --hard : 특정 버전으로 리셋
git revert : 특정 버전을 revert
```

-	Wokring Tree : 파일을 생성하고 수정하는 공간이며, 버전으로 만들어지기 전 단계이다.<br><br>
-	Stagging Area : 버전을 만들고자 하는 파일들이 있는 공간이다.<br><br>
-	Repository : 버전이 저장되는 공간이다.<br><br>
-	`git add` 명령어로 Working Tree의 파일을 Stagging Area로 올려 놓는다.<br><br>
-	`git commit` 명령어로 Stagging Area의 파일을 버전으로 만든다.<br><br>
-	`git commit -am` 명령어는 untracked file은 제외한다.<br><br>
-	`git commit` 만 입력할 경우 커맨드 라인에서 직접 메시지를 작성한다.<br><br>

Branch & Conflict
-----------------

![image](https://user-images.githubusercontent.com/56240505/72680007-3c86f780-3af8-11ea-8b53-16adc3518f02.png)<br>

```git
git log --graph --oneline --all : 브랜치 정보 및 흐름을 그래프화
git merge xx : xx  branch를 master로 merge
git mergtool
```

-	공통의 조상을 base 라고 칭한다.<br><br>
-	conflict가 발생하면 충돌 부위를 수정 후 `git add` 한다.<br><br>
-	이후 `git commit` 명령어를 입력하면 병합이 자동 완료된다.<br><br>
-	GIT은 3 way merge 기법으로 병합 및 충돌을 관리한다.<br><br>
-	mergtool을 이용해서 충돌을 관리할 수 있다.<br><br>
-	head는 기본적으로 master을 가리키며, `git checkout` 을 통해 Head가 가리키는 대상을 변경한다.<br><br>
-	`git reset` 은 현재 checkout이 된 branch가 특정 버전을 가리키도록 한다.<br><br>

Pull vs. Clone vs. Fetch
------------------------

-	`git clone` 은 `git remote add` + `git pull` 과 거의 같다.<br><br>
-	`git pull` 은 `git fetch` + `git merge FETCH_HEAD` 와 (거의) 같다.<br><br>
-	`git clone` 은 remote repository를 local repository로 복사하는 것이며, `git pull` 은 remote repository에 변경된 내용을 받아 local repository의 기존 작업을 업데이트 한다.<br><br>
-	비슷한 개념이지만 `git clone` 은 대부분 최초 1회만 실행한다.<br><br>
-	`git pull` 은 자동 병합이 발생하기 때문에 remote repository의 내용만 확인하고 병합은 신중하게 선택할 때 `git fetch` 를 사용한다.<br><br>
-	`git fetch` 는 remote repository의 최신 이력을 확인하며, 이 때 가져온 최신 커밋 이력은 이름 없는 브랜치로 local repository에 가져온다.<br><br>
-	이 브랜치는 'FETCH_HEAD'의 이름으로 체크아웃할 수 있다.<br><br>
-	local repository가 remote repository보다 버전이 뒤쳐져있기 때문에, FETCH_HEAD 브랜치를 local repository의 master branch에 merge해야 `git pull` 과 같은 효과를 낸다.<br><br>

Patch
-----

```git
git format-patch 0877214 //변경 이전의 최신 version
git am -3 -i *.patch //모든 patch 파일들에 대해 3 way merge를 적용하며, interactive mode 사용
```

-	오픈 소스를 다른 이용자가 받아 새로 commit해도, 저장소에 대해 권한이 없으면 push가 불가능하다.<br><br>
-	따라서 `git patch` 를 통해 수정한 내용을 저장소의 관리자에게 보낸다.<br><br>
-	관리자는 검토 후 merge를 선택하여 변경 내용을 적용시킬 수 있다.<br><br>

Fork & Pull Request
-------------------

-	Clone이 특정 저장소를 local machine에 복사하는 것이라면, Fork는 타인의 저장소를 내 Github의 저장소로 복사하는 것이다.<br><br>
-	GIT 자체의 기능이라기 보다는, GIT을 호스팅하는 서비스에서 제공하는 기능이다.<br><br>
-	복사한 저장소를 통해 자유롭게 commit한 뒤, Pull Request를 통해 기존 저장소의 관리자에게 변경 사항 추가를 요청한다.<br><br>
-	해당 관리자는 검토 후 merge를 선택할 수 있다.<br><br>

Cherry-Pick
-----------

![image](https://user-images.githubusercontent.com/56240505/72680040-8ff94580-3af8-11ea-9b98-20f3eb1419cc.png)<br>

```git
git checkout master
git cherry-pick 025ba18 //다른 브랜치의 특정 commit 지점인 t2를 master branch에 적용
git cherry-pick --continue //충돌 지점 관리 후 실행하는 명령어
```

-	다른 브랜치에 있는 특정 commit을 선택적으로 내 branch에 적용시킬 때 사용하는 명령어이다.<br><br>
-	특정 commit 버전과 그 이전 버전 및 pick을 적용시키려는 branch의 버전 사이에서 3 way merge가 발생한다.<br><br>
-	충돌 부분은 merge tool을 통해 별도로 관리한 뒤 cherry-pick을 적용시킨다.<br><br>

Rebase
------

![image](https://user-images.githubusercontent.com/56240505/72680066-e9617480-3af8-11ea-85eb-a913b4d6b093.png)<br>

![image](https://user-images.githubusercontent.com/56240505/72680123-9dfb9600-3af9-11ea-8008-ff3973568558.png)<br>

```GIT
git checkout master
git rebase topic //topic branch가 가리키는 commit을 base로 옮김
git am --show-current-patch //충돌 부위 확인
git rebase --continue //충돌 관리 이후 계속 진행
```

-	병합 전략 중 하나이며, branch의 공통 조상이 되는 base를 다른 branch의 commit 지점으로 바꾸는 것이다.<br><br>
-	Rebase는 base를 옮기려는 branch가 원격 저장소로 push 되기 전 시점에서만 사용해야 혼선을 방지할 수 있다.<br><br>
-	충돌이 일어나더라도 Rebase 명령어가 실행되면 head는 새로운 base로 이동한다.<br><br>
-	작업이 선형적으로 일어나기 때문에, 기존의 base와 위치가 이동되는 branch의 그 다음 commit version 및 새로운 base 사이에서 3 way merge가 계속 발생한다.<br><br>
-	마찬가지로 merge tool을 이용해서 충돌을 관리한다.<br><br>

Reference
---------

-	[생활코딩](https://opentutorials.org/course/3838)

---
