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

## Stack Class 구현하기
준비물
- Constructor
- Push
- Pop
- Top
- Empty
- Size
  
### *Array Implementation*
```cpp
template <class Item>
class stack{
    item data[CAPACITY];
    size_t used;
}
```

Constructor
```cpp
template <class Item>
stack<Item>::stack(){
    used=0;
}
```

Push
```cpp
template <class Item>
void stack<Item>::push(const item& entry){
    assert(size()<CAPACITY);
    data[used]=entry;
    used++;
}
```

Pop
```cpp
template <class Item>
void stack<Item>::pop(){
    assert(!empty());
    used--;
}
```

Top
```cpp
template <class Item>
Item stack<Item>::top() const{
    assert(!empty());
    return data[used-1];
}
```

Empty
```cpp
template <class Item>
bool stack<Item>::empty() const{
    return used==0;
}
```

Size
```cpp
template <class Item>
size_t stack<Item>::size() const{
    return used;
}
```
<br>

### *Linked List Implementation*
> can grow or shrink during execution  
> head_ptr == top

Class Definition
```cpp
template<class Item>
class stack{
    node<Item> *top_ptr;  // == head_ptr
}
```
Copy constructor
```cpp
template<class Item>
stack<Item>::stack(const stack<Item>& source){
    node<Item> *tail_ptr;
    list_copy(source.top_ptr, top_ptr, tail_ptr);
}
```
Push
```cpp
template<class Item>
void stack<Item>::push(const item& entry){
    list_head_insert(top_ptr, entry);
}
```
Pop
```cpp
template<class Item>
void stack<Item>::pop(){
    assert(!empty());
    list_head_remove(top_ptr);
}
```
Empty
```cpp
template<class Item>
bool satck<Item>::empty() const{
    return top_ptr==NULL;
}
```

Top
```cpp
template<class Item>
Item stack<Item>::top() const{
    assert(!empty());
    return top_ptr->data();
}
```

## More Complex Stack Applications  
Arithmetic Expressions:
```
1. infix    :  a + b 
2. prefix   :  + a b
3. postfix  :  a b +  
```

#### postfix 계산하기  
> 3 2 + 7 *

1. stack<double> number;
2. if(number) number.push(n);
3. else if(operator) number에서 두개의 숫자를 빼서 계산
4. 계산 결과를 number에 다시 push


주어진 infix를 postfix notation으로 변환하기  
//현재는 모든 수식이 괄호가 되어있다는 가정하에 진행한다.
1. '('는 stack에 넣는다
2. 숫자는 출력한다.
3. operator도 stack에 넣는다
4. ')'이면 '('가 나올때 까지 stack을 뺀다.

//이제 모든 operation이 괄호가 되어있는 것이 아닐때.
-> 연산자 우선규칙을 따라야 한다.
자신보다 연산자 우선순위가 높다면 빼고, 낮거나 같으면 빼지않는다.
