# 시멘틱

UI를 설명하는 트리 구조의 Composable과 마찬가지로    
**접근성** 서비스와 **테스트** 프레임워크에서 이해할 수 있는 대체 방식으로 UI를 설명하는 트리 

Foundation 혹은 Material 라이브러리의 Composable과 Modifier로 구성되어 있다면 자동으로 채워지고 생성     
그러나 맞춤 하위 수준 컴포저블을 추가할 때는 시멘틱을 수동으로 제공해야 함    

### 시멘틱 속성
시멘틱 트리의 노드에는 상응하는 Composable의 의미를 전달하는 속성이 포함되어 있음    
시멘틱 트리를 시각화하려면 ```Layout Inspector``` 도구 혹은 ```printToLog()``` 함수를 사용

### 병합된 시맨틱 트리와 병합되지 않은 시맨틱 트리
```Modifier.semantics (mergeDescendants = true) {}```     
화면에 표시되는 내용의 정확한 의미를 전달하기 위해 노드의 특정 하위 트리를 병합하여 하나로 처리하는 것도 유용함    
이를 통해 전체로서 일련의 노드를 추론 가능    

### 트리 검사
병합되지 않는 트리를 인쇄하려면 ```useUnmergedTree``` 속성을 사용   
병합에 포함되지 않게 하려면 ```mergeDescendants``` 속성을 사용    
