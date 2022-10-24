# 단계

### 프레임 랜더링 단계
- Composition: 표시할 UI, Composable 함수를 실행
- Layout: 레이아웃 트리 내의 각 노드의 레이아웃의 요소를 **측정**과 **배치**를 진행
- Drawing: 캔버스에 UI 랜더링

```BoxWithConstraints```, ```LazyColumn```, ```LazyRow```는 예외로 상위 요소의 레이아웃 단계에 따라 달라짐

### 상태 읽기
프레임 렌더링 단계 중에서 읽은 상태 값을 자동으로 추적하여 값이 변경될때마다 작업을 다시 실행 가능 (Recomposition)    
일반적으로는 ```mutableState```를 사용

### 단계적 상태 읽기

- Composition

  상태 값이 변경되면 ```Recomposer```는 상태 값을 읽는 모든 Composable 함수의 재실행을 예약    
  상태 값이 변경되지 않을 경우 건너뛰고, 콘텐츠가 동일하게 유지되고 크기와 레이아웃이 변경되지 않는다면 Layout 및 Drawing 단계도 건너뛸 수 있음    
  
  
- Layout

  측정에서는 측정 람다와 ```MeasureScope.measure``` 등의 메서드,    
  배치에서는 ```layout``` 함수의 배치 블록과 ```Modifier.offest```의 람다 블록 등으로 인한 영향 발생   
  
  상태 값이 변경되면 Layout 단계를 예약하고, 크기나 위치가 변경된 경우 Drawing 단계 실행         
  배치 단계에서 측정 단계를 다시 호출하지는 않음
  
- Drawing
   
  ```Canvas()```, ```Modifier.drawBehind```, ```Modifier.drawWithContent```등으로 인해 영향 발생     
  상태 값이 변경될 경우, Drawing 단계만 실행
  
  
