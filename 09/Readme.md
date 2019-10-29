# Reculsive Thinking

### Reculsive Function
대부분의 reculsion 함수는 if문이 있다.
1. 바로 풀 수 있는 경우
2. 바로 풀 수 없는 경우


예시  

**음이아닌 정수**를 입력받아 수직으로 화면에 출력하기
```cpp
void write_vertical(unsigned int number){
    if(number<10) //stopping case
        cout<<number<<endl;
    else{
        write_vertical(number/10);
        cout << number % 10 << endl;
    }
}
```
**정수**를 입력받아 수직으로 화면에 출력하기
```cpp
void super_write_vertical(int number){
    if(number<0){
        cout<<'-'<<endl;
        super_write_vertical(abs(number));
    }
    else if(number<10)
        cout<<number<<endl;
    else{
        write_vertical(number/10);
        cout << number % 10 << endl;
    }
}
```

Activation Record for a function..36p 정리

46p
49p

```cpp
void random_fractal(double left_height, double right_height, double width, double epsilon){
    double mid_height;
    assert(width>0);
    assert(epsilon>0);
    if(width<=epsilon>)
        display(right_height);
    else{
        mid_height=(left_height+right_height)/2;
        mid_height+=random_real(-width,width);
        random_fractal(left_height,mid_height,width/2,epsilon);
        random_fractal(mid_height,right_height,width/2,epsilon);
    }
}
```