
IBOutlet, IBAction의 역할은 StoryBoard와의 연결고리를 담당한다. 변수나 함수를 정의할 때, @IBOutlet, @IBAction 키워드를 붙여서 Storyboard에서 컴포넌트와 연결이 가능하다.   
`IB`는 `Interface Builder`로, `@IB`로 시작하면 `Interface builder`와 관련된 변수 또는 함수를 의미한다. 


# Outlet 변수
스토리보드에서 추가한 객체의 내용을 변경하거나 특정한 동작을 하기 위해서 해당하는 객체에 접근하기 위한 변수. 데이터가 들어오면 해당 데이터를 저장하는 변수라고 할 수 있다. `Label`, `Button` 과 같은 객체의 데이터를 가지고 올 수 있다.

```swift
@IBOutlet var quoteLabel: UILabel!
```

- **@IBOutlet**: `Outlet` 변수의 정의, 객체를 소스코드에서 참조하기 위한 키워드. 주로 객체의 색상, 크기, 텍스트 내용 등의 속성을 제어하기 위함. 
- **var quoteLabel**: `var` 키워드는 변수를 선언할 때 사용하는 키워드, 이를 통해 변수를 선언하고 `Outlet` 변수의 이름을 입력하여 변수 선언한다.
- **UILabel!**: 변수의 타입. `Label` 객체의 변수를 선언할 때에는 해당 객체의 변수 타입을 표시한다. 

<br>

# Action 함수
동작을 정의하는 함수로, 어떤 동작을 할 수 있도록 정의하고 연결시켜주는 역할을 한다. 예를 들어, 버튼을 클릭하면 입력된 텍스트를 표시하고자 할 때, 데이터를 표시하는 함수를 만들고 해당 버튼과 연결시켜 이벤트를 처리하는 역할을 하는 함수이다. 

```swift
@IBAction func quoteButton(_ sender: UIButton){}
```

- **@IBAction**: 객체의 이벤트를 제어하기 위해 사용.
- **func quoteButton**: 함수를 선언하고 Action 함수의 이름을 입력해서 선언.
- **(_sender: UIButton)**: Action 함수가 실행될 수 있도록 이벤트를 보내는 객체를 나타낸다. 버튼 객체를 클릭했을 때, 이벤트가 발생할 수 있도록 하기 위해 UIButton 클래스 타입 선언.

<br>

---

### 🚨주의
Storyboard와의 연결을 끊을 때는 아래 사진에서 Referencing Outlets을 끊어준다.  
→ 대체로 이름을 잘못 선언했을 때 사용하게 된다.

<img width="255" alt="스크린샷 2022-01-04 오후 12 56 09" src="https://user-images.githubusercontent.com/63290629/148007635-703eec4f-fb5c-4a54-ad4a-9bcf4509f0a5.png">
