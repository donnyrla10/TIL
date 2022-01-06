# ViewController

앱의 근간. 화면 하나를 관리하는 단위
- 데이터 변화에 따라 `view` 콘텐츠 업데이트
- `view`와 함께 사용자 상호작용에 응답
- 전체적인 인터페이스 `layout` 관리
- 다른 `ViewController`와 함께 앱 구성

`ViewController.swift` 와 연결되어 이 파일에서 `ViewController`의 동작 제어 코드를 작성한다.

`ViewController`는 크게 두 가지 역할을 한다.


## 1. `Content ViewController`

모든 뷰를 단독으로 관리. 화면을 구성하는 뷰를 직접 구현하고 관련된 이벤트를 처리한다. `storyboard` 생성 시, 기본으로 생성되고 `navigation stack`에 있는 `view`를 보여주는 `ViewController`
→ `UIViewController, UITableViewController, UICollectionViewController` 등

## 2. `Container ViewController`

`ViewController` 간에 부모 - 자식 관계를 형성하고 자식을 관리한다. (참조)
content 관리하는 것이 아니라! `root view`만 관리해 `container design`에 따라 크기를 조정한다. 
→ `Layout`과 화면 전환을 담당한다. 화면 구성과 이벤트 관리는 `'Child ViewController'`에서 한다.
→ `UINavigationController, UITabBarController, UIPageViewController` 등

---


## 🍎 Container ViewController는 왜 필요할까?

***Navigation 로직을 분리해 단일 책임 원칙을 지키도록***

하나의 `ViewController`가 너무 많은 역할(책임)을 하면 유지보수와 테스트가 어려워진다. `child ViewController`를 만들어서 그 곳으로 넘기면 역할(책임)을 분산할 수 있다.

예를 들어, 

```swift
@IBAction func pushButton(_ sender: Any){
    //childVC 생성 코드

    navigationController?.pushViewController(childvc, animated: true)
}
```

→ 자신(`ViewController`)이 화면 처리 메소드를 직접 호출하는 것이 아니라 `navigationController`에게 화면에 표시할 `ViewController(childvc)`을 전달해 트랜지션과 화면 표시를 맡긴다. 


***View 자체 구역을 분리해 유지보수 용이하도록***

<img alt="viewController" src="https://user-images.githubusercontent.com/63290629/148337840-b735c21f-5616-4551-806d-3be6085d71cf.png">

`Container ViewController`의 `root view`를 구역으로 나눠서 구역별 특성에 맞게 각각의 `View Controller`를 두고 따로 관리하면 책임도 분리되고 유지보수도 좀 더 쉬워진다. 위의 이미지는 `UIKit`이 제공하는 `UISplitViewController`로, `root view`를 두 개의 구역(우측, 좌측)으로 나눠 구역별로 다른 책임을 가지는 `child ViewController`를 관리한다.

✚ `Contrainer ViewController`는 하나의 개녀믕로, 기존에 존재하는 클래스를 이용해 Custom 구현
→ 기존에 있던 `UINavigationController, UITabBarController`와 같은 `System Container View Controller` 사용할 수 있다. 이 클래스들은 자신들만의 방식으로 `child ViewController`를 관리한다. 특성에 따라 용도에 맞게 사용하면 되고, 인터페이스를 구성하는 것은 개발자의 몫이다.