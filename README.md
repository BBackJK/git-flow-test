# git flow pattern test


## 깃 브랜치 전략

여러가지 Git 브랜치 전략 중 git-flow 브랜치 전략을 사용해보고자 한다.

### How it works

어떻게 동작하는가?


#### Develop and Master(Main) Branches

``master(main)`` 브랜치는 공식 릴리즈 기록을 저장
``develop`` 브랜치는 분기 기능 개발들의 통합 지점 역할

첫번 째 단계는 ``master(main)`` 브랜치에서 ``develop`` 브랜치를 생성하고 remote로 push.

```bash
git branch develop
git push -u origin develop
```


#### Feature Branches

각각의 새로운 기능은 새로운 브랜치(``feature``)를 생성하여 개발하며
백업 / 협업을 위해 서버로 푸시할수 있다.

대신 이러한 새로운 기능을 만들 때 만드는 새로운 브랜치는 기존에 존재하는 ``master(main)`` 브랜치와 ``develop`` 브랜치 중 ``develop`` 브랜치를 사용하여서 branch를 생성해야 한다.

> ``master(main)`` 브랜치는 공식 릴리즈 기록만을 저장하기 때문에.

```bash
git checkout develop                            // develop 브랜치로 변경
git checkout -b feature/(new func name)         // feature/(new func name) 브랜치를 생성함과 동시에 변경
```


#### Finish Feature

새로운 기능을 개발 완료했다면, 새로운 기능을 만들기 위해 브랜치를 생성한 것(``feature/(new func name)``)을 원래 브랜치(``develop``)로 병합해야합니다.

```bash
git checkout develop                // 병합하기 위해 branch 변경
git merge feature/(new func name)   // 뻗어나갔던(새로운 기능을 만들려고 새로만들었던) 브랜치를 병합.
```

#### 기능 개발이 끝나고 릴리즈할 시점이 완료했다면..?

``release`` 브랜치를 ``develop`` 브랜치로부터 생성하여 릴리즈 버전 업로드 준비 시작.

이 ``release`` 브랜치에서는 새로운 기능 개발은 No! 

단지 버그 수정, 설명서 생성 및 기타에 대한 변경사항을 적용합니다.

``release`` 브랜치가 출시를 위한 준비가 모두 끝났다면 출시를 위해

``release`` 브랜치를 ``master(main)`` 브랜치에 병합하고 ``버전 정보를 태그`` 합니다.

또한 다음 기능 개발을 위하여 ``develop`` 브랜치에도 병합하여줍니다.

```bash
git checkout develop
git checkout -b release/0.1.0
```

```bash
git checkout master(main)           
git merge release/0.1.0                 // master(main) 브랜치에 병합
git checkout devleop
git merge release/0.1.0                 // develop 브랜치에 병합
git branch -d release/0.1.0             // 병합을 완료했으므로 브랜치 삭제(옵션; -d)
```


#### 출시를 한 후 예상치 못한 버그가 발견되었다면..?

``hotfix/fix name`` 브랜치를 생성하여 릴리즈를 빠르게 패치하는데 사용합니다.

``hotfix/fix name``는 ``master(main)`` 브랜치를 이용하여 생성해줍니다.

``hotfix/fix name`` 브랜치는 수정사항이 완료된 후 ``release`` 브랜치처럼 ``master(main)``브랜치와 ``develop`` 브랜치에 각각 병합하여 줍니다

```bash
git checkout master(main)
git checkout -b hotfix/(fix name)   // master 브랜치에서 버그 수정할 새로운 브랜치 생성하고 바로 변경
```

수정사항 완료 후 기존에 있는 ``master(main)`` 브랜치와 ``develop`` 브랜치에 모두 병합 후 ``hotfix/(fix name)`` 브랜치는 삭제

```bash
git checkout master(main)
git merge hotfix/(fix name)
git checkout develop
git merge hotfix/(fix name)
git branch -d hotfix/(fix name)
```
