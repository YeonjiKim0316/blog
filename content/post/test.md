---
title: "블로그 만들기 (Hugo + Git)"
date: 2021-05-10T00:03:13+09:00
categories: [TIL]
tags: [Hugo, Hugo blog, Git]
cover: ""
---

규칙적으로 무언가를 습득하던 교육과정이 끝나고 스스로 공부를 해야 하기에 블로그를 만들기로 했다.
깃 블로그를 이용코자 검색하던 중 언어는 Hugo로 결정


## 0. Hugo를 이용한 블로그 3줄 요약

- 새로운 사이트를 생성하고
- 테마를 적용한 뒤
- 글을 작성할 때마다 깃 푸시



## 1. Hugo

- Go 언어로 만들어진 Static Blog Generator 
- Elastic의 Beats와 같은 언어라 익숙



## 2. 블로그 설치과정

- hugo release에서 최신버전을 다운받는다. (https://github.com/gohugoio/hugo/releases)
- C:\Hugo\bin\에 압축을 해제한다. 
- Hugo는 portable형 exe 파일이라. cmd에서 언제든지 사용하기 위해서는 환경변수에 추가해 줘야한다.
- window + Q로 검색창을 연 뒤 환경 변수를 검색해서 시스템 환경 변수 편집에 들어간다.
- 오른쪽 아래 환경 변수 버튼을 눌러 맨 위의 박스에서 Path를 더블클릭한다.
- 새로 만들기를 클릭한 다음 아까 압축을 풀었던 곳인 C:\Hugo\bin를 등록해준다.
- 이제 아무데서나 $ hugo version을 쳐봐서 잘 등록 되었나 확인해본다.




## 3. Hugo 프로젝트 만들기

- 프로젝트를 만들 폴더 경로로 들어간다.
- CMD 창에 
'''
    $ hugo new site <프로젝트 이름>
''' 


## 4. theme 적용

- Hugo themes(https://themes.gohugo.io/) 사이트로 들어가서 Theme을 고른다.
- cmd에서 프로젝트 폴더로 이동한다
```
    cd themes
    git clone "다운받을 theme의 url" theme별명

(나는 git clone https://github.com/datacobra/hugo-vitae.git hugo-vitae 를 선택했다.)

```
- config.toml을 편집기로 열어 가장 아래줄에
``` 
    theme = "테마이름"
```
추가


## 5. 글 작성할 때 

```
    $ hugo new {폴더명}/{파일명}.md
```
- 생성된 파일을 편집기로 들어가 편집해준다.
   * 주의 : draft = true 로 되어 있는 경우, 사이트로 Publishing되지 않으므로 작성 완료 후에는 해당 줄을 삭제
- cmd 창에 $ hugo server -D  입력 후 http://localhost:1313 에서 미리보기로 확인
- cmd 창에 $ hugo 입력하여 public 디렉토리에 배포 파일 생성

## 6. Git과 연동

### 가. Github 저장소 2개 만들기
참조: https://github.com/Integerous/Integerous.github.io (Hugo로 github.io 블로그 만들기)

- 하나는 Hugo의 컨텐츠와 소스파일들을 포함할 <YOUR-PROJECT> 저장소 생성한다. (나는 blog라는 이름으로 생성)
- 다른 하나는 렌더링된 버전의 Hugo 웹사이트를 포함할 <USERNAME>.github.io 저장소 생성 (Yeonjikim0316.github.io)

### 나. 내 컴퓨터에서 Remote(관리용)과 Submodule(배포용) 폴더 설정
- Remote와 Submodule 설정
- 깃헙에 만든 blog 저장소를 local의 blog 디렉토리의 remote로 등록한다.
```
    $ cd /blog
    $ git init
    $ git remote add origin git@github.com:Yeonjikim0316/blog.git
```
- Yeonjikim0316.github.io 저장소를 blog의 submodule로 등록한다.
```
    $ git submodule add -b master git@github.com:Yeonjikim0316/Yeonjikim0316.github.io.git public
```

이렇게 되면 hugo 명령으로 public에 웹사이트를 만들 때, 만들어진 public 디렉토리는 다른 remote origin을 가지게 된다. 
- public이 결국 웹사이트다!

## 7. Github에 배포하기
- 우선 project 폴더 내에서 hugo -t 테마이름을 실행한다. 
- public 폴더까지 만들었으면, git push을 통해 각각 git에 저장한다.
- public에서 git push 한 것은 your_name.github.io에 저장될 것이다. 
- 그리고, 프로젝트 폴더로 와서 전체 git push를 진행하면 끝이다.

### 가. public 폴더에서 yeonjikim0316.github.io로
다음 코드처럼 입력하면 된다. 
project 폴더에 있다는 것으로 가정한다.
public을 먼저 push 할 것이다. 

```
    $ cd public # public으로 경로 이동
    $ git add * # 
    $ git commit -m "your commit message"
    $ git push origin master
``` 
 
### 나. project 폴더에서 blog로
```
    $ cd ..
    $ git add *
    $ git commit -m "your commit message"
    $ git push origin master
```

정상적으로 git push가 되었다면, http://user-id.github.io 복사 후 URL에 붙여넣기 하면 완성된 페이지를 볼 수 있을 것이다. 

## 기타 : 설치 과정에서 겪은 오류들

  가. HTML이 적용 안 될 때
- Hugo는 markdown이 기본형이나 html으로 커스터마이징이 가능하다.
- 그러나 버전에 따라 기본적으로 html 태그를 disabled 시킨 버전이 존재하므로 이 경우

config.toml에
```
    [markup.goldmark.renderer]
    unsafe= true
```

를 지정해주면 된다

 나. 글의 포맷을 바꾸고 싶을 때
- Hugo는 테마에서 제공되는 archetypes(기본 글 구조)를 사이트에 적용시키므로
- 글의 포맷을 바꾸고 싶을 때는 archetypes/default.md를 수정하면 된다.

``` 
    ---
    title: "{{ replace .Name "-" " " | title }}"
    date: {{ .Date }}
    categories: []
    tags: []
    cover: ""
    draft: true
    ---

```

### 참고 :
      https://ialy1595.github.io/post/blog-construct-1/
      https://gohugo.io/getting-started/quick-start/
      https://github.com/Integerous/Integerous.github.io
      https://medium.com/deliverytechkorea/github-pages%EB%A5%BC-%EC%9D%B4%EC%9A%A9%ED%95%9C-%EA%B8%B0%EC%88%A0-%EB%B8%94%EB%A1%9C%EA%B7%B8-%EC%A0%9C%EC%9E%91-%ED%9B%84%EA%B8%B0-77ce4b5e5564
      https://cozydatascientist.tistory.com/78
      https://cozydatascientist.tistory.com/79?category=795512
