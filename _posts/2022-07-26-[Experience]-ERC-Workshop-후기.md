---
layout: post
title: "[Experience] 2022 ERC Workshop 포스터 발표 후기"
subtitle: "소프트웨어재난연구센터 여름정기 워크숍"
categories: Experiences
tags: [erc,starr,workshop]
---

[포스터 pdf](/assets/resources/erc22-poster.pdf)

### 배경

지난 학기 KCC에 제출했던 논문을 바탕으로 ERC 워크숍에서 포스터 발표를 할 수 있는 기회가 생겼다. 2저자로 참여한 논문이지만 이번 포스터 제작과 발표는 내가 주도적으로 맡게 되었다. 제주에서 열린 KCC 행사에 이어 이번 기회를 통해 논문의 내용을 한번 더 소화할 수 있었고, 나에게 있어 이 경험들이 가지는 의미를 다시 한 번 생각해볼 수 있었다.

### 포스터 제작

포스터 제작은 논문의 내용과 KCC 발표 PPT를 바탕으로 틀을 만든 뒤 더 구체적인 예제들을 찾고, 예제들로부터 의미 있는 발견들을 꺼내는 것으로부터 시작되었다. 포스터를 통해 논문의 내용을 잘 보여주는 것을 넘어서, 논문에서 충분히 설명하지 않은 부분들을 더 구체적으로 이야기할 수 있도록 해야 했다. 논문을 쓰고 발표자료를 준비하는 과정에서 이미 많이 봤다고 생각했던 데이터들을 보고, 다시 보고, 또 다시 보고, 때로는 추가적인 세부 분류를 하기도 하며 '이 버그 케이스로부터 어떤 insight를 얻을 수 있을까, 어떤 과정을 자동화함으로써 fuzzing target 개발자들을 도울 수 있을까' 를 고민하고 또 고민하는 과정의 연속이었다. *더해서, 그 내용들을 한정된 지면 안에 가독성을 유지하며 채워 넣는 일의 반복이었다.*

쉽지 않은 과정이었지만 버그 케이스로부터 좋은 아이디어를 생각해 냈을 때 묘한 짜릿함이 있었다. 뿐만 아니라, 데이터를 다시 보고 분석하고 이해하는 과정, 포스터를 구성하는 과정에서 내가 여전히 제대로 이해하지 못하고 있던 논문의 내용을 조금 더 소화할 수 있었다.

발표 전날 밤이 되어서야 다양한 예제와 그로부터 꺼낸 논의사항들로 가득 찬, 말 그대로 가득 찬 포스터를 완성할 수 있었다.

### 포스터 발표

발표 전 쉬는시간, 압도적으로 내용이 많은 것 같은 우리의 포스터를 붙여두고 보며 어떻게 발표를 해야할지 머릿속으로 그려 보고 있었는데 교수님께서 옆에 오셔서 넌지시 말씀하셨다. "서예야, 사람들 오면 네가 적극적으로 설명해!!". 그 말씀을 듣고 KCC에서 봤던 포스터 발표들을 떠올리며 나도 전투적으로 발표해 봐야겠다고 다시 한 번 마음을 먹었으나... 논문의 1저자였던 박사과정 연구생 오빠의 3분 발표가 끝나니 점점 정신이 아득해지는 것이 느껴졌다.. (~~'내가 뭐부터 말하려고 생각했더라;;;'~~)

그리고 발표 시간, 분명 무척 춥다고 느꼈던 학회장이었는데 발표 하는 동안은 등에 땀이 줄줄 흐르는 걸 느꼈다. 캡스톤 페스티벌 때 포스터 세션 비슷한 무언가를 경험해 보긴 했지만 지금 나에게 질문을 하고 설명을 듣고 있는 사람들이 fuzzing을 하고 있거나 최소 SE 전공을 하고 있는 대학원생들, 혹은 교수님들이라는 생각을 하니 다른 느낌으로 어렵고 긴장이 되었던 것 같다. 동시에 짜릿하기도 했다. 질문들을 듣고 대답하다 보면 이 분들이 나를 이 연구를 진행한 '연구자'로 인정하고 존중해 주신다는 느낌을 받았기 때문이다.

