# 암기목록

모든 container들은 다음의 코드로 전부 훑어볼 수 있겠다.
```cpp
    for(iter=actors.begin();iter!=actors.end();++iter){
        cout<<*iter<<endl;
    }
```
        
### multiset Operator
1. multiset<T>::iterator find (const value_type& target)
2. erase (iterator i)  : 이터레이터가 가리키는 곳의 값을 삭제  
3. etc ...

### iterator
- 객체가 const라면 const_iterator밖에 사용할 수 없다.  
- const iterator 용 begin과 end가 따로 있다.  

### Standard Categories of Iterators
1. Forward Iterator :   " * , ++ , == , != "
2. Bidirectional Iterator  " * , ++ , -- , == , != "
3. Random Access Iterator  p[ i ] :" * , ++ , -- , += , -= , + , - "
4. Input Iterator " 
5. Output Iterator

