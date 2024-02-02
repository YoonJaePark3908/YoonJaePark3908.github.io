---
title: 티스토리에서 깃허브로 이전한 이유
author: jaepark
date: 2024-01-30 14:49:00 +0900
categories: [Etc, Blog]
tags: [getting started]
pin: true
img_path: '/assets/img'
---
## 코드블럭

티스토리 블로그의 코드블럭은 개인적으로 아쉬운 마음이 많이 들었습니다. 여러가지 테마를 알아봤지만 결국 마음에 드는 테마를 찾을 수 없었습니다. 
직접 테마를 커스터마이징 하는 방법도 있지만, 그러기에는 들어가는 리소스가 많다고 판단하여 보류하면서 블로그를 이용해 왔습니다.
그러다가 여러 개발 관련 지식이 담긴 글을 읽어오면서 깃허브 블로그가 있다는것을 알게 되었고, 깃허브 블로그의 대부분의 코드 블럭 UI가 깔끔했습니다. 
그 중 가장 마음에 들었던 테마는 [**Chirpy**][chirpy] 이였고 바로 적용하였습니다.

- 티스토리 블로그 코드블럭
<img width="822" alt="tistory_code_block" src="https://github.com/YoonJaePark3908/StockPortfolio/assets/54883589/c06ab3b8-ac76-4846-8370-41c3aa51fae2">

- 깃허브 블로그 코드블럭
``` kotlin
fun pathToBitmap(path: String?): Bitmap? {
    return try {
        val f = File(path)
        val options = BitmapFactory.Options()
        options.inPreferredConfig = Bitmap.Config.ARGB_8888
        BitmapFactory.decodeStream(FileInputStream(f), null, options)
    } catch (e: Exception) {
        e.printStackTrace()
        null
    }
}
```

## 깔끔한 테마와 UI
티스토리와 비교 했을 때 [**Chirpy**][chirpy]테마는 UI 또한 제 마음에 들었습니다. 아래는 그 예시들입니다.  
<kbd>버튼 UI</kbd> `강조 Text`

> Normal type UI

> Info type UI
{: .prompt-info }

> tip type UI
{: .prompt-tip }

> warning type UI
{: .prompt-warning }

> danger type UI
{: .prompt-danger }

1. text
2. text
3. text

## 마크다운 공부
깃허브의 오픈 소스의 문서 또는 사이드 프로젝트의 문서를 작성하기 위해서 README 파일을 작성해야 하는데, 이는 마크다운으로 돼있습니다. 물론, 티스토리 블로그에도 
마크다운으로 블로그를 작성이 가능하지만 강제성은 없습니다. 하지만 깃허브 블로그는 .md 파일을 생성하여 블로그 글을 작성 해야합니다. 
이번 기회를 통해 좀 더 마크다운과 친해지는 계기가 되려고 합니다. 

[chirpy]: https://github.com/cotes2020/jekyll-theme-chirpy
