---

title: Jekyll을 이용하여 블로그에 Katex 적용하기
date: 2022-05-04 23:16:00 +09:00
categories: [Github 블로그 만들기]
tags: [github, 블로그, jekyll, chirpy, Katex]     # TAG names should always be lowercase

---

## Katex란?
--- 
Katex는 웹 상에 수식 표기를 해주는 라이브러리 중 하나이다.
웹 상에 수학 수식을 입력하는 방법들 중 제일 간편하다고 생각한다. 

문법만 어느 정도 숙달 된다면 이만큼 수식 표현하기 좋은 방법도 없다고 생각한다. 
해당 [링크](https://katex.org/docs/supported.html)를 통해 Katex의 전반적인 문법을 한눈에 살펴볼 수 있다.  

Katex는 엄연히 라이브러리이기 때문에 적용법을 알아야 한다.
여기서는 Jekyll에서 Katex를 적용하는 방법에 대해서 알아보고자 한다. 
(Chirpy 테마를 기준으로 설명하기 때문에 다른 테마는 적용법이 다를 수 있음을 참고하자.)
<br>
<br>

## Jekyll-Katex
---
Katex 라이브러리를 위한 Jekyll 플러그인을 적용해보자. 

### 설치 및 세팅
##### _config.yml 수정  
설정 파일 `_config.yml`에서 맨 마지막에  jekyll-katex를 추가한다. 
```yaml
# 추가 플러그인
plugins:
	- jekyll-katex
```

##### Gemfile 수정
`Gemfile` 루비 파일에서 플러그인 블록에 jekyll-katex를 추가한다. 
```ruby
group :jekyll_plugins do
	gem 'jekyll-katex'
end
```

##### bundler를 이용해 설치
bundler를 이용하여 라이브러리에 필요한 파일들을 설치한다. 
터미널을 이용하여 다음 명령어를 입력하자.
```bash
bundle install
``` 

##### Katex 스타일 추가
이제 Katex용 CSS 파일과 폰트를 추가한다.
로컬상에 설치하여 세팅해도 되지만 여기서는 외부문서로 연결한다. 
(로컬 설치가 궁금한 사람들은 [해당링크](https://github.com/KaTeX/KaTeX)를 참조하자.)

`head.html` 파일에서 포스팅 시 적용되는 블록을 찾는다. 

![head](/assets/img/posting/adjust-katex/head.png){:width="100%"}

그리고 다음 내용을 추가한다. 
```html
<!-- KaTeX CSS -->
<link  rel="stylesheet"  href="https://cdn.jsdelivr.net/npm/katex@0.11.1/dist/katex.min.css"  integrity="sha384-zB1R0rpPzHqg7Kpt0Aljp8JPLqbXI3bhnPWROx27a9N0Ll6ZP/+DiW/UqRcLbRjq"  crossorigin="anonymous">
``` 
<br>
<br>

### 사용 방법 
두 가지 방법이 있다. 

 1. `katex` liquid 태그를 이용하는 방법
 2. `katexmm`  liquid 태그를 이용하는 방법

katex의 경우 내용을 제외한 수식부분을 liquid 태그로 감싸줘야 하는 번거로움이 있다. 
여기서는 katexmm을 이용한 방법을 설명한다. katexmm은 수식모드(\$, \$\$)로 간편하게 수식을 추가할 수 있기 때문에 굉장히 편하다. 

우선 `post.html` 파일에서 내용이 추가되는 부분에 `katexmm` liquid 태그를 다음과 같이 감싸준다.

![katexmm](/assets/img/posting/adjust-katex/katexmm.png){:width="100%"}

이제 수식모드를 사용하면 포스팅시 멋진 수식으로 변환되는 것을 볼 수 있을 것이다.
<br>

##### 주의사항
여기서 주의해야 할 점이 있는데 포스팅 내용은 항상 `katexmm` liquid 태그로 감싸져 있으므로,  수식 표현이 아닌 일반적인 `$` 문자를 사용하면 ** katex가 올바른 수식으로 인식하지 못하여 오류**가 생길 수 있다.

때문에 `$` 문자를 사용하고 싶은 경우 앞에 항상 `\` 를 붙여줘야 한다.  
<br>
<br>

### \$\$(Katex Display 모드) 적용 문제 해결
\$는 잘 되는데  \$\$가 수식으로 적용되지 않는다면, Katex가 아닌 Jekyll 자체에서  \$\$에 대해 수식 변환을 시도하는 것이 원인일 수 있다. 
(<https://github.com/linjer/jekyll-katex/issues/29>)

이런 경우 `_config.yml`파일에 다음 내용을 추가하면 해결 할 수 있다.
```yaml
kramdown:
	# (중간 내용 생략...)
	math_engine: null	# 추가, For Katex display mode
```
<br>
<br>

