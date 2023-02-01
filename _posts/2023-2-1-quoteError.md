---
title: Gitblog Error - invalid attribute name 
author: delaying
date: 2023-2-1 12:44:00 +0900
categories: [Develop, Gitblog]
tags: [github, TrobleShooting]
published: true
---

## 발생 원인
gitblog 작성 후 git push를 했는데 빌드에 실패했다.


## 발생 에러
build 경고는 다음과 같았다.
```
The `set-output` command is deprecated and will be disabled soon. Please upgrade to using Environment Files. For more information see: https://github.blog/changelog/2022-10-11-github-actions-deprecating-save-state-and-set-output-commands/
```

build 에러 내부로 들어가보니 이런 에러가 떠있었다.
```
Error:  invalid attribute name `"'
```

## 에러 해결과정
1. 처음에는 포스트 한개씩 커밋을 나누려고 이전꺼를 push하고 빌드가 덜되었는데 다음커밋을 push해서 빌드에러가 난줄알았다.<br/>
    이전에 push한 커밋들을 `git reset HEAD^`명령어로 제거 후 다시 하나씩 진행해봤으나 같은 에러가 발생했다.

2. build 경고를 확인하고 구글링했는데 workflow를 수정하라고 나와있었다.<br/>
    그러나 나는 해당사항이 없었다. (경고는 경고일 뿐이었는데..)

3. 내가 사용하고 있는 gitblog 테마 git issue에서 build문제를 검색해보았다.<br/>
    이때만해도 경고떠있는게 문제인줄 알고 그와 관련해서 찾다보니 해결할 방법이 나오지 않았다.
   
4. build Error메시지를 뒤늦게 확인했더니 `Error:  invalid attribute name~` 이러한 메시지가 있었다.<br/>
    gitblog 테마의 issue에 검색했더니 [이 글](https://github.com/cotes2020/jekyll-theme-chirpy/issues/496)을 찾았다.<br/>

    제목에 큰따옴표를 쓰면 안된다는 거였다..!!!<br/>
    바로 제목을 수정하고 push하니 정상적으로 build되었다.


## 결과
에러메시지를 잘 확인할 것!

경고메시지보다 에러메시지를 먼저 확인할 것!!!

생각보다 issue란은 많은 도움이 된다.