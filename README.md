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
