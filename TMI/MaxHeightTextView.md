# MaxHeightTextView

> 텍스트뷰는 다양한방법으로 Height 를 고정하고, 스크롤이 가능하게 만들수 있다. 
>
> [<StackOverFlow> Making TextView Scrollable](https://stackoverflow.com/questions/1748977/making-textview-scrollable-on-android)



### 1. XML

```xml
android:scrollbars="vertical" // 스크롤 가능
android:requiresFadingEdge="vertical" // 스크롤의 각 엣지에 fading 옵션 추가 가능 
android:fadingEdgeLength="40dp" // fading 높이 조절가능
```



### 2. JAVA

```java
// 터치 시, 스크롤 가능 
'yourTextView'.setMovementMethod(new ScrollingMovementMethod());

// Nested Scroll 상태라면,부모뷰의 터치를 막을 수 있는 'requestDisallowInterceptTouchEvent' 를 이용
```

