---
layout: post
title: 오픈소스 문서 만들어보기
---

여러 블로그에서 기술 문서는 예전부터 많이 중요성이 주목받는 편이지만, 오픈소스에 관련된 문서들은 뭔가 소외되는 느낌이 들어서, 오늘은 오픈소스 프로젝트를 시작하고 작성하면 좋은 문서들을 모아서 소개해드리려 합니다.

## 라이선스

매우 중요해서 맨 앞에서 서술합니다.

우선 정의에 의하면, 모든 오픈소스 프로젝트는 반드시 오픈소스 라이선스를 가져야 합니다. 
만약 프로젝트가 라이선스를 가지지 않는다면, 오픈소스가 아니라고 할 수 있습니다.

간혹가다가 라이선스가 한국어로 번역되어 작성하는 경우가 있는데, 이는 다른 언어로의 번역에 따른 위험 부담으로 인해 라이선스의 효력이 없을 수도 있기 때문에 주의를 요구합니다.

예 : [GPL 한국어 번역문](http://korea.gnu.org/documents/copyleft/gpl.ko.html) 최상단 참고.

또한, 주의해야 할 점은 라이선스보다 소스 코드를 먼저 올리게 되면 해당 커밋은 러이선스가 없는 것으로 간주할 수도 있습니다.

* tip : https://choosealicense.com 에 방문해보셔서 본인의 프로젝트에 맞는 라이선스를 찾아보시는 것도 좋습니다 :)

## Readme

README 파일은 해당 오픈소스 프로젝트를 처음 방문하는 사람들에게 친절히 설명하는 문서입니다.

또한, 왜 이 프로젝트가 유용한지, 이 프로젝트를 어떻게 시작하는지를 설명해줄 수 있습니다.

아래는 대표적으로 readme에 들어가면 좋은 문항입니다.

* 이 프로젝트는 무엇을 합니까?

* 이 프로젝트가 왜 유용한가요?

* 시작하려면 어떻게 해야 합니까?

* 필요한 경우 어디에서 더 많은 도움을 받을 수 있습니까?


만약 어떻게 Readme를 작성해야 될지 모를 때는 타 프로젝트를 참고해보는 것이 좋으며, 대표적으로 [README-Template.md](https://gist.github.com/PurpleBooth/109311bb0361f32d87a2) - 'A template to make good README'라는 커스텀 템플릿이 있습니다.

제가 제시한 위 예시에서는 Project Title,description, Getting Started, Running the tests, Deployment, Built With, Contributing, Versioning, Authors, License 등이 포함되어있습니다.

## CONTRIBUTING

Readme 문서와 다르게 CONTRIBUTING 문서는 기여하는 사람들에게도 유용한 문서입니다.

보통, 기여할때 필요한 지식과 기여 방식을 기록해둡니다.

이 파트부터는 모든 프로젝트가 적용하고 있는 사항은 아니지만, 큰 프로젝트에서는 자주 쓰이며 최근 사용빈도가 점차 늘어나는 추세입니다.

아래 항목들이 CONTRIBUTING 문서에 들어간 내용입니다.

* 버그 보고서를 제출하는 방법

* 새로운 기능을 제안하는 방법

* 환경 설정 및 테스트 실행 방법

제가 본 잘 만들어진 CONTRIBUTING문서는 역시 github에서 운영하는 저장소였습니다.
(다른 우수 사례가 있다면 댓글로 알려주시기 바랍니다.)

* github의 classroom 저장소 [CONTRIBUTING 파일](https://github.com/education/classroom/blob/master/CONTRIBUTING.md)

간략히 Opening an issue, Submitting a pull request, Resources로 구성되어 있으며, 

눈에 띄는 점은 ```Thank you for taking the time to open an issue```라고 고마움을 나타내고 있다는 점입니다.

(저만 그런 것인지 모르겠지만) 사소한 문구조차 기여를 하고싶은 욕심이 들게 합니다.

## CODE OF CONDUCT

code of conduct는 기여자에 대한 기본 원칙을 설정하는 문서입니다.

이 또한 CONTRIBUTING 문서처럼 모든 프로젝트가 CODE_OF_CONDUCT 문서를 가지고 있는 것은 아니지만, 기여하기 좋은 환경의 프로젝트임을 알립니다.

아래 항목들이 CODE_OF_CONDUCT에 들어갑니다.

* 적용되는 범위

* 위반 시 대응 사항

* 위반 사례를 신고할 수 있는 방법

## Issue or pull request template 

이는 깃허브에서만 적용할 수 있는 사항이라 넣을까 말까 고민했지만, 가장 아래에 서술하기로 했습니다.

Issue 혹은 pull request template은 말 그대로 이슈와 pull request를 작성할 때, 기본 템플릿을 제공하는 기능입니다.

버그 해결에 중요한 사용자 정보와 재현 방법 등을 미리 요구할 수 있습니다.

이러한 문서는 최상단 .github 폴더에서 작동되며, 마크다운 문서로 작성할 수 있습니다. 


## 해당 문서에 대해 고지
> 본 문서는 법적 책임과 보증의 의무가 없으며, Github에서 작성한 opensource.guide 문서를 참고하여 제작하였습니다. 