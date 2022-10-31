# CompositionLocal     

Compose에서 자주 널리 사용되는 데이터의 경우, 함수의 매개변수 종속 항목으로 전달할 필요가 없도록 CompositionLocal을 제공    
UI의 트리의 특정 노드 값과 함께 제공되며, 매개변수로 선언하지 않아도 Composable 하위 요소에서 사용 가능

### CompositionLocal 사용 여부 결정
- CompositionLocal에는 적절한 기본값이 필요
- 트리 범위 혹은 하위 계층 구조 범위로 간주되지 않는 개념에는 사용X

### CompositionLocal 만들기
- compositionLocalOf: ```current``` 값을 읽는 콘텐츠만 재구성
- staticCompositionLocalOf: ```current``` 뿐만 아니라 ```CompositionLocal```이 제공된 람다 전체가 재구성
