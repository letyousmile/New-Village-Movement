## LifeCycle

> ### Activity LifeCycle
>
> onCreate -> onStart -> onResume -> onPause -> onStop -> onDestroy 
> - onCreate
>   - **Activity**가 처음 생성 될때 실행되며, 최초 **한번**만 실행된다. **View생성**, **View Resourse Bind** 등의 일을 하며 **Budle을 통해 이전 상태의 데이터들을 저장**하고 있다.
> - onStart
>   - **Activity**가 사용자에게 보여주기 직전에 실행된다. **보통 방송수신자를 이부분에서 등록**한다.
> - onResume
>   - **Activity**가 사용자에게 포커싱되었을때 실행된다.  사용자에게 화면이 보여지는 상태
> - onPause
>   - 사용자에게 화면이 안보이는 상태, 다른 **Activity**가 실행되어 현재 실행중이던 **Activity**의 화면이 안보이거나 일부분만 노출, 투명화되면 호출된다. 이부분에서 **작업을 많이 부여하면 다른 Activity가 실행되는데 지연**된다. **영구적인 Data를 저장하는 부분**이다.
> - onStop
>   - **Activity**가 다른 **Activity**에 의해 완전히 가려져서 안보일경우 실행된다. 홈키를 누르경우에도 실행된다. 다시 **Activity**가 화면에 불려지면 **onRestart**가 실행된다.
> - onDestroy
>   - **Activity**가 완전히 스택에서 제거 되었을경우 실행된다. **finish()**가 실행되거나 메모리를 관리해야할 경우 실행된다.
>
>  **onStop**는 프로세스에 의해 강제종료되었을 경우 실행되지 않을수도 있다.(메모리부족때문)
>
> ### Fragment LifeCycle
>
> onAttach -> onCreate -> onCreateView -> onActivityCreated -> onStart -> onResume -> onPause -> onStop -> onDestroyView -> onDestroy -> onDetach
> - onAttach
>   - Fragment를 Activity에 붙히는 부분, Activity의 Context가 매개변수로 넘어온다. Activity와 Fragment간의 통신을 위해 Listener를 구현할때 넘어온 Context를 활용하면 된다.
> - onCreate
>   - Activity의 onCreate와 비슷한 일을 하지만 Fragment는 이부분에서 View를 초기화 하지 않는다.  Fragment를 생성할떄 Bundle에 담아 넘긴 데이터를 이부분에서 받아온다.
> - onCreateView
>   - Layout과 Fragment를 inflate 하는 부분, 보통 이부분에서 View들도 같이 초기화 한다.
> - onActivityCreated
>    - Activity와 Fragment 모두 생성된 상태 , View들을 수정할수 있다.(?)
> - onDestroyView 
>    - Fragment와 관련된 View들 삭제하는 부분
> - onDestroy
> - onDetach
>   - Activity에서 Fragment가 분리되어서 완전히 사라지는 부분.
>
> **onStop**이 실행되기전에 저장하고 싶은 데이터를 onSavedInstanceState를 호출하여 데이터를 저장하고 , **화면이 회전했을 떄 onCreate 에서 savedInstanceState 를 통해 이전에 저장했던 데이터를 호출**해서 화면상에 불러온다. Fragment에서도 똑같이 onCreateView 나 onActityCreated 에서 사용하면 된다.