---

title: Jekyll을 이용하여 Chirpy 테마 블로그 만들기(MacOS)
date: 2022-05-04 11:15:00 +09:00
categories: [Github 블로그 만들기]
tags: [github, 블로그, jekyll, chirpy, mac, macOS]     # TAG names should always be lowercase

---

## 들어가면서
--- 
요즘 깃허브 블로그를 시작하는 분들이 많은데, 방법을 몰라서 헤매는 분들이 많을거라고 생각된다. 개발을 전혀 모르시는 분이라면 어렵고, 헷갈린다.. 

나 또한 블로그 처음 만들때 하루 종일 걸렸던 기억이 있다. 본인처럼 고통받는 사람이 더 이상 없기를 하는 마음에서 포스팅을 작성한다.
<br>
<br>

## 사전 환경
---
해당 블로그 만들기는 Chirpy라는 테마를 적용하기 때문에 이에 맞춰 설명한다.

사전에 필요한 환경과 프로그램은 다음과 같다.    

 - MacOS (본인은 Catalina 10.15.7 버전이지만, 버전은 상관없으리라..)
 - 64bit
 - Git, Github Desktop
 - Visual Studio Code
 
<br>
<br>

## Jekyll 환경 설치 및 세팅
---
우선 테마 적용에 앞서서 필요한 것 부터 설치하고 세팅하도록 하자.
먼저 터미널을 연다.

### Command Line Tools 설정
먼저, Native 확장기능을 컴파일할 수 있게 해주는 명령행 도구를 설치해야 하므로, 다음 명령어를 터미널에 입력한다.
```bash
xcode-select --install
```

###  루비 설치
Jekyll은 루비 2.4.0 버전 이상을 필요로 한다. 여기서는 2.6.3 버전을 기준으로 
기본적으로 Catalina 10.15는 루비 2.6.3이 기본 포함되어 있으므로 아무런 문제가 없다. 

이전 버전을 MacOS 시스템을 사용중이라면, 새로운 버전의 루비를 설치해야 한다. 

#### Homebrew 설치
rbenv를 설치하기 위해서는 Homebrew가 필요하다. 
```bash
# Homebrew 설치
/usr/bin/ruby -e  "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```
#### rbenv 설치 및 사용
많은 사람들이 rbenv로 여러 루비를 관리한다. 각각의 프로젝트마다 다른 버전의 루비를 실행해야 할 때 아주 유용하다. 
```bash
# rbenv 와 ruby-build 설치
brew install rbenv

# 쉘 환경에 rbenv 가 연동되도록 설정
rbenv init
# 만약에 해당 명령어 실행시
# Load rbenv automatically by appending...
# 이 뜨는 경우 해당 명령어를 추가로 수행해준다.
eval "$(rbenv init - bash)"

# 설치 상태 검사
curl -fsSL https://github.com/rbenv/rbenv-installer/raw/HEAD/bin/rbenv-doctor | bash
# OK가 모두 떠야한다.
```
터미널을 재시작하면 변경사항이 적용된다. 이제 원하는 버전의 루비를 설치할 수 있다. 
```bash
rbenv install 2.6.3
rbenv global 2.6.3
ruby -v

# 해당 줄이 출력되면 정상
ruby 2.6.3p62 (2019-04-16 revision 67580) 
```
<br>

### Jekyll 설치
이제 gem을 이용해 Jekyll에 필요한 프로그램들을 설치한다.
```bash
gem install --user-install bundler jekyll
```
설치된 루비 버전을 확인한다.
```bash
ruby -v
ruby 2.6.3p62 (2019-04-16 revision 67580) 
```
이제 아래 내용을 쉘 환경에 추가한다.
```bash
echo  'export PATH="$HOME/.gem/ruby/2.6.0/bin:$PATH"'  >> ~/.bash_profile

# 바로 적용
source ~/.bash_profile
```
그 다음으로 Gem 경로가 홈 디렉토리를 가리키고 있는지 다음 명령어를 실행하여  확인한다. 
```bash
gem env
```
 `GEM PATHS:` 가 홈 디렉토리 내 경로를 가리키고 있는지 확인한다.
 <br>
 <br>

## 블로그 리포지토리 생성 및 설정
---
### 리포지토리 생성
블로그로 사용할 리포지토리를 생성한다.

![create-repo](/assets/img/posting/make-blog/create-repo.jpeg) {: width="70%"}

리포지토리의 이름은 **`<깃허브닉네임>.github.io`**으로 해야 한다. 이미 이러한 이름의 리포지토리를 가지고 있다면 삭제부터 하고 다시 시도하자.
( 본인은 이미 같은 이름으로 만들어 놨기 때문에 에러가 뜬다. )
<br>

### 리포지토리 설정
사전 설정이 필요한 부분은 두 가지이다.
#### main 브렌치 이름 변경
chirpy 테마 기준에 맞추기 위해 main 브랜치의 이름을 master로 변경한다. 
우선 리포지토리에서 `Settings > Branches`에 들어간다. 그리고 브랜치의 이름을 `master`로 변경한다. 

