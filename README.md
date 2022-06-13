# 🧮 계산기 프로젝트 

> 기간: 2022-05-16 ~ 2022-05-27

## 목차

1. [프로젝트 소개](#프로젝트-소개)
2. [STEP1](#STEP-1)
3. [STEP2](#STEP-2)
4. [STEP3](#STEP-3)

## 프로젝트 소개

### 개발환경 및 라이브러리
[![swift](https://img.shields.io/badge/swift-5.6-orange)]()
[![xcode](https://img.shields.io/badge/Xcode-13.3-blue)]()

### 핵심경험  
 - TDD 시작하기
 - 기존의 프로젝트에 Test Target 추가
 - Queue 자료구조의 이해와 구현
 - List 자료구조 직접 구현해보기
 - 리스트를 활용하여 Queue 구현
 - UML을 기반으로 한 코드구현
 - 숫자와 연산자 입력에 큐 활용
 - TDD를 기반으로 코드 작성하기
 - IBOutlet / IBAction의 활용
 - 스택뷰의 활용
 - 스크롤뷰의 활용

### 기능구현

![](https://i.imgur.com/69rIa2n.gif)

### UML

![Untitled Diagram drawio (2)](https://user-images.githubusercontent.com/101521502/170734702-c5c77729-f014-46f6-83b8-32e6b386408b.png)

## STEP 1

### 고민한점 🤔

1. Array or 연결리스트 ?

이번 프로젝트에서 큐를 배열(Array)로 구성할 경우 구현이 단순하다는 장점이 있지만 dequeue과정에서 오버헤드가 발생한다는 단점이 있다는 사실을 알게 되었다. [시간복잡도: O(n)] 

반면 LinkedList는 각각 떨어진 공간에 존재하는 데이터를 참조값을 통해 연결해 놓은 것(동적할당)이라는 점에서 배열처럼 중간에 요소를 삽입 또는 삭제 시 재배치에 발생하는 오버헤드가 발생하지 않는다. [시간복잡도: O(1)]

코드의 효율성 측면에서 연결리스트로 구현

2. 양뱡향 or 단방향?

단방향 연결리스트는 원하는 데이터를 찾으려면 head부터 순회해야 하기 때문에, 모든 연결 리스트를 순회해야하는 단점이 존재한다.

이를 보완한 것이 양방향리스트 인데 단방향과 마찬가지로 head를 가지고 있고, 가장 마지막 노드를 가리키는 tail을 두고 양방향에서 탐색이 가능하게 하는 것이 양방향 연결 리스트이다.

하지만 이번 계산기 프로젝트에서는 굳이 양방향으로 구현할 필요성을 찾지 못하여 단방향으로 구현하기로 결정하였다.

### 배운개념 📚

노드를 구현할 당시 다음 데이터를 참조하여 주소값을 저장하여야 하므로 클래스를 사용하여 구현하였다. 그 다음 연결리스트와 큐를 구현할 때 구조체를 사용하여 구현하였다(클래스를 사용해야 할 이유를 찾지 못했기 때문). 하지만 이미 노드의 데이터 값이 참조타입이므로 이를 프로퍼티로 사용하는 두개의 구조체는 결과적으로는 참조타입으로 데이터를 저장하게 되어 구조체로 구현한 의미가 사라지게 된다. 그렇기 때문에 구조체와 클래스를 선택할 때 이러한 점도 염두에 둬야 한다는 것을 깨닫게 되었다.

## STEP 2

해당 UML을 보고 코드를 구현

![](https://i.imgur.com/dWmVnBd.png)

**extension String**
- split(with target: Character) -> [String] : target을 기준으로 나뉜 문자열 배열을 반환

**Operator** 

연산기호에 따른 연산 진행
- calculate(lhs: Double, rhs: Double) throws -> Double
- add(lhs: Double, rhs: Double) -> Double
- subtract(lhs: Double, rhs: Double) -> Double
- devide(lhs: Double, rhs: Double) throws -> Double
- multiply(lhs: Double, rhs: Double) -> Double

**Formula** 

연산자 큐와 피연산자 큐를 이용하여 결과 반환

- result() throws -> Double

**ExpressionParser**

- parse(from input: String) -> Formula : 연산자와 숫자에 따라 큐에 할당하는 역할
- componentsByOperators(from input: String) -> [String] : 연산자를 기준으로 식을 분리

### 고민한점 🤔

expressionParser 열거형의 역할이 무엇일가에 대한 고민을 아주 많이 했다. 고민끝에 내린 결론은 String으로 받은 연산식을 Double(숫자)과 Operator(연산기호)로 나누어 각각의 큐에 분리 해주는 역할을 한다고 결론을 내렸다. 

이 과정에서 componentsByOperators함수를 구현을 했어야 했는데, parse함수의 역할과 많이 중복되다는 생각이 들었다. 그래서componensByOperators함수 내부에서 공백으로 split만 해주는 간단한 역할만 부여 한다음 parse함수 내부에서 고차함수를 이용해 분리작업을 진행하였다. 

## STEP 3

구현한 코드를 계산기 UI와 연동하여 계산기 앱을 구현

### 배운개념 📚

**NumberFormatter**

숫자값을 문자열화 시켜주는 fomatter

```swift=
let formatter = NumberFormatter()
        formatter.numberStyle = .decimal
        formatter.minimumIntegerDigits = 1
        formatter.minimumFractionDigits = 0
        formatter.maximumFractionDigits = CalculatorString.maximumNumber
        formatter.maximumIntegerDigits = CalculatorString.maximumNumber
        
```

세자리수 마다 콤마 넣기, 소수점 처리, 자릿수 편집 등이 가능하다.

**ScrollView 구현**
```swift=
func generateStackLabels() -> (UILabel, UILabel)? {
    let validNumber = (operandLabel.text ?? CalculatorString.emptyString).replacingOccurrences(of: ",", with: CalculatorString.emptyString)
        
    guard let doubleNumber = Double(validNumber) else { return nil }
    let number = doNumberFormatter(number: doubleNumber)
        
    guard let `operator` = operatorLabel.text else { return nil }
        
    let operatorStackLabel = setLabel(`operator`)
    let numberStackLabel = setLabel(number)
        
    return (operatorStackLabel, numberStackLabel)
}
```
스택뷰에 쌓여질 레이블에 숫자와 연산기호를 할당해 준다.
```swift=
func setLabel(_ label: String) -> UILabel {
    let newLabel = UILabel()
    newLabel.textColor = .white
    newLabel.text = label
        
    return newLabel
}
```
할당한 레이블의 글자의 색을 설정해 주었다.
```swift=
func generateStack() -> UIStackView? {
    guard let (operatorStackLabel, numberStackLabel) = generateStackLabels() else { return nil }
    let stack = UIStackView()
        
    stack.axis = .horizontal
    stack.spacing = 8
        
    stack.addArrangedSubview(operatorStackLabel)
    stack.addArrangedSubview(numberStackLabel)
        
    return stack
}
```
레이블을 쌓을 스택뷰를 생성한 다음 스택에 레이블을 할당해 주었다.
```swift=
func addInputStack() {
    guard let stack = generateStack() else { return }
        
    inputStackView.addArrangedSubview(stack)
    scrollToBottom(scrollView)
}
```
생성한 스택뷰를 서브뷰에 더해줌으로 인해 실제 화면에 나타나도록 설정 하였다.
```swift=
func scrollToBottom(_ scrollView: UIScrollView) {
    scrollView.layoutIfNeeded()
    scrollView.setContentOffset(CGPoint(x: 0, y: scrollView.contentSize.height - scrollView.frame.height), animated: false)
}
```
레이블이 쌓일때마다 아래로 스크롤 되도록 설정해 주었다.

> 입력한 값이 스택뷰에 쌓인다음 즉시 업데이트 되지 않는 문제를 겪었다. 
>
>그래서 해결방법을 구글을 통해 찾게 되었는데, scrollView.layoutIfNeeded 메소드가 update cycle이 올 때까지 기다린 다음 layoutSubviews를 호출시키는 것이 아니라 즉각적으로 layoutSubviews를 발동시키는 메소드라는 사실을 알게 >되었다. 
>
>그렇기 때문에 즉시 값이 변경되어야 하는 애니매이션에서 layoutIfNeeded 메서드가 주로 사용된다길래 사용하게 되었다.
