---
title: gitblog error - layout home Index page -
author: delaying
date: 2023-2-11 9:34:00 +0900
categories: [Develop, Gitblog]
tags: [github, blog, TrobleShooting]
published: true
---

github blog를 평소처럼 커밋 푸쉬했고, 빌드도 성공적으로 끝났는데

페이지에 접속했을 때 `--- layout: home # Index page ---` 이 문구만 뜨는 경우가 있다.

## 시도

1. 빈커밋을 푸쉬해서 다시 빌드해보기 (예전에 성공했으나, 최근발생했을 때는 해결되지않음)

2. jekyll 테마를 사용하고 있어서 관련 [issue](https://github.com/cotes2020/jekyll-theme-chirpy/issues/628)를 찾아보았다.

   2가지의 해결방법이 제시되었다.

   - 번들러 단계에서 빌드가 실패할 경우, `bundle lock --add-platform x86_64-linux`을 실행한다.
   - github 'Build and deployment'를 'Github Actions'로 수동 업데이트한다.

## 해결 방법

내 상황은 번들러 단계에서 빌드가 실패한것이 아니기 때문에 첫번째는 내 상황의 해결 방법이 아니었다.

두번째 해결방법인 수동 업데이트를 위해 github.io 프로젝트의 `action` 탭에 들어간다.

`Build and Deploy`의 제일 최근 커밋에서 `Re-run all jobs` 버튼을 클릭하여 수동으로 재빌드 하니 문제가 바로 해결되었다!