![rename-branch](/assets/img/posting/make-blog/rename-branch.png){: width="70%" }

#### 자동배포 설정 
블로그 작업 내용을 깃허브가 자동으로 배포할 때 읽기, 쓰기 권한을 부여해야 한다. 그렇지 않으면 빌드에 실패하게 된다.  
`Settings > Actions > General`로 이동하여 다음 사진처럼 설정한다. 

![workflows-reset](/assets/img/posting/make-blog/workflows-reset.png) {: width="70%" } 
<br>

### 리포지토리 클론
Github Desktop을 이용하여 생성한 리포지토리를 클론해준다. 

![clone-repo](/assets/img/posting/make-blog/clone-repo.png) {: width="70%" } 

<br>
<br>

## Chirpy 테마 적용
---
### Chirpy 테마 초기화
이제 Chirpy 테마를 적용할 차례이다. [jekyll-theme-chirpy](https://github.com/cotes2020/jekyll-theme-chirpy) 링크로 들어가서 zip파일을 다운로드 받는다. 

압축을 풀면 다음과 같은 파일들이 있다. 숨김파일까지 모두 복사한다.
(숨김파일이 안보인다면 `command(⌘) + shift + .` 을 입력하자)

![chirpy-files](/assets/img/posting/make-blog/chirpy-files.png) {: width="70%" } 

이제 복사한 파일들을 리포지토리로 옮긴다. 
<br>

### _config.yml 수정 
---
Github Desktop에서 `Open In Visual Studio Code` 버튼을 눌러  로컬파일을 VSCode로 연다. 

![git-desktop-menu](/assets/img/posting/make-blog/git-desktop-menu.png) {: width="70%" } 
 
 _config.yml 파일을 찾아 알맞은 설정값으로 수정해야 한다.
```yml

theme: jekyll-theme-chirpy # import하는 테마 명입니다. 디폴트로 사용중인 테마 명이 들어가 있으므로, 수정은 하지 않습니다.

baseurl: '' # 사용자 페이지를 만들었을 경우, 빈칸으로 둡니다. 프로젝트 페이지를 만든 경우 프로젝트 명을 적어줍니다.

lang: ko-KR # 사용하는 언어 설정을 진행합니다. http://www.lingoes.net/en/translator/langcode.htm 로 접속하여 확인가능합니다.

timezone: Asia/Seoul #timezone설정입니다. http://www.timezoneconverter.com/cgi-bin/findzone/findzone 에서 확인가능합니다.

# jekyll-seo-tag settings › https://github.com/jekyll/jekyll-seo-tag/blob/master/docs/usage.md

# ↓ --------------------------

title: Lipcoder # 블로그 이름입니다. 설정하면 브라우저 상단에 설정된 이름이 확인가능합니다.

tagline: Lipcoder Sub # 서브 타이틀 입니다. 설정하면 블로그 첫 페이지 좌측에서 확인 가능합니다.

description: >- # "used by seo meta and the atom feed"라고 나옵니다. 저는 설정을 그대로 두었습니다.

A minimal, portfolio, sidebar,

bootstrap Jekyll theme with responsive web design

and focuses on text presentation.

url: 'https://lipcoder.github.io' # 'https://username.github.io'와 같이 설정합니다. 설정이 잘 못 되면 곤란합니다.

# 잘 적어넣도록 합니다.

github:

username: lipcoder # 본인의 github username을 적습니다.

#twitter:

# username: twitter_username # 본인의 twitter username을 적습니다. 저는 트위터는 사용하지 않아 주석 처리 해 두었습니다.

social:

# Change to your full name.

# It will be displayed as the default author of the posts and the copyright owner in the Footer

name: Lipcoder

email: sb921204@naver.com # change to your email address

links:

# The first element serves as the copyright owner's link

#- https://twitter.com/username # change to your twitter homepage

- https://github.com/LipCoder # change to your github homepage

# Uncomment below to add more social links

# - https://www.facebook.com/username

# - https://www.linkedin.com/in/username

# 상단은 social관련 내용입니다. 본인의 이름, 이메일, 링크 등을 작성합니다. 저는 깃허브만 올려두었습니다.

google_site_verification: 000 # Google Search Console관련 내용입니다. 이후 포스팅에서 다루겠습니다.

# ↑ --------------------------

google_analytics:

id: '000' # Google Analytics ID입니다. 이 또한 이후 포스팅에서 다루겠습니다.

pv:

proxy_endpoint:

cache_path:

theme_mode: # chirpy테마는 [light|dark]테마를 지원합니다. 비워두시면 사용자의 디폴트 값이 설정되고, light 또는 dark로 입력해두시면 페이지의 기본 테마가 설정됩니다.

img_cdn: '' #cdn 이미지 설정입니다. 저는 따로 진행하지 않았으나 진행하시려면 url을 작성해주시면 됩니다.

avatar: /assets/img/profile.png # 대표이미지 라고 생각하시면 됩니다. /assets/img경로에 사진을 넣은 뒤 작성하시면 됩니다.

toc: true # toc(Table of contents)입니다. 블로그 보시다 보면 포스팅 옆에서 스크롤을 따라오는 목차같은 녀석이 있습니다.

# 사용하시려면 true, 아니라면 false를 적으시면 됩니다.

disqus:

comments: true # disqus라는 덧글기능을 하는 녀석입니다. 사용하시려면 true, 아니라면 false를 적으시면 됩니다.

shortname: '' # 사용하신다면 https://disqus.com/ 에 가입 후, shortname을 넣어줍니다.

paginate: 10

# ------------ 아래로는 크게 손 볼 것 없어서 생략합니다. ------------------
```
추가 설정을 하고 싶은 경우 <https://github.com/cotes2020/jekyll-theme-chirpy> 으로 접속하면 더 다양한 내용을 찾아 볼 수 있다.

이제 Github Desktop을 이용하여 Commit 한 후 Push를 해주자. 
<br>

### Jekyll로 Chirpy 테마 초기화
Chirpy를 테마로 사용하기 위해서는 초기화를 한 번 해줘야 한다.  
```bash
# 리포지토리 경로상으로 이동한다.
cd [깃허브 폴더 경로]/lipcoder.github.io

# chirpy를 초기화
bash tools/init.sh
[INFO] Initialization successful!  # 이런 메세지가 나오면 성공!
```
이제 Chirpy에 필요한 의존성 있는 모듈들을 모두 설치해야 한다. 이미 Chirpy에 기본설정이 되어 있기 때문에 해당 명령만으로 모든 것이 설치된다. 
```bash
# 의존 모듈을 설치한다.
bundle
```
많은 내용들이 설치되는 것을 볼 수 있다. 그리고 다음 명령을 사용해 jekyll을 실행한다. 
```bash
jekyll serve
```
정상적으로 수행되었다면 다음과 같이 출력된다.

![jekyll-serve-running](/assets/img/posting/make-blog/jekyll-serve-running.png) {: width="70%" } 

이제 브라우저에서 `http://localhost:4000/` 이나 `http://127.0.0.1:4000/`으로 접속하면 기본 블로그 화면이 나타날 것이다. 

![first-meet-chirpy](/assets/img/posting/make-blog/first-meet-chirpy.png) {: width="70%" } 

위 화면이 잘 나오면 로컬 테스트는 성공이다.
<br>

### .gitIgnore 편집  
배포를 하기전에 `Gemfile.lock` 파일을 제외하는 설정을 먼저 해줘야 한다. 이 파일이 git에 올라가면 빌드/배포 에러를 내는 경우가 많기 때문이다. 

`.gitignore`파일을 열어서 아래 내용을 한 줄 추가한 후 저장한다.
```
Gemfile.lock
```
이제 모든 내용을 Github Desktop을 이용하여 Commit 한 후 Push를 해주자. 
<br>

### 배포 
Push를 마치면 github는 자동으로 블로그 페이지를 만들어 준다. 시간이 좀 걸리기 때문에 잘 빌드가 되고 있는지 살펴보자. 

github 저장소에 가보면, `Actions` 라는 상단 탭이 있다. 이를 클릭해보면 아래와 같이 나타난다. 

![github-commit-action](/assets/img/posting/make-blog/github-commit-action.png) {: width="70%" } 

정상적으로 빌드/배포를 마쳤다면 초록색 아이콘이 생긴다. 

소스를 push하면, github는 자동으로 page build와 deployment를 수행한다. 
이제 중요한 것이 있는데, github에서 `gh-pages`라는 브렌치를 자동으로 생성해 준다는 것이다. 

![build-gh-pages](/assets/img/posting/make-blog/build-gh-pages.png) {: width="70%" } 

블로그 사이트는 실제로 이 브랜치에서 실제 내용이 구성된다. 우리가 Push한 브랜치는 master이고, 여기에는 사이트에 대한 기본 구조가 없기 때문이다. 
<br>

### 서비스용 브랜치 변경하기
저장소에서 `Settings > Pages`탭으로 이동하면, 브렌치 설정을 할 수 있는데 여기서 브렌치를 `gh-pages`로 변경한 후 `Save` 버튼을 클릭한다. 
<br>

### 블로그 확인하기
자동 배포가 완료되면 블로그 세팅이 완료된 것이다. 
https://\<github닉네임\>.github.io/ 로 접속하여 본인의 블로그가 잘 나오면 성공한 것이다. 
<br>

참고 : 
<https://www.irgroup.org/posts/jekyll-chirpy/>
<https://wlqmffl0102.github.io/posts/Making-Git-blogs-for-beginners-3/>
