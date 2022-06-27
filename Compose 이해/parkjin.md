### 기존 View의 단점
- getter와 setter를 노출하여 상태에 따라 수동으로 View를 설정
- ```textView.text = ...```, ```recyclerView.adapter = ...``` View에 직접 접근하여 수동으로 내부 상태를 변경함.
- 수동으로 변경하면, View의 상태를 업데이트 하는 것을 잊어버리거나 예기치 못한 상황이 발생할 수 있음.

### Compose UI의 장점
- getter와 setter를 노출하지 않고, 상태에 따라 자동으로 View 설정
- 화면 전체를 생성 후, 필요한 변경사항만 적용할 수 있도록 작동함.
- 데이터에 따라 View를 자동으로 업데이트하여 View의 복잡성을 방지할 수 있음

    
``` kotlin
@Composable
fun Greeting(names: List<String>) {
    for (name in names) {
        Text("Hello $name")
    }
}
```
- 기존 Kotlin의 문법과 Compose를 유연하게 사용하여 View를 구성 가능함.

### 구성 가능한 함수는 동시에 실행할 수 있음
- Composable 함수는 IO 스레드에서 동작하여 동시에 실행될 수 있음.
- 다만, 부작용이 있다면 UI 스레드에서 실행되는 ```onClick```과 같은 Callback에서 문제가 생길 수 있음

```kotlin
@Composable
@Deprecated("Example with bug")
fun ListWithBug(myList: List<String>) {
    var items = 0

    Row(horizontalArrangement = Arrangement.SpaceBetween) {
        Column {
            for (item in myList) {
                Text("Item: $item")
                items++ // Avoid! Side-effect of the column recomposing.
            }
        }
        Text("Count: $items")
    }
}
