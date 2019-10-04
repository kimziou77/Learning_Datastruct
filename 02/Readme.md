# ADT and C++ Classes
. h 파일에 다음과 같이 선언문들을 작성해준다.  
You know **how to define** a new class type, and **place** the definition in a header file.
```cpp
class ThinkingCap{
    char green_string[50];
    char red_string[50];
    public:
        void slots(char new_green[],char new_red[]);
        void push_green() const;
        void push_red() const;
}
```
cpp 파일에는 다음과 같은 함수의 body들을 적어준다.
```cpp
    void ThinkingCap::slots(char new_green[],char new_red[]){
        assert(strlen(new_green)<50);
        assert(strlen(new_red)<50);
        strcpy(green_string, new_green);
        strcpy(red_string, new_red);
    }

    void ThinkingCap::push_green(){
        cout<<green_string<<endl;
    }
```
Doc : 주석문만 보고 사용할 수 있을 정도로 작성 되어야 한다 -> ( . h )  
Class Definition : 구현코드가 들어간다. -> ( . cxx )

Quiz 풀어보기  
## 요약
- 클래스는 멤버변수와 멤버함수가 있다.
- 새로운 클래스타입을 정의하고 그 멤버함수들을 구현하는 방법과,  
  클래스타입을 사용하는 방법을 알아야 한다.  
- 대충 클래스의 멤버함수들은 종종 getter와 setter가 있다는 뜻같음  
<br><br>
- - - -

## Class와 Parameter  
<br>

### `Default Argument`  
> Constructor을 다양하게 만드는게 일반적이나, 귀찮으므로 `Default Argument`를 사용한다.
```cpp
int date_check(int year, int month=1, int day=1);
```
이렇게 된다면 다음 세가지는 모두 유효한 문장이다.
- date_check(2000, 7, 7)
- data_check(2000, 7) == data_check(2000, 7, 1)
- data_check(2000) == data_check(2000, 1, 1)


<br>


### `Value Parameter`  
> 기본적으로 전달되는 인자인것 같다  

```cpp
int example(int a, int b);
```
`Actual Parameter`의 사이즈가 **작고** , <u>값을 변경할 필요가 없을 때</u> 사용한다.

<br>



### `Reference Parameter`
> 함수body 에서 이 인자들을 사용한다면 전달 받은 값에 직접 접근 할 수 있다.
```cpp
int example (int& a, int& b);
```
`Actual Parameter`의 값을 변경해야 할 때 사용한다.

<br>

### `Const Reference Parameter`
>  `Reference Parameter`와 동일하지만, parameter가 함수호출 후에 절대로 **변하지 않는다**는 것을 보증해준다.
```cpp
int example (cosnt int& a, const int& b);
```
`Actual Parameter`의 사이즈가 **크고!** , <u>값을 변경할 필요가 없을 때</u> 사용한다. 
<br><br>

- - -

## Operator Overloading  
> Defining a new meaning for an operator

```cpp
bool operator == (const point & p1, const point & p2){
    return (p1.get_x( )==p2.get_x()) && (p1.get_y()==p2.get_y());
}
/* postcondition : p1과 p2가 동일하다면 true를 반환한다. */

bool operator != (const point & p1, const point & p2){
    return !(p1==p2) ;
}
// 위의 overloading 된 operator을 사용하여 작성해주었다.
```


