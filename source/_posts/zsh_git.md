---
title: oh-my-zsh의 단축 Git 커맨드
date: 2018-11-13 01:49:16
categories:
  - development
---

Z-셸 설정 관리 프레임워크인 `oh-my-zsh`은 다양한 커맨드를 Aliasing 해놓았는데, Git 관련 커맨드만 따로 정리한다.

참고로, 터미널에 `zsh_stats`를 입력하면 내가 어떤 커맨드를 많이 사용하는지 보여준다. 나는 Git 관련 커맨드를 60% 이상 사용하고 있었다. 만약 9자를 입력하는 대신 3자를 입력할 수 있다면 터미널에서 커맨드를 입력하는 시간을 3분의 1로 절약할 수 있지 않을까?

## Git 단축 커맨드

내가 쓸만한 것들만 정리하였다.

- `gst`:	`git status`
- `ga`:	`git add`
- `gco`:	`git checkout`
- `gcmsg`:	`git commit -m`
- `gd`:	`git diff`
- `gl`:	`git pull`
- `gp`:	`git push`

---

- `gcs`:	`git commit -S`
- `gcp`:	`git cherry-pick`
- `gdca`:	`git diff --cached`
- `gf`:	`git fetch`
- `grh`:	`git reset HEAD`
- `gstl`:	`git stash list`
- `gstp`:	`git stash pop`

## Links
- https://github.com/robbyrussell/oh-my-zsh/wiki/Cheatsheet