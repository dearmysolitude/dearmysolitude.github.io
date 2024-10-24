---
title: "지킬 블로그를 만들면서"
excerpt: "lunr 검색 한글 적용과 sidebar, category 사용자 정의하기"

image: "assets/images/teaser-image.png"
categories:
  - Activity
tags:
  - featured
---

## 1. lunr 검색

이미 프로젝트에 lunr 모듈이 포함되어 있어 한국어가 검색이 가능하도록 모듈을 추가하였다.
1. 추가하는 과정에서 vscode에서 auto formatting 때문에 하드코딩 되어 있는 search script가 변형되어서 js script가 제대로 동작 안하게 됨: 하드코딩된 그 문장을 되돌리는데에는 노력이 필요했으므로 repo를 reset(당연하게도 여럿이서 쓰는 레포는 리셋하면 안된다!)하고 auto formatting을 끄는 것으로 해결
2. 한글 적용은 하단의 블로그를 참고하였다. js 모듈을 불러오는 데 문제가 있어 브라우저가 사용하지 못하는 require를 사용하지 않고 직접 불러온 후, 적절한 순서로 불러와 lunr.js 가 제대로 동작하도록 수정하였다.

참고: https://devshjeon.github.io/12, https://github.com/MihaiValentin/lunr-languages


## 2. sidebar 적용

가장 기본적인 틀이 되는 _layout의 default.html에 sidebar로 사용할 요소 두가지: 아이콘과 html 모듈을 불러와 아이콘 클릭에 따라 해당 모듈이 보이거나 보이지 않게 설정하였다.

## 3. category 생성

jekyll의 모듈인 jekyll-toc는 git pages의 사용 가능한 whitelist에 없기 때문에 사용할 수가 없다. 또한 jekyll의 문법인 `*TOC {:toc}`의 경우 md이 html로 번역되는 과정에서 적용되므로 sidebar형태로 사용하려면, 즉 html에 바로 toc를 뿌려주고 싶은 경우에는 사용할 수 가 없다. Js나 liquid 문법으로 toc를 추출하는 로직을 html 모듈로 따로 작성하여 post를 불러오는 경우 default.html에 뿌려주도록 직접 구현해야 할 것 같다.

참고: https://blog.enjoydev.com/2020/jekyll-table-of-contents/

## 4. 기타
프로젝트 구조 자체가 매우 간단하지만, 기능도 적고, 적은 수의 모듈에 모두 몰아 구현되어 있어 구조를 파악하기가 힘들다. 그래도 정리가 잘 되어있어 간단한 블로그로는 사용할 수 있을 것 같다. 하지만 역시 글이 많아지거나 사진이 많아지면 느려질 수 밖에 없으므로 임시적으로 사용할 수밖에 없을 것 같다. 특히, 검색을 위해 사이트 빌드 시점에서 미리 포스트 리스트를 검색하여 페이지로 전달하기 때문에 글이 많아지면 초기 페이지 로딩 시 데이터가 증가한다. Onedrive로 이미지를 호스팅하여 뿌려주는데 여기에서 이미 페이지의 로딩 시간이 상당히 필요하다.

### 4-1. lunr
현재 정적 페이지 검색을 위한 구현 방식
1. jekyll의 정적 페이지는: 빌드 시점에 각 페이지를 완전한 HTML로 만들어 파일로 생성한다(정적 페이지!)
2. 빌드 시, liquid가 document 라는 변수에 모든 글을 조회하여 js가 사용할 수 있는 형태로 저장한다.
3. 검색 시 이 변수에 저장된 데이터를 조회하여 빠르게 검색을 수행할 수 있다.

이 경우, 서버와의 통신 없이 페이지 검색을 빠르게 수행할 수 있다. 하지만 빌드를 수행하지 않으면 데이터 추가가 되지 않으며 글이 많아질수록 페이지를 불러오는 데에 시간이 많이 필요할 수 있다.