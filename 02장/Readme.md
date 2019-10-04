# ADT and C++ Classes
. h 파일에 다음과 같이 선언문들을 작성해준다.  
You know how to define a new class type, and place the definition in a header file.
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