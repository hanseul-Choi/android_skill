# harrypotter app clone coding

참조 : https://github.com/hongbeomi/HarryPotter

<br>

## 버전 관리

그래들의 다양한 라이브러리를 이용하는 과정에서 그래들의 버전 관리를 프로젝트 수준에서 ext.versions = [] 를 이용하여 관리한다.

<br>

## 라이브러리

### 플러그인
apply plugin: 'kotlin-android-extensions' , findviewbyid를 사용하지 않고 view binding을 이용하기 위한 플러그인 + 직렬화 제공(parcelize) 
* kotlin 1.4.20 버전 이후부터는 migration되었기 때문에, 뷰 바인딩은 kotlin-kapt 플러그인으로 진행하며 직렬화는 kotlin-parcelize Plugin으로 변경한다. 

apply plugin: 'kotlin-kapt' , annotation을 제공하기위한 plugin + 뷰 바인딩 제공 <br>


### dependencies
<br>

| dependencies                                                                             | 기능                                                        |
|------------------------------------------------------------------------------------------|-------------------------------------------------------------|
| implementation "androidx.cardview:cardview:${versions.card}"                             | 카드 뷰 제공                                                |
| implementation "androidx.recyclerview:recyclerview:${versions.recyclerView}"             | 리사이클러뷰 제공                                           |
| implementation "androidx.lifecycle:lifecycle-livedata-ktx:${versions.lifecycle}"         | livedata 제공                                               |
| implementation "androidx.lifecycle:lifecycle-extensions:${versions.lifecycle}"           | 전체적인 aac를 제공                                         |
| implementation "androidx.lifecycle:lifecycle-viewmodel-ktx:${versions.lifecycle}"        | viewmodel 제공                                              |
| kapt "androidx.lifecycle:lifecycle-compiler:${versions.archLifecycle}"                   | annotaion을 자바 및 코틀린을 이용하기 위해서 사용           |
| implementation "com.squareup.retrofit2:retrofit:${versions.retrofit}"                    | retrofit 제공                                               |
| implementation "com.squareup.retrofit2:converter-gson:${versions.retrofit}"              | json변환을 위한 retrofit library                            |
| implementation "com.squareup.okhttp3:okhttp:${versions.okhttp}"                          | okhttp 이용                                                 |
| implementation "com.squareup.okhttp3:okhttp-urlconnection:${versions.okhttp}"            | cookie나 인증 이용                                          |
| implementation "com.squareup.okhttp3:logging-interceptor:${versions.okhttp}"             | network가 연결되는 상황 및 상태를 확인할 수 있음(패킷 캡쳐) |
| implementation "com.github.bumptech.glide:glide:${versions.glide}"                       | image 띄우는 glide 이용                                     |
| kapt "com.github.bumptech.glide:compiler:${versions.glide}"                              | image caching기능 이용                                      |
| implementation "com.yarolegovich:discrete-scrollview:${versions.discreteScrollview}"     | 분리 view 이용                                              |
| implementation "org.jetbrains.kotlinx:kotlinx-coroutines-core:${versions.coroutines}"    | coroutine 이용                                              |
| implementation "org.jetbrains.kotlinx:kotlinx-coroutines-android:${versions.coroutines}" | coroutine 이용                                              |
| implementation "org.koin:koin-android:${versions.koin}"                                  | koin 이용                                                   |
| implementation "org.koin:koin-android-viewmodel:${versions.koin}"                        | koin 이용                                                   |
| implementation "com.airbnb.android:lottie:${versions.lottie}"                            | 동적 이미지인 lottie 이용                                   |
| implementation "com.afollestad.material-dialogs:core:${versions.dialog}"                 | dialog 이용                                                 |

<br>

* lifecycle-extensions는 lifecycle 2.3.0버전 이후부터 deprecated되었다. 따라서, 특정 aac 부분을 직접 implementation하여 사용해야 한다.
<br>
* kapt란 자바 파일의 annotation 뿐만아니라 kotlin의 annotation을 포함하기 위해 사용되는 것으로, 보통 데이터 바인딩 시 bindingAdapter annotation을 이용하기 위해 사용된다.

<br>

## 코드 리뷰

### package
패키지의 경우, 기능에 따라 분류하는 것이 큰 프로젝트에 있어 도움이 된다. 특히 MVVM 패턴을 이용할 것이기 때문에 이를 나누는 과정도 중요하다. <br>
1. base - activity의 기본 뼈대 클래스를 저장
2. data - remote, repository, service 총 3개로 나뉘어서 관리하고 있다. remote(character list 가져옴), repository(remote를 이용하여 service 접근), service(network 부분) <br> * remote의 경우, repository와 데이터의 의존성을 느슨하게 하기 위함이다. remote의 경우 보통 서버api 등을 이용하고, local Data Source의 경우 내부의 room이나 sharedPreferences를 이용한다. 
3. di
4. extension
5. model
6. ui

<br>

### kotlin keyword
- generic : 타입을 추론하는 형식 , (보통 T 문자를 이용) ArrayList처럼 추론이 가능한 타입이 다양하게 들어갈 수 있다. <br>
- refied : generic보다 구체적인 generic이며, 일반적인 generic의 경우 타입에 직접 접근시 오류가 발생할 수 있다. 그렇지만 refied는 타입에 직접 접근이 가능하다. 또한 inline 함수에서만 사용이 가능하다. <br>
- inline : 호출부에서 컴파일할 때, 코드를 그대로 복사하며 실행하는 특성이 있음. (타고타고 들어가는 것이 아니기 때문에 성능이 좋아짐)
- by : 상속하지 않고 기존 기능을 이용하는 방식. 위임.
- companion object : 자바의 static과 비슷한 느. 다른 점은 companion object는 한 객체에 한번만 사용이 가능.
- lazy : 먼저 변수명을 선언하고 변수 값을 나중에 할당하는 방식
