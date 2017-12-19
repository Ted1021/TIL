# 1. MVP 란 ?

- 모댈과 뷰의 종속성을 끊어 내는것
- 각 개체마다 인터페이스를 가진다 (서로간의 종속성을 최소화 하기 위함)
- todo-mvp
- droid MVP - view 는 최대한 멍청하게만들자 ㅋㅋ



# 2. MVVM 이란?

- model, view, viewModel 의 약자
- view model 은 view 에 대해 전혀 모르는 상태, 그저 데이터가 업데이트 되었음만 알린다
- view 쪽은 java 코드를 작성하지 않는다 (databinding / data notify 를 이용)
- view 가 생성될 때 초기화 하는 방법
- @bindable : get 메소드 위에 해당 어노테이션을 적용시키면 static 한 xml 파일에 값을 동적으로 변화 시킬 수 있음
- Class:ObservableField<T> - 해당 클래스를 사용하면 값이 변경되었을때 notify 호출
- Event Listener - 기본 xml 클릭리스너 옵션에 view model 의 이벤트 함수를 파라미터로 넣어준다
  - view 말고 특정 데이터만 깔끔하게 전달 할 수 있다
- 리턴 형식을 반드시 지켜 줘야함. 데이터바인딩은 에러를 잘 찾을 수 없으므로 조심해야함
- viewStub - 처음에 아예 화면에 inflate 가 되지 않음 (gone 조차도 되지 않음)



# 3. Data binding

- 간단하게는 버터나이프의 역할을 하게 됨
- <layout>
- <data> + 바인딩 방법 android:text =@{tag.(원하는필드값)}


- mvvm 은view model 은 view 에대한 인스턴스를 가지고 있지 않는것이 핵심