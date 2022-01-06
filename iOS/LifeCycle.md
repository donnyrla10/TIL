
# ViewController Life Cycle

단일 스크린 위에 여러 ViewController로 화면 전환이 가능하도록 한다. 앱이 복잡해질수록 ViewController를 잘 관리해야 한다. UIViewController에는 view 관리 메소드가 있다. iOS system에 의해 자동 호출된다. 
ViewController의 High Class를 생성할 때, 정의된 메소드들을 overriding 해서 life cycle 상황에 맞게 적절한 로직을 추가할 수 있다.

> 시스템이 어떤 메소드들을 언제 호출할지를 먼저 이해해야 그 시점에 맞춰 UI 변화나 data 변화를 잘 처리할 수 있다.



![LC](https://user-images.githubusercontent.com/63290629/148335104-ecb61e4e-4465-496c-a6e3-680d73f06d77.JPG)



**view의 상태**  
  Appearing / Disappearing / Appeared / Disappeared

**view의 관계**  
  DidAppear / DidDisappear / WillAppear / WillDisappear


### view의 상태 변화 감지 메소드

- `viewDidLoad`: 모든 뷰가 메모리에 로드되었을 때, 호출되는 메소드. 1회 호출되며 주로 뷰의 초기화 작업 담당

- `viewWillAppear`: 뷰가 뷰 계층에 추가되고 화면에 표시되기 직전에 호출되는 메소드. 화면에 새로 올라올 때마다 수행
- `viewDidAppear`: 뷰가 뷰 계층에 추가되고 화면에 표시되면 호출되는 메소드. view 보여줄 때 필요한 추가 작업 (+ 애니메이션 시작 작업)
- `viewWillDisappear`: 뷰가 뷰 계층에서 사라지기 직전에 호출되는 메소드. 생성 후 했던 행동 되돌리는 작업, 정보 삭제 전 저장 작업 (최조 데이터 저장 작업)
- `viewDidDisappear`: 뷰가 뷰 계층에서 사라진 후에 호출되는 메소드. 뷰를 숨기는 것과 관련된 추가 작업


---


### 화면 전환 예시로 보는 메소드 호출

- 상태 변화 `Show` → 첫 화면의 상태
  - 화면 시작 : `viewDidLoad()` → `viewWillAppear()` → `viewDidAppear()`  
  
  - 화면 전환 : `viewWillDisappear()` → `viewDidDisappear()`

  - 다시 화면으로 돌아옴 : `viewWillAppear()` → `viewDidAppear()`


- 상태 변화 `Modal` → present할 때 기존 뷰가 사라지는 것이 아니라 그 위로 다른 뷰가 보여지는 것
  - 화면 시작 : `viewDidLoad()` → `viewWillAppear()` → `viewDidAppear()`

  - 화면 전환 : `X`