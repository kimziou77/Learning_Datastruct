# Stack
> 순서가 있는 entry에 대한 자료구조로, 삽입과 삭제가 한쪽에서 밖에 없다.  
> **Last-In-First-Out (LIFO)**

### STL
- pop()
- push(const item& entry)
- empty()
- size()
- top()

#### Stack's Error
1. stack overflow
2. stack underflow

### Applications

**Balanced Parentheses : 연산자의 우선순위를 바꿔줌**
1. expression에서 다음 char을 하나 읽어들인다.
2. 괄호가 아니면, 다시 1로 간다.
3. '(' 라면 stack에 넣는다.
4. ')' 라면 스택에서 '('를 꺼낸다.
5. expression의 끝에 도달할 때까지 반복한다.
6. 이때 스택이 비어있다면 괄호가 balanced 하게 잘 쳐져 있다는 뜻이다.

**Evaluating Arithmetic Expressions**  
input이 fully parenthesized numeric일때를 가정해보자.  
모든 연산에는 괄호가 있어서 그 괄호로 계산을 한다.  
> 스택 두개를 사용한다. 1 (operands Stack) , 2(operator Stack)  
> '(' 'operator' 'operands' ')' 4가지를 고려한다.

1. 왼쪽->오른쪽으로 검사한다.
2. '('는 무시한다 
3. '숫자'는 operands Stack에 넣는다.
4. '연산자'는 operator stack에 넣는다.
5. ')'가 나오면 action을 취한다 : 계산을 한다.
6. 76p임 정리해서 작성할 것