# Compose의 부수 효과

## 상태 및 효과 사용 사례
- 컴포저블에는 부수 효과가 없어야한다.
- 앱 상태를 변경해야 하는 경우 이러한 부수 효과가 예측 가능한 방식으로 실행되도록 Effect API를 사용해야 한다.

## LaunchedEffect: 컴포저블의 범위에서 정지 함수 실행
- 컴포저블 내에서 안전하게 정지 함수를 호출하려면 LaunchedEffect 컴포저블을 사용해야 한다.
- LaunchedEffect가 컴포지션을 시작하면 매개변수로 전달된 코드 블록으로 코루틴이 실행된다..
- LaunchedEffect가 컴포지션을 종료하면 코루틴이 취소된다.
- LaunchedEffect가 다른 키로 재구성되면 기존 코루틴이 취소되고 새 코루틴에서 새 정지 함수가 실행된다.

## rememberCoroutineScope: 컴포지션 인식 범위를 확보하여 컴포저블 외부에서 코루틴 실행
- LaunchedEffect는 구성 가능한 함수이므로 구성 가능한 다른 함수 내에서만 사용할 수 있다. 
- 컴포저블 외부에 있지만 컴포지션을 종료한 후 자동으로 취소되도록 범위가 지정된 코루틴을 실행할때 사용된다.
- 코루틴 하나 이상의 수명 주기를 수동으로 관리해야 할 때마다 사용된다.

## rememberUpdatedState: 값이 변경되는 경우 다시 시작되지 않아야 하는 효과에서 값 참조
- 주요 매개변수 중 하나가 변경되면 LaunchedEffect가 다시 시작하지만 rememberUpdatedState를 통해 이것을 막을수있다.

## DisposableEffect: 정리가 필요한 효과
- 키가 변경되거나 컴포저블이 컴포지션을 종료한 후 작업이 필요한 경우 DisposableEffect를 사용하면 된다.
- DisposableEffect 키가 변경되면 컴포저블이 현재 효과를 삭제하고 효과를 다시 호출하여 재설정해야 된다.
- 예를 들어 LifecycleObserver를 사용하여 Lifecycle 이벤트를 기반으로 애널리틱스 이벤트를 전송할 수 있습니다.

``` kotlin
DisposableEffect(lifecycleOwner) {
        val observer = LifecycleEventObserver { _, event ->
            if (event == Lifecycle.Event.ON_START) {
                currentOnStart()
            } else if (event == Lifecycle.Event.ON_STOP) {
                currentOnStop()
            }
        }
        lifecycleOwner.lifecycle.addObserver(observer)
        onDispose {
            lifecycleOwner.lifecycle.removeObserver(observer)
        }
    }
```

## 부수 효과: Compose 상태를 비 Compose 코드에 게시
- Compose 상태를 Compose에서 관리하지 않는 객체와 공유하려면 리컴포지션 성공 시마다 호출되는 SideEffect 컴포저블을 사용하면 된다.

## produceState: 비 Compose 상태를 Compose 상태로 변환
- produceState는 반환된 State로 값을 푸시할 수 있는 컴포지션으로 범위가 지정된 코루틴을 한다.
- produceState가 컴포지션을 시작하면 프로듀서가 실행되고 컴포지션을 종료하면 취소된다. 반환된 State는 되며 동일한 값을 설정해도 리컴포지션이 트리거되지 않습니다.

## derivedStateOf: 하나 이상의 상태 객체를 다른 상태로 변환
- 다른 상태 객체에서 특정 상태가 계산되거나 파생되는 경우 derivedStateOf를 사용하면된다.
- 이 함수를 사용하면 계산에서 사용되는 상태 중 하나가 변경될 때만 계산이 실행된다.

## snapshotFlow: Compose의 상태를 Flow로 변환
- snapshotFlow를 사용하여 State<T> 객체를 콜드 Flow로 변환한다.
- snapshotFlow는 수집될 때 블록을 실행하고 읽은 State 객체의 결과를 내보낸다.
- snapshotFlow 블록 내에서 읽은 State 객체의 하나가 변경되면 새 값이 이전에 내보낸 값과 같지 않은 경우 Flow에서 새 값을 수집기에 내보낸다.
