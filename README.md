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
