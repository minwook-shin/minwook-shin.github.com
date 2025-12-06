---
layout: post
title: gaming on ubuntu
---

안녕하세요. 오늘은 약 3년 전으로 거슬러 올라가서, 제가 우분투 한국 커뮤니티에서 발표한 내용을 서술해보고 2018년이 다가오는 현재는 어떠한지 끄적여볼 예정입니다.

## 우분투에서 게임이 될까요

우선 2017년 기준은 마지막에 설명할 예정이니 패스하고, 2년 전 발표했을 때를 기억하며 말씀드리자면 

"되긴 되는데 음..." 정도였습니다.
우분투에서 게임 잘되냐고 묻는 타인의 질문에도 "잘 안되는데?"라고만 대답할 정도죠. 

근데 저는 되게 해보고 싶었습니다.

윈도우는 구동시키기 어려운 (지금은 사라진 유물인)넷북을 쓰면서, 우분투에서 게임이 되나 혼자서 트러블 슈팅했습니다.

## 덕질을 하니 발표할 기회가 찾아왔습니다

뭐든지 집중해서 하다보면 뭔가 기회가 오더군요.

15년 3월인가 4월즈음에 이러한 주제로 발표해보겠다고 말해보니, 바로 성사되었습니다.

## 1편 

<iframe src="//www.slideshare.net/slideshow/embed_code/key/5gl0FXeYHXI3Jf" width="595" height="485" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="border:1px solid #CCC; border-width:1px; margin-bottom:5px; max-width: 100%;" allowfullscreen> </iframe> <div style="margin-bottom:5px"> <strong> <a href="//www.slideshare.net/UbuntuKorea/minwook-shin-201505-2015y05m30d" title="우분투에서 게이밍은? | 신민욱 Minwook Shin | 2015.05 (2015Y05M30D)" target="_blank">우분투에서 게이밍은? | 신민욱 Minwook Shin | 2015.05 (2015Y05M30D)</a> </strong> from <strong><a href="https://www.slideshare.net/UbuntuKorea" target="_blank">Ubuntu Korea Community</a></strong> </div>

1편은 우분투 한국 커뮤니티 5월 세미나에서 여태까지 삽질한 경험을 위주로 써보았습니다.
 
1편의 주용 내용은 제가 중학생때 삽질한 기억부터 시작해서 현재 우분투에서 게임을 해보려고 삽질한 내용을 공유한 것입니다.

만약 삽질해서 성공했다면, 어떻게 해서 성공했는지도 언급했습니다.

발표했던 날짜를 기준으로는 정상적으로 한글이 출력되게 게임하려면 꽤 많은 관문(?)을 통과해야됬었습니다만,
다행히도 지금 이 시점에서는 이미 고쳐져서 안해도 무난하게 작동되고 있습니다.

# 2편

2편은 영광스럽게도 [kcd 2015](http://kcd2015.onoffmix.com/)에서 진행했습니다 ^^ (다시는 오지않을 기회였죠.)

1편의 대부분을 먼저 (재탕) 발표하고, 그동안 바뀐 점에 대하여 언급하며 이어서 2편하는 형태입니다.

2편에서는 주로 html로 게임할 수 있다!는 내용을 담았고, 깃허브를 이용해 간단히 html로 이루워진 게임을 포크로 담아온 후 즐기자는 내용이 있습니다.

웹은 그 당시 기준으로도 웹브라우저별 표준 지원능력(?)이 비슷해서 웹 gooood!이라고 언급하며 끝냈던 기억이 납니다. 

[이 곳](http://kcd2015.onoffmix.com/decks/Track1/Track1_Session1_신민욱_gaming_on_ubuntu_2.pdf)에서 PDF 형식으로 보실 수 있습니다.

## 그렇다면 현재는 게임이 되나요

결과부터 말씀드리자면 똑같습니다.
아니... 비슷하다고 해야될지 모르겠습니다.

여전히 국내 게임은 미지원이며, 모두가 아는 전설의 N사 독점 드라이버는 여전히 잘 나옵니다.

하지만, 스팀의 리눅스 지원 게임은 나날히 늘어나고 있고, 최근(12월 27일) Vulkan driver를 사용하는 ["Radeon Software for Linux"](http://support.amd.com/en-us/kb-articles/Pages/Radeon-Software-for-Linux-Release-Notes.aspx)를 오픈소스로 공개해서 여전히 그대로라고는 느끼진 않습니다.

또한 올해 모질라에서 [Firefox Quantum](https://www.mozilla.org/ko/firefox/)을 출시하여 우분투 기본 탑제 브라우저인 파이어폭스의 인터넷 서핑 환경을 쾌적하게 만들어 주기도 했습니다.

(추가로 알아보니, [WebAssembly](https://developer.mozilla.org/ko/docs/WebAssembly) 프로젝트도 진행되고 있었습니다.)

<blockquote class="twitter-tweet" data-lang="ko"><p lang="en" dir="ltr">Another round of renderer optimizations done, large CTF levels are now running at a solid 60 FPS <a href="https://twitter.com/hashtag/quakejs?src=hash&amp;ref_src=twsrc%5Etfw">#quakejs</a> <a href="http://t.co/Dsvci3Ad">pic.twitter.com/Dsvci3Ad</a></p>&mdash; Anthony Pesch (@inolen) <a href="https://twitter.com/inolen/status/275752910862831616?ref_src=twsrc%5Etfw">2012년 12월 4일</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>


## 마무리

체감하기엔 아직 멀게 느껴지지만, 점점 우분투에서 게임하기 좋은 환경으로 변하고 있는 것은 맞습니다.

이제부터 기반이 잡힌다면, 게임사에서 LINUX 지원을 고려하지 않을까... 싶습니다. 
아마도 계속 기다려주면 언젠가 될거 같습니다.