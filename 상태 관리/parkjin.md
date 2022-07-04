# 상태 및 Jetpack Compose

Compose는 ```TextField```와 같이 명령형 XML 기반 뷰와 다르게 자체적으로 자동 업데이트 되지 않음.

### remember
- Composable의 상태를 저장하기 위해 ```remeber```를 사용하여 메모리에 저장 가능.
- ```remeber``` 객체를 컴포지션에 저장하고, ```remeber```를 호출한 컴포저블이 컴포지션에서 삭제되면 그 객체를 잊음.
- ```rememberSaveable```은 ```Bundle```에 저장할 수 있는 모든 값을 자동으로 저장함.

### 선언 방법 
- ```val mutableState = remember { mutableStateOf(default) }```
- ```var value by remember { mutableStateOf(default) }```
- ```val (value, setValue) = remember { mutableStateOf(default) }```

### Stateful, Stateless
- Composable 내부에 state를 지니고 있으면 Stateful
- Composable 내부에 State가 존재하지 않으면 Stateless

### Stateful
- 호출하는 곳에서 상태를 관리하지 않아도 되어 편리함.
- 재사용이 힘들고, 테스트 하기 힘듦.

### 상태 호이스팅 
- Stateless로 만들기 위해 상태를 컴포저블의 호출자로 옮기는 패턴
- **Single source of truth**: 상태를 상위로 옮겼기 때문에 컴포저블 함수가 중복되더라도 소스 저장소는 하나만 존재함.
- **Encapsulated**: Stateful 컴포저블 상태만 변경 가능, Stateless 컴포저블 상태는 변경불가함.
- **Shareable**: 호이스팅한 상태를 여러 컴포저블과 공유가 가능함.
- **Interceptable**: Stateless 컴포저블의 호출자로 상태를 변경하기 전에 이벤트를 무시할지 결정 가능.
- **Decoupled**: Stateless의 상태는 어디에서나 저장 가능함.
