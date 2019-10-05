# Container Classes

### Container Class
> ###  요소들을 저장할 수 있는 데이터 타입
컨테이너 클래스들은 요소들을 추가, 삭제, 조사 하는 멤버함수들과 함께 클래스에 정의되어 있다.

### Bag

- 가방의 초기상태
- 가방에 숫자 넣기 (같은 숫자 여러개 넣는것도 가능함)
- 가방 조사하기 (4가 있니? -> 웅 4는 2개 있어)
- 가방에서 숫자 지우기 (한번에 한숫자만 가능)
- 가방안에 몇개가 있는지 조사하기

```cpp
typedef int value_type;

class bag{

    int data[CAPACITY];
    size_t used; //트래킹 하기 위한 변수

    public:
        static const size_t CAPACITY = 20;
        bag();

        void insert(const value_type& new_entry);
        /* precondition : 가방은 꽉자있지 않다. */
        /* postcondition : 가방에 new_entry가 추가된다. */

        std::size_t size() const;
        /* postcondition : 가방 안에 있는 정수 개수를 반환한다. */

        void remove(const value_type& target);
        /* postcondition : 타겟이 가방안에 있으면 삭제, 아니면 변하지 않는다. */
}
```
```cpp

void bag::insert(const value_type& new_entry){
    assert(size()<CAPACITY);
    data[used++]=new_entry;
}

std::size_t bag::size() const{

}

void bag::remove(const value_type& target){
    size_type index=0;
    while((index<used) && (data[index]!=target)) ++idex;
    if (inex == used) return false;
    --used;
    data[index]= data[used];
    return true;
}


void bag::operator+= (const bag& addend){
    assert(size() + addend.size() <= CAPACITY);
    copy(addend.data, addend.data + addend.used, data+used);
    /* (Beginning location, Ending location(not copied), Destination) */
    used += addend.used;
}

```
<br>


# 요약
  - `container class`는 `collection item을 보관할 수 있는 클래스 이다.
  - 컨테이너 클래스는 C++의 클래스로 구현될 수 있다.
  - 클래스는 `헤더파일`(doc와 class definition) 과 `구현파일`(멤버함수의 구현을 포함)  로 구현되었다.


Quiz 풀어보기

