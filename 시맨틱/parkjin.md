# 시멘틱

UI를 설명하는 트리 구조의 Composable과 마찬가지로    
**접근성** 서비스와 **테스트** 프레임워크에서 이해할 수 있는 대체 방식으로 UI를 설명하는 트리 

Foundation 혹은 Material 라이브러리의 Composable과 Modifier로 구성되어 있다면 자동으로 채워지고 생성     
그러나 맞춤 하위 수준 컴포저블을 추가할 때는 시멘틱을 수동으로 제공해야 함    

### 시멘틱 속성
시멘틱 트리의 노드에는 상응하는 Composable의 의미를 전달하는 속성이 포함되어 있음    
시멘틱 트리를 시각화하려면 ```Layout Inspector``` 도구 혹은 ```printToLog()``` 함수를 사용


