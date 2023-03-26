---
layout: post
title:  "[Jekyll] Blog 포스팅 하는 방법"
excerpt : "md 파일에 마크다운 문법으로 작성하여 Github 원격 저장소에 업로드 해보자. 에디터는 Visual Studio code 사용! 로컬 서버에서 확인도 해보자."

categories:
 - Blog
tags:
 - [Blog, Jekyll, Git, Github]

toc: true
toc_sticky : true

date:   2023-03-26
last_modified_at: 2023-03-26
---
***

# 1. 깃허브 블로그 생성
GitHub 에서 블로그 용으로 쓰일 새 Repository를 생성한 후 Ruby 설치 이후 Jekyll 테마를 이용해 블로그 테마 적용

# 2. 깃허브 페이지 서버와 연결
git add.
git commit -m "커밋 메시지"
git push -u origin main

# 3. 마크다운을 지원하는 에디터 선정
VS code 에디터로 결정

# 4. 로컬 서버에서 블로그에 적용될 모습 확인
1. 명령 프롬프트 cmd를 켜고 '깃허브계정아이디.github.io' 폴더로 이동 
2. **bundle exec jekyll serve**
3. http://127.0.0.1:4000/ 로 접속 


출처 : https://ansohxxn.github.io/blog/posting/