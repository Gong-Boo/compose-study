# Compose의 부수효과
함수 밖에서 발생하는 상태에 관한 변경사항.    
부수 효과가 없는 것이 좋지만, 특정 상태나 조건에 따라 UI를 표시하는 이벤트를 트리거 하는 경우엔 필요.

## Effect API

### LaunchedEffect: Composable 내에서 Suspend 함수 실행
- LaunchedEffect가 컴포지션을 시작하면 코루틴이 실행됨, 컴포지션을 종료하면 코루틴이 취소됨.
- LaunchedEffect가 다른 키로 리컴포지션되면 새 코루틴에서 새 suspend 함수가 실행됨.

### rememberCoroutineScope: Composable 외부에서 코루틴 실행
- Composable 외부에서 컴포지션을 종료한 후 자동으로 취소되도록 범위가 지정된 코루틴을 실행 가능함.
- 하나 이상의 코루틴 수명주기를 수동으로 관리해야 할 때마다 사용.

### rememberUpdatedState: 값이 변경되는 경우 다시 시작되지 않아야 하는 효과에서 값 참조
 - 매개변수가 변경되면 LaunchedEffect가 다시 시작되지만, rememberUpdatedState를 사용하여 다시 시작하지 않도록 캡처가 가능함.

### DisposableEffect: 정리가 필요한 효과
- 키가 변경되거나 Composable이 컴포지션을 종료한 후 정리해야 하는 부수효과의 경우 사용.

### SideEffect: Compose 상태를 Compose가 아닌 코드에 게시
- Compose의 상태를 Compose에서 관리하는 않는 객체와 공유하기 위해 사용.
- 리컴포지션 성공 시마다 호출됨.

### produceState: Compos가 아닌 상태를 Compose 상태로 변환
- Compose가 아닌 Flow, RxJava, LiveData와 같은 구동 기반 상태를 컴포지션 형태로 변환하기 위해 사용.
- 컴포지션을 시작하면 실행되고, 컴포지션을 종료하면 취소됨.
- 동일한 값을 설정하면 트리거되지 않음.

```kotlin
@Composable
fun <T> produceState(
    initialValue: T,
    key1: Any?,
    key2: Any?,
    @BuilderInference producer: suspend ProduceStateScope<T>.() -> Unit
): State<T> {
    val result = remember { mutableStateOf(initialValue) }
    LaunchedEffect(key1, key2) {
        ProduceStateScopeImpl(result, coroutineContext).producer()
    }
    return result
}
```
- 내부적으로 LaunchedEffect, remember, state를 이용하여 result를 업데이트 함.

### derivedStateOf: 하나 이상의 상태 객체를 다른 상태로 변환
- 다른 상태 객체에서 특정 상태가 계산되거나 파생되는 경우 사용.
- 계산에서 사용되는 상태 중 하나가 변경되더라도 계산이 실행됨.

### snapshotFlow: State를 Flow로 변환
- snapshotFlow 블록 내에서 읽은 State 객체가 변경되면 기존값과 다를 경우 새 값을 방출함.

### 기타
- 반환 타입이 있는 Composable 함수: 대문자로 시작
- 반환 타입이 없는 Composable 함수: 소문자로 시작
