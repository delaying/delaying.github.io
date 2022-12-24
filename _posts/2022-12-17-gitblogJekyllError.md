---
title: GitHub Pages blog jekyll theme설정과 build 에러
author: delaying
date: 2022-12-17 22:38:00 +0900
categories: [Develop, Gitblog]
tags: [github,blog]
published: true
---

첫 게시물로 이 github pages 블로그를 만드느라 겪었던 수난 과정을 기록하려고 한다.

혹시 나와 같은 문제를 만난 사람들이 나처럼 시간을 허비하지 않고 빨리 해결하길 바란다.

다른 기본적인 설정방법들은 다른 블로그에도 많기 때문에 내가 찾기 어려웠던 내용들을 작성하도록 하겠다.



MacOS 기반으로 작성되었다.

## jekyll theme 적용 에러
#### 첫 번째 에러

  ```
  bundle install
  ```
  위 명령어는 거의 필수적으로 사용됐는데 나는 여기부터 막혔었다.

  ```
  Your user account is not allowed to install to the system RubyGems.
    You can cancel this installation and run:

        bundle install --path vendor/bundle

    to install the gems into ./vendor/bundle/, or you can enter your password
    and install the bundled gems to RubyGems using sudo.

    Password:
  ```
  여기서 갑자기 Password를 입력하라길래 무슨 비밀번호인지 몰라서 3번틀려서 아래오류들도 많이 불러일으켰다

  >입력해야할 Password값은 macbook 비밀번호였다..


#### 두번째 에러

  3번 틀리니까 다음과 같은 새로운 에러가 발생했었다.

  ```

  Gem::Ext::BuildError: ERROR: Failed to build gem native extension.

  An error occurred while installing eventmachine (1.2.7),
  and Bundler cannot continue.
  ```

  위 에러가 발생해서 다음 명령어를 실행하니까 또 에러가 바뀌었다.
  ```
  gem install --user-install bundler jekyll
  ```


#### 세번째 에러

  ```
  An error occurred while installing racc (1.6.1), and
  Bundler cannot continue.
  ```

  이를 해결하기 위해 터미널 루트경로에서 다음과 같은 행동들을 취했다.

  ```
  ➜ ~ gem install bundler
  ```
  bundler를 먼저 설치해주어야 하는데 아래와 같은 에러가 떴다.
  ```
  ERROR:  While executing gem ... (Gem::FilePermissionError)
      You don't have write permissions for the /Library/Ruby/Gems/2.6.0 directory.

  ```
  ruby 버전을 바꿔주었다.
  ```
  ➜  ~ rbenv versions
    system
  * 2.7.2 (set by /Users/delay/.rbenv/version)
  ```
  ```
  ➜  ~ rbenv install -l
  2.7.7
  3.0.5
  3.1.3
  jruby-9.4.0.0
  mruby-3.1.0
  picoruby-3.0.0
  rbx-5.0
  truffleruby-22.3.0
  truffleruby+graalvm-22.3.0

  ```

  ```
  ➜  ~ rbenv install 2.7.7
  ```
  설치가 완료되면 다음처럼 해주면 된다.

  ```
  ➜  ~ rbenv versions
    system
  * 2.7.2 (set by /Users/delay/.rbenv/version)
    2.7.7
  ➜  ~ rbenv global 2.7.7
  ➜  ~ rbenv versions
    system
    2.7.2
  * 2.7.7 (set by /Users/delay/.rbenv/version)
  ```

  그 후 ~/.zshrc 파일에 코드를 추가해주어야한다.

  ```
  ➜  ~ vim ~/.zshrc
  ```
  i를 누르고 다음 코드를 추가한 후 esc -> :wq 로 저장하고 나오면 된다.
  ```
  [[ -d ~/.rbenv  ]] && \
    export PATH=${HOME}/.rbenv/bin:${PATH} && \
    eval "$(rbenv init -)"
  ```


  그리고 다음 명령어를 입력한다.
  ```
  ➜  ~ source ~/.zshrc
  ```

  그리고 프로젝트 터미널에서 bundle install 해주니까 드디어 에러가 해결되었다.

  ```
  ➜  ~ gem install bundler
  Fetching bundler-2.3.26.gem
  Successfully installed bundler-2.3.26
  Parsing documentation for bundler-2.3.26
  Installing ri documentation for bundler-2.3.26
  Done installing documentation for bundler after 0 seconds
  1 gem installed
  ```

  

  성공 후 
  ```
  bundle exec jekyll s
  ```
  명령어 실행하고 [`localhost`](http://127.0.0.1:4000/) 들어가서 확인하면 된다.




## git build Error

파일들을 수정하고 푸쉬했는데 자꾸 다음과 같은 에러로 빌드에러가 발생했다.

```
github-pages 227 | Error:  The jekyll-theme-chirpy theme could not be found.
```

git에서 jekyll theme를 clone이 아닌 zip파일로 다운받아서 폴더에 파일들을 복사 붙여넣기 했을 때

macos같은 경우 .으로 시작하는 폴더나 파일들은 숨겨져 있어서 보이지 않는데

나는 그 파일들이 보이지 않는 상태로 전체파일 복사붙여넣기해도 그 파일들은 복사되지 않는다는 점을 간과해서 많은 시간을 허비하였다.


파일에서 `command + shift +  . `으로 가려진파일들을 확인해서 꼭 모든 파일을 복붙해야한다.


모든 파일을 다시 내 폴더에 저장하고 
내가 사용하는 테마 [`document`](https://chirpy.cotes.page/posts/getting-started/)를 따라갔더니 정상적으로 빌드되었다.