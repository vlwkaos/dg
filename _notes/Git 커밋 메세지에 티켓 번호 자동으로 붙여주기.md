---
title: 'Git 커밋 메세지에 티켓 번호 자동으로 붙여주기'
author: 'vlwkaos'
created: 2021-02-25:01:42:47
published: true
---

일반적으로 Git Flow를 따르고 이슈 관리 도구가 있는 경우, develop 브랜치에서 새 브랜치를 만들 때 이슈 관리 도구에서 생성한 이슈의 티켓 번호를 이름으로 사용한다. 예를 들면 `feature/TICKET-001` 이런 식이다. 그리고 커밋 메세지에 티켓 번호도 같이 넣어서 자동으로 이슈가 브랜치 추적을 할 수 있도록 연동해 놓는다. 

매번 커밋 메세지에 티켓 번호를 넣는게 귀찮다면 아래의 방법을 사용하면 된다.

Git으로 버전관리를 하고 있는 프로젝트 최상위 폴더에 `./.git/hooks/prepare-commit-msg` 파일을 텍스트 편집기로 열고 아래를 추가한다. 참고로 `prepare-commit-msg`는 [Git Hook](https://git-scm.com/docs/githooks) 중 하나로, 파일 이름에서 알 수 있듯이 커밋 메세지를 작성하기 전에 일련의 작업을 할 수 있도록 도와준다.

```
#!/bin/bash
FILE=$1
MESSAGE=$(cat $FILE)
CHECK=$(git rev-parse --abbrev-ref HEAD)
if [[ $CHECK == *"develop"* 
  || $CHECK == *"master"* 
  || $CHECK == *"release"* 
  || $CHECK == *"hotfix"* ]]; then
  if [[ $MESSAGE == *"TICKET"* 
    || $MESSAGE == *"Merge"* 
	|| $MESSAGE == *"Squashed"* ]]; then
    exit 0;
  fi
  red=`tput setaf 1`
  reset=`tput sgr0`
  echo "${red} 배포 브랜치이므로 티켓 번호가 자동으로 붙지 않습니다. TICKET-이슈번호 붙여주세요!${reset}"
  # user stdin to keyboard
  exec < /dev/tty
  read -p '티켓번호 입력 TICKET-' NTICKET
  echo
  echo "TICKET-$NTICKET $MESSAGE" > $FILE
  exit 0;
 fi
 TICKET=$(git rev-parse --abbrev-ref HEAD | grep -Eo '^(\w+/)?(\w+[-_])?[0-9]+' | grep -Eo '(\w+[-])?[0-9]+' | tr "[:lower:]" "[:upper:]")
 
 if [[ $TICKET == "[]" || "$MESSAGE" == "$TICKET"* ]]; then
   exit 0;
 fi
 
 echo "$TICKET $MESSAGE" > $FILE
 ```
 
### 설명
 
 1. `FILE=$1` 현재 커밋 메세지 파일을 변수로 저장
 2. `MESSAGE=$(cat FILE)` 입력한 커밋 메세지를 변수로 저장
 3. `CHECK=$(git rev-parse --abbrev-ref HEAD)` 현재 브랜치 이름을 가져와 변수로 저장
 4. 브랜치 이름이 develop, release, master(main), hotfix를 포함할 때 커밋 메세지에 `TICKET-001`과 같은 티켓 번호가 추가되어 있는지, 혹은 Merge, Squash 커밋 메세지가 있는지 확인한다. 있다면 스크립트를 종료. 없으면 TICKET 번호를 입력받아서 커밋 메세지에 끼워 넣는다. 이게 필요한 이유는 티켓 번호가 브랜치 이름에 없는 경우를 위해서다.
 5. 만약 `feature/TICKET-001` 형태로 티켓 번호를 포함하는 경우 TICKET 변수에 `TICKET-001`을 정규 표현식으로 긁어서 저장
 6. (티켓 번호 + 이전에 입력한 커밋 메세지 내용)으로 현재 커밋 메세지가 수정된다.

대충 만들어진 스크립트라 좀 더 간결하게 만들 수 있을 것 같지만 이 상태로도 잘 작동한다. 이제 메세지만 입력해도 티켓 번호가 앞에 자동으로 붙는다.