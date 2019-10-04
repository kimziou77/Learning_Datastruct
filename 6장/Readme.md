# 06 장 Templates , Iterators , and STL
### Template  
클래스를 템플릿으로 만들면 멤버들은 모두 템플릿화 된다.
```cpp
    template<class item>
    bool bag<item>::insert(const item& entry) ...
```

모든 container들은 다음의 코드로 전부 훑어볼 수 있겠다.  
```cpp
    for(iter=actors.begin();iter!=actors.end();++iter){
        cout<<*iter<<endl;
    }
```
- - -     
### multiset Operator(나중에 수정할것)
1. multiset<T>::iterator find (const value_type& target)
2. erase (iterator i)  : 이터레이터가 가리키는 곳의 값을 삭제  
3. etc ...

- - -
### "operator <"
operator < 는 다음의 세가지가 잘 정의되어야 한다.
```
    1. x<x is never ture
    2. x!=y 이면 둘중 하나가 커야한다.
    3. (x<y) and (y<z), then (x<z) 를 만족 해야 한다.
```
### iterator
- 프로그래머가 쉽게 컨테이너의 모든 요소를 볼 수 있도록 허용한다.
- 객체가 const라면 const_iterator밖에 사용할 수 없다.  
- const iterator 용 begin과 end가 따로 있다.  
  


  
### Standard Categories of Iterators
1. Forward Iterator :   " * , ++ , == , != "
2. Bidirectional Iterator  : " * , ++ , -- , == , != "
3. Random Access Iterator  p[ i ] : " * , ++ , -- , += , -= , + , - , == , != "
4. Input Iterator
5. Output Iterator

```cpp
struct input_iterator_tag {};  
struct output_iterator_tag {};  
struct forward_iterator_tag : public input_iterator_tag {};  
struct bidirection_iterator_tag : public forward_iterator_tag {};  
struct random_access_iterator_tag : public     bidirection_iterator_tag {};
```

operator++ (void) : prefix (++p)  -> 얘가 좀더 빠르다
```cpp
    node_iterator& operator++ (void){
        current =current->link();
        return *this;
    }
```
operator++ ( int ) : Postfix (p++) 
```cpp
    node_iterator operator++ (int){
        node_iterator original(current);
        current = current->link();
        return original;
    }
```
Q. 왜 얘는 반환타입이 node_iterator고 위는 &이거일까..?


operator==(const node_iterator other) const;
```cpp
    operator==(const node_iterator other) const{
        return current==otehr.current;
    }
```
operator!=(const node_iterator other) const;
```cpp
    operator!=(const node_iterator other) const{
        return current!=otehr.current;
    }
```
Q. 재귀인가 이거는..? 아니면 const가 아닌 비교연산자를 불러온건가?