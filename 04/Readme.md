# Pointers and Dynamic Arrays

동적배열의 크기는 `RunTime` 시기에 결정된다.  
C++에서 동적배열은 `pointer`와 `동적할당`으로 구현되어있다.

## Pointer
> ### 변수의 메모리 주소

### 사용
```cpp
double * ptr;
char * c1_ptr, *c2_ptr;
 
 Q
/* char * c1_ptr, c2_ptr 이랑 같다고 하는데, 내가 저번에 해보니까 안되던 것 같았는데 어떻게 된걸까 ㅜㅠ 그냥 둘다 붙여주는게 맘편하긴 할 것 같은데 */
```

### `Dynamic Variable`  
- **not declared.** but 프로그램의 실행도중에 생성됨  

### `"new" Operator`
- 특정 데이터 타입의 메모리를 얻어와서 그 할당받은 주소값을 return 해줌  
- Pointer 변수와 같이 쓰인다

### `"delete" Operator`
- 사용중인 `Dynamic Variable`을 메모리에서 해제해준다.

- - -

## Pointers and Arrays as Parameters  
- Value parameters that are pointers
- Array parameters
- Const parameters that are pointers or arrays
- Reference parameters that are pointers
### Value parameters that are pointers
```cpp
void example(double *data, size_t n);
```
### Array parameters
```cpp
void example(double data[], size_t n);
```

### Const parameters that are pointers or arrays
```cpp
void example(const double data[], size_t n);
void example(const double *data, size_t n);
```


### Reference parameters that are pointers
```cpp
/* double *형 자료형에 p라는 이름을 붙여서 불러들여 온다. */
void allocate_doubles(double* & p, size_t &n){
    cotu<<"Enter the number of doubles: ";
    cin>>n;
    p=new double[n];
}
```

### Bag class with a Dynamic Array
기존의 `Static` 가방은 컴파일 타임에 사이즈가 결정되어 모든 가방의 사이즈가 동일하다.  
이 `Dynamic` 가방은 런타임에 사이즈가 결정되며, 각 가방마다 다른 사이즈를 가질 수 있다.

### 다이나믹 가방 만들기
#### 1. 생성자
```cpp
bag(size_type init_capacity = Default_Capapcity);
bag::bag(size_type init_capacity){
    data = new balue_type[init_capacity];
    capacity= init_capacity;
    used=0;
}
```
#### 2. 복사 생성자
```cpp
bag(const bag& source);
bag::bag(const bag& source){//deep copy_ver
    data = new value_type[source.capacity]; //data = source.data(x)
    capacity = source.capacity;
    used = source.used;
    copy(source.data, source.data + used, data);
}
```
- #### 복사생성자의 사용
    - bag y( x ); //( same as : bag y = x )
    - ***bag*** operator + (const bag & b1, const bag & b2);
    - int example (***bag*** b);

#### 3. 소멸자
동적할당된 Object를 해제시킬 의무가 있다.
```cpp
~bag();
bag::~bag() { delete [] data; }
```

#### 4. 대입연산자
가장면저 a=a 를 체크한다.  
만약 사이즈가 같지않으면, 새로운 메모리영역을 할당한다.  
새롭게 할당된 공간에 데이터를 카피한다.
```cpp
/* &를 return 하는 경우 : chaning 하기 위해서! ) */
bag& bag::operator= (const bag& source){
    value_type * new_data;
    if( this == &source ) return *this; // a = a
    if(capacity != source.capacity ){
        new_data=new value_type[source.capacity];
        delete [] data;
        data = new_data;
        capacity = source.capacity;
    }
    
}
```
#### 5. `"reserve"` function
```cpp
/* 내용물을 보존하며, 가방을 키우겠다는 느낌 */
void bag::reserve (size_type new_capacity){
    value_type * larger_array;
    if ( new_capacity == capacity ) return;
    if ( new_capacity < used ) new_capacity = used;

    larger_array = new value_type[new_capacity];
    copy(data, data + used, larger_array);

    delete [] data;
    data = larger_array;
    capacity= new_capacity;
}
```

#### 6. `"insert"` function
```cpp
void bag::insert (const value_type & entry){
    if (used == capacity) reserve(used*2);
    data[used++]=entry;
}
/* static에서 하지 못했던 기능 : 사이즈가 달랐다면 assert 했던 부분이 reserve로 대체되었다 */
```
#### 7. Operator "+"
```cpp
bag operator + (const bag& b1, const bag& b2){
    bag answer(b1.size() + b2.size());
    answer +=b1;
    answer +=b2;
    return answer;
}
```
#### 8. Operator "+="
```cpp
void bag::operator+= (const bag& addend){
    if (used + addend.used > capacity)
        reserve (used + addend.used);
    copy (addend.data, addend.data + addend.used, data+used);
    used += addend.used;
}
```

## `Member Operator` vs `NonMember Operator`
### Member Operator
> **`장점`**  
> private에 직접 접근 가능  

> **`단점`**  
> a+3은 가능하지만 3+a 불가능.  
> 주인과 주인 아님사이에 다른결과가 나오므로 원하지 않던 결과가 일어날 수 있음.
### NonMember Operator
> **`장점`**  
> 음... Member Operator의 단점을 커버할 수 있찌 ㅎ.. 

> **`단점`**  
> private에 접근할 수 없어서 네임스페이스 bag:: 이런거 써줘야됨.  



- - - 
## Q
```
static과 dynamic의 장단점을 각각 말해보자.
```
