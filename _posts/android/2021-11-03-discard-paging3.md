---
title: Android Paging3 Library 를 버린 사연..
author: jaepark
date: 2021-11-03 17:28:00 +0900
categories: [Android]
tags: [paging3]
pin: false
img_path: '/assets/img'
---
이번 포스팅은 Paging Library3를 사용하다가 버린 경험을 공유 하고자 올립니다.

**페이징 라이브러리 UI를 구성할 때 Compose가 아닌 xml로 작성 되었음을 미리 알려드립니다.**

## **페이징 라이브러리는 장점이 명확해서 널리 쓰인다!**

<img alt="image_1" src="https://github.com/YoonJaePark3908/StockPortfolio/assets/54883589/6734a6a6-561c-40d9-82c6-c3503c6b89e4">

- Google에서 런칭된 JetPack 라이브러리의 하나인 Paging Library는 디자인 패턴 MVVM에 적용하기에 아주 적합합니다.
- refresh, filtering을 통한 리스트 초기화 등등 여러가지로 간편하게 사용할 수 있습니다. 
ex) pagingAdapter.refresh(), pagingSource.invalidate(), getRefreshKey 함수, filtering을 통해 리스트 정리 -> 서버에서
리스트를 가져 온다면, request parameter만 바꿔서 초기화 후 변경 등등... 
- page key 값, size 컨트롤 및 선언이 편리합니다. ex) params: LoadParams<Int>의 params.key 값을 통한 페이지 변수 컨트롤, PagingConfig(pageSize = 20,,,) 
- Configration change 같이 같은 화면에서의 data load 시 자동 캐싱 기능

## **이런 좋은 라이브러리에도 사용하면서 한계를 느꼈던 부분이 있습니다..**
저의 사이드 프로젝트 앱에서 페이징 처리는 필요한데, 리스트를 추가, 삭제, 수정하는 시나리오가 있습니다. 

<img alt="image_2" src="https://github.com/YoonJaePark3908/StockPortfolio/assets/54883589/f5970cb3-ddb7-42fa-a9dd-8c849e0cc5e7">

이러한 시나리오에서 페이징 라이브러리를 사용하면서 라이브러리의 리스트 컨트롤이 쉽지 않다는 것을 느꼈습니다. 이슈는 다음과 같습니다.
1. 리스트의 아이템을 삭제 시 스크롤이 중간에서 다시 초기화 되는 문제
2. 리스트 아이템 삭제 시 OutOfBounceException이 생기는 문제
<img alt="image_3" src="https://github.com/YoonJaePark3908/StockPortfolio/assets/54883589/b7afd5f4-199c-4966-ad3d-2c4805e8f3cd">
OutOfBounceException이 나는 시나리오는 다음과 같습니다.  
① Item 2 삭제  
② 페이징 라이브러리에서 제공하는 refresh() 함수 실행  
③ item 3 삭제 요청 (OutOfBounceException 발생)  

list는 이미 item 3의 position은 1번으로 재 조정 되어있는 상태이지만, 
**Item 3의 이전 position이였던 2번 position을 삭제 요청하기 때문에  OutOfBounceException이 납니다...**  
item3을 삭제 요청하면 바뀐 position인 1번을 요청할 것이라는 저의 생각에 완전히 벗어난 동작이였습니다.

refresh()를 통해서 분명 size는 바뀌었는데 왜 요청하는 position이 바뀌지 않는걸까요..? adapter에 온갖 noti를 시도해봤지만 그 역시나 실패....

<img alt="image_4" src="https://github.com/YoonJaePark3908/StockPortfolio/assets/54883589/351840f8-bb3e-4d72-b643-164dae4db9a4">

왜지? 도대체.. 

## **결국 페이징 라이브러리를 삭제하고 RecyclerView Scroll Event를 받아서 페이징 처리하는 방법으로 우회하여 해결**
앞으로는 편집, 삭제, 추가에대한 시나리오가 있는 리스트는 페이징라이브러리 사용 안하려 합니다. 저같은 시행착오 겪지 않았으면 합니다.
추후 업데이트 및 다른 방법을 찾아서 꼭 해결 해보려 합니다. 읽어 주셔서 감사합니다.
