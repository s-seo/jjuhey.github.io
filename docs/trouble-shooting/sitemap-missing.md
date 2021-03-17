---
layout: post
title: Sitemap 필수요소 누락
parent: 오답노트(trouble shooting)
nav_order: 2
last_modified_date: 2021-03-17 11:11
lastmod: 2021-03-17 11:11
---

## Sitemap tag missing

### **Problem**
* Google Search Console에 sitemap 제출하는데, 계속 기본정보가 누락되었다고 나온다.
```
필수 태그가 누락되었습니다. 해당 태그를 추가한 후 다시 제출하십시오.
예: 4행
상위 태그: urlset
태그: url
```
* 처음엔 진짜 필수태그를 누락했는데, 그래서 다시 추가해줬다.
* 그런데 올바르게 고쳐도 똑같은 문제가 계속 되었다.

### **Solution**
* 차라리 아예 새로운 sitemap을 생성해보자고 생각해서, `sitemap2.xml`을 만들고 사이트맵에 추가했더니, 잘나온다.
* 아직도 문제가 무엇인지 정확하게 파악은 못했지만, 구글이 인식을 먼가 잘못하고 있는 느낌이다.
  * `sitemap.xml`의 경우 유형이 <span class='text-purple-000'>'Sitemap 색인'</span>으로 되어있고,
  * 성공한 `sitemap2.xml`의 경우 유형이 <span class='text-purple-000'>'Sitemap'</span>으로 되어있다.
* Possible Cause
  1. sitemap plug-in과의 충돌
  2. google에서 `sitemap.xml`을 잘못 인식