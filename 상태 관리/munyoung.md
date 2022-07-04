# 상태 관리

## 컴포저블의 상태
- 리멤버 함수는 초기 컴포지션 중에 컴포지션에 저장되고 저장된 값은 리컴포지션 중에 반환됩니다. remember는 변경 가능한 객체뿐만 아니라 변경할 수 없는 객체를 저장하는 데 사용할 수 있습니다.
- 또한 remember는 객체를 컴포지션에 저장하고, remember를 호출한 컴포저블이 컴포지션에서 삭제되면 그 객체를 잊습니다.
- 컴포저블에서 MutableState 객체를 선언하는 데는 세 가지 방법이 있습니다.
  - val mutableState = remember { mutableStateOf(default) }
  - var value by remember { mutableStateOf(default) }
  - val (value, setValue) = remember { mutableStateOf(default) 

## 지원되는 기타 상태 유형
- 상태를 보존하기 위해 MutableState<T>을 꼭 사용할 필요는 없지만 flow, rxJava2, LiveData를 사용해도 결국 State<T>의 형태로는 만들어 주어야 한다.
  
## 상태 호이스팅
- 상태 호이스팅이란 composed에서 컴포저블을 Stateless로 만들기 위해 상태를 컴포저블의 호출자로 옮기는 패턴입니다.
- 상태 호이스팅에는 중요한 속성 으로 단일 소스 저장소, 캡슐화됨, 공유 가능함, 가로채기 가능, 분리됨이 있습니다.
- 개인적으로 상태 호이스팅을 잘못사용하면 콜백 지옥에 빠질수도 있을꺼같아 조심해서 변수를 사용해야 할꺼같습니다.
  
## Compose에서 상태 복원
- Compose에서 상태를 복원 하기 위해서는 rememberSavable을 사용하여 상태를 저장하고 복원 할수 있습니다.
- rememberSavable이 상태를 저장하고 복원 가능한 이유는 자동으로 bundle에 저장 되어 다시 화면이 생성되었을때도 복원이 가능하기 때문입니다.
- 상태를 저장하는법에는 이러한 것이 있습니다.
  - Parcelize
  - MapSaver
  - ListSaver
  
## Compose에서 상태 관리
- 각각 ViewModel, Class, Composable로 크게 나눌수 있다.
- ViewModel에서는 비지니스 로직등을 관리하며 데이터를 Composable로 보내주어 Composable에서 관측하며 UI를 다시 recomposition할수있도록 해주며
- Class에서는 Composable에서 처리하기 힘든 Ui로직 스낵바 띄우기, 네비게이션 로직등을 Composable에서 콜백형태로 이벤트를 받아 처리하며
- Composable에서는 Class에 이벤트, ViewModeld에서 state바뀌는등을 보며 UI변경등을 수행합니다.
