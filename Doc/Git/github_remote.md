Github 협업 플로우
=================

### 1. 저장소 권한 부여

- Settings > Collaborators > 권한 부여할 ID 입력.

![gitremote01](https://user-images.githubusercontent.com/43169339/48525004-d1bfa800-e8c5-11e8-8309-a8d5132efcfb.PNG)

- 권한 받은 사용자는 자신의 Email에 들어가 버튼 클릭.

![gitremote02](https://user-images.githubusercontent.com/43169339/48525191-84900600-e8c6-11e8-9a3e-d28b54a9a977.PNG)
***



### 2. 작업환경 셋팅

- 저장소 복사할 폴더 생성.

![gitremote03](https://user-images.githubusercontent.com/43169339/48525513-c8cfd600-e8c7-11e8-97da-0ab8233373c9.PNG)



- 폴더 생성후 저장소 복사.
```sh
$ git clone 저장소 주소 . (끝에 . 입력해야 생성한 폴더에 파일만 복사함)
```
![gitremote04](https://user-images.githubusercontent.com/43169339/48525731-86f35f80-e8c8-11e8-8b45-09d749d7d3f3.PNG)



- 저장소 REMOTE 연결 및 확인.
```sh
$ git remote add origin 저장소 주소

$ git remote -v (remote 확인)

$ git remote remove origin (remote 삭제 - 연결이 잘못되어 있으면 삭제 후 다시 연결)
```
![gitremote05](https://user-images.githubusercontent.com/43169339/48526074-a50d8f80-e8c9-11e8-9732-adc4dd3f915e.PNG)
***


### 3. 저장소 업로드

- 수정된 파일 또는 추가된 파일 추가.
```sh
$ git add Test.txt

$ git add . (전체 파일 추가)
```


- 파일 커밋.
```sh
$ git commit -m "Test"
```


- 푸쉬
```sh
$ git push
```
![gitremote06](https://user-images.githubusercontent.com/43169339/48526364-c02ccf00-e8ca-11e8-92c4-7e0a51316629.PNG)
***
***

FORK를 이용한 협업 플로우
========================

### 1. FORK 및 자신의 로컬에 복사

- 원격 저장소를 자신의 저장소로 FORK.

![gitworkflow01](https://user-images.githubusercontent.com/43169339/48595829-b8882b80-e999-11e8-8fc7-891efaeefcb0.PNG)

- FORK 후 자신의 로컬에 복사
```sh
$ git clone FORK한 저장소 주소
```
![gitworkflow02](https://user-images.githubusercontent.com/43169339/48595966-406e3580-e99a-11e8-8096-33fb9b3db7f6.PNG)

- 타인의 저장소 REMOTE
```sh
$ git remote add 저장소 이름 저장소 주소
```
![gitworkflow03](https://user-images.githubusercontent.com/43169339/48599183-f3925b00-e9a9-11e8-8796-c3f5a5952194.PNG)
***

### 2. Pull requests

- 수정 및 추가된 파일 추가, 커밋, 푸쉬
```sh
$ git add 수정 및 추가된 파일 ($ git add .)

$ git commit -m "msg"

$ git push
```
![gitworkflow04](https://user-images.githubusercontent.com/43169339/48599413-df9b2900-e9aa-11e8-9fb6-43838b0227f5.PNG)

- 자신의 저장소에서 New pull request 클릭

![gitworkflow05](https://user-images.githubusercontent.com/43169339/48599738-600e5980-e9ac-11e8-9d13-698f5d6793e7.PNG)

- 타인의 저장소로 이동 후 커밋 및 수정 내용 확인 가능, 확인 후 이상이 없으면 Create new pull request 클릭

![gitworkflow06](https://user-images.githubusercontent.com/43169339/48599740-60a6f000-e9ac-11e8-9a8d-c4b16ff1289e.PNG)

- 코멘트 입력 후 Create pull request 클릭

![gitworkflow07](https://user-images.githubusercontent.com/43169339/48599741-60a6f000-e9ac-11e8-9ee4-cde6f2e934be.PNG)

- 담당자는 Pull request 검토 후 이상이 없으면 merge 승인

![gitworkflow08](https://user-images.githubusercontent.com/43169339/48599743-60a6f000-e9ac-11e8-812a-9fe5ff84d727.PNG)
***

### 3. FORK한 저장소 최신화

```sh
$ git fetch 등록한 저장소 이름

$ git merge 등록한 저장소 이름/브랜치
```
![gitworkflow09](https://user-images.githubusercontent.com/43169339/48600040-9f897580-e9ad-11e8-80a6-02cb2fbcffc2.PNG)
***
***

Feature Branch Workflow
=======================

### 1. Branch 생성 후 Checkout

```sh
$ git branch 브랜치이름

$ git checkout 브랜치이름
```
![gitflow01](https://user-images.githubusercontent.com/43169339/48683627-735e3680-ebf1-11e8-9261-1f823c6bd15d.PNG)
***

### 2. Push

- 생성한 브랜치를 중앙저장소에 푸시하는 명령어. (--set-upstream : -u), 한번 연결 후 git push만 입력해도 됨.
```sh
$ git push -u 저장소이름 브랜치이름

$ git push
```
![gitflow02](https://user-images.githubusercontent.com/43169339/48686722-d4d9d180-ec00-11e8-88fe-aa35496af6dd.PNG)
***

### 3. Pull requests 및 Merge

- Pull requests 후 병합하기로 결정이 나면 작업 진행(권한이 있는 누구든 병합 가능)
```sh
$ git checkout master

$ git fetch 저장소이름 브랜치이름

$ git merge 저장소이름/브랜치이름

$ git push 저장소이름 master
```
![gitflow03](https://user-images.githubusercontent.com/43169339/48687097-cee4f000-ec02-11e8-92c7-31afa463274e.PNG)



***
### 참고사이트

- Git을 이용한 협업 워크플로우 (https://lhy.kr/git-workflow)
- Fork를 이용한 깃 협업플로우 (http://playinlion.tistory.com/29)