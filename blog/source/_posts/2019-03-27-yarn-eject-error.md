---
title: yarn eject 중 에러가 발생했다
date: 2019-03-27 10:58:18
tags:
categories:
- 일상
---

# `git commit -am "commit message"`

`create-react-app`으로 프로젝트를 생성 후 관련 설정을 바꾸기 위해  `yarn eject`을 실행하면 에러가 난다. 뭐라더라, 깃으로 쫓을 수 없는 파일들과 관련한 에러라고 어디선가 읽었는데, 이 에러는 많은 이들에게 발생해서인지 트러블슈팅이 꽤나 잘 되어있는 편.

정답은 깃에 커밋을 한 뒤에 시도하면 된다... 였는데

문제는 나의 경우 커밋을 해도 에러가 났다...

또 다시 시작되는 방황... 속에 만난 [해결 방안](https://stackoverflow.com/questions/48854585/error-with-run-npm-run-eject-error-remove-untracked-files-stash-or-commit-a)!

다른 건 별로 없고, 커밋하는 방식의 차이였다. `-m`이 아니라 `-am`, 즉 명령어 `a`

무슨 역할을 해주는지 궁금해져서 다시 [찾아봤는데](https://stackoverflow.com/questions/19877818/git-commit-m-vs-git-commit-am) `git add .` 를 해주는 것과 같다고 했다. 근데 나는 분명히 커밋하기 전에 `git add .`를 사용한 뒤 커밋했는데, 왜 깃이 변경된 파일을 제대로 못 잡는 건진 잘 모르겠다. 하여튼 이 명령어를 치니까 잘된다.