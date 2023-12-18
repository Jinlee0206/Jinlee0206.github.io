---
layout: post
title:  "[Jekyll] HydeJack Theme 수식 적용"
categories: develop
tags: blog

toc: true
comments : true

date:   2023-12-18
last_modified_at: 2023-12-18
---

* this unordered seed list will be replaced by the toc
{:toc}


## 참고 블로그
[Airvw님의 블로그](https://airvw.github.io/github/2020-12-14-blog-mathjax/)

## Mathjax를 hydejack 테마 블로그에 적용
1. _config.yml 파일 설정  
  - _config.yml에 이미 해당 설정이 존재하기 때문에 굳이 변경할 필요가 없다고 생각해서 수정하지 않았다.

>
```yml
kramdown:
math_engine: mathjax
math_engine_opts: {}

footnote_backlink: "&#x21a9;&#xfe0e;"
```

2. _includes 디렉토리의 my-head.html파일의 마지막에 다음 내용을 추가한다.

> 
```html
<script type="text/x-mathjax-config">
    MathJax.Hub.Config({
        TeX: {
        equationNumbers: {
            autoNumber: "AMS"
        }
        },
        tex2jax: {
        inlineMath: [ ['$', '$'] ],
        displayMath: [ ['$$', '$$'] ],
        processEscapes: true,
    }
    });
    MathJax.Hub.Register.MessageHook("Math Processing Error",function (message) {
        alert("Math Processing Error: "+message[1]);
        });
    MathJax.Hub.Register.MessageHook("TeX Jax - parse error",function (message) {
        alert("Math Processing Error: "+message[1]);
        });
</script>
<script type="text/javascript" async
src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-MML-AM_CHTML">
</script>
```

3. 모든 페이지에서 수식을 허용하므로 아래 내용을 굳이 삽입하지 않음

> 
```html
{% if page.use_math %} {% include mathjax_support.html %} {% endif %}
```

## 오류 발생 부분
처음에는 수식이 적용되지 않다가 새로고침하면 적용 된다. 아시는 분은 해결 방안 좀 공유 부탁드립니다...

