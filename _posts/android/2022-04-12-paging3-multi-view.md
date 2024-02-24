---
title: Android Paging3에서 멀티뷰를 구현하는 방법에 대한 연구
author: jaepark
date: 2022-04-12 16:58:00 +0900
categories: [Android]
tags: [paging3]
pin: false
img_path: '/assets/img'
---
>본문에 들어가기 앞서, 풀 소스코드 링크 공유합니다. 본 포스팅은 Paging3 라이브러리를 사용하여 리스트를 구성해본적이 있어야 쉽게 이해 하실 수 있습니다. 
>Paging3 라이브러리를 사용방법은 구글링 해보면 많이 나오니 참고하시기 바랍니다.

[**GitHub 소스 코드 링크**](https://github.com/YoonJaePark3908/AndroidLaboratory/tree/main/PagingMultiView)

일반적인 페이징에서는 다음과 같이 한 개의 뷰 타입만이 고려가 됩니다.

![image_1](/android/paging3/multiview/image_1.gif){: width="350"}

이렇게 페이징을 적용하고 그대로 간다면 좋겠지만 개발자에게는 다양한 요구사항이 들어옵니다.<br>
**현재 대전시의 관광지 리스트를 페이징 적용한 상태에서 리스트 사이 사이에 음식점 광고를 넣어달라하면 어떻게 해야할까요?**<br>
대전시 관광 리스트 API, 음식점 광고 API가 따로 있고 페이징 라이브러리가 적용된 상황에서, 멀티뷰를 다음과 같이 적용하려면 어떻게 할까 고민을 했습니다.

![image_2](/android/paging3/multiview/image_2.png){: width="350"}

**제 고민의 결론은 페이징 라이브러리에 2개의 데이터(관광지, 음식점)를 통합한 sealed interface를 하나 만들어서 보내면 될 것 같다는 생각을 했습니다.**

과정은 다음과 같습니다.
1. Paging에 적재할 두개의 데이터를 통합한 sealed interface안에 data class를 만듭니다.
```kotlin
sealed interface MainPagingModel {
	//음식점
    data class RestaurantModel(
        val idx: String = "",
        val name: String = "",
        val contents1: String = "",
        val topMenu: String = ""
    ): MainPagingModel
	//관광지
    data class TouristModel(
        val id: String = "",
        val addr1: String = "",
        val name: String = "",
    ): MainPagingModel
}
```
2. MainPagingSource에서 보낼 Model을 변경 후 사이사이에 끼워주는 알고리즘을 작성합니다.
![image_3](/android/paging3/multiview/image_3.png)<br>
2개의 데이터를 통합한 모델을 페이징 라이브러리에 적재 후 알고리즘을 적용 한 코드는 다음과 같습니다.
```kotlin
class MainPagingSource: PagingSource<Int, MainPagingModel>() {
    override suspend fun load(
      params: LoadParams<Int>
    ): LoadResult<Int, MainPagingModel> =
        try { 
            val next = params.key ?: 0
            //관광지 api를 요청했다고 가정
            val touristResponse = touristTestDataList[next] 
            //음식점 광고 api를 요청했다고 가정
            // 만약 다음 페이지에 음식점 광고 api 데이터가 없다면 찌르지 않게 설정
            val restaurantResponse = 
                if (next <= restaurantTestDataList.size - 1) {
                    restaurantTestDataList[next]
                } else {
                    listOf()
                } 
            val nextKey = if (touristTestDataList.size - 1 <= next) null else next + 1
            val pagingModelList = mappingPagingModel(touristResponse, restaurantResponse)
            LoadResult.Page(
                data = pagingModelList,
                prevKey = if (next == 0) null else next - 1,
                nextKey = nextKey
            )
        } catch (e: Exception) {
            LoadResult.Error(e)
        }

    private fun mappingPagingModel(
        touristList: List<MainPagingModel.TouristModel>,
        restaurantList: List<MainPagingModel.RestaurantModel>
    ): List<MainPagingModel> {
        val result = LinkedList<MainPagingModel>()
        for (index in 0 until 10) { //10개 임의로 고정
            if (restaurantList.isNotEmpty()) {
                result.add(
                    restaurantList[index].copy(
                        name = "${restaurantList[index].idx}번째 음식점 이름",
                        contents1 = "${restaurantList[index].idx}번째 음식점 소개 내용",
                        topMenu = "${restaurantList[index].idx}번째 음식점 인기 메뉴"
                    )
                )
            }
            if (touristList.isNotEmpty()) {
                result.add(
                    touristList[index].copy(
                        addr1 = "${touristList[index].id}번째 주소명",
                        name = "${touristList[index].id}번째 여행지 이름"
                    )
                )
            }
        }
        return result
    }
    //...
}
```
3. PagingAdapter에 ViewType을 받은 데이터로 구분하여 적재합니다.
```kotlin
override fun getItemViewType(position: Int): Int { 
  getItem(position)?.let {
    return if (it is MainPagingModel.TouristModel) VIEW_TYPE_TOURIST 
    else VIEW_TYPE_RESTAURANT
  }
  return 0
}
```

### **결과물**

![image_4](/android/paging3/multiview/image_4.gif){: width="350"}

### **상용앱 적용 사례: ifland**

![ifland](/android/paging3/multiview/ifland.gif){: width="350"}

이 외의 적재 순서를 다르게 하고싶다면 알고리즘 add 순서를 바꿔서 데이터에 넣어주면 됩니다.<br>
또한 뷰 타입이 늘어나거나 API가 늘어나도 확장을 쉽게 할 수 있습니다.