더해서, 발표를 하다 보니 내가 어느 부분을 잘못 알고 있었는지, 어느 부분을 아직도 헷갈려 하는지가 적나라하게 드러났다. 그 때 마다 바로잡아 주시거나 내용 보충을 해주시는 1저자 연구생을 보며 존경심이 들면서도(ㅎㅎ) '아 저런 이야기들을 해줘야 하는구나', '여기서는 이런 생각을 해야 하는구나', '이 부분을 내가 잘못 알고 있었구나' 같은 생각을 할 수 있었다.

딱 한 가지 아쉬운 것이 있다면, 공간이 협소하고 내용이 많아 설명을 한번 하는데 워낙 오랜 시간이 걸리다 보니, 뒤에 기다리는 사람들은 많았는데 그만큼 많이 듣고 가진 못하신 것 같다는 점이었다.

### 기타 워크샵 일정

KCC와 마찬가지로 정말 많은 발표들을 압축적으로 들었던 것 같다. 우리 연구실 교수님의 발표를 들으면서는 내가 학기중에 참여했던 연구들이 연구실의 큰 흐름 안에서 어떤 부분을 차지하는지, 지금 어떤 방향성을 가지고 연구들을 진행해 나가고 계시는지를 느낄 수 있어서 신기하고 즐거웠다. ~~무엇보다 교수님이 너무 멋있었다...~~

다른 발표들을 들으면서도 이런저런 생각들을 했지만, 특히 '이 발표를 들으면서는 내가 무엇을 생각하고 얻어가야 할까'를 고민하려고 했던 것 같다. 실제로 우리 연구실 대학원생들은 무슨 생각을 하며 발표를 듣고 있는지 궁금해 물어보기도 했다. SE 분야의 발표들을 듣다 보니 조금씩 익숙해지는 키워드들이 있어 궁금해지기도 했고, fuzzing을 하는 분들이 생각보다 많아 신기하고 재밌기도 했다. 나는 1년정도 'fuzzing이 뭐다'를 공부한 정도일텐데도 fuzzing이나 테스팅 쪽 발표가 더 잘 들리고 들어오는 것이 신기하기도 했다. 그래도 여전히 나에게 익숙하지 않은 분야의 발표도 잘 들을 수 있으면 좋겠다는 생각을 많이 했고, 이런저런 세부 분야와 교수님들께서 하고 계신 연구들에 관심을 가지고 가능성을 열어두고 들으려고 노력한 것 같다. 내가 앞으로 어떤 연구를 하게 될지는 아무도 모르는 거니까.

무엇보다, **이 자리에 내가 참여하고 있다는 점이 뿌듯하고 감사했다.**

이번 워크샵은 상당히 자유로운 분위기로 진행된 편이라고 한다. 실제로 그렇게 느꼈다. 교수님께서 준비해 주신 social session들이 있어서 대학원생들과 함께 어울리고, 밥을 먹고, 산책을 하는 시간이 있었는데, 이런저런 얘기를 많이 들을 수 있어 재밌었던 것 같다. (~~잘 모르겠지만 학부 연구생인 나를 조금 신기해하는 것 같기도 했다...~~) 똑같이 사람 사는 얘기 같으면서도, 하고 있는 연구나 연구실 얘기를 할 때 짓는 표정들이나 하는 말들을 들으면 또 신기하기도 하고, 뭐랄까 내내 기분이 묘했다.

둘째 날 우연히 육식을 하지 않는 외국인 학생의 식사 주문을 돕게 되었는데, 그게 인연이 되어 다음 날 식사도 함께 하고 포스터 발표도 듣고 이런저런 이야기를 나누게 되었다. 다른 대학원생들은 그래도 좀 어려운 부분이 있는데, 그 분은 왠지 모르게 더 편하게 대할 수 있어 예상 외의 즐거운 시간을 보내기도 했다.

다시 생각해도 참 감사한 시간이었다. 마침 이번에 포항에서 열려서 학부 연구생으로 참여할 수 있게 되었고, 포스터 발표도 할 수 있었고, 값진 경험을 할 수 있었다. 이런저런 사람들도 만나보고 심지어 이 분야 연구를 막 시작한 고등학교 동창도 만났다(!). 막학기를 시작하기 전 이렇게 좋은 기회를 가질 수 있음에 감사하다.