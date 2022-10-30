## CompositionLocal
CompositionLocal은 암시적으로 컴포지션을 통해 데이터를 전달하는 도구입니다.

### CompositionLocal 소개
일반적으로 Compose에서 데이터는 구성 가능한 함수의 매개변수로 UI 트리를 통해 아래로 흐릅니다.

색상을 대부분의 컴포저블에 명시적 매개변수 종속 항목으로 전달할 필요가 없도록 지원하기 위해 Compose는 CompositionLocal을 제공합니다.
이를 통해 UI 트리를 통해 데이터 흐름이 발생하는 암시적 방법으로 사용할 수 있는 트리 범위의 명명된 객체를 만들 수 있습니다.

CompositionLocal 요소는 일반적으로 UI 트리의 특정 노드의 값과 함께 제공됩니다.
이 값은 구성 가능한 함수에서 CompositionLocal을 매개변수로 선언하지 않아도, 구성 가능한 하위 요소에 사용할 수 있습니다.

컴포지션은 구성 가능한 함수의 호출 그래프 레코드입니다.
UI 트리나 UI 계층 구조는 컴포지션 프로그레스로 구성되고 업데이트되며 유지되는 LayoutNode의 트리입니다.

CompositionLocal은 Material 테마에서 내부적으로 사용하는 것입니다.
MaterialTheme는 나중에 컴포지션의 하위 부분에서 가져올 수 있는 세 개의 CompositionLocal 인스턴스(색상, 서체, 도형)를 제공하는 객체입니다.
이러한 인스턴스는 구체적으로 LocalColors, LocalShapes, LocalTypography 속성으로 MaterialTheme, colors, typography, shapes 속성 등을 액세스할 수 있습니다.

CompositionLocal 인스턴스는 컴포지션의 일부로 범위가 지정되므로 트리의 여러 수준에서 다양한 값을 제공할 수 있습니다.
CompositionLocal의 current 값은 컴포지션에서 범위가 지정된 부분의 상위 요소가 제공한 가장 가까운 값에 대응합니다.
새 값을 CompositionLocal에 제공하려면 CompositionLocalProvider와 CompositionLocal 키를 value에 연결하는 provides 중위 함수를 사용합니다.
CompositionLocal의 content 람다는 CompositionLocal의 current 속성에 액세스할 때 제공된 값을 제공합니다.
새 값이 제공되면 Compose는 CompositionLocal을 읽는 컴포지션의 부분을 재구성합니다.
CompositionLocal 객체나 상수에는 일반적으로 Local 접두사가 붙어 IDE에서 자동 완성 기능을 통한 검색 가능성을 개선할 수 있습니다.

### 자체 CompositionLocal 만들기
CompositionLocal 사용을 위한 또 다른 주요 신호는 매개변수가 크로스 커팅이고 구현의 중간 레이어가 그 존재를 인식해서는 안 되는 경우입니다.
CompositionLocal을 과도하게 사용하지 않는 것이 좋습니다.

CompositionLocal은 컴포저블의 동작을 추론하기 어렵게 만듭니다.
또한 이 종속 항목은 컴포지션의 모든 부분에서 변경될 수 있으므로 종속 항목에 관한 명확한 정보 소스가 없을 수도 있습니다.
따라서 문제가 발생할 때 앱을 디버깅하는 것이 더 어려울 수 있습니다.
컴포지션을 탐색하여 current 값이 제공된 위치를 확인해야 하기 때문입니다.

#### CompositionLocal 사용 여부 결정
CompositionLocal에는 적절한 기본값이 있어야 합니다.

트리 범위 또는 하위 계층 구조 범위로 간주되지 않는 개념에는 CompositionLocal을 사용하지 않습니다.
CompositionLocal은 잠재적으로 일부 하위 요소가 아닌 모든 하위 요소에서 사용할 수 있을 때 적합합니다.

이 방법이 좋지 않은 이유는 특정 UI 트리 아래의 모든 컴포저블이 ViewModel에 관해 알 필요는 없기 때문입니다.

#### CompositionLocal 만들기
compositonLocalOf: 재구성 중에 제공된 값을 변경하면 current 값을 읽는 콘텐츠만 무효화됩니다.
staticCompositionLocalOf: Compose에서 추적하지 않습니다. 값을 변경하면 컴포지션에서 current 값을 읽는 위치만이 아니라 CompositionLocal이 제공된 content 람다 전체가 재구성됩니다.

CompositionLocal에 제공된 값이 변경될 가능성이 거의 없거나 변경되지 않는다면 staticCompositionLocalOf를 사용하여 성능 이점을 얻으세요.

#### CompositionLocal에 값 제공
CompositionLocalProvider 컴포저블은 주어진 계층 구조의 CompositionLocal 인스턴스에 값을 바인딩합니다.

#### CompositionLocal 사용
CompositionLocal.current는 CompositionLocal에 값을 제공하는 가장 가까운 CompositionLocalProvider에서 제공한 값을 반환합니다.

### 고려할 대안
일부 사용 사례에서는 CompositionLocal이 지나친 솔루션일 수 있습니다.

#### 명시적 매개변수 전달
컴포저블의 종속 항목에 관해 명시적인 것이 좋습니다.
컴포저블에는 필요한 것만 전달하는 것이 좋습니다.
컴포저블의 분리와 재사용을 촉진하려면 각 컴포저블이 가능한 가장 적은 정보를 보유해야 합니다.

#### 컨트롤 역전
하위 요소가 일부 로직을 실행하는 종속 항목을 가져오지 않고 대신 상위 요소가 실행합니다.
마찬가지로 @Composable 콘텐츠 람다를 같은 방식으로 사용하여 동일한 이점을 얻을 수 있습니다.