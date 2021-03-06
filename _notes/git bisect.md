---
title: 'git bisect'
author: 'vlwkaos'
created: 2021-02-27:10:13:04
published: true
---

간단히 말하자면 `git bisect`을 사용하면 특정 커밋 해쉬를 이진 탐색으로 찾아낼 수 있다. 

현재 버전이 **2.5.5**이고, **2.5.5** 버전의 코드에 **버그 A**가 있다고 하자. 

**버그 A**가 없는 버전이 **2.3.1** 인 것을 안다면, **버그 A**는 **2.3.1 ~ 2.5.5** 사이에 있다. 

그 **버그 A**를 찾기 위해 일일히 커밋 히스토리를 따라 checkout 할 수도 있지만 이는 매우 비효율적이다. 대신 `git bisect`를 사용할 수 있다.

1. 먼저 `git bisect start`를 통해 이진 탐색모드를 시작한다.
2. 버그가 있는 버전(커밋)에서 `git bisect bad`를 입력하거나 `git bisect bad <bad-commit-hash>`를 입력하여 탐색할 범위의 끝을 정한다.
3. 탐색 범위의 시작을 정하기 위해 `git bisect good <good-commit-hash>`를 입력한다.
4. 탐색 범위가 설정되고 나면 알아서 중간 지점의 커밋으로 checkout 해준다. 그리고 몇번의 탐색이 더 남았는지를 알려준다. 테스트를 한다.
5. 만약 현재 checkout 된 커밋에 버그가 여전히 있다면 `git bisect bad`, 버그가 사라졌다면 `git bisect good`를 다시한번 입력한다. 이렇게 점점 범위를 좁혀나가다 보면 버그가 발생하는 특정 커밋을 찾을 수 있을 것이다.
6. 버그를 찾고나면 `git bisect reset` 커맨드로 탐색 모드를 종료하고 1번에서 checkout 했던 브랜치로 돌아갈 수 있다.
