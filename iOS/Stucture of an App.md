# Stucture of an App

## 앱 실행과정

<img width="810" alt="앱 실행과정" src="https://user-images.githubusercontent.com/63290629/148333763-04c95657-90f9-46a8-95ac-56ea19f4612d.png">


1. `main` 함수 실행
2. `main` 함수에서 `UIApplicationMain` 함수 호출한다.
3. `UIApplicationMain` 함수에서 `UIApplication` 객체(앱의 본체) 생성한다.
4. `AppDelegate`를 생성한다. 
    - `iOS` 앱 내에서 오직 하나의 인스턴스만 생성됨을 보장받는다. 앱이 처음 만들어질 때 객체가 생성되고, 앱이 실행되는 동안 유지되다가 앱이 종료되면 그 때 함께 소멸하는 등 앱 전체 생명주기와 함께한다. 
    - `appliction(_ :willFinishLaunchingWitOptions:)`, `applicaiton(_ : didFinishLaundhingWithOptions:)`를 호출한다. → 해당 메소드 내에서 사용자 데이터 구조를 초기화하게 된다. 메소드 내에 작업들은 동기적으로 작동되므로, 두 메소드 안에서 오랜 작업을 수행한다면 앱을 시작하는데에 오랜 시간이 소요된다. 값이 반환된 이후 앱의 상태를 변경시킨다.
5. 메인 스토리보드와 `nib` 파일을 로드한다. 앱은 초기 `ViewController`를 찾기 위해 시도한다. 찾아서 인스턴스를 생성한다.
6. `SceneDelegate`를 생성한다. `Scene`은 디바이스에서 실행되고 있는 앱의 `UI` 인스턴스를 나타낸다. 사용자 스크린에 `UI`를 나타내기 위한 `window`, `view`, `viewcontroller`들을 관리하기 위해 `scene` 객체를 사용한다는 것이다. `UI` 인스턴스 생성은 제공받은 정보에 의존(`Info.plist`)
7. `Window` 객체 생성 → `ViewController`의 인스턴스를 `window`의 `RootViewController`로 할당한다.
8. `ViewController`의 `view`가 로드된다. 


> `UIApplicationMain` 함수는 앱의 시작점(`entry point`)이며 앱 실행을 위한 주요 객체들을 생성하는 함수. 


### ☃️ 정리
앱 시작: `UIApplicationMain` 함수 → 중요 객체 생성, 앱 실행  
모든 `iOS` 앱 중심에는 시스템과 여러 객체간의 대화를 가능하게 해주는 `UIApplication` 객체가 있다. 
대체로 `MVC` 구조를 사용한다 → design pattern으로, 앱의 `data`와 `business logic`을 `UI`요소로부터 분리한다. (서로 다른 디바이스에서도 같은 동작 가능)

---

## UIApplicationMain Function

`Cocoa Touch Framework`에서 앱 `Life Cycle`을 시작하는 함수. 
→ `UIApplication` 객체를 만들고, 해당 객체의 앱으로 기능하기 위한 기반 마련하는 과정 = `app loading process`

AppDelegate.swift 파일 맨 위에 @UIApplicationMain 어노테이션이 붙는다.   
만약 main을 다루고 싶으면 main.swift를 만들어서 사용하자. (어노테이션 지워야 됨)

```swift
public func UIApplicationMain(_ 
    argc: Int32, argv: UnsafeMutablePointer<UnsafeMutablePointer<Int8>>!,
    principalClassName: String?,
    delegateClassName: String?) -> Int32
}
```
- argc, argv: shell에서 프로그램 실행을 할 때, 프로그램 실행 명령어, 인자값
- principalClassName: UIApplication 클래스를 서브 클래싱한 경우, 해당 클래스 이름 전달
- delegateClassName: AppDelegate 클래스 이름

→ main에서 실행되는 UIApplicationMain 함수는 앱에 몇 가지 중요 객체를 생성하고 storyboard에서 ui 로딩하고 앱의 초기 세팅값(Info.plist)을 불러오고 앱을 run loop 에 올려놓으며 함수를 진행한다.

---

## The Main Run Loop

→ 앱의 `main thread`에서 실행된다.   
`main run loop`는 사용자 관련 이벤트를 받은 순서대로 처리한다. (이벤트 처리 동작에 대한 구조) `UIApplication` 객체는 앱이 실행될 때, `main run loop`을 실행하고 이 `loop`로 이벤트르를 처리한다.

![TheMainRunLoop](https://user-images.githubusercontent.com/63290629/148332238-e7324f35-ba35-480e-9bd9-bd3b12931828.JPG)

유저가 디바이스에서 특정 액션을 취하면, 그 액션에 해당하는 이벤트가 시스템(OS)에 의해 생성되어 `UIKit`에서 생성한 port를 통해 앱에 전당한다. 전달된 이벤트는 `queue`에 보관되고 하나씩 `main run loop`에 전달되어 처리된다. 

---

## 앱의 상태 변화

![앱의상태변화](https://user-images.githubusercontent.com/63290629/148332498-47c3019e-cf19-4cea-8ec7-668e18e6beb4.JPG)


- `Not Running`: 실행되지 않았거나 시스템에 의해 종료된 상태
- `Inactive`: 실행 중이지만 이벤트를 받지 않는 상태.   
**ex) 실행 중 미리 알림 또는 일정 alert이 화면에 덮여 앱이 실질적으로 이벤트를 받지 않는 상태**
- `Active`: 앱 실행 (활동 중)
- `Background`: 백그라운드에서 실질적으로 동작하는 상태   
**ex) 백그라운드 음악재생, 트랙킹 등**
- `Suspended`: 백그라운드에서 활동 멈춤 상태. 빠른 재실행을 위해 메모리 적재된 상태지만 실질적 동작은 하지 않는다. 메모리가 부족하면 강제 종료된다.


🍒 상태전환은 `AppDelegate의` 메소드 호출을 거친다.  
→ `UIResponder`, `UIApplicationDelegate` 상속 및 `Delegate` 참조


### 💡 UIApplicationDelegate: UIApplication 객체 작업에 개발자가 접근할 수 있도록 메소드를 담고 있다.

- `application: willFinishLaunchingWithOptions`: 앱이 최초 실행될 때
- `application: didFinishLaunchingWithOptions`: 앱 실행된 직후 사용자의 화면에 보여지기 직전
- `applicationDidBecomeActive`: 앱이 Active 상태로 전환된 직후
- `applicationWillResignActive`: 앱이 Inactive 상태로 전환된 직전
- `applicationDidEnterBackground`: 앱이 백그라운드 상태로 전환된 직후
- `applicationWillEnterForeground`: 앱이 Active 상태되기 직전에, 화면에 보여지기 직전
- `applicationWillTerminate`: 앱이 종료되기 직전


---


## 앱 실행 (Foreground, Background)

앱 시작: 시스템은 process, main thread, main 함수 생성
main에서 UIKit Framework를 즉시 다룰수 있고, 앱 초기화 && 실행 준비

![앱 전면 실행](https://user-images.githubusercontent.com/63290629/148333372-2bab6feb-9aa5-41bb-bc60-bb820d4b46cb.JPG)

![앱 후면 실행](https://user-images.githubusercontent.com/63290629/148333442-2e26595c-3c10-4b88-981f-e0d5563f3383.JPG)