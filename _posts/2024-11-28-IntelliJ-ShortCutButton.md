---
layout: post
title:  " [IntelliJ] Mac에서 IntelliJ 단축키키"
categories: IntelliJ
author: start-easy
---
* content
{:toc}

Mac에서 IntelliJ 단축키 모두 알고 계신가요?

보통 사용하는 키만 사용하고 나머지는 사용하지 않고 계신건 아닌가요? 

Tobby님의 강연을 들으면서 IntelliJ에 대해 얼마나 자세히 알고 있는지 물어보셨을때 

매일 쓰면서도 자세히 모른다는 사실을 직면했습니다.

 

우선 [jet brain 사이트](https://www.jetbrains.com/help/idea/getting-started.html)로로 가시면 자세히 설명되어 있는 것을 알수 있습니다.

command + , 를 통해 설정창으로 들어갑니다. → 색맹, 커스텀 ui, keymap(short key)등 수정가능 합니다.

command + d 를 파일 2개를 클릭한 상태에서 실행하면 2 파일의 코드를 비교합니다.

![](/assets/img/intellij/command-plus-d-1.png)

![](/assets/img/intellij/command-plus-d-2.png)


command + g 를 blank 처리한 상태에서 실행하면 동일 단어(대소문자 인식)를 찾아 같이 변경할 수 있게 합니다.

![](/assets/img/intellij/command-plus-g.png)

command + breakpoint break point를 누르고 command 클릭을 하면 해당 break point에 라벨을 만들수 있습니다다. 또한 중단점일지 그냥 보낼지도 선택할 수 있습니다.

![](/assets/img/intellij/command-plus-breakpoint.png)

command + j 지금 필요한 케럿을 불러옵니다.

![](/assets/img/intellij/command-plus-j.png)

application 파일에서 ctrl + r 파일이 빌드, 실행됩니다.

command + x 해당 줄을 자릅니다.

command + enter 새로운 문장으로 시작한다. 문장 중간에 있더라도 엔터와는 달리 뒤 문장을 밑의 문장으로 내려보내지 않는다.

command + shift + 방향키 커서를 기준으로 한 문장을 블럭처리합니다.

command + delete 한 줄을 삭제합니다.

ctrl + shift +j블럭처리한 문장을 한 문장으로 만듭니다.

![](/assets/img/intellij/command-plus-j-1.png)

![](/assets/img/intellij/command-plus-j-2.png)

command + . command + 숫자패드'+' 또한 보이는 형식을 묶을지 풀어줄지 결정한다.

command + shift + u 한 범위 내 글자를 모두 소문자로 변경

command + [ 범위의 시작으로 이동

command + ] 범위의 끝으로 이동

command + shift + 좌우 한 문장을 커서 기준으로 한쪽 방향 전체 블럭처리

option + shift + click 기존 블럭에서 해당 블럭을 추가한다. 더블클릭시 해당 단어 전체 블럭을 추가한다.(caret 처리 추가 /삭제)

option + click 드래그 범위를 블럭처리한다.

command + n generate 창 켜기

ctrl + tab swithcer 창으로 이동

shift + shift 전체 검색창 켜기

command + r replace 창 켜기

command + shift + a 실행 창 켜기

ctrl + ctrl run anything

ctrl + t rename이나 refactor

키보드 중 사용이 되지 않는 keymap은 fn 을 사용하면 적용이된다.

F2 error check

이상입니다. 감사합니다.