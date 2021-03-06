# CustomView

<br>

## 1. 정의

> __CustomvView__ 는 크게 아래의 _세가지 종류_ 로 나뉜다.

- __기능의 확장__

  - 단순히 기존 위젯을 상속받아 기능을 ___수정___ 하거나 ___추가___ 한다

- __기능의 병합__

  - __서로 다른 두개 이상의 위젯__ 을 합쳐 ___복잡한 동작___ 을 수행 할 수 있도록 한다.

- __새로운 위젯을 생성__

  - 기존에 존재하지 않는 완전히 새로운 위젯을 생성.

  - View 를 상속받아 구현하는 것으로써, 위젯이 그려지는데에 필요한 모든 메소드 ( _ex) onMeasure(), onDraw 등_ ) 를

    직접 구현해야 하지만, 그만큼 자유로운 위젯을 생성 할 수 있음.

<br>

## 2. 구현

> 새로운 위젯을 생성하기 위해서는 아래의 __세가지 메소드__ 가 필요하다.

- __onMeasure()__
  - 커스텀 위젯의 ___"크기"___ 를 정의하는 메소드
- __onLayout()__
  - 커스텀 위젯이 부모 레이아웃의 ___"어디에"___ 위치해야 하는지를 정의하는 메소드
- __onDraw()__
  - 위치와 크기가 정해진 커스텀 위젯을 직접 ___"그리게"___ 되는 메소드

<br>

#### __1) onMeasure()__

> 커스텀 위젯이 생성 될 범위(좌표계) 를 설정하는 단계로써, 위젯의 ___가로___, ___세로___ 길이를 설정하게 된다. 
>
> 자식 위젯의 크기는 ___부모 레이아웃과의 대화___ 를 통해 이루어지며,
>
> 아래 순서의 메소드들을 통해 대화를 진행한다.

<br>

- __measure()__

  - 부모 레이아웃은 자식 뷰들을 그리기 위해 그 크기를 조사하게 되는데, 

    이때 각 자식뷰의 __measure()__ 메소드를 호출한다.

  - 하지만, measure() 메소드는 단순 크기를 정하는 메소드가 아닌

    _강제 레이아웃, 크기 변경빈도 최소화, 치명적인 에러처리_ 등의 역할을 수행한다.

  - 따라서, 실질적인 크기를 지정하는 메소드는 measure() 메소드 내에 존재하는 __onMeasure()__ 이다.

- __onMeasure()__

  - 이 메소드는 부모레이아웃이 자식 뷰에게 __여유공간을 제안__ 하는 메소드이다. (부모 -> 자식)

  - 부모는 자식에게 __공간의 크기 (widthSpec, heightSpec)__ 및 __모드__ 를 제안하게 된다.

    - ___공간의 크기___

      - widthSpec 과 heightSpec 은 각각 __크기__ 와 __모드__ 정보를 갖는다.

        이는 크기와 모드정보를 따로 두게 된다면 관리해야할 파라미터의 수가 많아지기 때문이다.

      - 따라서, *Spec 정보는 다음과 같이 관리 되고 있다. 

        ```text
        Spec (32bit) = mode (2bit) + size (30bit)
        ```

      - __View.MeasureSpec__ 클래스내의 다음과 같은 메소드들을 통해 Spec 값을 __통합 또는 분리__ 할 수 있다.

        - __int getMode(int measureSpec)__                        : 스펙으로부터 __모드정보__ 를 추출한다.
        - __int getSize(int measureSpec)__                           : 스펙으로부터 __크기정보__ 를 추출한다.
        - __int makeMeasureSpec(int size, int mode)__    : 크기와 모드정보로 __spec 인자를 생성__ 한다.

        <br>

    - ___모드___

      - __모드__ 가 부모와 자식사이에서 대화의 핵심이 된다.

      - 다음의 세가지 종류로 모드를 나눌 수 있다.

        - __AT_MOST__ : _"상한선"_ 이다. 자식뷰는 최대 특정 크기까지 커질 수 있다.

        - __EXACTLY__  : "고정선" 이다. 자식뷰는 해당크기를 정확하게 가져야한다.

        - __UNSPECIPIED__ : "무제한" 이다. 자식뷰는 원하는만큼 크기를 할당받을 수 있다.

          ​		        	_(물론 부모보다는 작아야 한다.)_

          <br>

- __setMeasuredDimension()__

  - 부모로부터 공간할당을 제안받게되면, 자식은 제안을 바탕으로 본인이 생각하는 크기를 제시한다.

  - 이 메소드는 리턴값이 없지만, 실제 동작에선 파라미터 입력을 통해

     __가로/세로__ 크기를 반환하는것처럼 동작한다. 즉, 부모에게 본인의 __최종 크기를 알리는__ 메소드이다.

    ```java
    void setMeasuredDimension(int measuredWidth, int measuredHeight)
    ```

  - ___주의해야할 점은,___ onMeasure() 메소드 내에서 __setMeasuredDimension()__ 을 __호출하지 않게되면,__

    _IllegalStateException_ 을 마주하게 된다.

  - 부모가 크기를 제안했음에도 자식의 크기를 알 수 없어 뷰를 그릴 수 없는 상태가 되기때문이다.

    따라서, 반드시 해당 메소드를 호출해 크기 지정을 마무리 해 주어야 한다.

<br>
#### 2. __onLayout()__

> onDraw() 에서 크기가 정해진 자식 뷰들의 __그려질 영역__ 을 설정하는 메소드이다.
즉, 자식뷰가 그려질 레이아웃을 설정하는 구간이다.

> 단, 레이아웃의 설정은 부모 레이아웃을 기준으로 하는것이 아니라, 어플리케이션 영역 전체를
기준으로 한 위치를 지정하게 된다.



- __onFinishInflate()__
  - xml - inflate    
